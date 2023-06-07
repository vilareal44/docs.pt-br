---
title: MDA invalidFunctionPointerInDelegate
description: Examine o MDA (Assistente de depuração gerenciada) invalidFunctionPointerInDelegate, que será invocado se um ponteiro de função inválido for passado para criar um delegado.
ms.date: 03/30/2017
helpviewer_keywords:
- invalidFunctionPointerInDelegate MDA
- managed debugging assistants (MDAs), invalid function pointer to delegates
- MDAs (managed debugging assistants), invalid function pointer to delegates
- function pointers, invalid
- marshaling, run-time errors
- managed debugging assistants (MDAs), marshaling
- MDAs (managed debugging assistants), marshaling
- invalid function pointers
ms.assetid: 99ae44f1-783e-49a9-9009-24f54bbd0f09
ms.openlocfilehash: a17427d117c62ba782af3c9549c84623a3013b06
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051734"
---
# <a name="invalidfunctionpointerindelegate-mda"></a>MDA invalidFunctionPointerInDelegate
O `invalidFunctionPointerInDelegate` MDA (assistente de depuração gerenciada) é ativado quando é informado um ponteiro de função inválido para construir um representante ao invés de um ponteiro de função nativo.  
  
## <a name="symptoms"></a>Sintomas  
 Violações de acesso ou dano inesperado à memória ao usar um representante no lugar de um ponteiro de função.  
  
## <a name="cause"></a>Causa  
 Foi especificado um ponteiro de função inválido.  
  
## <a name="resolution"></a>Resolução  
 Especifique um ponteiro de função válido  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Esse MDA não tem efeito sobre o CLR.  
  
## <a name="output"></a>Saída  
 O ponteiro de função inválido.  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <invalidFunctionPointerInDelegate />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
- [Realizando marshaling de interoperabilidade](../interop/interop-marshaling.md)
