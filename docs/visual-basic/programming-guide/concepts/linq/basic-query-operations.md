---
title: Operações de consulta básica
ms.date: 07/20/2015
helpviewer_keywords:
- data sources [LINQ in Visual Basic]
- Join clause [LINQ in Visual Basic]
- ordering data [LINQ in Visual Basic]
- projections [LINQ in Visual Basic]
- LINQ [Visual Basic], query operations
- Order By clause [LINQ in Visual Basic]
- joining data [LINQ in Visual Basic]
- queries [LINQ in Visual Basic], basic operations
- selecting data [LINQ in Visual Basic]
- Group By clause [LINQ in Visual Basic]
- grouping data [LINQ in Visual Basic]
- Select clause [LINQ in Visual Basic]
ms.assetid: 1146f6d0-fcb8-4f4d-8223-c9db52620d21
ms.openlocfilehash: 92ac5beb70526795eb140bd794e47981cebfea93
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410910"
---
# <a name="basic-query-operations-visual-basic"></a>Operações de consulta básica (Visual Basic)
Este tópico fornece uma breve introdução às expressões de LINQ (consulta integrada à linguagem) no Visual Basic e a alguns dos tipos típicos de operações que você executa em uma consulta. Para obter mais informações, consulte estes tópicos:  
  
 [Introdução a LINQ no Visual Basic](../../language-features/linq/introduction-to-linq.md)  
  
 [Consultas](../../../language-reference/queries/index.md)  
  
 [Instruções passo a passo: escrevendo consultas em Visual Basic](walkthrough-writing-queries.md)  
  
