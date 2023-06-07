---
title: Armadilhas em potencial em dados e paralelismo da tarefa
description: Saiba mais sobre as possíveis armadilhas em paralelismo de dados e tarefas, pois o paralelismo adiciona complexidade que não é encontrada em código sequencial.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parallel programming, pitfalls
ms.assetid: 1e357177-e699-4b8f-9e49-56d3513ed128
ms.openlocfilehash: 05d934b80e60a8630db5b70e16a07c014598487a
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599757"
---
# <a name="potential-pitfalls-in-data-and-task-parallelism"></a>Armadilhas em potencial em dados e paralelismo da tarefa
Em muitos casos, o <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> e <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> podem melhorar consideravelmente o desempenho em comparação com consultas sequenciais comuns. No entanto, o trabalho de paralelizar o loop apresenta complexidade que pode levar a problemas que, em código sequencial, não são tão comuns ou não são encontrados. Este tópico lista algumas práticas a serem evitadas ao escrever loops paralelos.  
  
## <a name="do-not-assume-that-parallel-is-always-faster"></a>Não suponha que paralelo é sempre mais rápido  
 Em certos casos, um loop paralelo pode executar com mais lentidão do que seu equivalente sequencial. A regra básica é que os loops paralelos que têm poucas interações e delegados de usuários rápidos provavelmente não acelerarão muito. No entanto, como muitos fatores estão envolvidos no desempenho, recomendamos sempre a medição dos resultados reais.  
  
## <a name="avoid-writing-to-shared-memory-locations"></a>Evite gravar em locais de memória compartilhada  
 No código sequencial, não é incomum ler ou gravar em variáveis estáticas ou campos de classe. No entanto, sempre que vários threads estão acessando essas variáveis simultaneamente, existe um grande potencial para condições de corrida. Embora você possa usar bloqueios para sincronizar o acesso à variável, o custo da sincronização pode prejudicar o desempenho. Portanto, recomendamos que você evite, ou pelo menos limite, o acesso ao estado compartilhado em um loop paralelo tanto quanto possível. A melhor maneira de fazer isso é usar as sobrecargas de <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> e <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> que usam uma variável <xref:System.Threading.ThreadLocal%601?displayProperty=nameWithType> para armazenar o estado de local de thread durante a execução do loop. Para obter mais informações, consulte [Como gravar um loop Parallel.For com variáveis locais de thread](how-to-write-a-parallel-for-loop-with-thread-local-variables.md) e [Como gravar um loop Parallel.ForEach com variáveis locais de partição](how-to-write-a-parallel-foreach-loop-with-partition-local-variables.md).  
  
## <a name="avoid-over-parallelization"></a>Evite o excesso de paralelização  
 Ao usar loops paralelos, você incorre em custos indiretos de particionar a coleção de origem e sincronizar os threads de trabalho. Os benefícios da paralelização ainda estão limitados pelo número de processadores no computador. Não há nenhum aumento de velocidade a ser obtido executando vários threads vinculados à computação em apenas um processador. Portanto, você deve ter cuidado para não paralelizar excessivamente um loop.  
  
 O cenário mais comum em que a paralelização excessiva pode ocorrer é em loops aninhados. Na maioria dos casos, é melhor paralelizar apenas o loop externo, a menos que uma ou mais das seguintes condições se apliquem:  
  
- O loop interno é conhecido por ser muito longo.  
  
- Você está realizando uma computação cara em cada ordem. (A operação mostrada no exemplo não é cara).  
  
- O sistema de destino tem processadores suficientes para lidar com o número de threads que serão produzidos paralelizando a consulta em `cust.Orders`.  
  
 Em todos os casos, a melhor maneira de determinar a forma ideal da consulta é testar e medir.  
  
