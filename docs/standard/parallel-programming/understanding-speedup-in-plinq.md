---
title: Noções básicas sobre agilização em PLINQ
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, performance tuning
ms.assetid: 53706c7e-397d-467a-98cd-c0d1fd63ba5e
ms.openlocfilehash: 627f1327a9fe87fc226dfbb40df50ec4855edfb9
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84284891"
---
# <a name="understanding-speedup-in-plinq"></a>Noções básicas sobre agilização em PLINQ
O principal objetivo do PLINQ é acelerar a execução das consultas do LINQ to Objects executando os delegados da consulta em paralelo em computadores com vários núcleos. O PLINQ funciona melhor quando o processamento de cada elemento em uma coleção de origem é independente, sem nenhum estado compartilhado envolvido entre os delegados individuais. Tais operações são comuns em LINQ to Objects e PLINQ, e muitas vezes são chamadas de "*fantasticamente paralelas*", pois elas se prestam facilmente ao agendamento em múltiplos segmentos. No entanto, nem todas as consultas consistem por completo em operações fantasticamente paralelas; na maioria dos casos, uma consulta envolve alguns operadores que não podem ser paralelizados ou que retardam a execução paralela. E mesmo com consultas que são fantasticamente paralelas por completo, o PLINQ ainda deve particionar a fonte de dados e agendar o trabalho nos threads e, normalmente, mesclar os resultados quando a consulta for concluída. Todas essas operações aumentam o custo computacional da paralelização; esses custos de adição de paralelização são chamados de *sobrecarga*. Para obter o melhor desempenho em uma consulta PLINQ, o objetivo é maximizar as partes que são fantasticamente paralelas e minimizar as partes que exigem sobrecarga. Este artigo fornece informações que ajudarão você a gravar consultas PLINQ que sejam tão eficientes quanto possível, enquanto ainda produzem resultados corretos.  
  
## <a name="factors-that-impact-plinq-query-performance"></a>Fatores que afetam o desempenho da consulta PLINQ  
 As seções a seguir enumeram alguns dos fatores mais importantes que influenciam o desempenho da consulta paralela. Essas são declarações gerais que, por si só, não são suficientes para prever o desempenho da consulta em todos os casos. Como sempre, é importante medir o desempenho real de consultas específicas em computadores com uma variedade de configurações e cargas representativas.  
  
1. Custo computacional do trabalho geral.  
  
     Para obter aceleração, uma consulta PLINQ deve ter um trabalho bastante paralelo agradável para compensar a sobrecarga. O trabalho pode ser expresso como o custo computacional de cada delegado multiplicado pelo número de elementos na coleção de origem. Supondo que uma operação possa ser paralelizada, quanto mais computacionalmente cara for, maior será a oportunidade de aceleração. Por exemplo, se uma função tiver um milissegundo para executar, uma consulta sequencial sobre 1000 elementos levará um segundo para executar essa operação, enquanto uma consulta paralela em um computador com quatro núcleos pode demorar apenas 250 milissegundos. Isso resulta em um aumento de velocidade de 750 milissegundos. Se a função exigisse um segundo para executar para cada elemento, a velocidade seria de 750 segundos. Se o delegado for muito caro, o PLINQ poderá oferecer uma aceleração significativa com apenas alguns itens na coleção de origem. Por outro lado, coleções de origem pequenas com delegados triviais geralmente não são boas candidatas para PLINQ.  
  
     No exemplo a seguir, queryA provavelmente é uma boa candidata para PLINQ, supondo que sua função Select envolva uma grande parte do trabalho. A queryB provavelmente não é uma boa candidata, pois não há trabalho suficiente na instrução Select e a sobrecarga de paralelização irá compensar a maioria ou a totalidade da aceleração.  
  
    ```vb  
    Dim queryA = From num In numberList.AsParallel()  
                 Select ExpensiveFunction(num); 'good for PLINQ  
  
    Dim queryB = From num In numberList.AsParallel()  
                 Where num Mod 2 > 0  
                 Select num; 'not as good for PLINQ  
    ```  
  
    ```csharp  
    var queryA = from num in numberList.AsParallel()  
                 select ExpensiveFunction(num); //good for PLINQ  
  
    var queryB = from num in numberList.AsParallel()  
                 where num % 2 > 0  
                 select num; //not as good for PLINQ  
    ```  
  
2. O número de núcleos lógicos no sistema (grau de paralelismo).  
  
     Esse ponto é um resultado óbvio para a seção anterior; as consultas que são fantasticamente paralelas são executadas de forma mais rápida em máquinas com mais núcleos, pois o trabalho pode ser dividido entre mais threads concorrentes. A quantidade total de aceleração depende da porcentagem do trabalho geral da consulta paralelizada. No entanto, não presuma que todas as consultas serão executadas duas vezes mais rápido em um computador de oito núcleos em comparação com um computador de quatro núcleos. Ao sintonizar consultas para um desempenho ideal, é importante medir resultados reais em computadores com vários números de núcleos. Este ponto está relacionado ao ponto 1: conjuntos de dados maiores são necessários para tirar proveito de maiores recursos de computação.  
  
