---
title: MDA nonComVisibleBaseClass
description: Consulte o MDA (Assistente de depuração gerenciada) nonComVisibleBaseClass, que é invocado em chamadas de QueryInterface de código nativo falhando com COR_E_INVALIDOPERATION.
ms.date: 03/30/2017
helpviewer_keywords:
- visible classes
- managed debugging assistants (MDAs), COM visible classes
- nonComVisibleBaseClass MDA
- COM visible classes
- QueryInterface call failures
- MDAs (managed debugging assistants), COM visible classes
ms.assetid: 9ec1af27-604b-477e-9ee2-e833eb10d3ce
ms.openlocfilehash: 9f32b2c57f50fcd900b1fd78f4f8df1ec656a6db
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803906"
---
# <a name="noncomvisiblebaseclass-mda"></a>MDA nonComVisibleBaseClass
O MDA (Assistente de Depuração Gerenciado) de `nonComVisibleBaseClass` é ativado quando uma chamada `QueryInterface` é feita por código não gerenciado ou nativo no CCW (COM Callable Wrapper) de uma classe gerenciada visível em COM derivada de uma classe base que não é visível em COM.  A chamada `QueryInterface` faz com que o MDA seja ativado apenas em casos nos quais a chamada solicita a interface de classe ou o `IDispatch` padrão da classe gerenciada visível em COM.  O MDA não é ativado quando o `QueryInterface` é para uma interface explícita que tem o atributo <xref:System.Runtime.InteropServices.ClassInterfaceAttribute> aplicado e é implementado explicitamente pela classe visível em COM.  
  
## <a name="symptoms"></a>Sintomas  
 Uma chamada `QueryInterface` feita por meio do código nativo que está falhando com um HRESULT COR_E_INVALIDOPERATION.  O HRESULT pode ser devido ao runtime não permitir chamadas `QueryInterface` que causariam a ativação desse MDA.  
  
## <a name="cause"></a>Causa  
 O runtime não pode permitir chamadas `QueryInterface` para a interface de classe ou a interface `IDispatch` padrão de uma classe visível em COM que deriva de uma classe não visível em COM devido a possíveis problemas de controle de versão.  Por exemplo, se os membros públicos foram adicionados à a classe base que não é visível em COM, os clientes COM existentes usando a classe derivada podem ser interrompidos porque a vtable da classe derivada, que contém os membros da classe base, seria modificada por uma alteração desse tipo.  Interfaces explícitas expostas a COM não têm esse problema porque elas não incluem os membros base das interfaces na vtable.  
  
## <a name="resolution"></a>Resolução  
 Não expor a interface de classe. Definir uma interface explícita e aplicar o atributo <xref:System.Runtime.InteropServices.ClassInterfaceAttribute> a ele.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Esse MDA não tem efeito sobre o CLR.  
  
## <a name="output"></a>Saída  
 A seguir está um exemplo de mensagem para uma chamada `QueryInterface` em uma classe visível em COM `Derived` que deriva de uma classe não visível em COM `Base`.  
  
```output
A QueryInterface call was made requesting the class interface of COM
visible managed class 'Derived'. However since this class derives from
non COM visible class 'Base', the QueryInterface call will fail. This
is done to prevent the non COM visible base class from being
constrained by the COM versioning rules.
```  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <nonComVisibleBaseClass />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
- [Realizando marshaling de interoperabilidade](../interop/interop-marshaling.md)
