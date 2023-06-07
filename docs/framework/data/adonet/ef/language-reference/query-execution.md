---
title: Execução da consulta
description: Saiba mais sobre diferentes maneiras em que uma consulta LINQ to Entities é executada, incluindo execução de consulta adiada, execução de consulta imediata e execução da loja.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: c0e6cf23-63ac-47dd-bfe9-d5bdca826fac
ms.openlocfilehash: e776df6d35b6cc8c24cd83e902bc4d050347343b
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286786"
---
# <a name="query-execution"></a>Execução da consulta
Depois de ser criada por um usuário, uma consulta LINQ é convertida em uma árvore de comando. Uma árvore de comando é uma representação de uma consulta compatível com o Entity Framework. Em seguida, a árvore de comando é executada na fonte de dados. Em tempo de execução de consulta, todas as expressões e consulta (isto é, todos os componentes da consulta) são avaliados, incluindo essas expressões usadas na materialização do resultado.  
  
 O ponto onde as expressões de consulta são executadas pode variar. As consultas LINQ são sempre executadas quando a variável de consulta é iterada sobre, não quando a variável de consulta é criada. Isso é chamado de *execução adiada*. Você também pode forçar a execução imediata de uma consulta, o que é útil para armazenar os resultados da consulta em cache. Isso é descrito mais adiante neste tópico.  
  
 Quando uma consulta LINQ to Entities é executada, algumas expressões da consulta podem ser executadas no servidor e algumas partes podem ser executadas localmente no cliente. A avaliação de uma expressão do lado do cliente ocorre antes de a consulta ser executada no servidor. Se uma expressão for avaliada no cliente, o resultado da avaliação será substituído para a expressão na consulta, e a consulta será executada no servidor. Como as consultas são executadas na fonte de dados, a configuração da fonte de dados substitui o comportamento especificado no cliente. Por exemplo, manipulação de valor nulo e a precisão numérica dependem das configurações do servidor. Todas as exceções geradas durante a execução da consulta no servidor são passadas diretamente até o cliente.  

