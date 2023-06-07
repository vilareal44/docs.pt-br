---
title: Contadores de desempenho e aplicativos lado a lado em processo
description: Examine os contadores de desempenho e os aplicativos lado a lado no processo no .NET. Use Perfmon.exe para diferenciar os contadores de desempenho em cada tempo de execução.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- performance counters
- performance counters,and in-process side-by-side applications
- performance,.NET Framework applications
- performance monitoring,counters
ms.assetid: 6888f9be-c65b-4b03-a07b-df7ebdee2436
ms.openlocfilehash: eb05d9f5f930420827c6b3d94ea0ed34f64464fd
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803853"
---
# <a name="performance-counters-and-in-process-side-by-side-applications"></a>Contadores de desempenho e aplicativos lado a lado em processo
Usando o Monitor de Desempenho (Perfmon.exe), é possível diferenciar os contadores de desempenho por runtime. Este tópico descreve a alteração do Registro necessária para habilitar essa funcionalidade.  
  
## <a name="the-default-behavior"></a>O comportamento padrão  
 Por padrão, o Monitor de Desempenho exibe os contadores de desempenho classificados por aplicativo. No entanto, há dois cenários em que isso torna-se um problema:  
  
- Ao monitorar dois aplicativos que têm o mesmo nome. Por exemplo, se ambos os aplicativos são nomeados myapp.exe, um é exibido como **myapp** e o outro como **myapp#1** na coluna **Instância**. Nesse caso, é difícil corresponder um contador de desempenho a um aplicativo específico. Não está claro se os dados coletados para **myapp#1** referem-se ao primeiro myapp.exe ou ao segundo.  
  
- Quando um aplicativo usa várias instâncias do Common Language Runtime. O .NET Framework 4 dá suporte a cenários de hospedagem lado a lado no processo; ou seja, um único processo ou aplicativo pode carregar várias instâncias do Common Language Runtime. Se um único aplicativo chamado myapp.exe carrega duas instâncias de runtime, por padrão, elas serão designadas na coluna **Instância** como **myapp** e **myapp#1**. Nesse caso, não está claro se **myapp** e **myapp#1** referem-se a dois aplicativos de mesmo nome ou ao mesmo aplicativo com dois runtimes. Se vários aplicativos com o mesmo nome carregam vários runtimes, a ambiguidade é ainda maior.  
  
 Você pode definir uma chave do Registro para eliminar essa ambiguidade. Para aplicativos desenvolvidos usando o .NET Framework 4, essa alteração de registro adiciona um identificador de processo seguido por um identificador de instância de tempo de execução para o nome do aplicativo na coluna **instância** . Em vez *de #1 de aplicativo ou* *aplicativo*, o aplicativo agora é identificado como *Application*_ `p` *ID* \_ `r` *runtimeId* na coluna de **instância** . Se um aplicativo tiver sido desenvolvido usando uma versão anterior do Common Language Runtime, essa instância será representada como ProcessId do * \_ aplicativo*, `p` *processID* desde que o .NET Framework 4 esteja instalado.  
  
## <a name="performance-counters-for-in-process-side-by-side-applications"></a>Contadores de desempenho para aplicativos lado a lado em processo  
 Para lidar com contadores de desempenho para várias versões do Common Language Runtime hospedadas em um único aplicativo, você deve alterar uma única configuração de chave do Registro, conforme mostrado na tabela a seguir.  
  
|||  
|-|-|  
|Nome da chave|HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\\.NETFramework\Performance|  
|Nome do valor|ProcessNameFormat|  
|Tipo de valor|REG_DWORD|  
|Valor|1 (0x00000001)|  
  
 Um valor de 0 para `ProcessNameFormat` indica que o comportamento padrão está habilitado, ou seja, Perfmon.exe exibe os contadores de desempenho por aplicativo. Quando você define esse valor como 1, Perfmon.exe elimina a ambiguidade de várias versões de um aplicativo e fornece contadores de desempenho por runtime. Qualquer outro valor para a configuração da chave do Registro `ProcessNameFormat` não tem suporte e é reservada para uso futuro.  
  
 Depois de atualizar a configuração da chave do Registro `ProcessNameFormat`, você deve reiniciar Perfmon.exe ou outros consumidores de contadores de desempenho para que o novo recurso de nomeação de instância funcione corretamente.  
  
 O exemplo a seguir mostra como alterar o valor de `ProcessNameFormat` programaticamente.  
  
 [!code-csharp[Conceptual.PerfCounters.InProSxS#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.perfcounters.inprosxs/cs/regsetting1.cs#1)]
 [!code-vb[Conceptual.PerfCounters.InProSxS#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.perfcounters.inprosxs/vb/regsetting1.vb#1)]  
  
 Quando você fizer essa alteração no registro, Perfmon.exe exibirá os nomes dos aplicativos que visam o .NET Framework 4 como *Application*_ `p` *ID* \_ `r` *runtimeId*, em que *Application* é o nome do aplicativo, *ProcessId* é o identificador de processo do aplicativo e *runtimeId* é um identificador de Common Language Runtime. Por exemplo, se um aplicativo chamado myapp.exe carrega duas instâncias nomeadas do Common Language Runtime, Perfmon.exe pode identificar uma instância como myapp_p1416_r10 e a segunda como myapp_p3160_r10. O identificador de runtime apenas retira a ambiguidade os runtimes dentro de um processo; ele não fornece nenhuma outra informação sobre o runtime. (Por exemplo, a ID de runtime não tem nenhuma relação com a versão ou o SKU do runtime.)  
  
 Se o .NET Framework 4 estiver instalado, a alteração do registro também afetará os aplicativos que foram desenvolvidos usando versões anteriores do .NET Framework. Eles aparecem em Perfmon.exe como *application_* `p` *ProcessId*, em que *Application* é o nome do aplicativo e *ProcessId* é o identificador do processo. Por exemplo, se os contadores de desempenho de dois aplicativos chamados myapp.exe são monitorados, um pode aparecer como myapp_p23900 e o outro como myapp_p24908.  
  
> [!NOTE]
> O identificador de processo elimina a ambiguidade da resolução de dois aplicativos com o mesmo nome que usam versões anteriores do runtime. Um identificador de tempo de execução não é necessário para versões anteriores, porque versões anteriores do Common Language Runtime não dão suporte a cenários lado a lado.  
  
 Se o .NET Framework 4 não estiver presente ou tiver sido desinstalado, a definição da chave do registro não terá nenhum efeito. Isso significa que dois aplicativos com o mesmo nome continuarão a aparecer no Perfmon.exe como *aplicativo* e *aplicativo#1* (por exemplo, como **myapp** e **myapp#1**).