3. O número e o tipo de operações.  
  
     O PLINQ fornece o operador AsOrdered para situações em que é necessário manter a ordem dos elementos na sequência de origem. Há um custo associado com a ordenação, mas esse custo é geralmente pequeno. Da mesma forma, operações de GroupBy e Join causam sobrecarga. O PLINQ funciona melhor quando tem permissão para processar elementos na coleção de origem em qualquer ordem e passá-los para o próximo operador assim que estiverem prontos. Para saber mais, veja [Preservação da ordem em PLINQ](order-preservation-in-plinq.md).  
  
4. A forma de execução da consulta.  
  
     Se você está armazenando os resultados de uma consulta chamando ToArray ou ToList, os resultados de todos os segmentos paralelos devem ser incorporados na estrutura de dados única. Isso envolve um custo computacional inevitável. Da mesma forma, se você iterar os resultados usando um loop foreach (For Each in Visual Basic), os resultados dos threads de trabalho precisam ser serializados no thread do enumerador. Mas se você quiser apenas executar alguma ação com base no resultado de cada thread, você pode usar o método ForAll para executar este trabalho em vários threads.  
  
5. O tipo de opções de mesclagem.  
  
     O PLINQ pode ser configurado para armazenar sua saída em buffer e produzir em pedaços ou tudo de uma vez após o conjunto de resultados inteiro ser produzido, ou então transmitir resultados individuais à medida que eles são produzidos. Os resultados anteriores diminuem o tempo de execução geral e o último resulta em latência diminuída entre os elementos produzidos.  Embora as opções de mesclagem nem sempre tenham um grande impacto no desempenho geral da consulta, elas podem afetar o desempenho percebido, pois elas controlam quanto tempo um usuário deve aguardar para ver os resultados. Para saber mais, veja [Opções de mesclagem em PLINQ](merge-options-in-plinq.md).  
  
6. O tipo de particionamento.  
  
     Em alguns casos, uma consulta PLINQ sobre uma coleção de fonte indexável pode resultar em uma carga de trabalho desbalanceada. Quando isso ocorre, você poderá aumentar o desempenho da consulta criando um particionador personalizado. Para saber mais, veja [Particionadores personalizados para PLINQ e TPL](custom-partitioners-for-plinq-and-tpl.md).  
  
## <a name="when-plinq-chooses-sequential-mode"></a>Quando o PLINQ escolhe o modo sequencial  
 PLINQ sempre tentará executar uma consulta pelo menos tão rápido quanto a consulta seria executada sequencialmente. Embora o PLINQ não observe o quanto delegados de usuário são computacionalmente caros, ou quanto a fonte de entrada é grande, ele procura determinadas "formas" de consulta. Especificamente, ele busca operadores de consulta ou combinações de operadores que geralmente fazem com que uma consulta seja executada mais lentamente em modo paralelo. Quando encontradas essas formas, o PLINQ por padrão voltará ao modo sequencial.  
  
 No entanto, depois de medir o desempenho de uma consulta específica, você pode determinar que ele realmente é executado mais rápido no modo paralelo. Nesses casos, você pode usar o sinalizador <xref:System.Linq.ParallelExecutionMode.ForceParallelism?displayProperty=nameWithType> através do método <xref:System.Linq.ParallelEnumerable.WithExecutionMode%2A> para instruir o PLINQ para paralelizar a consulta. Para saber mais, veja [Como especificar o modo de execução em PLINQ](how-to-specify-the-execution-mode-in-plinq.md).  
  
 A lista a seguir descreve as formas de consulta que o PLINQ executará, por padrão, no modo sequencial:  
  
- Consultas que contêm uma opção Select, indexed Where, indexed SelectMany ou ElementAt após um operador de ordenação ou filtragem que removeu ou reorganizou os índices originais.  
  
- Consultas que contêm um operador Take, TakeWhile, Skip, SkipWhile e onde índices na sequência de origem não estão na ordem original.  
  
- Consultas que contenham Zip ou SequenceEquals, a menos que uma das fontes de dados tenha um índice originalmente solicitado e a outra fonte de dados seja indexável (ou seja, uma matriz ou IList (T)).  
  
- Consultas que contêm Concat, a menos que ela seja aplicada a fontes de dados indexáveis.  
  
- Consultas que contêm Reverse, a menos que ela seja aplicada a fontes de dados indexáveis.  
  
## <a name="see-also"></a>Veja também

- [LINQ paralelo (PLINQ)](introduction-to-plinq.md)
