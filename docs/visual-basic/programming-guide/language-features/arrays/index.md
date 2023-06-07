---
title: Matrizes
ms.date: 12/06/2017
f1_keywords:
- vb.Array
helpviewer_keywords:
- arrays [Visual Basic]
- Visual Basic, arrays
ms.assetid: dbf29737-b589-4443-bee6-a27588d9c67e
ms.openlocfilehash: 5093f28f05c5b72294dce9a4e69723acafb31a9f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84413086"
---
# <a name="arrays-in-visual-basic"></a>Matrizes no Visual Basic

Uma matriz é um conjunto de valores, que são *elementos*com termo, que estão logicamente relacionados entre si. Por exemplo, uma matriz pode consistir no número de alunos em cada nível em uma escola gramatical; cada elemento da matriz é o número de alunos em uma única classificação. Da mesma forma, uma matriz pode consistir em notas de um aluno para uma classe; cada elemento da matriz é uma única classificação.

É possível que as variáveis individuais armazenem cada um de nossos itens de dados. Por exemplo, se nosso aplicativo analisar as notas dos alunos, podemos usar uma variável separada para a classificação de cada aluno, como `englishGrade1` , `englishGrade2` etc. Essa abordagem tem três limitações principais:

- Precisamos saber em tempo de design exatamente quantas notas temos para lidar.
- Manipular grandes números de notas rapidamente torna-se difícil. Isso, por sua vez, torna um aplicativo muito mais provável de ter bugs sérios.
- É difícil de manter. Cada nova classificação que adicionamos exige que o aplicativo seja modificado, recompilado e reimplantado.

Usando uma matriz, você pode consultar esses valores relacionados com o mesmo nome e usar um número chamado de *índice* ou *subscrito* para identificar um elemento individual com base em sua posição na matriz. Os índices de um intervalo de matriz de 0 para um menor que o número total de elementos na matriz. Quando você usa a sintaxe Visual Basic para definir o tamanho de uma matriz, você especifica seu índice mais alto, não o número total de elementos na matriz. Você pode trabalhar com a matriz como uma unidade e a capacidade de iterar seus elementos libera a necessidade de saber exatamente quantos elementos ele contém em tempo de design.

Alguns exemplos rápidos antes da explicação:

```vb
' Declare a single-dimension array of 5 numbers.
Dim numbers(4) As Integer

' Declare a single-dimension array and set its 4 values.
Dim numbers = New Integer() {1, 2, 4, 8}

' Change the size of an existing array to 16 elements and retain the current values.
ReDim Preserve numbers(15)

' Redefine the size of an existing array and reset the values.
ReDim numbers(15)

' Declare a 6 x 6 multidimensional array.
Dim matrix(5, 5) As Double

' Declare a 4 x 3 multidimensional array and set array element values.
Dim matrix = New Integer(3, 2) {{1, 2, 3}, {2, 3, 4}, {3, 4, 5}, {4, 5, 6}}

' Declare a jagged array
Dim sales()() As Double = New Double(11)() {}
```

## <a name="array-elements-in-a-simple-array"></a>Elementos de matriz em uma matriz simples

Vamos criar uma matriz chamada `students` para armazenar o número de alunos em cada nível em uma escola gramatical. Os índices dos elementos variam de 0 a 6. Usar essa matriz é mais simples do que declarar sete variáveis.

A ilustração a seguir mostra a `students` matriz. Para cada elemento da matriz:

- O índice do elemento representa a classificação (o índice 0 representa o jardim de infância).

- O valor contido no elemento representa o número de alunos nessa série.

![Diagrama mostrando uma matriz dos números de alunos](./media/index/students-array-elements.gif)

O exemplo a seguir contém o código de Visual Basic que cria e usa a matriz:

[!code-vb[simple-array](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/simple-array.vb)]

O exemplo faz três coisas:

