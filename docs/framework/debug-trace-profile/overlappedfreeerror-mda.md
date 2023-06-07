---
title: MDA overlappedFreeError
description: Examine o MDA (Assistente de depuração gerenciada) do overlappedFreeError no .NET, que pode ser ativado em violações de acesso ou corrupção do heap coletado por lixo.
ms.date: 03/30/2017
helpviewer_keywords:
- OverlappedFreeError MDA
- overlapped free method call error
- managed debugging assistants (MDAs), overlapped structures
- overlapped structures
- MDAs (managed debugging assistants), overlapped structures
- freeing overlapped structures
ms.assetid: b6ab2d48-6eee-4bab-97a3-046b3b0a5470
ms.openlocfilehash: 9be33c59723ecb2743f2bc610d7fb69d24ff388c
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803905"
---
# <a name="overlappedfreeerror-mda"></a>MDA overlappedFreeError
O MDA (Assistente de Depuração Gerenciado) de `overlappedFreeError` é ativado quando o método <xref:System.Threading.Overlapped.Free%28System.Threading.NativeOverlapped%2A%29?displayProperty=nameWithType> é chamado antes da conclusão da operação sobreposta.  
  
## <a name="symptoms"></a>Sintomas  
 Violações de acesso ou corrupção de heap coletado como lixo.  
  
## <a name="cause"></a>Causa  
 Uma estrutura sobreposta foi liberada antes da operação ter sido concluída. A função que está usando o ponteiro sobreposto pode gravar a estrutura mais tarde, depois de ele ter sido liberado. Isso pode causar corrupção de heap porque outro objeto agora pode ocupar essa região.  
  
 Esse MDA poderá não representar um erro se a operação sobreposta não for iniciada com êxito.  
  
## <a name="resolution"></a>Resolução  
 Verifique se a operação de E/S usando a estrutura sobreposta foi concluída antes de chamar o método <xref:System.Threading.Overlapped.Free%28System.Threading.NativeOverlapped%2A%29>.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Esse MDA não tem efeito sobre o CLR.  
  
## <a name="output"></a>Saída  
 O demonstrado a seguir é uma saída de exemplo para esse MDA.  
  
 `An overlapped pointer (0x00ea3430) that was not allocated on the GC heap was passed via Pinvoke to the win32 function 'WriteFile' in module 'KERNEL32.DLL'. If the AppDomain is shut down, this can cause heap corruption when the async I/O completes. The best solution is to pass a NativeOverlappedStructure retrieved from a call to System.Threading.Overlapped.Pack(). If the AppDomain exits, the CLR will keep this structure alive and pinned until the I/O completes.`  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <overlappedFreeError/>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
- [Realizando marshaling de interoperabilidade](../interop/interop-marshaling.md)
