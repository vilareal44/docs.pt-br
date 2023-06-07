---
title: MDA pInvokeLog
description: Entenda o MDA (Assistente de depuração gerenciada) pInvokeLog, que é ativado para cada assinatura de invocação de plataforma exclusiva usada durante a execução no .NET.
ms.date: 03/30/2017
helpviewer_keywords:
- signatures, platform invoke
- MDAs (managed debugging assistants), platform invoke
- platform invoke, run-time errors
- platform invoke log
- PInvokeLog MDA
- managed debugging assistants (MDAs), platform invoke
ms.assetid: b830444a-5003-49fe-b89b-b8bee22f7b1a
ms.openlocfilehash: 05af4e17a91f7c0d8f3576a86d3d784ef6666aed
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803684"
---
# <a name="pinvokelog-mda"></a>MDA pInvokeLog
O MDA (Assistente de Depuração Gerenciado) de `pInvokeLog` é ativado para cada assinatura de invocação de plataforma exclusiva usada durante a execução.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Esse MDA não tem efeito sobre o CLR.  
  
## <a name="output"></a>Saída  
 Uma mensagem que indica a assinatura de invocação de plataforma usada durante a execução.  
  
## <a name="configuration"></a>Configuração  
 Cada elemento de correspondência filtra os arquivos .dll para os quais as chamadas de invocação de plataforma são feitas.  
  
```xml  
<mdaConfig>  
  <assistants>  
    <pInvokeLog>  
      <filter>  
        <match dllName="user32.dll"/>  
        <match dllName="kernel32.dll"/>  
      </filter>  
    </pInvokeLog>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>Consulte também

- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
- [Consumindo funções de DLL não gerenciadas](../interop/consuming-unmanaged-dll-functions.md)
