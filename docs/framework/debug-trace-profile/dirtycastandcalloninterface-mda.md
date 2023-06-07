---
title: MDA dirtyCastAndCallOnInterface
description: Examine o assistente de depuração gerenciada do dirtyCastAndCallOnInterface, que é invocado quando chamadas vtable de ligação antecipada são feitas em interfaces de classe somente de associação tardia.
ms.date: 03/30/2017
helpviewer_keywords:
- managed debugging assistants (MDAs), early bound calls AutoDispatch
- dispatch-only mode
- dirtyCastAndCallOnInterface
- early-bound managed classes
- early bound calls on AutoDispatch
- MDAs (managed debugging assistants), early bound calls AutoDispatch
- EarlyBoundCallOnAutorDispatchClassInteface MDA
ms.assetid: aa388ed3-7e3d-48ea-a0b5-c47ae19cec38
ms.openlocfilehash: 2ed5589909915a261a22c48490e469ae52659c8c
ms.sourcegitcommit: a2c8b19e813a52b91facbb5d7e3c062c7188b457
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85416064"
---
# <a name="dirtycastandcalloninterface-mda"></a>MDA dirtyCastAndCallOnInterface
O MDA (assistente para depuração gerenciada) `dirtyCastAndCallOnInterface` é ativado quando há uma tentativa de realizar uma chamada de associação inicial por meio de uma vtable em uma interface de classe que foi marcada como somente associação tardia.  
  
## <a name="symptoms"></a>Sintomas  
 Um aplicativo gera uma violação de acesso ou tem um comportamento inesperado quando você faz uma chamada de associação inicial por meio do COM ao CLR.  
  
## <a name="cause"></a>Causa  
 O código está tentando fazer uma chamada de associação inicial por meio de uma vtable em uma interface de classe que é somente associação tardia. Observe que, por padrão, as interfaces de classe são identificadas como sendo somente de associação tardia. Elas também podem ser identificadas como sendo de associação tardia com o atributo <xref:System.Runtime.InteropServices.ClassInterfaceAttribute> com um valor <xref:System.Runtime.InteropServices.ClassInterfaceType.AutoDispatch> (`[ClassInterface(ClassInterfaceType.AutoDispatch)]`).  
  
## <a name="resolution"></a>Resolução  
 A resolução recomendada é definir uma interface explícita para uso pelo COM e fazer com que os clientes COM façam a chamada por meio dessa interface, em vez de por meio da interface de classe gerada automaticamente. Como alternativa, a chamada do COM pode ser transformada em uma chamada de associação tardia por meio de `IDispatch`.  
  
 Por fim, é possível identificar a classe como <xref:System.Runtime.InteropServices.ClassInterfaceType.AutoDual> (`[ClassInterface(ClassInterfaceType.AutoDual)]`) para permitir que as chamadas de associação inicial sejam feitas por meio do COM; no entanto, o uso de <xref:System.Runtime.InteropServices.ClassInterfaceType.AutoDual> não é recomendado devido às limitações de controle de versão descritas no <xref:System.Runtime.InteropServices.ClassInterfaceAttribute>.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Esse MDA não tem efeito sobre o CLR. Ele apenas relata dados sobre chamadas de associação inicial em interfaces de associação tardia.  
  
## <a name="output"></a>Saída  
 O nome do método ou o nome do campo que está sendo acessado com associação inicial.  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <dirtyCastAndCallOnInterface />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Runtime.InteropServices.ClassInterfaceAttribute>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
