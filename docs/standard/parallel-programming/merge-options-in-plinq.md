---
title: Opções de mesclagem em PLINQ
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, merge options
ms.assetid: e8f7be3b-88de-4f33-ab14-dc008e76c1ba
ms.openlocfilehash: a2c238cb66c5018cd1dd4085c6541ef3c9371beb
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290636"
---
# <a name="merge-options-in-plinq"></a>Opções de mesclagem em PLINQ
Quando uma consulta está sendo executada como paralela, o PLINQ faz a partição da sequência de origem para que várias threads possam funcionar em diferentes partes simultaneamente, normalmente em threads separados. Se os resultados forem consumidos em um thread, por exemplo, em um loop `foreach` (`For Each` em Visual Basic), os resultados de cada thread precisarão ser mesclados novamente em uma sequência. O tipo de mesclagem executado pelo PLINQ depende dos operadores que estão presentes na consulta. Por exemplo, os operadores que impõem uma nova ordem aos resultados devem armazenar em buffer todos os elementos em todos os threads. Do ponto de vista do thread de consumo (que também é o thread do usuário do aplicativo), uma consulta totalmente armazenada em buffer pode ser executada por um período significativo de tempo antes de produzir seu primeiro resultado. Por padrão, outros operadores são parcialmente armazenados em buffer e geram seus resultados em lotes. Um operador <xref:System.Linq.ParallelEnumerable.ForAll%2A> não é armazenado em buffer por padrão. Ele gera todos os elementos de todos os threads imediatamente.  
  
 Usando o método <xref:System.Linq.ParallelEnumerable.WithMergeOptions%2A>, conforme mostrado no exemplo a seguir, você pode fornecer uma dica ao PLINQ indicando o tipo de mesclagem que deverá ser executada.  
  
 [!code-csharp[PLINQ#26](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#26)]
 [!code-vb[PLINQ#26](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#26)]  
  
 Para ver o exemplo completo, confira [Como especificar opções de mesclagem em PLINQ](how-to-specify-merge-options-in-plinq.md).  
  
 Se a consulta em questão não oferecer suporte à opção solicitada, a opção será ignorada. Na maioria dos casos, você não precisa especificar uma opção de mesclagem para uma consulta PLINQ. No entanto, em alguns casos você pode achar, ao testar e medir, que uma consulta terá uma melhor execução em um modo diferente do padrão. Uma forma comum de usar essa opção é forçando um operador de mesclagem de blocos a transmitir os resultados para fornecer uma interface de usuário mais ágil na resposta.  
  
## <a name="parallelmergeoptions"></a>ParallelMergeOptions  
 A enumeração <xref:System.Linq.ParallelMergeOptions> inclui as seguintes opções que especificam, para formas de consulta com suporte, como a saída final da consulta será produzida quando os resultados forem consumidos em um thread:  
  
- `Not Buffered`  
  
     A opção <xref:System.Linq.ParallelMergeOptions.NotBuffered> faz com que cada elemento processado seja retornado de cada thread assim que ele for produzido. Esse comportamento é semelhante a fazer "streaming" da saída. Se o operador <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> estiver presente na consulta, `NotBuffered` preservará a ordem dos elementos de origem. Embora `NotBuffered` o comece a gerar resultados assim que estiverem disponíveis, o tempo total para produzir todos os resultados ainda pode ser maior do que usar uma das outras opções de mesclagem.  
  
- `Auto Buffered`  
  
     A opção <xref:System.Linq.ParallelMergeOptions.AutoBuffered> faz com que a consulta colete elementos em um buffer e, em seguida, gere periodicamente todo o conteúdo do buffer de uma vez para o thread de consumo. Isso equivale a gerar os dados de origem em "partes" em vez de usar o comportamento de "streaming" de `NotBuffered`. `AutoBuffered` pode demorar mais do que `NotBuffered` para disponibilizar o primeiro elemento no thread de consumo. O tamanho do buffer e o comportamento exato do retorno não são configuráveis e podem variar dependendo de vários fatores relacionados à consulta.  
  
- `FullyBuffered`  
  
     A opção <xref:System.Linq.ParallelMergeOptions.FullyBuffered> faz com que a saída de toda a consulta seja armazenada em buffer antes que qualquer um dos elementos seja gerado. Quando você usa essa opção, pode demorar mais tempo para que o primeiro elemento fique disponível no thread de consumo, mas os resultados completos podem ainda ser produzidos mais rapidamente em comparação com as outras opções.  
  
## <a name="query-operators-that-support-merge-options"></a>Operadores de consulta que dão suporte a opções de mesclagem  
 A tabela a seguir lista os operadores que dão suporte a todos os modos de opções de mesclagem sujeitos às restrições especificadas.  
  
|Operador|Restrições|  
|--------------|------------------|  
|<xref:System.Linq.ParallelEnumerable.AsEnumerable%2A>|Nenhum|  
|<xref:System.Linq.ParallelEnumerable.Cast%2A>|Nenhum|  
|<xref:System.Linq.ParallelEnumerable.Concat%2A>|Consultas não ordenadas que têm apenas uma fonte de dados de Matriz ou Lista.|  
|<xref:System.Linq.ParallelEnumerable.DefaultIfEmpty%2A>|Nenhum|  
|<xref:System.Linq.ParallelEnumerable.OfType%2A>|Nenhum|  
|<xref:System.Linq.ParallelEnumerable.Reverse%2A>|Consultas não ordenadas que têm apenas uma fonte de dados de Matriz ou Lista.|  
|<xref:System.Linq.ParallelEnumerable.Select%2A>|Nenhum|  
|<xref:System.Linq.ParallelEnumerable.SelectMany%2A>|Nenhum|  
|<xref:System.Linq.ParallelEnumerable.Skip%2A>|Nenhum|  
|<xref:System.Linq.ParallelEnumerable.Take%2A>|Nenhum|  
|<xref:System.Linq.ParallelEnumerable.Where%2A>|Nenhum|  
  
 Todos os outros operadores da consulta PLINQ podem ignorar opções de mesclagem fornecidas pelo usuário. Alguns operadores de consulta, por exemplo, <xref:System.Linq.ParallelEnumerable.Reverse%2A> e <xref:System.Linq.ParallelEnumerable.OrderBy%2A>, não conseguem gerar elementos até que todos eles sejam produzidos e reordenados. Portanto, quando <xref:System.Linq.ParallelMergeOptions> é usado em uma consulta que também contém um operador, como <xref:System.Linq.ParallelEnumerable.Reverse%2A>, o comportamento de mesclagem não será aplicado à consulta até que esse operador produza seus resultados.  
  
 A capacidade de alguns operadores para lidar com as opções de mesclagem depende do tipo da sequência de origem e se o operador <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> foi usado anteriormente na consulta. <xref:System.Linq.ParallelEnumerable.ForAll%2A> é sempre <xref:System.Linq.ParallelMergeOptions.NotBuffered>; ele suspende seus elementos imediatamente. <xref:System.Linq.ParallelEnumerable.OrderBy%2A> é sempre <xref:System.Linq.ParallelMergeOptions.FullyBuffered>; ele precisa classificar a lista inteira antes de suspendê-la.  
  
## <a name="see-also"></a>Veja também

- [LINQ paralelo (PLINQ)](introduction-to-plinq.md)
- [Como: Especificar opções de mesclagem em PLINQ](how-to-specify-merge-options-in-plinq.md)