## <a name="specifying-the-data-source-from"></a>Especificando a Fonte de Dados (De)  
 Em uma consulta LINQ, a primeira etapa é especificar a fonte de dados que você deseja consultar. Portanto, a `From` cláusula em uma consulta sempre é exibida primeiro. Operadores de consulta Selecione e formate o resultado com base no tipo de origem.  
  
 [!code-vb[VbLINQBasicOps#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#1)]  
  
 A `From` cláusula Especifica a fonte de dados, `customers` e uma *variável de intervalo*, `cust` . A variável de intervalo é como uma variável de iteração de loop, exceto que em uma expressão de consulta, nenhuma iteração real ocorre. Quando a consulta é executada, muitas vezes usando um `For Each` loop, a variável Range serve como uma referência a cada elemento sucessivo no `customers` . Uma vez que o compilador pode inferir o tipo de `cust`, você não precisa especificá-lo explicitamente. Para obter exemplos de consultas escritas com e sem digitação explícita, consulte [relações de tipo em operações de consulta (Visual Basic)](type-relationships-in-query-operations.md).  
  
 Para obter mais informações sobre como usar a `From` cláusula em Visual Basic, consulte a [cláusula from](../../../language-reference/queries/from-clause.md).  
  
## <a name="filtering-data-where"></a>Filtrando Dados (Onde)  
 Provavelmente, a operação de consulta mais comum é aplicar um filtro na forma de uma expressão booliana. Em seguida, a consulta retorna somente os elementos para os quais a expressão é verdadeira. Uma `Where` cláusula é usada para executar a filtragem. O filtro especifica quais elementos na fonte de dados incluir na sequência resultante. No exemplo a seguir, somente os clientes que têm um endereço em Londres estão incluídos.  
  
 [!code-vb[VbLINQBasicOps#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#2)]  
  
 Você pode usar operadores lógicos, como `And` e `Or` para combinar expressões de filtro em uma `Where` cláusula. Por exemplo, para retornar somente os clientes que são de Londres e cujo nome é Devon, use o seguinte código:  
  
```vb  
Where cust.City = "London" And cust.Name = "Devon"
```  
  
 Para retornar os clientes de Londres ou Paris, use o seguinte código:  
  
```vb  
Where cust.City = "London" Or cust.City = "Paris"
```  
  
 Para obter mais informações sobre como usar a `Where` cláusula em Visual Basic, consulte [cláusula WHERE](../../../language-reference/queries/where-clause.md).  
  
## <a name="ordering-data-order-by"></a>Ordenando Dados (Ordenar Por)  
 Geralmente, é conveniente classificar os dados retornados em uma ordem específica. A `Order By` cláusula fará com que os elementos na sequência retornada sejam classificados em um campo ou campos especificados. Por exemplo, a consulta a seguir classifica os resultados com base na `Name` propriedade. Como `Name` é uma cadeia de caracteres, os dados retornados serão classificados alfabeticamente, de a a Z.  
  
 [!code-vb[VbLINQBasicOps#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#3)]  
  
 Para ordenar os resultados na ordem inversa, de Z para a, use a cláusula `Order By...Descending`. O padrão é `Ascending` quando nem `Ascending` nem `Descending` é especificado.  
  
 Para obter mais informações sobre como usar a `Order By` cláusula em Visual Basic, consulte [cláusula order by](../../../language-reference/queries/order-by-clause.md).  
  
## <a name="selecting-data-select"></a>Selecionando Dados (Selecionar)  
 A `Select` cláusula Especifica o formulário e o conteúdo dos elementos retornados. Por exemplo, você pode especificar se os resultados consistirão em `Customer` objetos completos, apenas uma `Customer` propriedade, um subconjunto de propriedades, uma combinação de propriedades de várias fontes de dados ou algum novo tipo de resultado com base em uma computação. Quando a cláusula `Select` produz algo diferente de uma cópia do elemento de origem, a operação é chamada de *projeção*.  
  
 Para recuperar uma coleção que consiste em `Customer` objetos completos, selecione a variável de intervalo em si:  
  
 [!code-vb[VbLINQBasicOps#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#4)]  
  
 Se uma `Customer` instância for um objeto grande que tem muitos campos, e tudo o que você deseja recuperar for o nome, você poderá selecionar `cust.Name` , conforme mostrado no exemplo a seguir. A inferência de tipo local reconhece que isso altera o tipo de resultado de uma coleção de `Customer` objetos para uma coleção de cadeias de caracteres.  
  
 [!code-vb[VbLINQBasicOps#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#5)]  
  
 Para selecionar vários campos da fonte de dados, você tem duas opções:  
  
- Na `Select` cláusula, especifique os campos que você deseja incluir no resultado. O compilador irá definir um tipo anônimo que tenha esses campos como suas propriedades. Para obter mais informações, consulte [Tipos Anônimos](../../language-features/objects-and-classes/anonymous-types.md).  
  
     Como os elementos retornados no exemplo a seguir são instâncias de um tipo anônimo, você não pode fazer referência ao tipo por nome em outro lugar no seu código. O nome designado pelo compilador para o tipo contém caracteres que não são válidos no código de Visual Basic normal. No exemplo a seguir, os elementos na coleção que são retornados pela consulta no `londonCusts4` são instâncias de um tipo anônimo  
  
     [!code-vb[VbLINQBasicOps#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#6)]  
  
     -ou-  
  
- Defina um tipo nomeado que contenha os campos específicos que você deseja incluir no resultado e crie e inicialize instâncias do tipo na `Select` cláusula. Use esta opção somente se você precisar usar resultados individuais fora da coleção na qual eles são retornados ou se você tiver que passá-los como parâmetros nas chamadas de método. O tipo de `londonCusts5` no exemplo a seguir é IEnumerable (Of NamePhone).  
  
     [!code-vb[VbLINQBasicOps#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#7)]  
  
     [!code-vb[VbLINQBasicOps#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#8)]  
  
 Para obter mais informações sobre como usar a `Select` cláusula em Visual Basic, consulte [cláusula SELECT](../../../language-reference/queries/select-clause.md).  
  
## <a name="joining-data-join-and-group-join"></a>Ingressando Dados (Ingressar e Agrupar Junções)  
 Você pode combinar mais de uma fonte de dados na `From` cláusula de várias maneiras. Por exemplo, o código a seguir usa duas fontes de dados e combina implicitamente as propriedades de ambas elas no resultado. A consulta seleciona os alunos cujos últimos nomes começam com uma vogal.  
  
 [!code-vb[VbLINQBasicOps#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#9)]  
  
> [!NOTE]
> Você pode executar esse código com a lista de alunos criados em [como: criar uma lista de itens](how-to-create-a-list-of-items.md).  
  
 A `Join` palavra-chave é equivalente a um `INNER JOIN` em SQL. Ele combina duas coleções com base em valores de chave correspondentes entre elementos nas duas coleções. A consulta retorna todos ou parte dos elementos da coleção que têm valores de chave correspondentes. Por exemplo, o código a seguir duplica a ação da junção implícita anterior.  
  
 [!code-vb[VbLINQBasicOps#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#10)]  
  
 `Group Join`combina coleções em uma única coleção hierárquica, assim como `LEFT JOIN` em um SQL. Para obter mais informações, consulte cláusula [Join](../../../language-reference/queries/join-clause.md) e [cláusula Group Join](../../../language-reference/queries/group-join-clause.md).  
  
## <a name="grouping-data-group-by"></a>Agrupando Dados (Agrupar Por)  
 Você pode adicionar uma `Group By` cláusula para agrupar os elementos em um resultado de consulta de acordo com um ou mais campos dos elementos. Por exemplo, o código a seguir agrupa alunos por ano de aula.  
  
 [!code-vb[VbLINQBasicOps#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#11)]  
  
 Se você executar esse código usando a lista de alunos criados em [como: criar uma lista de itens](how-to-create-a-list-of-items.md), a saída da `For Each` instrução será:  
  
 Ano: Júnior  
  
 Tucker, Michael  
  
 Garcia, Hugo  
  
 Garcia, Débora  
  
 Tucker, lance  
  
 Ano: sênior  
  
 Omelchenko, Svetlana  
  
 Osada, Michiko  
  
 Fakhouri, Fadi  
  
 Feng, Hanying  
  
 Adams, Terry  
  
 Ano: atualizador  
  
 Mortensen, Sven  
  
 Garcia, Cesar  
  
 A variação mostrada no código a seguir ordena a classe Years e, em seguida, ordena os alunos em cada ano pelo sobrenome.  
  
 [!code-vb[VbLINQBasicOps#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#12)]  
  
 Para obter mais informações sobre `Group By` , consulte [a cláusula Group by](../../../language-reference/queries/group-by-clause.md).  
  
## <a name="see-also"></a>Confira também

- <xref:System.Collections.Generic.IEnumerable%601>
- [Introdução a LINQ no Visual Basic](getting-started-with-linq.md)
- [Consultas](../../../language-reference/queries/index.md)
- [Visão geral de operadores de consulta padrão (Visual Basic)](standard-query-operators-overview.md)
- [LINQ](../../language-features/linq/index.md)
