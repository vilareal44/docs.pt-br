---
title: MDA marshalCleanupError
description: Examine o MDA (Assistente de depuração gerenciada) marshalCleanupError, que é invocado porque ocorreu um erro inesperado ao limpar estruturas temporárias.
ms.date: 03/30/2017
helpviewer_keywords:
- cleanup operations
- marshaling, run-time errors
- managed debugging assistants (MDAs), marshaling
- MDAs (managed debugging assistants), marshaling
- marshaling cleanup error
- MarshalCleanupError MDA
- memory, cleanup errors
ms.assetid: 2f5d9e7c-ae51-4155-a435-54347aa1f091
ms.openlocfilehash: 3c7498d6f51484de3a2e84d611a2634f53724ab6
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051604"
---
# <a name="marshalcleanuperror-mda"></a>MDA marshalCleanupError
O MDA (assistente de depuração gerenciado) do `marshalCleanupError` é ativado quando o CLR (Common Language Runtime) encontra um erro ao tentar limpar estruturas temporárias e a memória usada para realizar marshaling de tipos de dados entre limites de código gerenciado e nativo.  
  
## <a name="symptoms"></a>Sintomas  
 A perda de memória ocorre em transações de código gerenciado e nativo, no estado de runtime, como quando a cultura de thread não é restaurada ou quando há um erro na limpeza de <xref:System.Runtime.InteropServices.SafeHandle>.  
  
## <a name="cause"></a>Causa  
 Ocorreu um erro inesperado durante a limpeza das estruturas temporárias.  
  
## <a name="resolution"></a>Resolução  
 Verifique se há erro em todas as implementações do destruidor, do finalizador e do marshaler personalizado <xref:System.Runtime.InteropServices.SafeHandle>.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Esse MDA não tem efeito sobre o CLR.  
  
## <a name="output"></a>Saída  
 Uma mensagem que indica que a operação falhou durante a limpeza.  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <marshalCleanupError />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
- [Realizando marshaling de interoperabilidade](../interop/interop-marshaling.md)
