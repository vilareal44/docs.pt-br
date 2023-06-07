---
title: Notificações sobre a coleta de lixo
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- garbage collection, notifications
ms.assetid: e12d8e74-31e3-4035-a87d-f3e66f0a9b89
ms.openlocfilehash: 389e851782edb82578c216951be440070b92723c
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84285996"
---
# <a name="garbage-collection-notifications"></a>Notificações sobre a coleta de lixo
Há situações em que uma coleta de lixo completa (ou seja, uma coleta de geração 2) pelo common language runtime pode afetar negativamente o desempenho. Isso pode ser um problema particularmente com servidores que processam grandes volumes de solicitações; Nesse caso, uma longa coleta de lixo pode causar um tempo limite de solicitação. Para evitar que uma coleção completa ocorra durante um período crítico, você pode ser notificado de que uma coleta de lixo completa está se aproximando e, em seguida, tomará medidas para redirecionar a carga de trabalho para outra instância de servidor. Você também pode induzir uma coleta por conta própria, desde que a instância atual do servidor não precise processar solicitações.  
  
 O método <xref:System.GC.RegisterForFullGCNotification%2A> registra uma notificação para ser gerado quando o runtime detectar que uma coleta de lixo completa está se aproximando. Essa notificação é composta por duas partes: quando a coleta de lixo completa está se aproximando e quando a coleta de lixo completa for concluída.  
  
> [!WARNING]
> Apenas o bloqueio de coletas de lixo geram notificações. Quando o [\<gcConcurrent>](../../framework/configure-apps/file-schema/runtime/gcconcurrent-element.md) elemento de configuração estiver habilitado, as coletas de lixo em segundo plano não gerarão notificações.  
  
 Para determinar quando uma notificação foi gerada, use os métodos <xref:System.GC.WaitForFullGCApproach%2A> e <xref:System.GC.WaitForFullGCComplete%2A>. Normalmente, você pode usar esses métodos em um loop `while` para obter continuamente uma enumeração <xref:System.GCNotificationStatus> que mostra o status da notificação. Se esse valor for <xref:System.GCNotificationStatus.Succeeded>, você pode fazer o seguinte:  
  
- Em resposta a uma notificação obtida com o método <xref:System.GC.WaitForFullGCApproach%2A>, você pode redirecionar a carga de trabalho e, possivelmente, induzir uma coleção por conta própria.  
  
- Em resposta a uma notificação obtida com o método <xref:System.GC.WaitForFullGCComplete%2A>, você pode tornar a instância atual do servidor disponível para processar solicitações novamente. Você também pode coletar informações. Por exemplo, você pode usar o método <xref:System.GC.CollectionCount%2A> para registrar o número de coleções.  
  
 Os métodos <xref:System.GC.WaitForFullGCApproach%2A> e <xref:System.GC.WaitForFullGCComplete%2A> são projetados para trabalhar juntos. Usar um sem o outro pode produzir resultados indeterminados.  
  
## <a name="full-garbage-collection"></a>Coleta de lixo completa  
 O runtime resultará em uma coleta de lixo completa quando qualquer um dos cenários a seguir for verdadeiro:  
  
- Foi promovida memória suficiente para a geração 2 para gerar a próxima coleta de geração 2.  
  
- Foi promovida memória suficiente para o heap de objeto grande para gerar a próxima coleta de geração 2.  
  
- Outros fatores escalam uma coleta de geração 1 para uma coleta de geração 2.  
  
 Os limites que você especificar no método <xref:System.GC.RegisterForFullGCNotification%2A> serão aplicados aos primeiros dois cenários. No entanto, no primeiro cenário, você nem sempre receberá a notificação no momento proporcional aos valores de limite que você especificar por dois motivos:  
  
- O runtime não verifica todas as alocações de objeto pequeno (por motivos de desempenho).  
  
- Somente as coletas da geração 1 promovem a memória na geração 2.  
  
 O terceiro cenário também contribui para a incerteza de quando você receberá a notificação. Embora não seja uma garantia, essa é uma maneira útil de reduzir os efeitos de uma coleta de lixo completa inoportuna ao redirecionar as solicitações durante esse período ou você mesmo induzir a coleta para quando ela puder ser melhor hospedada.  
  
## <a name="notification-threshold-parameters"></a>Parâmetros de limite de notificação  
 O método <xref:System.GC.RegisterForFullGCNotification%2A> tem dois parâmetros para especificar os valores de limite do heap de objeto grande e dos objetos de geração 2. Quando esses valores forem atendidos, uma notificação de coleta de lixo deverá ser gerada. A tabela a seguir descreve esses parâmetros.  
  
|Parâmetro|Description|  
|---------------|-----------------|  
|`maxGenerationThreshold`|Um número entre 1 e 99 que especifica quando a notificação deve ser gerada com base nos objetos promovidos na geração 2.|  
|`largeObjectHeapThreshold`|Um número entre 1 e 99 que especifica quando a notificação deve ser gerada com base nos objetos alocados no heap de objetos grandes.|  
  
 Se você especificar um valor muito alto, a probabilidade de receber uma notificação será muito elevada. No entanto, pode demorar muito até que o runtime gere uma coleta. Se você mesmo induzir uma coleta, poderá recuperar mais objetos que seriam recuperados se o runtime gerasse a coleta.  
  
 Se você especificar um valor muito baixo, o runtime poderá gerar a coleta antes de você ter tido tempo suficiente para ser notificado.  
  
