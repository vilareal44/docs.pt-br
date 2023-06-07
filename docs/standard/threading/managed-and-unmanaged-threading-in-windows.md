---
title: Threading gerenciado e não gerenciado no Windows
ms.date: 10/24/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- threading [.NET Framework], unmanaged
- threading [.NET Framework], managed
- threading [.NET], managed
- threads and fibers [.NET]
- managed threading
ms.assetid: 4fb6452f-c071-420d-9e71-da16dee7a1eb
ms.openlocfilehash: de823297540d5ce3740a26614dbb9a82881decf3
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86924377"
---
# <a name="managed-and-unmanaged-threading-in-windows"></a>Threading gerenciado e não gerenciado no Windows

O gerenciamento de todos os threads, inclusive de threads criados pelo Common Language Runtime e fora do tempo de execução que ingressam no ambiente gerenciado para executar o código, é feito pela classe <xref:System.Threading.Thread>. O runtime monitora todos os threads de seu processo que já executaram o código no ambiente de execução gerenciado. Ele não rastreia nenhum outro thread. Os threads podem ingressar no ambiente de execução gerenciado por meio da interoperabilidade COM (porque o runtime expõe os objetos gerenciados como objetos COM ao mundo não gerenciado), da função COM [DllGetClassObject](/windows/desktop/api/combaseapi/nf-combaseapi-dllgetclassobject) e da invocação de plataforma.  
  
 Quando um thread não gerenciado, por exemplo, um COM Callable Wrapper, ingressa no runtime, o sistema verifica o repositório de threads locais do thread em questão para procurar um objeto <xref:System.Threading.Thread> interno gerenciado. Caso o sistema encontre um thread desse tipo, o runtime fica ciente sobre ele. Caso contrário, o runtime compila um novo objeto <xref:System.Threading.Thread> e o instala no repositório de threads locais do thread em questão.  
  
 No threading gerenciado, <xref:System.Threading.Thread.GetHashCode%2A?displayProperty=nameWithType> é a identificação estável do thread gerenciado. Durante seu tempo de vida, o thread não colidirá com os valores de outros threads, independente do domínio do aplicativo para o qual você obtém esse valor.  
  
## <a name="mapping-from-win32-threading-to-managed-threading"></a>Mapeando do threading do Win32 para o threading gerenciado

 A tabela a seguir mapeia os elementos do threading do Win32 com seu equivalente aproximado no runtime. Esse mapeamento não representa uma funcionalidade idêntica. Por exemplo, **TerminateThread** não executa cláusulas **finally** nem libera recursos, e não é possível evitá-lo. No entanto, <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> executa todo o código de reversão, recupera todos os recursos e pode ser negado com <xref:System.Threading.Thread.ResetAbort%2A>. Leia a documentação com atenção e não faça deduções sobre a funcionalidade.  
  
|No Win32|No Common Language Runtime|  
|--------------|------------------------------------|  
|**CreateThread**|Combinação de **Thread** e <xref:System.Threading.ThreadStart>|  
|**TerminateThread**|<xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>|  
|**SuspendThread**|<xref:System.Threading.Thread.Suspend%2A?displayProperty=nameWithType>|  
|**ResumeThread**|<xref:System.Threading.Thread.Resume%2A?displayProperty=nameWithType>|  
|**Modo de suspensão**|<xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType>|  
|**WaitForSingleObject** no identificador de thread|<xref:System.Threading.Thread.Join%2A?displayProperty=nameWithType>|  
|**ExitThread**|Sem equivalente|  
|**GetCurrentThread**|<xref:System.Threading.Thread.CurrentThread%2A?displayProperty=nameWithType>|  
|**SetThreadPriority**|<xref:System.Threading.Thread.Priority%2A?displayProperty=nameWithType>|  
|Sem equivalente|<xref:System.Threading.Thread.Name%2A?displayProperty=nameWithType>|  
|Sem equivalente|<xref:System.Threading.Thread.IsBackground%2A?displayProperty=nameWithType>|  
|Próximo a **CoInitializeEx** (OLE32.DLL)|<xref:System.Threading.Thread.ApartmentState%2A?displayProperty=nameWithType>|  
  
## <a name="managed-threads-and-com-apartments"></a>Threads gerenciados e apartments COM

