---
title: MDA invalidVariant
description: Examine o assistente de depuração gerenciada do invalidVariant, que será invocado se uma variante inválida for encontrada em uma chamada de código nativo/não gerenciado para gerenciado.
ms.date: 03/30/2017
helpviewer_keywords:
- MDAs (managed debugging assistants), invalid variant
- VARIANT type errors
- InvalidVariant MDA
- invalid VARIANT types
- managed debugging assistants (MDAs), invalid variant
ms.assetid: d273e070-d1b1-4a53-a9c7-7af837b04a3d
ms.openlocfilehash: ab1233d9faa86ef1508fa8fe2b5af46cb37bd523
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051630"
---
# <a name="invalidvariant-mda"></a>MDA invalidVariant
O MDA (Assistente de Depuração Gerenciado) de `invalidVariant` é ativado quando uma estrutura `VARIANT` inválida é encontrada durante uma chamada de código não gerenciado ou nativo para código gerenciado.  
  
## <a name="symptoms"></a>Sintomas  
 Um comportamento inesperado durante a transição entre código nativo e gerenciado que envolve o marshaling de um `VARIANT` para um objeto.  
  
## <a name="cause"></a>Causa  
 Código nativo está passando uma estrutura `VARIANT` malformada para código gerenciado.  O runtime tenta realizar marshaling dessa `VARIANT` para um objeto e ativa o MDA se o `VARIANT` não é válido. Exemplos de `VARIANT`S inválidos incluem um `VARIANT` com `VARTYPE` VT_EMPTY &#124; VT_BYREF ou um `VARIANT` com `VARTYPE` VT_VARIANT.  
  
## <a name="resolution"></a>Resolução  
 O código não gerenciado ou nativo passando o `VARIANT` deve garantir que o `VARIANT` seja corretamente formado e inicializado.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 O MDA não tem nenhum efeito sobre o comportamento do runtime.  
  
## <a name="output"></a>Saída  
 Uma mensagem MDA indicando que o runtime detectou inválido `VARIANT` passado para código gerenciado por um módulo não gerenciado.  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <invalidVariant />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
- [Realizando marshaling de interoperabilidade](../interop/interop-marshaling.md)