- Ele declara uma `students` matriz com sete elementos. O número `6` na declaração de matriz indica o último índice na matriz; ele é um menor que o número de elementos na matriz.
- Ele atribui valores a cada elemento na matriz. Os elementos de matriz são acessados usando o nome da matriz e incluindo o índice do elemento individual entre parênteses.
- Ele lista cada valor da matriz. O exemplo usa uma [`For`](../../../language-reference/statements/for-next-statement.md) instrução para acessar cada elemento da matriz por seu número de índice.

A `students` matriz no exemplo anterior é uma matriz unidimensional porque usa um índice. Uma matriz que usa mais de um índice ou subscrito é chamada *multidimensional*. Para obter mais informações, consulte o restante deste artigo e [dimensões de matriz em Visual Basic](array-dimensions.md).

## <a name="creating-an-array"></a>Criando uma matriz

Você pode definir o tamanho de uma matriz de várias maneiras:

- Você pode especificar o tamanho quando a matriz é declarada:

  [!code-vb[creating1](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/create-array.vb#1)]

- Você pode usar uma `New` cláusula para fornecer o tamanho de uma matriz quando ela é criada:

  [!code-vb[creating2](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/create-array.vb#2)]

Se você tiver uma matriz existente, poderá redefinir seu tamanho usando a [`ReDim`](../../../language-reference/statements/redim-statement.md) instrução. Você pode especificar que a `ReDim` instrução Mantenha os valores que estão na matriz ou pode especificar que ele crie uma matriz vazia. O exemplo a seguir mostra os diferentes usos da instrução `ReDim` para modificar o tamanho de uma matriz existente.

[!code-vb[redimensioning](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/create-array.vb#3)]

Para obter mais informações, consulte a [instrução ReDim](../../../language-reference/statements/redim-statement.md).

## <a name="storing-values-in-an-array"></a>Armazenando valores em uma matriz

Você pode acessar cada local em uma matriz usando um índice do tipo `Integer`. Você pode armazenar e recuperar valores em uma matriz, fazendo referência a cada local de matriz usando seu índice entre parênteses. Os índices para matrizes multidimensionais são separados por vírgulas (,). Você precisa de um índice para cada dimensão de matriz.

O exemplo a seguir mostra algumas instruções que armazenam e recuperam valores em matrizes.

[!code-vb[store-and-retrieve](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/store-and-retrieve.vb)]

## <a name="populating-an-array-with-array-literals"></a>Populando uma matriz com literais de matriz

Usando um literal de matriz, você pode preencher uma matriz com um conjunto inicial de valores ao mesmo tempo em que você a cria. Um literal de matriz consiste em uma lista de valores separados por vírgulas que são colocados entre chaves (`{}`).

Ao criar uma matriz usando um literal de matriz, você pode fornecer o tipo de matriz ou usar inferência de tipos para determinar o tipo de matriz. O exemplo a seguir mostra as duas opções.

[!code-vb[create-with-literals](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/create-array.vb#4)]

Quando você usa a inferência de tipos, o tipo da matriz é determinado pelo *tipo dominante* na lista de valores literais. O tipo dominante é o tipo para o qual todos os outros tipos na matriz podem ser ampliados. Se esse tipo exclusivo não puder ser determinado, o tipo dominante será o tipo exclusivo ao qual todos os outros tipos na matriz poderão restringir. Se nenhum desses tipos exclusivos puder ser determinado, o tipo dominante será `Object`. Por exemplo, se a lista de valores que é fornecida para o literal de matriz contiver os valores do tipo `Integer`, `Long` e `Double`, a matriz resultante será do tipo `Double`. Porque `Integer` e `Long` se ampliam apenas para `Double` , `Double` é o tipo dominante. Para obter mais informações, consulte [Ampliando e restringindo conversões](../data-types/widening-and-narrowing-conversions.md).

> [!NOTE]
> Você pode usar a inferência de tipos somente para matrizes que são definidas como variáveis locais em um membro de tipo. Se uma definição de tipo explícita estiver ausente, as matrizes definidas com literais de matriz no nível de classe serão do tipo `Object[]` . Para obter mais informações, consulte [inferência de tipo local](../variables/local-type-inference.md).

Observe que o exemplo anterior define `values` como uma matriz do tipo `Double` , embora todos os literais de matriz sejam do tipo `Integer` . Você pode criar essa matriz porque os valores no literal de matriz podem ser ampliados para `Double` valores.

Você também pode criar e popular uma matriz multidimensional usando *literais de matriz aninhada*. Literais de matriz aninhada devem ter um número de dimensões que seja consistente com a matriz resultante. O exemplo a seguir cria uma matriz bidimensional de inteiros usando literais de matriz aninhada.

[!code-vb[nested-array-literals](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/create-array.vb#5)]

Ao usar literais de matriz aninhada para criar e preencher uma matriz, ocorrerá um erro se o número de elementos nos literais de matriz aninhada não corresponder. Também ocorrerá um erro se você declarar explicitamente a variável de matriz para ter um número diferente de dimensões do que os literais de matriz.

Assim como você pode para matrizes unidimensionais, você pode confiar na inferência de tipos ao criar uma matriz multidimensional com literais de matriz aninhada. O tipo inferido é o tipo dominante para todos os valores em todos os literais de matriz para todo o nível de aninhamento. O exemplo a seguir cria uma matriz bidimensional do tipo `Double[,]` de valores que são do tipo `Integer` e `Double` .

[!code-vb[nested-type-inference](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/create-array.vb#6)]

Para obter exemplos adicionais, consulte [Como inicializar uma variável de matriz no Visual Basic](how-to-initialize-an-array-variable.md).

## <a name="iterating-through-an-array"></a>Iterando por meio de uma matriz

Ao iterar por meio de uma matriz, você acessa cada elemento na matriz do índice mais baixo para o mais alto ou do mais alto para o mais baixo. Normalmente, use a opção [for... Próxima instrução](../../../language-reference/statements/for-next-statement.md) ou o [para cada... Próxima instrução](../../../language-reference/statements/for-each-next-statement.md) para iterar pelos elementos de uma matriz. Quando você não conhece os limites superiores da matriz, você pode chamar o <xref:System.Array.GetUpperBound%2A?displayProperty=nameWithType> método para obter o valor mais alto do índice. Embora o menor valor de índice seja quase sempre 0, você pode chamar o <xref:System.Array.GetLowerBound%2A?displayProperty=nameWithType> método para obter o valor mais baixo do índice.

O exemplo a seguir itera por meio de uma matriz unidimensional usando a [`For...Next`](../../../language-reference/statements/for-next-statement.md) instrução.

[!code-vb[iterate-one-dimensional-array](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/iterate1d.vb)]

O exemplo a seguir itera por meio de uma matriz multidimensional usando uma [`For...Next`](../../../language-reference/statements/for-next-statement.md) instrução. O método <xref:System.Array.GetUpperBound%2A> tem um parâmetro que especifica a dimensão. `GetUpperBound(0)`Retorna o índice mais alto da primeira dimensão e `GetUpperBound(1)` retorna o índice mais alto da segunda dimensão.

[!code-vb[iterate-two-dimensional-array](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/iterate2d.vb)]

O exemplo a seguir usa um [para cada... Próxima instrução](../../../language-reference/statements/for-each-next-statement.md)para iterar por meio de uma matriz unidimensional e uma matriz bidimensional.

[!code-vb[iterate-for-each-next](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/iterate-for-each-next.vb)]

## <a name="array-size"></a>Tamanho da matriz

O tamanho de uma matriz é o produto dos comprimentos de todas as suas dimensões. Ele representa o número total de elementos contidos no momento na matriz.  Por exemplo, o exemplo a seguir declara uma matriz bidimensional com quatro elementos em cada dimensão. Como a saída do exemplo mostra, o tamanho da matriz é 16 (ou (3 + 1) * (3 + 1).

[!code-vb[array-size](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/array-size.vb)]

> [!NOTE]
> Essa discussão sobre o tamanho da matriz não se aplica a matrizes denteadas. Para obter informações sobre matrizes denteadas e como determinar o tamanho de uma matriz denteada, consulte a seção [matrizes](#jagged-arrays) denteadas.

Você pode encontrar o tamanho de uma matriz usando a propriedade <xref:System.Array.Length%2A?displayProperty=nameWithType>. Você pode encontrar o comprimento de cada dimensão de uma matriz multidimensional usando o <xref:System.Array.GetLength%2A?displayProperty=nameWithType> método.

Você pode redimensionar uma variável de matriz atribuindo um novo objeto de matriz a ela ou usando a instrução de [ `ReDim` instrução](../../../language-reference/statements/redim-statement.md) . O exemplo a seguir usa a `ReDim` instrução para alterar uma matriz de elemento 100 para uma matriz de 51 elementos.

[!code-vb[resize-an-array](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/array-size2.vb)]

Há várias coisas para ter em mente ao lidar com o tamanho de uma matriz.

|||
|---|---|
|Tamanho da dimensão|O índice de cada dimensão é baseado em 0, o que significa que ele varia de 0 até seu limite superior. Portanto, o comprimento de uma determinada dimensão é um maior que o limite superior declarado dessa dimensão.|
|Limites de comprimento|O comprimento de cada dimensão de uma matriz é limitado ao valor máximo do tipo de `Integer` dados, que é <xref:System.Int32.MaxValue?displayProperty=nameWithType> ou (2 ^ 31)-1. No entanto, o tamanho total de uma matriz também é limitado pela memória disponível no sistema. Se você tentar inicializar uma matriz que exceda a quantidade de memória disponível, o tempo de execução lançará um <xref:System.OutOfMemoryException> .|
|Tamanho e tamanho do elemento|Um tamanho de matriz é independente do tipo de dados de seus elementos. O tamanho sempre representa o número total de elementos, não o número de bytes que eles consomem na memória.|
|Consumo de memória|Não é seguro fazer suposições sobre como uma matriz é armazenada na memória. O armazenamento varia em plataformas de larguras de dados diferentes, então a mesma matriz pode consumir mais memória em um sistema de 64 bits que em um sistema de 32 bits. Dependendo da configuração do sistema ao inicializar uma matriz, o CLR (Common Language Runtime) pode atribuir armazenamento para elementos do pacote o mais próximo possível, ou alinhá-los em limites naturais de hardware. Além disso, uma matriz de armazenamento requer sobrecarga de armazenamento para suas informações de controle, e essa sobrecarga aumenta com cada nova dimensão.|

## <a name="the-array-type"></a>O tipo de matriz

Cada matriz tem um tipo de dados, que difere do tipo de dados de seus elementos. Não há nenhum tipo de dados único para todas as matrizes. Em vez disso, o tipo de dados de uma matriz é determinado pelo número de dimensões, ou *classificação*, da matriz e o tipo de dados dos elementos na matriz. Duas variáveis de matriz são do mesmo tipo de dados somente quando têm a mesma classificação e seus elementos têm o mesmo tipo de dados. Os comprimentos das dimensões de uma matriz não influenciam o tipo de dados de matriz.

Cada matriz herda da classe <xref:System.Array?displayProperty=nameWithType> e você pode declarar uma variável para ser do tipo `Array`, mas não pode criar uma matriz do tipo `Array`. Por exemplo, embora o código a seguir declare que a `arr` variável seja do tipo `Array` e chame o <xref:System.Array.CreateInstance%2A?displayProperty=nameWithType> método para instanciar a matriz, o tipo da matriz comprova ser Object [].

[!code-vb[array-class](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/array-class.vb)]

Além disso, a [instrução ReDim](../../../language-reference/statements/redim-statement.md) não pode operar em uma variável declarada como tipo `Array`. Por esses motivos, e para a segurança de tipo, é aconselhável declarar cada matriz como um tipo específico.

Você pode descobrir o tipo de dados de uma matriz ou seus elementos de várias maneiras.

- Você pode chamar o <xref:System.Object.GetType%2A> método na variável para obter um <xref:System.Type> objeto que representa o tipo de tempo de execução da variável. O objeto <xref:System.Type> mantém informações abrangentes em suas propriedades e métodos.
- Você pode passar a variável para a <xref:Microsoft.VisualBasic.Information.TypeName%2A> função para obter um `String` com o nome do tipo de tempo de execução.

O exemplo a seguir chama o `GetType` método e a `TypeName` função para determinar o tipo de uma matriz. O tipo de matriz é `Byte(,)` . Observe que a <xref:System.Type.BaseType%2A?displayProperty=nameWithType> propriedade também indica que o tipo base da matriz de bytes é a <xref:System.Array> classe.

[!code-vb[array-type](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/array-type.vb)]

## <a name="arrays-as-return-values-and-parameters"></a>Matrizes como valores de retorno e parâmetros

Para retornar uma matriz de um procedimento `Function`, especifique o tipo de dados de matriz e o número de dimensões como o tipo de retorno de [instrução de função](../../../language-reference/statements/function-statement.md). Dentro da função, declare uma variável da matriz local com o mesmo tipo de dados e número de dimensões. Na [instrução Return](../../../language-reference/statements/return-statement.md), inclua a variável da matriz local sem parênteses.

Para especificar uma matriz como um parâmetro para um procedimento `Sub` ou `Function`, defina o parâmetro como uma matriz com um tipo de dados especificado e o número de dimensões. Na chamada para o procedimento, passe uma variável de matriz com o mesmo tipo de dados e número de dimensões.

No exemplo a seguir, a `GetNumbers` função retorna um `Integer()` , uma matriz unidimensional do tipo `Integer` . O procedimento `ShowNumbers` aceita um argumento `Integer()`.

[!code-vb[return-value-and-params](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/return-values-and-params.vb)]

No exemplo a seguir, a `GetNumbersMultiDim` função retorna um `Integer(,)` , uma matriz bidimensional do tipo `Integer` .  O procedimento `ShowNumbersMultiDim` aceita um argumento `Integer(,)`.

[!code-vb[multidimensional-return-value](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/return-values-and-params-2d.vb)]

## <a name="jagged-arrays"></a>Matrizes denteadas

Às vezes, a estrutura de dados em seu aplicativo é bidimensional, mas não retangular. Por exemplo, você pode usar uma matriz para armazenar dados sobre a alta temperatura de cada dia do mês. A primeira dimensão da matriz representa o mês, mas a segunda dimensão representa o número de dias e o número de dias em um mês não é uniforme. Uma *matriz denteada*, que também é chamada *de matriz de matrizes*, é projetada para esses cenários. Uma matriz denteada é uma matriz cujos elementos também são matrizes. Uma matriz denteada e cada elemento em uma matriz denteada podem ter uma ou mais dimensões.

O exemplo a seguir usa uma matriz de meses, cada elemento do qual é uma matriz de dias. O exemplo usa uma matriz denteada porque meses diferentes têm números de dias diferentes.  O exemplo mostra como criar uma matriz denteada, atribuir valores a ela e recuperar e exibir seus valores.

[!code-vb[jagged-arrays](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/jagged.vb)]

O exemplo anterior atribui valores à matriz denteada em um elemento por elemento usando um `For...Next` loop. Você também pode atribuir valores aos elementos de uma matriz denteada usando literais de matriz aninhada. No entanto, a tentativa de usar literais de matriz aninhada (por exemplo, `Dim valuesjagged = {{1, 2}, {2, 3, 4}}` ) gera o erro de compilador [BC30568](../../../misc/bc30568.md). Para corrigir o erro, coloque os literais da matriz interna entre parênteses. Os parênteses forçam a expressão literal de matriz a ser avaliada e os valores resultantes são usados com o literal de matriz externa, como mostra o exemplo a seguir.

[!code-vb[jagged-array-initialization](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/jagged-assign.vb)]

Uma matriz denteada é uma matriz unidimensional cujos elementos contêm matrizes. Portanto, a <xref:System.Array.Length%2A?displayProperty=nameWithType> propriedade e o `Array.GetLength(0)` método retornam o número de elementos na matriz unidimensional e `Array.GetLength(1)` gera um <xref:System.IndexOutOfRangeException> porque uma matriz denteada não é multidimensional. Você determina o número de elementos em cada submatriz recuperando o valor de cada propriedade da submatriz <xref:System.Array.Length%2A?displayProperty=nameWithType> . O exemplo a seguir ilustra como determinar o número de elementos em uma matriz denteada.

[!code-vb[jagged-array-size](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/jagged-length.vb)]

## <a name="zero-length-arrays"></a>Matrizes de comprimento zero

Visual Basic diferencia entre uma matriz não inicializada (uma matriz cujo valor é `Nothing` ) e uma matriz de *comprimento zero* ou uma matriz vazia (uma matriz que não tem elementos.) Uma matriz não inicializada é aquela que não foi dimensionável ou tinha valores atribuídos a ela. Por exemplo:

```vb
Dim arr() As String
```

Uma matriz de comprimento zero é declarada com uma dimensão de-1. Por exemplo:

```vb
Dim arrZ(-1) As String
```

Talvez seja necessário criar uma matriz de tamanho igual a zero nas seguintes circunstâncias:

- Sem arriscar uma <xref:System.NullReferenceException> exceção, seu código deve acessar membros da <xref:System.Array> classe, como <xref:System.Array.Length%2A> ou <xref:System.Array.Rank%2A> , ou chamar uma função de Visual Basic <xref:Microsoft.VisualBasic.Information.UBound%2A> , como.

- Você deseja manter seu código simples por não ter que verificar `Nothing` como um caso especial.

- Seu código interage com uma API (interface de programação do aplicativo) que exige que você passe uma matriz de tamanho igual a zero para um ou mais procedimentos ou retorna uma matriz de tamanho igual a zero de um ou mais procedimentos.

## <a name="splitting-an-array"></a>Dividindo uma matriz

Em alguns casos, talvez seja necessário dividir uma única matriz em várias matrizes. Isso envolve identificar o ponto ou pontos em que a matriz deve ser dividida e, em seguida, Spitting a matriz em duas ou mais matrizes separadas.

> [!NOTE]
> Esta seção não discute como dividir uma única cadeia de caracteres em uma matriz de cadeia de caracteres com base em algum delimitador. Para obter informações sobre como dividir uma cadeia de caracteres, consulte o <xref:System.String.Split%2A?displayProperty=nameWithType> método.

Os critérios mais comuns para dividir uma matriz são:

- O número de elementos na matriz. Por exemplo, você pode desejar dividir uma matriz de mais de um número especificado de elementos em um número de partes aproximadamente iguais. Para essa finalidade, você pode usar o valor retornado pelo <xref:System.Array.Length%2A?displayProperty=nameWithType> <xref:System.Array.GetLength%2A?displayProperty=nameWithType> método ou.

- O valor de um elemento, que serve como um delimitador que indica onde a matriz deve ser dividida. Você pode procurar um valor específico chamando os <xref:System.Array.FindIndex%2A?displayProperty=nameWithType> <xref:System.Array.FindLastIndex%2A?displayProperty=nameWithType> métodos e.

Depois de determinar o índice ou os índices nos quais a matriz deve ser dividida, você poderá criar as matrizes individuais chamando o <xref:System.Array.Copy%2A?displayProperty=nameWithType> método.

O exemplo a seguir divide uma matriz em duas matrizes de tamanho aproximadamente igual. (Se o número total de elementos de matriz for ímpar, a primeira matriz terá mais um elemento do que o segundo.)

[!code-vb[splitting-an-array-by-length](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/split1.vb)]

O exemplo a seguir divide uma matriz de cadeia de caracteres em duas matrizes com base na presença de um elemento cujo valor é "ZZZ", que serve como o delimitador de matriz. As novas matrizes não incluem o elemento que contém o delimitador.

[!code-vb[splitting-an-array-by-delimiter](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/split2.vb)]

## <a name="joining-arrays"></a>Unindo matrizes

Você também pode combinar um número de matrizes em uma única matriz maior. Para fazer isso, você também usa o <xref:System.Array.Copy%2A?displayProperty=nameWithType> método.

> [!NOTE]
> Esta seção não aborda a junção de uma matriz de cadeia de caracteres em uma única cadeia. Para obter informações sobre como ingressar em uma matriz de cadeia de caracteres, consulte o <xref:System.String.Join%2A?displayProperty=nameWithType> método.

Antes de copiar os elementos de cada matriz para a nova matriz, primeiro você deve garantir que tenha inicializado a matriz para que seja grande o suficiente para acomodar a nova matriz. É possível fazer isso de duas formas:

- Use a [`ReDim Preserve`](../../../language-reference/statements/redim-statement.md) instrução para expandir dinamicamente a matriz antes de adicionar novos elementos a ela. Essa é a técnica mais fácil, mas pode resultar em degradação de desempenho e consumo excessivo de memória quando você estiver copiando matrizes grandes.
- Calcule o número total de elementos necessários para a nova matriz grande e, em seguida, adicione os elementos de cada matriz de origem a ele.

O exemplo a seguir usa a segunda abordagem para adicionar quatro matrizes com dez elementos cada para uma única matriz.

[!code-vb[joining-an-array](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/join.vb)]

Como nesse caso, as matrizes de origem são todas pequenas, também podemos expandir dinamicamente a matriz à medida que adicionamos os elementos de cada nova matriz a ela. O exemplo a seguir faz isso.

[!code-vb[joining-an-array-dynamically](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/join2.vb)]

## <a name="collections-as-an-alternative-to-arrays"></a>Coleções como uma alternativa às matrizes

As matrizes são mais úteis para criar e trabalhar com um número fixo de objetos fortemente tipados. As coleções fornecem uma maneira mais flexível de trabalhar com grupos de objetos. Ao contrário de matrizes, que exigem que você altere explicitamente o tamanho de uma matriz com a [ `ReDim` instrução](../../../language-reference/statements/redim-statement.md), as coleções crescem e diminuem dinamicamente conforme as necessidades de um aplicativo são alteradas.

Quando você usa `ReDim` para redimensionar uma matriz, Visual Basic cria uma nova matriz e libera a anterior. Isso leva o tempo da execução. Portanto, se o número de itens com os quais você está trabalhando for alterado com frequência ou se você não puder prever o número máximo de itens de que precisa, geralmente obterá um melhor desempenho usando uma coleção.

Para algumas coleções, você pode atribuir uma chave para qualquer objeto que coloque na coleção para que você possa recuperar rapidamente o objeto usando a chave.

Se a coleção contiver elementos de apenas um tipo de dados, você poderá usar uma das classes no namespace <xref:System.Collections.Generic?displayProperty=nameWithType>. Uma coleção genérica impõe segurança de tipos para que nenhum outro tipo de dados possa ser adicionado a ela.

Para obter mais informações sobre coleções, consulte [Coleções](../../concepts/collections.md).

## <a name="related-topics"></a>Tópicos relacionados

|Termo|Definição|
|----------|----------------|
|[Dimensões de matriz no Visual Basic](array-dimensions.md)|Explica a classificação e as dimensões em matrizes.|
|[Como inicializar uma variável de matriz no Visual Basic](how-to-initialize-an-array-variable.md)|Descreve como preencher matrizes com valores iniciais.|
|[Como classificar uma matriz no Visual Basic](how-to-sort-an-array.md)|Mostra como classificar os elementos de uma matriz em ordem alfabética.|
|[Como atribuir uma matriz a outra matriz](how-to-assign-one-array-to-another-array.md)|Descreve as regras e as etapas para atribuir uma matriz a outra variável de matriz.|
|[Solução de problemas de matrizes](troubleshooting-arrays.md)|Aborda alguns problemas comuns que surgem ao trabalhar com matrizes.|

## <a name="see-also"></a>Confira também

- <xref:System.Array?displayProperty=nameWithType>
- [Instrução Dim](../../../language-reference/statements/dim-statement.md)
- [Instrução ReDim](../../../language-reference/statements/redim-statement.md)