Um thread gerenciado pode ser marcado para indicar que ele irá hospedar um [single-threaded apartament](/windows/desktop/com/single-threaded-apartments) ou um [multithread apartament](/windows/desktop/com/multithreaded-apartments). (Para obter mais informações sobre a arquitetura de Threading COM, consulte [Processes, threads e Apartments](/windows/desktop/com/processes--threads--and-apartments).) Os <xref:System.Threading.Thread.GetApartmentState%2A> <xref:System.Threading.Thread.SetApartmentState%2A> métodos, e <xref:System.Threading.Thread.TrySetApartmentState%2A> da <xref:System.Threading.Thread> classe retornam e atribuem o estado de apartment de um thread. Se o estado não tiver sido definido, <xref:System.Threading.Thread.GetApartmentState%2A> retorna <xref:System.Threading.ApartmentState.Unknown?displayProperty=nameWithType>.  
  
 Você só pode definir uma propriedade quando o thread estiver no estado <xref:System.Threading.ThreadState.Unstarted?displayProperty=nameWithType>; ela só pode ser definida uma vez por thread.  
  
 Se o estado do apartment não for definido antes do início do thread, o thread será iniciado como MTA (multithreaded apartment). O thread finalizador e todos os threads controlados por <xref:System.Threading.ThreadPool> são do tipo MTA.  
  
> [!IMPORTANT]
> No caso do código de inicialização do aplicativo, a única forma de controlar o estado do apartment é aplicar <xref:System.MTAThreadAttribute> ou <xref:System.STAThreadAttribute> ao procedimento do ponto de entrada. No .NET Framework 1.0 e 1.1, você pode definir a propriedade <xref:System.Threading.Thread.ApartmentState%2A> como a primeira linha de código. Isso não é permitido no .NET Framework 2.0.  
  
 Os objetos gerenciados que são expostos ao comportamento COM comportam-se como se houvesse um marshaler de thread livre agregado a eles. Em outras palavras, eles podem ser chamados por qualquer apartment COM no modo de threading livre. Os únicos objetos gerenciados que não apresentam esse comportamento são os objetos derivados de <xref:System.EnterpriseServices.ServicedComponent> ou <xref:System.Runtime.InteropServices.StandardOleMarshalObject>.  
  
 No mundo gerenciado, não há suporte a <xref:System.Runtime.Remoting.Contexts.SynchronizationAttribute>, a menos que você use contextos e instâncias gerenciadas vinculadas a um contexto. Se você estiver usando o Enterprise Services, seu objeto deve ser derivado de <xref:System.EnterpriseServices.ServicedComponent> (que, por sua vez, é derivado de <xref:System.ContextBoundObject>).  
  
 Quando o código gerenciado chama objetos COM, ele sempre segue as regras COM. Em outras palavras, ele faz a chamada por meio de proxies de apartment COM e COM+ 1.0 context wrappers, como ditado por OLE32.  
  
## <a name="blocking-issues"></a>Problemas de bloqueio  

Se um thread fizer uma chamada não gerenciada no sistema operacional que bloqueou o thread no código não gerenciado, o runtime não assumirá o controle para <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> nem <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>. No caso do <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>, o runtime marca o thread como **Abortar** e assume o controle dele quando esse ingressa no código gerenciado. Recomendamos que você use bloqueios gerenciados, em vez de não gerenciados. <xref:System.Threading.WaitHandle.WaitOne%2A?displayProperty=nameWithType>, <xref:System.Threading.WaitHandle.WaitAny%2A?displayProperty=nameWithType>, <xref:System.Threading.WaitHandle.WaitAll%2A?displayProperty=nameWithType>, <xref:System.Threading.Monitor.Enter%2A?displayProperty=nameWithType>, <xref:System.Threading.Monitor.TryEnter%2A?displayProperty=nameWithType>, <xref:System.Threading.Thread.Join%2A?displayProperty=nameWithType>, <xref:System.GC.WaitForPendingFinalizers%2A?displayProperty=nameWithType>, entre outros, respondem a <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> e a <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>. Além disso, se seu thread estiver em um single-threaded apartment, todas essas operações de bloqueio gerenciado enviarão mensagens para seu apartment enquanto o thread estiver bloqueado.  

## <a name="threads-and-fibers"></a>Threads e fibras

Modelo de threading do .NET não é compatível com [fibras](/windows/desktop/procthread/fibers). Você não deve chamar nenhuma função não gerenciada implementada usando fibras. Essas chamadas podem resultar em uma falha de runtime do .NET.

## <a name="see-also"></a>Confira também

- <xref:System.Threading.Thread.ApartmentState%2A?displayProperty=nameWithType>
- <xref:System.Threading.ThreadState>
- <xref:System.EnterpriseServices.ServicedComponent>
- <xref:System.Threading.Thread>
- <xref:System.Threading.Monitor>
