---
title: Operações de consulta LINQ básica (C#)
description: Apresente-se às expressões de consulta LINQ e algumas das operações que você pode executar em uma consulta.
ms.date: 07/20/2015
helpviewer_keywords:
- orderby clause [LINQ in C#]
- ordering data [LINQ in C#]
- selecting data [LINQ in C#]
- queries [LINQ in C#], basic operations
- grouping data [LINQ in C#]
- data sources [LINQ in C#]
- sorting data [LINQ in C#]
- projections [LINQ in C#]
- filtering data [LINQ in C#]
- joining data [LINQ in C#]
- select clause [LINQ in C#]
- LINQ [C#], basic query operations
- join clause [LINQ in C#]
- group clause [LINQ in C#]
ms.assetid: a7ea3421-1cf4-4df7-832a-aa22fe6379e9
ms.openlocfilehash: d9653be8b67ef4d971c157b8dd8d82b2ae3c2287
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105525"
---
# <a name="basic-linq-query-operations-c"></a>Operações de consulta LINQ básica (C#)
Este tópico fornece uma breve introdução às expressões de consulta LINQ e alguns dos tipos típicos de operações que você executa em uma consulta. Informações mais detalhadas estão nos tópicos a seguir:  
  
 [Expressões de Consulta LINQ](../../../linq/index.md)  
  
 [Visão geral de operadores de consulta padrão (C#)](./standard-query-operators-overview.md)  
  
 [Passo a passo: escrevendo consultas em C#](./walkthrough-writing-queries-linq.md)  
  
> [!NOTE]
> Se já estiver familiarizado com uma linguagem de consulta como SQL ou XQuery, você poderá ignorar a maior parte deste tópico. Leia sobre a " `from` cláusula" na próxima seção para saber mais sobre a ordem das cláusulas em expressões de consulta LINQ.  
  
## <a name="obtaining-a-data-source"></a>Obtendo uma Fonte de Dados  
 Em uma consulta LINQ, a primeira etapa é especificar a fonte de dados. No C#, como na maioria das linguagens de programação, uma variável deve ser declarada antes que possa ser usada. Em uma consulta LINQ, a `from` cláusula vem primeiro para apresentar a fonte de dados ( `customers` ) e a *variável de intervalo* ( `cust` ).  
  
 [!code-csharp[csLINQGettingStarted#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#23)]  
  
 A variável de intervalo é como a variável de iteração em um loop `foreach`, mas nenhuma iteração real ocorre em uma expressão de consulta. Quando a consulta é executada, a variável de intervalo servirá como uma referência para cada elemento sucessivo em `customers`. Uma vez que o compilador pode inferir o tipo de `cust`, você não precisa especificá-lo explicitamente. Variáveis de intervalo adicionais podem ser introduzidas por uma cláusula `let`. Para obter mais informações, consulte [Cláusula let](../../../language-reference/keywords/let-clause.md).  
  
> [!NOTE]
> Para fontes de dados não genéricas, como <xref:System.Collections.ArrayList>, a variável de intervalo deve ser tipada explicitamente. Para obter mais informações, consulte [como consultar uma ArrayList com LINQ (C#)](./how-to-query-an-arraylist-with-linq.md) e a [cláusula from](../../../language-reference/keywords/from-clause.md).  
  
## <a name="filtering"></a>Filtragem  
 Provavelmente, a operação de consulta mais comum é aplicar um filtro no formulário de uma expressão booliana. O filtro faz com que a consulta retorne apenas os elementos para os quais a expressão é verdadeira. O resultado é produzido usando a cláusula `where`. O filtro em vigor especifica os elementos a serem excluídos da sequência de origem. No exemplo a seguir, somente os `customers` que têm um endereço em Londres são retornados.  
  
 [!code-csharp[csLINQGettingStarted#24](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#24)]  
  
 Você pode usar os operadores lógicos `AND` e `OR` de C# para aplicar quantas expressões de filtro forem necessárias na cláusula `where`. Por exemplo, para retornar somente clientes de "Londres" `AND` cujo nome seja "Devon", você escreveria o seguinte código:  
  
 [!code-csharp[csLINQGettingStarted#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#25)]  
  
 Para retornar clientes de Londres ou Paris, você escreveria o seguinte código:  
  
 [!code-csharp[csLINQGettingStarted#26](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#26)]  
  
 Para obter mais informações, consulte a [cláusula WHERE](../../../language-reference/keywords/where-clause.md).  
  
## <a name="ordering"></a>Ordenando  
 Muitas vezes é conveniente classificar os dados retornados. A cláusula `orderby` fará com que os elementos na sequência retornada sejam classificados de acordo com o comparador padrão para o tipo que está sendo classificado. Por exemplo, a consulta a seguir pode ser estendida para classificar os resultados com base na propriedade `Name`. Como `Name` é uma cadeia de caracteres, o comparador padrão executa uma classificação em ordem alfabética de A a Z.  
  
 [!code-csharp[csLINQGettingStarted#27](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#27)]  
  
 Para ordenar os resultados na ordem inversa, de Z para a, use a cláusula `orderby…descending`.  
  
 Para obter mais informações, consulte [Cláusula orderby](../../../language-reference/keywords/orderby-clause.md).  
  
## <a name="grouping"></a>Agrupamento  
 A cláusula `group` permite agrupar seus resultados com base em uma chave que você especificar. Por exemplo, você pode especificar que os resultados sejam agrupados segundo o `City`, de modo que todos os clientes de Londres ou Paris fiquem em grupos individuais. Nesse caso, `cust.City` é a chave.  
  
 [!code-csharp[csLINQGettingStarted#28](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#28)]  
  
 Quando você terminar uma consulta com um cláusula `group`, seus resultados assumirão a forma de uma lista de listas. Cada elemento na lista é um objeto que tem um membro `Key` e uma lista dos elementos que estão agrupados sob essa chave. Quando itera em uma consulta que produz uma sequência de grupos, você deve usar um loop `foreach` aninhado. O loop externo itera em cada grupo e o loop interno itera nos membros de cada grupo.  
  
 Se precisar consultar os resultados de uma operação de grupo, você poderá usar a palavra-chave `into` para criar um identificador que pode ser consultado ainda mais. A consulta a seguir retorna apenas os grupos que contêm mais de dois clientes:  
  
 [!code-csharp[csLINQGettingStarted#29](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#29)]  
  
 Para obter mais informações, consulte [Cláusula group](../../../language-reference/keywords/group-clause.md).  
  
## <a name="joining"></a>Adição  
 Operações de junção criam associações entre sequências que não são modeladas explicitamente nas fontes de dados. Por exemplo, você pode executar uma junção para localizar todos os clientes e distribuidores que têm o mesmo local. No LINQ `join` , a cláusula sempre funciona em coleções de objetos em vez de tabelas de banco de dados diretamente.  
  
 [!code-csharp[csLINQGettingStarted#36](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#36)]  
  
 No LINQ, você não precisa usar com a `join` frequência que faz no SQL, porque as chaves estrangeiras no LINQ são representadas no modelo de objeto como propriedades que mantêm uma coleção de itens. Por exemplo, um objeto `Customer` que contém uma coleção de objetos `Order`. Em vez de executar uma junção, você pode acessar os pedidos usando notação de ponto:  
  
```csharp
from order in Customer.Orders...  
```  
  
 Para obter mais informações, consulte [cláusula JOIN](../../../language-reference/keywords/join-clause.md).  
  
## <a name="selecting-projections"></a>Selecionando (Projeções)  
 A cláusula `select` produz os resultados da consulta e especifica a "forma" ou o tipo de cada elemento retornado. Por exemplo, você pode especificar se os resultados consistirão em objetos `Customer` completos, apenas um membro, um subconjunto de membros ou algum tipo de resultado completamente diferente com base em um cálculo ou na criação de um novo objeto. Quando a cláusula `select` produz algo diferente de uma cópia do elemento de origem, a operação é chamada de *projeção*. O uso de projeções para transformar dados é um recurso poderoso de expressões de consulta LINQ. Para obter mais informações, consulte [Transformações de dados com LINQ (C#)](./data-transformations-with-linq.md) e [Cláusula select](../../../language-reference/keywords/select-clause.md).  
  
## <a name="see-also"></a>Confira também

- [Expressões de Consulta LINQ](../../../linq/index.md)
- [Passo a passo: escrevendo consultas em C#](./walkthrough-writing-queries-linq.md)
- [Palavras-chave de consulta (LINQ)](../../../language-reference/keywords/query-keywords.md)
- [Tipos anônimos](../../classes-and-structs/anonymous-types.md)
