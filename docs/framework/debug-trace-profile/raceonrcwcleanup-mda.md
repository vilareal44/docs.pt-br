---
title: MDA raceOnRCWCleanup
description: Examine o MDA (Assistente de depuração gerenciada) raceOnRCWCleanup, que será ativado se o RCW estiver em uso em outro thread ou na pilha de threads de liberação no .NET.
ms.date: 03/30/2017
helpviewer_keywords:
- RCW
- managed debugging assistants (MDAs), RCWs
- race on RCW cleanup
- MDAs (managed debugging assistants), RCWs
- RaceOnRCWCleanup MDA
- runtime callable wrappers
ms.assetid: bee1e9b1-50a8-4c89-9cd9-7dd6b2458187
ms.openlocfilehash: e86ef96bebb648c7927ae5fec8b68fc4429b268b
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803645"
---
# <a name="raceonrcwcleanup-mda"></a>MDA raceOnRCWCleanup
O MDA (Assistente de Depuração Gerenciado) de `raceOnRCWCleanup` é ativado quando o CLR (Common Language Runtime) detecta que um [RCW](../../standard/native-interop/runtime-callable-wrapper.md) (Runtime Callable Wrapper) está em uso quando uma chamada para liberá-lo é feita usando um comando, assim como o método <xref:System.Runtime.InteropServices.Marshal.ReleaseComObject%2A?displayProperty=nameWithType>.  
  
## <a name="symptoms"></a>Sintomas  
 Violações de acesso ou corrupção de memória durante após liberar um RCW usando <xref:System.Runtime.InteropServices.Marshal.ReleaseComObject%2A> ou um método semelhante.  
  
## <a name="cause"></a>Causa  
 O RCW está em uso em outro thread ou na pilha do thread de liberação.  Não é possível liberar um RCW que está em uso.  
  
## <a name="resolution"></a>Resolução  
 Não libere um RCW que possa estar em uso no thread atual ou em outros.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Esse MDA não tem efeito sobre o CLR.  
  
## <a name="output"></a>Saída  
 Uma mensagem que descreve o erro.  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <raceOnRCWCleanup/>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
- [Realizando marshaling de interoperabilidade](../interop/interop-marshaling.md)