## <a name="example"></a>Exemplo  
  
### <a name="description"></a>Descrição  
 No exemplo a seguir, um grupo de serviço de servidores controla as solicitações da Web recebidas. Para simular a carga de trabalho de processamento de solicitações, matrizes de bytes são adicionadas a uma coleta <xref:System.Collections.Generic.List%601>. Cada servidor registra uma notificação de coleta de lixo e, em seguida, inicia um thread no método de usuário `WaitForFullGCProc` para monitorar continuamente a enumeração <xref:System.GCNotificationStatus> que é retornada pelos métodos <xref:System.GC.WaitForFullGCApproach%2A> e <xref:System.GC.WaitForFullGCComplete%2A>.  
  
 Os métodos <xref:System.GC.WaitForFullGCApproach%2A> e <xref:System.GC.WaitForFullGCComplete%2A> chamam seus respectivos métodos de usuário de manipulação de eventos quando uma notificação é gerada:  
  
- `OnFullGCApproachNotify`  
  
     Este método chama o método de usuário `RedirectRequests` que instrui o servidor de enfileiramento de solicitações a suspender o envio de solicitações ao servidor. Isso é simulado definindo a variável de nível de classe `bAllocate` como `false` para que nenhum outro objeto seja alocado.  
  
     Em seguida, o método de usuário `FinishExistingRequests` é chamado para concluir o processamento das solicitações pendentes do servidor. Isso é simulado desmarcando a coleta <xref:System.Collections.Generic.List%601>.  
  
     Por fim, como a carga de trabalho é leve, uma coleta de lixo é induzida.  
  
- `OnFullGCCompleteNotify`  
  
     Este método chama o método de usuário `AcceptRequests` para retomar a aceitar de solicitações já que o servidor não está mais suscetível à coleta de lixo completa. Essa ação é simulada através da definição da variável `bAllocate` como `true` para que objetos possam continuar sendo adicionados à coleta <xref:System.Collections.Generic.List%601>.  
  
 O código a seguir contém o método `Main` do exemplo.  
  
 [!code-cpp[GCNotification#2](../../../samples/snippets/cpp/VS_Snippets_CLR/GCNotification/cpp/program.cpp#2)]
 [!code-csharp[GCNotification#2](../../../samples/snippets/csharp/VS_Snippets_CLR/GCNotification/cs/Program.cs#2)]
 [!code-vb[GCNotification#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GCNotification/vb/program.vb#2)]  
  
 O código a seguir contém o método de usuário `WaitForFullGCProc` que contém um loop while contínuo para verificar se há notificações de coleta de lixo.  
  
 [!code-cpp[GCNotification#8](../../../samples/snippets/cpp/VS_Snippets_CLR/GCNotification/cpp/program.cpp#8)]
 [!code-csharp[GCNotification#8](../../../samples/snippets/csharp/VS_Snippets_CLR/GCNotification/cs/Program.cs#8)]
 [!code-vb[GCNotification#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GCNotification/vb/program.vb#8)]  
  
 O código a seguir contém o método `OnFullGCApproachNotify` como chamado no  
  
 Método `WaitForFullGCProc`.  
  
 [!code-cpp[GCNotification#5](../../../samples/snippets/cpp/VS_Snippets_CLR/GCNotification/cpp/program.cpp#5)]
 [!code-csharp[GCNotification#5](../../../samples/snippets/csharp/VS_Snippets_CLR/GCNotification/cs/Program.cs#5)]
 [!code-vb[GCNotification#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GCNotification/vb/program.vb#5)]  
  
 O código a seguir contém o método `OnFullGCApproachComplete` como chamado no  
  
 Método `WaitForFullGCProc`.  
  
 [!code-cpp[GCNotification#6](../../../samples/snippets/cpp/VS_Snippets_CLR/GCNotification/cpp/program.cpp#6)]
 [!code-csharp[GCNotification#6](../../../samples/snippets/csharp/VS_Snippets_CLR/GCNotification/cs/Program.cs#6)]
 [!code-vb[GCNotification#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GCNotification/vb/program.vb#6)]  
  
 O código a seguir contém os métodos de usuário que são chamados nos métodos `OnFullGCApproachNotify` e `OnFullGCCompleteNotify`. Os métodos de usuário redirecionam as solicitações, concluem as solicitações existentes e, em seguida, retomam solicitações após a conclusão da coleta de lixo completa.  
  
 [!code-cpp[GCNotification#9](../../../samples/snippets/cpp/VS_Snippets_CLR/GCNotification/cpp/program.cpp#9)]
 [!code-csharp[GCNotification#9](../../../samples/snippets/csharp/VS_Snippets_CLR/GCNotification/cs/Program.cs#9)]
 [!code-vb[GCNotification#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GCNotification/vb/program.vb#9)]  
  
 O exemplo de código completo é o seguinte:  
  
 [!code-cpp[GCNotification#1](../../../samples/snippets/cpp/VS_Snippets_CLR/GCNotification/cpp/program.cpp#1)]
 [!code-csharp[GCNotification#1](../../../samples/snippets/csharp/VS_Snippets_CLR/GCNotification/cs/Program.cs#1)]
 [!code-vb[GCNotification#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GCNotification/vb/program.vb#1)]  
  
## <a name="see-also"></a>Veja também

- [Coleta de lixo](index.md)