## <a name="avoid-calls-to-non-thread-safe-methods"></a>Evite chamadas para métodos não thread-safe  
 Gravar para métodos de instância não thread-safe a partir de um loop paralelo pode levar à corrupção de dados, o que pode ou não ser detectada em seu programa. Isso também poderá levar a exceções. No exemplo a seguir, vários segmentos estão tentando chamar simultaneamente o método <xref:System.IO.FileStream.WriteByte%2A?displayProperty=nameWithType>, que não é compatível com a classe.  
  
 [!code-csharp[TPL_Pitfalls#04](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_pitfalls/cs/pitfalls.cs#04)]
 [!code-vb[TPL_Pitfalls#04](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_pitfalls/vb/pitfalls_vb.vb#04)]  
  
## <a name="limit-calls-to-thread-safe-methods"></a>Limite chamadas para métodos thread-safe  
 A maioria dos métodos estáticos no .NET Framework é thread-safe e pode ser chamada de vários threads simultaneamente. No entanto, mesmo nesses casos, a sincronização envolvida pode levar a uma desaceleração significativa na consulta.  
  
> [!NOTE]
> Você pode testar isso sozinho inserindo algumas chamadas para <xref:System.Console.WriteLine%2A> nas suas consultas. Embora esse método seja usado nos exemplos de documentação para fins de demonstração, não o use em loops paralelos, a menos que seja necessário.  
  
## <a name="be-aware-of-thread-affinity-issues"></a>Esteja ciente de questões de afinidade de thread  
 Algumas tecnologias, por exemplo, interoperabilidade COM para componentes de um único segmento (STA), Windows Forms e Windows Presentation Foundation (WPF), impõem restrições de afinidade de thread que exigem que o código seja executado em um thread específico. Por exemplo, tanto no Windows Forms quanto no WPF, um controle só pode ser acessado no thread em que foi criado. Por exemplo, isso significa que você não pode atualizar um controle de lista de um loop paralelo, a menos que você configure o agendador de thread para agendar o trabalho somente no thread da interface do usuário. Para obter mais informações, confira [Especificação de um contexto de sincronização](xref:System.Threading.Tasks.TaskScheduler#specifying-a-synchronization-context).  
  
## <a name="use-caution-when-waiting-in-delegates-that-are-called-by-parallelinvoke"></a>Tenha cuidado ao aguardar delegados que são chamados por Parallel.Invoke  
 Em determinadas circunstâncias, a biblioteca de paralelismo de tarefas embutirá uma tarefa, ou seja, ela será executada na tarefa no thread em execução no momento. (Para obter mais informações, consulte [agendadores de tarefas](xref:System.Threading.Tasks.TaskScheduler).) Essa otimização de desempenho pode levar ao deadlock em determinados casos. Por exemplo, duas tarefas podem executar o mesmo código de delegado, o qual sinaliza quando ocorre um evento e, em seguida, aguarda a sinalização da outra tarefa. Se a segunda tarefa for embutida no mesmo thread que a primeira, e a primeira entrar em um estado de Espera, a segunda tarefa nunca poderá sinalizar o evento. Para evitar isso, especifique um tempo limite para a operação de Espera, ou use constructos de thread explícitos para ajudar a garantir que uma tarefa não possa bloquear a outra.  
  
## <a name="do-not-assume-that-iterations-of-foreach-for-and-forall-always-execute-in-parallel"></a>Não suponha que iterações para ForEach, For e ForAll sempre sejam executadas em paralelo  
 É importante ter em mente que iterações individuais em um loop <xref:System.Threading.Tasks.Parallel.For%2A>, <xref:System.Threading.Tasks.Parallel.ForEach%2A> ou <xref:System.Linq.ParallelEnumerable.ForAll%2A> podem, mas não necessariamente têm que, ser executadas em paralelo. Portanto, você deve evitar gravar qualquer código que dependa da correção na execução paralela de iterações ou na execução de iterações em qualquer classificação específica. Por exemplo, esse código é provavelmente um deadlock:  
  
 [!code-csharp[TPL_Pitfalls#01](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_pitfalls/cs/pitfalls.cs#01)]
 [!code-vb[TPL_Pitfalls#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_pitfalls/vb/pitfalls_vb.vb#01)]  
  
 Neste exemplo, uma iteração define um evento e todas as outras iterações aguardam o evento. Nenhuma das iterações em espera pode ser concluída até que a iteração de configuração do evento tenha sido concluída. No entanto, é possível que as iterações em espera bloqueiem todos os threads que são usados para executar o loop paralelo, antes que a iteração de configuração do evento tenha tido a chance de ser executada. Isso resulta em um deadlock – a iteração de configuração do evento nunca será executada e as iterações em espera nunca serão ativadas.  
  
 Em particular, uma iteração de um loop paralelo nunca deve aguardar outra iteração do loop para progredir. Se o loop paralelo decidir agendar as iterações sequencialmente, mas em ordem oposta, ocorrerá um deadlock.  
  
## <a name="avoid-executing-parallel-loops-on-the-ui-thread"></a>Evitar loops paralelos em execução no thread de interface do usuário  
 É importante manter a interface do usuário (IU) do aplicativo responsiva. Se uma operação contiver trabalho suficiente para garantir a paralelização, provavelmente não deverá ser executada no thread da interface do usuário.  Em vez disso, deverá ser descarregada para execução em um thread em segundo plano. Por exemplo, se você quiser usar um loop paralelo para calcular alguns dados que, depois, deverão ser renderizados em um controle de interface do usuário, convém executar o loop dentro de uma instância de tarefa, em vez de diretamente em um manipulador de eventos de interface do usuário.  Somente após a conclusão do cálculo principal você deverá realizar marshaling da atualização da interface do usuário para o thread de interface do usuário.  
  
 Se você executar loops paralelos no thread da interface do usuário, tenha cuidado para evitar a atualização dos controles de interface do usuário de dentro do loop. A tentativa de atualizar os controles de interface do usuário de dentro de um loop paralelo em execução no thread da interface do usuário poderá causar a corrupção do estado, exceções, atraso nas atualizações e até mesmo deadlocks, dependendo de como a atualização da interface do usuário é invocada. No exemplo a seguir, o loop paralelo bloqueia o thread de interface do usuário no qual ele está em execução até que todas as iterações sejam concluídas. No entanto, se uma iteração do loop estiver em execução em um thread em segundo plano (como <xref:System.Threading.Tasks.Parallel.For%2A> pode fazer), a chamada a Invoke faz com que uma mensagem seja enviada ao thread de interface do usuário e causa um bloqueio aguardando o processo da mensagem. Como o thread da interface do usuário está bloqueado executando <xref:System.Threading.Tasks.Parallel.For%2A>, a mensagem nunca pode ser processada e o thread da interface do usuário sofre um deadlock.  
  
 [!code-csharp[TPL_Pitfalls#02](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_pitfalls/cs/pitfalls.cs#02)]
 [!code-vb[TPL_Pitfalls#02](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_pitfalls/vb/pitfalls_vb.vb#02)]  
  
 O exemplo a seguir mostra como evitar o deadlock, executando o loop dentro de uma instância de tarefa. O thread de interface do usuário não é bloqueado pelo loop, e a mensagem pode ser processada.  
  
 [!code-csharp[TPL_Pitfalls#03](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_pitfalls/cs/pitfalls.cs#03)]
 [!code-vb[TPL_Pitfalls#03](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_pitfalls/vb/pitfalls_vb.vb#03)]  
  
## <a name="see-also"></a>Consulte também

- [Programação paralela](index.md)
- [Armadilhas em potencial com PLINQ](potential-pitfalls-with-plinq.md)
- [Padrões para programação paralela: noções básicas e aplicação de padrões paralelos com o .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=19222)
