---
title: MDA invalidOverlappedToPinvoke
description: Examine o MDA (Assistente de depuração gerenciada) do invalidOverlappedToPinvoke no .NET, que pode ser ativado durante uma falha ou uma corrupção de heap inexplicativa.
ms.date: 03/30/2017
helpviewer_keywords:
- overlapped pointers
- managed debugging assistants (MDAs), overlapped pointers
- invalid overlapped pointer to platform invoke
- InvalidOverlappedToPinvoke MDA
- MDAs (managed debugging assistants), overlapped pointers
- pointers, overlapped
ms.assetid: 28876047-58bd-4fed-9452-c7da346d67c0
ms.openlocfilehash: 162efd55bf636cf2e8698706bd011379f2f6f11f
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051695"
---
# <a name="invalidoverlappedtopinvoke-mda"></a>MDA invalidOverlappedToPinvoke
O MDA (Assistente de Depuração Gerenciado) de `invalidOverlappedToPinvoke` é ativado quando um ponteiro sobreposto que não foi criado no heap de coleta de lixo é passado para funções específicas do Win32.  
  
> [!NOTE]
> Por padrão, esse MDA é ativado somente se a chamada de invocação de plataforma é definida no seu código e o depurador relata o status de JustMyCode de cada método. Um depurador que não entende JustMyCode (tal como MDbg.exe sem nenhuma extensão) não ativará esse MDA. Esse MDA pode ser habilitado para esses depuradores usando um arquivo de configuração e configurando `justMyCode="false"` explicitamente no `(<invalidOverlappedToPinvoke enable="true" justMyCode="false"/>` do arquivo .mda.config).  
  
## <a name="symptoms"></a>Sintomas  
 Falhas ou corrupção de heap inexplicáveis.  
  
## <a name="cause"></a>Causa  
 Um ponteiro sobreposto que não foi criado no heap de coleta de lixo é passado para as funções de sistema operacional específicas.  
  
 A tabela a seguir mostra as funções que esse MDA acompanha.  
  
|Módulo|Função|  
|------------|--------------|  
|HttpApi.dll|`HttpReceiveHttpRequest`|  
|IpHlpApi.dll|`NotifyAddrChange`|  
|kernel32.dll|`ReadFile`|  
|kernel32.dll|`ReadFileEx`|  
|kernel32.dll|`WriteFile`|  
|kernel32.dll|`WriteFileEx`|  
|kernel32.dll|`ReadDirectoryChangesW`|  
|kernel32.dll|`PostQueuedCompletionStatus`|  
|MSWSock.dll|`ConnectEx`|  
|WS2_32.dll|`WSASend`|  
|WS2_32.dll|`WSASendTo`|  
|WS2_32.dll|`WSARecv`|  
|WS2_32.dll|`WSARecvFrom`|  
|MQRT.dll|`MQReceiveMessage`|  
  
 O potencial de corrupção de heap é alto para essa condição porque o <xref:System.AppDomain> fazendo a chamada pode ser descarregado. Se o <xref:System.AppDomain> for descarregado, o código do aplicativo liberará a memória para o ponteiro sobreposto causando corrupção quando a operação for concluída ou então o código causará perda de memória, resultando em problemas mais tarde.  
  
## <a name="resolution"></a>Resolução  
 Use um objeto <xref:System.Threading.Overlapped>, chamando o método <xref:System.Threading.Overlapped.Pack%2A> para obter uma estrutura <xref:System.Threading.NativeOverlapped> que pode ser passada para a função. Se o <xref:System.AppDomain> for descarregado, o CLR aguardará até que a operação assíncrona seja concluída antes de liberar o ponteiro.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Esse MDA não teve efeito sobre o CLR.  
  
## <a name="output"></a>Saída  
 A seguir temos um exemplo de saída desse MDA.  
  
 `An overlapped pointer (0x00ea3430) that was not allocated on the GC heap was passed via Pinvoke to the Win32 function 'WriteFile' in module 'KERNEL32.DLL'. If the AppDomain is shut down, this can cause heap corruption when the async I/O completes. The best solution is to pass a NativeOverlapped structure retrieved from a call to System.Threading.Overlapped.Pack(). If the AppDomain exits, the CLR will keep this structure alive and pinned until the I/O completes.`  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <invalidOverlappedToPinvoke/>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
- [Realizando marshaling de interoperabilidade](../interop/interop-marshaling.md)