> [!TIP]
> Para obter um resumo conveniente dos operadores de consulta em formato de tabela, que permite identificar rapidamente o comportamento de execução de um operador, consulte [classificação de operadores de consulta padrão por meio da execução (C#)](../../../../../csharp/programming-guide/concepts/linq/classification-of-standard-query-operators-by-manner-of-execution.md).

## <a name="deferred-query-execution"></a>Execução de consulta adiada  
 Em uma consulta, que retorna uma sequência de valores, a variável de consulta nunca contém os resultados da consulta e armazena somente os comandos da consulta. A execução da consulta é adiada até que a variável da consulta seja iterada em um loop `foreach` ou `For Each`. Isso é conhecido como *execução adiada*; ou seja, a execução da consulta ocorre algum tempo depois que a consulta é construída. Isso significa que você pode executar uma consulta com a frequência que desejar. Isso é útil quando, por exemplo, você tem um banco de dados que está sendo atualizado por outros aplicativos. Em seu aplicativo, você pode criar uma consulta para recuperar as informações mais recentes e executar a consulta repetidamente, retornando informações atualizadas sempre.  
  
 A execução adiada permite que várias consultas sejam combinadas ou que uma consulta seja estendida. Quando é estendida, a consulta é modificada para incluir as novas operações, e a execução eventual refletirá as alterações. No exemplo a seguir, a primeira consulta retorna todos os produtos. A segunda consulta estende a primeira usando `Where` para retornar todos os produtos de tamanho “L”:  
  
 [!code-csharp[DP L2E Conceptual Examples#Composing1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#composing1)]
 [!code-vb[DP L2E Conceptual Examples#Composing1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#composing1)]  
  
 Depois que uma consulta foi executada, todas as consultas sucessivas usarão os operadores LINQ na memória. Iterar sobre a variável de consulta usando uma instrução `foreach` ou `For Each` ou chamando um dos operadores de conversão LINQ fará com que a execução seja imediata. Esses operadores de conversão incluem: <xref:System.Linq.Enumerable.ToList%2A>, <xref:System.Linq.Enumerable.ToArray%2A>, <xref:System.Linq.Enumerable.ToLookup%2A> e <xref:System.Linq.Enumerable.ToDictionary%2A>.  
  
## <a name="immediate-query-execution"></a>Execução imediata de consulta  
 Em comparação com a execução adiada de consultas que geram uma sequência de valores, as consultas que retornam um valor singleton são executadas imediatamente. Alguns exemplos de consultas singleton são <xref:System.Linq.Enumerable.Average%2A>, <xref:System.Linq.Enumerable.Count%2A>, <xref:System.Linq.Enumerable.First%2A> e <xref:System.Linq.Enumerable.Max%2A>. Essas consultas são executadas imediatamente porque a consulta deve gerar uma sequência para calcular o resultado singleton. Você também pode forçar a execução imediata. Isso é útil quando você deseja armazenar os resultados de uma consulta em cache. Para forçar a execução imediata de uma consulta que não produz um valor singleton, você pode chamar o método <xref:System.Linq.Enumerable.ToList%2A>, o método <xref:System.Linq.Enumerable.ToDictionary%2A> ou o método <xref:System.Linq.Enumerable.ToArray%2A> em uma consulta ou em uma variável de consulta. O exemplo a seguir usa o método <xref:System.Linq.Enumerable.ToArray%2A> para avaliar imediatamente uma sequência em uma matriz.  
  
 [!code-csharp[DP L2E Examples#ToArray](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Examples/CS/Program.cs#toarray)]
 [!code-vb[DP L2E Examples#ToArray](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Examples/VB/Module1.vb#toarray)]  
  
 Você também pode forçar a execução colocando o loop `foreach` ou `For Each` imediatamente após a expressão da consulta, mas chamando <xref:System.Linq.Enumerable.ToList%2A> ou <xref:System.Linq.Enumerable.ToArray%2A> você armazena em cache todos os dados em um único objeto de coleção.  
  
## <a name="store-execution"></a>Execução de armazenamento  
 Em geral, as expressões no LINQ to Entities são avaliadas no servidor, e não se deve esperar que o comportamento da expressão siga a semântica do CLR (Common Language Runtime), mas sim a da fonte de dados. No entanto, existem exceções, como quando a expressão é executada no cliente. Isso pode provocar resultados inesperados, por exemplo, quando o servidor e o cliente estão em fusos horários diferentes.  
  
 Algumas expressões da consulta podem ser executadas no cliente. Em geral, espera-se que a maior parte da execução da consulta ocorra no servidor. Com exceção dos métodos executados em elementos de consulta mapeados para a fonte de dados, frequentemente há expressões na consulta que podem ser executadas localmente. A execução local de uma expressão de consulta produz um valor que pode ser usado na execução da consulta ou na construção do resultado.  
  
 Determinadas operações são sempre executadas no cliente, como a associação de valores, as subexpressões, as subconsultas de fechamentos e a materialização de objetos nos resultados da consulta. O efeito líquido disso é que esses elementos (por exemplo, valores de parâmetros) não podem ser atualizados durante a execução. Tipos anônimos também podem ser construídos embutidos na fonte de dados, mas não devem ser supostos a fazer isso. Os agrupamentos embutidos podem ser construídos na fonte de dados, também, mas isso não deve ser suposto em cada instância. Em geral, é melhor não fazer nenhuma suposição sobre o que é construído no servidor.  
  
 Esta seção descreve os cenários em que o código é executado localmente no cliente. Para obter mais informações sobre quais tipos de expressões são executados localmente, consulte [expressões em consultas LINQ to Entities](expressions-in-linq-to-entities-queries.md).  
  
### <a name="literals-and-parameters"></a>Literais e parâmetros  
 Variáveis locais, como a variável `orderID` no exemplo a seguir, são avaliadas no cliente.  
  
 [!code-csharp[DP L2E Conceptual Examples#LiteralParameter1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#literalparameter1)]
 [!code-vb[DP L2E Conceptual Examples#LiteralParameter1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#literalparameter1)]  
  
 Os parâmetros do método também são avaliados no cliente. O parâmetro `orderID` passado para o método `MethodParameterExample`, abaixo, é um exemplo.  
  
 [!code-csharp[DP L2E Conceptual Examples#MethodParameterExample](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#methodparameterexample)]
 [!code-vb[DP L2E Conceptual Examples#MethodParameterExample](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#methodparameterexample)]  
  
### <a name="casting-literals-on-the-client"></a>Literais de conversão no cliente  
 A conversão de `null` em um tipo CLR é executada no cliente:  
  
 [!code-csharp[DP L2E Conceptual Examples#NullCastToString](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#nullcasttostring)]
 [!code-vb[DP L2E Conceptual Examples#NullCastToString](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#nullcasttostring)]  
  
 A conversão em um tipo, como <xref:System.Decimal> anulável, é executada no cliente:  
  
 [!code-csharp[DP L2E Conceptual Examples#CastToNullable](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#casttonullable)]
 [!code-vb[DP L2E Conceptual Examples#CastToNullable](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#casttonullable)]  
  
### <a name="constructors-for-literals"></a>Construtores de literais  
 Os novos tipos CLR que podem ser mapeados para os tipos de modelo conceitual são executados no cliente:  
  
 [!code-csharp[DP L2E Conceptual Examples#ConstructorForLiteral](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#constructorforliteral)]
 [!code-vb[DP L2E Conceptual Examples#ConstructorForLiteral](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#constructorforliteral)]  
  
 As novas matrizes também são executadas no cliente.  
  
## <a name="store-exceptions"></a>Exceções de armazenamento  
 Todos os erros de armazenamento encontrados durante a execução da consulta são passados até o cliente e não são mapeados ou tratados.  
  
## <a name="store-configuration"></a>Configuração do repositório  
 Quando a consulta é executada no repositório, a configuração do repositório substitui todos os comportamentos do cliente e a semântica do repositório é expressa para todas as operações e expressões. Isso pode resultar em uma diferença no comportamento entre a execução no CLR e no repositório em áreas como comparações nulas, pedidos de GUID, precisão de operações que envolvem tipos de dados não precisos (como tipos de ponto de flutuação ou <xref:System.DateTime>) e operações de cadeia de caracteres. É importante lembrar-se disso ao examinar os resultados da consulta.  
  
 Por exemplo, a seguir estão algumas diferenças de comportamento entre o CLR e o SQL Server:  
  
- O SQL Server pede GUIDs de maneira diferente do CLR.  
  
- Também pode haver diferenças na precisão do resultado ao manipular o tipo decimal no SQL Server. Isso é devido aos requisitos fixos de precisão do tipo decimal do SQL Server. Por exemplo, a média dos valores <xref:System.Decimal> 0,0, 0,0, e 1,0 é 0,3333333333333333333333333333 na memória no cliente, mas é 0,333333 no repositório (baseado na precisão padrão do tipo decimal do SQL Server).  
  
- Algumas operações de comparação de cadeia de caracteres também são manipuladas de maneira diferente no SQL Server em relação ao CLR. O comportamento de comparação de cadeia de caracteres depende das configurações de ordenação no servidor.  
  
- Chamadas de função ou de método, quando incluídas em uma consulta LINQ to Entities, são mapeadas para funções canônicas no Entity Framework, que são então convertidas em Transact-SQL e executadas no banco de dados SQL Server. Há casos em que o comportamento que essas funções mapeadas exibem pode diferir na implementação nas bibliotecas de classes base. Por exemplo, a chamada dos métodos <xref:System.String.Contains%2A>, <xref:System.String.StartsWith%2A> e <xref:System.String.EndsWith%2A> com uma cadeia de caracteres vazia como um parâmetro retornará `true` quando executada no CLR, mas retornará `false` quando executada no SQL Server. O método <xref:System.String.EndsWith%2A> também pode retornar resultados diferentes porque o SQL Server considera que duas cadeias de caracteres serão iguais se diferirem apenas no espaço em branco à direita, enquanto o CLR não as considera iguais. Isso é ilustrado pelo exemplo a seguir:  
  
 [!code-csharp[DP L2E Conceptual Examples#CanonicalFuncVsCLRBaseType](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#canonicalfuncvsclrbasetype)]
 [!code-vb[DP L2E Conceptual Examples#CanonicalFuncVsCLRBaseType](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#canonicalfuncvsclrbasetype)]
