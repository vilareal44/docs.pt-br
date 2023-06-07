---
title: Relacionamentos entre tipos em operações de consulta
ms.date: 07/20/2015
helpviewer_keywords:
- variable relationships [LINQ in Visual Basic]
- type information inferred [LINQ in Visual Basic]
- type relationships [LINQ in Visual Basic]
- queries [LINQ in Visual Basic], type relationships
- data sources [LINQ in Visual Basic], type relationships
- LINQ [Visual Basic], type relationships
- inferring type information [LINQ in Visual Basic]
- relationships [LINQ in Visual Basic]
ms.assetid: b5ff4da5-f3fd-4a8e-aaac-1cbf52fa16f6
ms.openlocfilehash: 73a287541ddf115510bf6ab5c830eafac370cc3a
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406723"
---
# <a name="type-relationships-in-query-operations-visual-basic"></a>Relacionamentos de tipo em operações de consulta (Visual Basic)

As variáveis usadas em operações de consulta do LINQ (consulta integrada à linguagem) são fortemente tipadas e devem ser compatíveis entre si. A tipagem forte é usada na fonte de dados, na própria consulta e na execução da consulta. A ilustração a seguir identifica os termos usados para descrever uma consulta LINQ. Para obter mais informações sobre as partes de uma consulta, consulte [Basic Query Operations (Visual Basic)](basic-query-operations.md).

![Captura de tela mostrando uma consulta de pseudocódigo com elementos realçados.](./media/type-relationships-in-query-operations/linq-query-description-terms.png)

O tipo da variável de intervalo na consulta deve ser compatível com o tipo dos elementos na fonte de dados. O tipo da variável de consulta deve ser compatível com o elemento Sequence definido na `Select` cláusula. Por fim, o tipo dos elementos de sequência também deve ser compatível com o tipo da variável de controle de loop que é usada na `For Each` instrução que executa a consulta. Essa tipagem forte facilita a identificação de erros de tipo no momento da compilação.

Visual Basic facilita a digitação de rigidez, implementando a inferência de tipo local, também conhecida como *digitação implícita*. Esse recurso é usado no exemplo anterior e você verá que ele é usado em todos os exemplos e documentação do LINQ. Em Visual Basic, a inferência de tipo local é realizada simplesmente usando uma `Dim` instrução sem uma `As` cláusula. No exemplo a seguir, `city` é fortemente tipado como uma cadeia de caracteres.

[!code-vb[VbLINQTypeRels#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQTypeRels/VB/Class1.vb#1)]

> [!NOTE]
> A inferência de tipo local só funciona quando `Option Infer` é definido como `On` . Para obter mais informações, consulte [instrução Option Infer](../../../language-reference/statements/option-infer-statement.md).

No entanto, mesmo se você usar a inferência de tipo local em uma consulta, as mesmas relações de tipo estarão presentes entre as variáveis na fonte de dados, a variável de consulta e o loop de execução da consulta. É útil ter um entendimento básico dessas relações de tipo quando você estiver escrevendo consultas LINQ ou trabalhando com exemplos e códigos de exemplo na documentação.

Talvez seja necessário especificar um tipo explícito para uma variável de intervalo que não corresponda ao tipo retornado da fonte de dados. Você pode especificar o tipo da variável de intervalo usando uma `As` cláusula. No entanto, isso resultará em um erro se a conversão for uma [conversão de restrição](../../language-features/data-types/widening-and-narrowing-conversions.md) e `Option Strict` for definida como `On` . Portanto, recomendamos que você execute a conversão nos valores recuperados da fonte de dados. Você pode converter os valores da fonte de dados para o tipo de variável de intervalo explícito usando o <xref:System.Linq.Enumerable.Cast%2A> método. Você também pode converter os valores selecionados na `Select` cláusula para um tipo explícito que seja diferente do tipo da variável de intervalo. Esses pontos são ilustrados no código a seguir.

[!code-vb[VbLINQTypeRels#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQTypeRels/VB/Class1.vb#4)]

## <a name="queries-that-return-entire-elements-of-the-source-data"></a>Consultas que retornam elementos inteiros dos dados de origem

O exemplo a seguir mostra uma operação de consulta LINQ que retorna uma sequência de elementos selecionados a partir dos dados de origem. A origem, `names` , contém uma matriz de cadeias de caracteres e a saída da consulta é uma sequência que contém cadeias de caracteres que começam com a letra M.

[!code-vb[VbLINQTypeRels#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQTypeRels/VB/Class1.vb#2)]

Isso é equivalente ao código a seguir, mas é muito mais curto e mais fácil de escrever. A dependência da inferência de tipo local em consultas é o estilo preferencial em Visual Basic.

[!code-vb[VbLINQTypeRels#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQTypeRels/VB/Class1.vb#3)]

As relações a seguir existem nos dois exemplos de código anteriores, independentemente de os tipos serem determinados implicitamente ou explicitamente.

1. O tipo dos elementos na fonte de dados, `names` , é o tipo da variável de intervalo, `name` , na consulta.

2. O tipo do objeto selecionado, `name` , determina o tipo da variável de consulta, `mNames` . Aqui `name` está uma cadeia de caracteres, portanto, a variável de consulta é IEnumerable (Of String) em Visual Basic.

3. A consulta definida em `mNames` é executada no `For Each` loop. O loop itera sobre o resultado da execução da consulta. Como `mNames` , quando é executado, retornará uma sequência de cadeias de caracteres, a variável de iteração de loop, `nm` , também é uma cadeia de caracteres.

## <a name="queries-that-return-one-field-from-selected-elements"></a>Consultas que retornam um campo dos elementos selecionados

O exemplo a seguir mostra uma [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] operação de consulta que retorna uma sequência contendo apenas uma parte de cada elemento selecionado na fonte de dados. A consulta usa uma coleção de `Customer` objetos como sua fonte de dados e projeta apenas a `Name` propriedade no resultado. Como o nome do cliente é uma cadeia de caracteres, a consulta produz uma sequência de cadeias de caracteres como saída.

```vb
' Method GetTable returns a table of Customer objects.
Dim customers = db.GetTable(Of Customer)()
Dim custNames = From cust In customers
                Where cust.City = "London"
                Select cust.Name

For Each custName In custNames
    Console.WriteLine(custName)
Next
```

As relações entre as variáveis são como as do exemplo mais simples.

1. O tipo dos elementos na fonte de dados, `customers` , é o tipo da variável de intervalo, `cust` , na consulta. Neste exemplo, esse tipo é `Customer` .

2. A `Select` instrução retorna a `Name` propriedade de cada `Customer` objeto em vez de todo o objeto. Como `Name` é uma cadeia de caracteres, a variável de consulta, `custNames` , novamente será IEnumerable (Of String), não de `Customer` .

3. Como `custNames` representa uma sequência de cadeias de caracteres, a `For Each` variável de iteração do loop, `custName` , deve ser uma cadeia de caracteres.

Sem a inferência de tipo local, o exemplo anterior seria mais complicado de escrever e entender, como mostra o exemplo a seguir.

```vb
' Method GetTable returns a table of Customer objects.
 Dim customers As Table(Of Customer) = db.GetTable(Of Customer)()
 Dim custNames As IEnumerable(Of String) =
     From cust As Customer In customers
     Where cust.City = "London"
     Select cust.Name

 For Each custName As String In custNames
     Console.WriteLine(custName)
 Next
```

## <a name="queries-that-require-anonymous-types"></a>Consultas que exigem tipos anônimos

O exemplo a seguir mostra uma situação mais complexa. No exemplo anterior, era inconveniente especificar tipos para todas as variáveis explicitamente. Neste exemplo, é impossível. Em vez de selecionar `Customer` elementos inteiros da fonte de dados, ou um único campo de cada elemento, a `Select` cláusula nessa consulta retorna duas propriedades do objeto original `Customer` : `Name` e `City` . Em resposta à `Select` cláusula, o compilador define um tipo anônimo que contém essas duas propriedades. O resultado da execução `nameCityQuery` no `For Each` loop é uma coleção de instâncias do novo tipo anônimo. Como o tipo anônimo não tem nome utilizável, você não pode especificar o tipo de `nameCityQuery` ou `custInfo` explicitamente. Ou seja, com um tipo anônimo, você não tem nenhum nome de tipo a ser usado no lugar do `String` `IEnumerable(Of String)` . Para obter mais informações, consulte [Tipos Anônimos](../../language-features/objects-and-classes/anonymous-types.md).

```vb
' Method GetTable returns a table of Customer objects.
Dim customers = db.GetTable(Of Customer)()
Dim nameCityQuery = From cust In customers
                    Where cust.City = "London"
                    Select cust.Name, cust.City

For Each custInfo In nameCityQuery
    Console.WriteLine(custInfo.Name)
Next
```

Embora não seja possível especificar tipos para todas as variáveis no exemplo anterior, as relações permanecem as mesmas.

1. O tipo dos elementos na fonte de dados é novamente o tipo da variável de intervalo na consulta. Neste exemplo, `cust` é uma instância do `Customer` .

2. Como a `Select` instrução produz um tipo anônimo, a variável de consulta, `nameCityQuery` , deve ser digitada implicitamente como um tipo anônimo. Um tipo anônimo não tem nome utilizável e, portanto, não pode ser especificado explicitamente.

3. O tipo da variável de iteração no `For Each` loop é o tipo anônimo criado na etapa 2. Como o tipo não tem nenhum nome utilizável, o tipo da variável de iteração de loop deve ser determinado implicitamente.

## <a name="see-also"></a>Confira também

- [Introdução a LINQ no Visual Basic](getting-started-with-linq.md)
- [Tipos anônimos](../../language-features/objects-and-classes/anonymous-types.md)
- [Inferência de Tipo de Variável Local](../../language-features/variables/local-type-inference.md)
- [Introdução a LINQ no Visual Basic](../../language-features/linq/introduction-to-linq.md)
- [LINQ](../../language-features/linq/index.md)
- [Consultas](../../../language-reference/queries/index.md)
