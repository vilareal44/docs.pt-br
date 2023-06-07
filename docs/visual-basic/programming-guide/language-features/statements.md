---
title: Instruções
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], declaring
- colons (:) [Visual Basic]
- constants [Visual Basic], defining
- underlines
- constants [Visual Basic], statements
- blue underline [Visual Basic]
- procedures [Visual Basic], statements
- variables [Visual Basic], assigning
- line breaks [Visual Basic], in code
- executable statements [Visual Basic]
- variables [Visual Basic], defining
- statements [Visual Basic], about statements
ms.assetid: fcfdee1a-82b7-4846-98f7-9ca3f5160089
ms.openlocfilehash: 09fe53f4bc2b6d025b762c6595c5337263456bae
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401973"
---
# <a name="statements-in-visual-basic"></a>Instruções no Visual Basic

Uma instrução no Visual Basic é uma instrução completa. Ele pode conter palavras-chave, operadores, variáveis, constantes e expressões. Cada instrução pertence a uma das seguintes categorias:

- **Instruções de declaração**, que têm o nome de uma variável, constante ou procedimento, e também podem especificar um tipo de dados.

- **Instruções Executáveis**, que iniciam ações. Essas instruções podem chamar um método ou uma função e podem executar loops ou ramificar por meio de blocos de código. As instruções Executáveis incluem **instruções de atribuição**, que atribuem um valor ou uma expressão a uma variável ou constante.

Este tópico descreve cada categoria. Além disso, este tópico descreve como combinar várias instruções em uma única linha e como continuar uma instrução em várias linhas.

## <a name="declaration-statements"></a>Instruções de declaração

Você usa instruções de declaração para nomear e definir procedimentos, variáveis, propriedades, matrizes e constantes. Ao declarar um elemento de programação, você também pode definir seu tipo de dados, nível de acesso e escopo. Para obter mais informações, consulte [características de elemento declaradas](./declared-elements/declared-element-characteristics.md).

O exemplo a seguir contém três declarações.

[!code-vb[VbVbalrStatements#80](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#80)]

A primeira declaração é a `Sub` instrução. Junto com sua `End Sub` instrução correspondente, ele declara um procedimento chamado `applyFormat` . Ele também especifica isso `applyFormat` `Public` , o que significa que qualquer código que possa fazer referência a ele pode chamá-lo.

A segunda declaração é a `Const` instrução, que declara a constante `limit` , especificando o `Integer` tipo de dados e um valor de 33.

A terceira declaração é a `Dim` instrução, que declara a variável `thisWidget` . O tipo de dados é um objeto específico, ou seja, um objeto criado a partir da `Widget` classe. Você pode declarar uma variável para ser de qualquer tipo de dados elementar ou de qualquer tipo de objeto que seja exposto no aplicativo que você está usando.

### <a name="initial-values"></a>Valores iniciais

Quando o código que contém uma instrução de declaração é executado, Visual Basic reserva a memória necessária para o elemento declarado. Se o elemento mantiver um valor, Visual Basic o inicializará para o valor padrão para seu tipo de dados. Para obter mais informações, consulte "Behavior" na [instrução Dim](../../language-reference/statements/dim-statement.md).

Você pode atribuir um valor inicial a uma variável como parte de sua declaração, como ilustra o exemplo a seguir.

[!code-vb[VbVbalrStatements#81](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#81)]

Se uma variável for uma variável de objeto, você poderá criar explicitamente uma instância de sua classe ao declará-la usando a palavra-chave [New Operator](../../language-reference/operators/new-operator.md) , como ilustra o exemplo a seguir.

[!code-vb[VbVbalrStatements#82](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#82)]

Observe que o valor inicial especificado em uma instrução de declaração não é atribuído a uma variável até que a execução atinja sua instrução de declaração. Até esse momento, a variável contém o valor padrão para seu tipo de dados.

## <a name="executable-statements"></a>Instruções Executáveis

Uma instrução executável executa uma ação. Ele pode chamar um procedimento, Branch para outro lugar no código, executar um loop através de várias instruções ou avaliar uma expressão. Uma instrução de atribuição é um caso especial de uma instrução executável.

O exemplo a seguir usa uma `If...Then...Else` estrutura de controle para executar diferentes blocos de código com base no valor de uma variável. Dentro de cada bloco de código, um `For...Next` loop é executado um número especificado de vezes.

[!code-vb[VbVbalrStatements#83](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#83)]

A `If` instrução no exemplo anterior verifica o valor do parâmetro `clockwise` . Se o valor for `True` , ele chamará o `spinClockwise` método de `aWidget` . Se o valor for `False` , ele chamará o `spinCounterClockwise` método de `aWidget` . A `If...Then...Else` estrutura de controle termina com `End If` .

O `For...Next` loop dentro de cada bloco chama o método apropriado um número de vezes igual ao valor do `revolutions` parâmetro.

## <a name="assignment-statements"></a>Instruções de atribuição

As instruções de atribuição executam operações de atribuição, que consistem em pegar o valor no lado direito do operador de atribuição ( `=` ) e armazená-lo no elemento à esquerda, como no exemplo a seguir.

[!code-vb[VbVbalrStatements#73](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#73)]

No exemplo anterior, a instrução de atribuição armazena o valor literal 42 na variável `v` .

### <a name="eligible-programming-elements"></a>Elementos de programação qualificados

O elemento de programação no lado esquerdo do operador de atribuição deve ser capaz de aceitar e armazenar um valor. Isso significa que ele deve ser uma variável ou propriedade que não seja [ReadOnly](../../language-reference/modifiers/readonly.md)ou deve ser um elemento de matriz. No contexto de uma instrução de atribuição, esse elemento, às vezes, é chamado de *lvalue*, para "valor esquerdo".

O valor no lado direito do operador de atribuição é gerado por uma expressão, que pode consistir em qualquer combinação de literais, constantes, variáveis, propriedades, elementos de matriz, outras expressões ou chamadas de função. O exemplo a seguir ilustra isto.

[!code-vb[VbVbalrStatements#74](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#74)]

O exemplo anterior adiciona o valor mantido em variável `y` ao valor mantido em variável `z` e, em seguida, adiciona o valor retornado pela chamada para function `findResult` . Em seguida, o valor total dessa expressão é armazenado em variável `x` .

### <a name="data-types-in-assignment-statements"></a>Tipos de dados em instruções de atribuição

Além dos valores numéricos, o operador de atribuição também pode atribuir `String` valores, como ilustra o exemplo a seguir.

[!code-vb[VbVbalrStatements#75](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#75)]

Você também pode atribuir `Boolean` valores, usando um `Boolean` literal ou uma `Boolean` expressão, como ilustra o exemplo a seguir.

[!code-vb[VbVbalrStatements#76](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#76)]

Da mesma forma, você pode atribuir valores apropriados para elementos de programação do `Char` `Date` tipo de dados, ou `Object` . Você também pode atribuir uma instância de objeto a um elemento declarado como sendo da classe da qual essa instância é criada.

### <a name="compound-assignment-statements"></a>Instruções de atribuição composta

As *instruções de atribuição composta* primeiro executam uma operação em uma expressão antes de atribuí-la a um elemento de programação. O exemplo a seguir ilustra um desses operadores, `+=` , que incrementa o valor da variável no lado esquerdo do operador pelo valor da expressão à direita.

[!code-vb[VbVbalrStatements#77](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#77)]

O exemplo anterior adiciona 1 ao valor de `n` e, em seguida, armazena esse novo valor em `n` . É um equivalente abreviado da seguinte instrução:

[!code-vb[VbVbalrStatements#78](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#78)]

Uma variedade de operações de atribuição compostas pode ser executada usando operadores desse tipo. Para obter uma lista desses operadores e mais informações sobre eles, consulte [operadores de atribuição](../../language-reference/operators/assignment-operators.md).

O operador de atribuição de concatenação ( `&=` ) é útil para adicionar uma cadeia de caracteres ao final de cadeias de caracteres já existentes, como ilustra o exemplo a seguir.

[!code-vb[VbVbalrStatements#79](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#79)]

### <a name="type-conversions-in-assignment-statements"></a>Conversões de tipo em instruções de atribuição

O valor que você atribui a uma variável, propriedade ou elemento de matriz deve ser de um tipo de dados apropriado para esse elemento de destino. Em geral, você deve tentar gerar um valor do mesmo tipo de dados que o do elemento de destino. No entanto, alguns tipos podem ser convertidos em outros tipos durante a atribuição.

Para obter informações sobre a conversão entre tipos de dados, consulte [conversões de tipo em Visual Basic](./data-types/type-conversions.md). Em resumo, Visual Basic converte automaticamente um valor de um determinado tipo para qualquer outro tipo para o qual ele amplia. Uma *conversão de ampliação* é aquela na qual sempre é bem sucedido em tempo de execução e não perde nenhum dado. Por exemplo, Visual Basic converte um `Integer` valor em `Double` quando apropriado, porque `Integer` amplia para `Double` . Para obter mais informações, consulte [Ampliando e restringindo conversões](./data-types/widening-and-narrowing-conversions.md).

*Restringir conversões* (aquelas que não estão ampliando) tem um risco de falha em tempo de execução ou de perda de dados. Você pode executar uma conversão de restrição explicitamente usando uma função de conversão de tipo ou pode direcionar o compilador para executar todas as conversões implicitamente definindo `Option Strict Off` . Para obter mais informações, consulte [conversões implícitas e explícitas](./data-types/implicit-and-explicit-conversions.md).

## <a name="putting-multiple-statements-on-one-line"></a>Colocando várias instruções em uma linha

Você pode ter várias instruções em uma única linha separada pelo caractere de dois-pontos ( `:` ). O exemplo a seguir ilustra isto.

[!code-vb[VbVbalrStatements#70](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#70)]

Embora ocasionalmente conveniente, essa forma de sintaxe torna seu código difícil de ler e manter. Portanto, é recomendável que você mantenha uma instrução em uma linha.

## <a name="continuing-a-statement-over-multiple-lines"></a>Continuando uma instrução em várias linhas

Uma instrução geralmente se ajusta a uma linha, mas quando é muito longa, você pode continuar na próxima linha usando uma sequência de continuação de linha, que consiste em um espaço seguido por um caractere de sublinhado ( `_` ) seguido por um retorno de carro. No exemplo a seguir, a `MsgBox` instrução executável continua em duas linhas.

[!code-vb[VbVbalrStatements#71](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#71)]

### <a name="implicit-line-continuation"></a>Continuação de linha implícita

Em muitos casos, você pode continuar uma instrução na próxima linha consecutiva sem usar o caractere de sublinhado ( `_` ). Os seguintes elementos de sintaxe continuam implicitamente a instrução na próxima linha de código.

- Após uma vírgula ( `,` ). Por exemplo:

   [!code-vb[VbVbalrLineContinuation#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#1)]

- Após um parênteses de abertura ( `(` ) ou antes de um parêntese de fechamento ( `)` ). Por exemplo:

   [!code-vb[VbVbalrLineContinuation#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#2)]

- Após uma chave aberta ( `{` ) ou antes de uma chave de fechamento ( `}` ). Por exemplo:

    [!code-vb[VbVbalrLineContinuation#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#3)]

    Para obter mais informações, consulte [inicializadores de objeto: tipos nomeados e anônimos](./objects-and-classes/object-initializers-named-and-anonymous-types.md) ou [inicializadores de coleção](./collection-initializers/index.md).

- Após uma expressão inserida aberta ( `<%=` ) ou antes do fechamento de uma expressão inserida ( `%>` ) em um literal XML. Por exemplo:

   [!code-vb[VbVbalrLineContinuation#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#4)]

   Para obter mais informações, consulte [expressões inseridas em XML](./xml/embedded-expressions-in-xml.md).

- Após o operador de concatenação ( `&` ). Por exemplo:

   [!code-vb[VbVbcnConventions#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnConventions/vb/Class1.vb#9)]

   Para obter mais informações, consulte [operadores listados por funcionalidade](../../language-reference/operators/operators-listed-by-functionality.md).

- Após os operadores de atribuição ( `=` , `&=` , `:=` , `+=` , `-=` ,, `*=` `/=` , `\=` , `^=` , `<<=` , `>>=` ). Por exemplo:

   [!code-vb[VbVbalrLineContinuation#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#5)]

   Para obter mais informações, consulte [operadores listados por funcionalidade](../../language-reference/operators/operators-listed-by-functionality.md).

- Após operadores binários (,,,,,,, `+` `-` `/` `*` `Mod` `<>` `<` `>` , `<=` , `>=` , `^` ,, `>>` `<<` , `And` ,, `AndAlso` `Or` , `OrElse` , `Like` , `Xor` ) dentro de uma expressão. Por exemplo:

   [!code-vb[VbVbalrLineContinuation#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#7)]

   Para obter mais informações, consulte [operadores listados por funcionalidade](../../language-reference/operators/operators-listed-by-functionality.md).

- Após os `Is` `IsNot` operadores e. Por exemplo:

   [!code-vb[VbVbalrLineContinuation#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#8)]

   Para obter mais informações, consulte [operadores listados por funcionalidade](../../language-reference/operators/operators-listed-by-functionality.md).

- Após um caractere qualificador de membro ( `.` ) e antes do nome do membro. Por exemplo:

   [!code-vb[VbVbalrLineContinuation#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#5)]

   No entanto, você deve incluir um caractere de continuação de linha ( `_` ) após um caractere qualificador de membro quando você estiver usando a `With` instrução ou fornecendo valores na lista de inicialização para um tipo. Considere quebrar a linha após o operador de atribuição (por exemplo, `=` ) quando você estiver usando `With` instruções ou listas de inicialização de objeto. Por exemplo:

   [!code-vb[VbVbalrLineContinuation#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#14)]

   Para obter mais informações, consulte [com... Terminar com](../../language-reference/statements/with-end-with-statement.md) inicializadores de instrução ou de [objeto: tipos nomeados e anônimos](./objects-and-classes/object-initializers-named-and-anonymous-types.md).

- Após um qualificador de propriedade de eixo XML ( `.` ou `.@` ou `...` ). No entanto, você deve incluir um caractere de continuação de linha ( `_` ) quando você especificar um qualificador de membro quando estiver usando a `With` palavra-chave. Por exemplo:

   [!code-vb[VbVbalrLineContinuation#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#9)]

   Para obter mais informações, consulte [Propriedades do eixo XML](../../language-reference/xml-axis/index.md).

- Após um sinal de menor que (<) ou antes de um sinal maior que ( `>` ) quando você especifica um atributo. Além disso, após um sinal de maior que ( `>` ) quando você especifica um atributo. No entanto, você deve incluir um caractere de continuação de linha ( `_` ) quando especificar atributos de nível de assembly ou de módulo. Por exemplo:

   [!code-vb[VbVbalrLineContinuation#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#10)]

   Para obter mais informações, consulte [visão geral de atributos](../concepts/attributes/index.md).

- Operadores de consulta before e After ( `Aggregate` , `Distinct` ,,,,, `From` `Group By` `Group Join` `Join` `Let` , `Order By` , `Select` ,,, `Skip` `Skip While` `Take` , `Take While` , `Where` ,,, `In` `Into` `On` , `Ascending` e `Descending` ). Não é possível quebrar uma linha entre as palavras-chave dos operadores de consulta compostos por várias palavras-chave ( `Order By` ,, `Group Join` `Take While` e `Skip While` ). Por exemplo:

   [!code-vb[VbVbalrLineContinuation#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#11)]

   Para obter mais informações, consulte [consultas](../../language-reference/queries/index.md).

- Após a `In` palavra-chave em uma `For Each` instrução. Por exemplo:

   [!code-vb[VbVbalrLineContinuation#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#12)]

   Para obter mais informações, consulte [para cada... Próxima instrução](../../language-reference/statements/for-each-next-statement.md).

- Após a `From` palavra-chave em um inicializador de coleção. Por exemplo:

   [!code-vb[VbVbalrLineContinuation#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#13)]

   Para obter mais informações, consulte [Inicializadores de coleção](collection-initializers/index.md).

## <a name="adding-comments"></a>Como adicionar comentários

O código-fonte nem sempre é auto-explicativo, até mesmo ao programador que o escreveu. Para ajudar a documentar seu código, portanto, a maioria dos programadores faz uso liberal de comentários inseridos. Os comentários no código podem explicar um procedimento ou uma instrução específica para qualquer pessoa que leia ou trabalhe com ele mais tarde. Visual Basic ignora os comentários durante a compilação e eles não afetam o código compilado.

As linhas de comentário começam com um apóstrofo ( `'` ) ou `REM` seguido por um espaço. Eles podem ser adicionados em qualquer lugar no código, exceto em uma cadeia de caracteres. Para acrescentar um comentário a uma instrução, insira um apóstrofo ou `REM` depois da instrução, seguido pelo comentário. Os comentários também podem ir em sua própria linha separada. O exemplo a seguir demonstra essas possibilidades.

[!code-vb[VbVbalrStatements#72](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#72)]

## <a name="checking-compilation-errors"></a>Verificando erros de compilação

Se, depois de digitar uma linha de código, a linha for exibida com um sublinhado azul ondulado (uma mensagem de erro também pode aparecer), haverá um erro de sintaxe na instrução. Você deve descobrir o que há de errado com a instrução (examinando a lista de tarefas ou focalizando o erro com o ponteiro do mouse e lendo a mensagem de erro) e corrigi-lo. Até que você tenha corrigido todos os erros de sintaxe em seu código, o programa não será compilado corretamente.

## <a name="related-sections"></a>Seções relacionadas

|Termo|Definição|
|---|---|
|[Operadores de atribuição](../../language-reference/operators/assignment-operators.md)|Fornece links para páginas de referência de idioma que abrangem operadores de atribuição, como `=` , `*=` e `&=` .|
|[Operadores e expressões](./operators-and-expressions/index.md)|Mostra como combinar elementos com operadores para gerar novos valores.|
|[Como: Quebrar e combinar instruções no código](../program-structure/how-to-break-and-combine-statements-in-code.md)|Mostra como dividir uma única instrução em várias linhas e como posicionar várias instruções na mesma linha.|
|[Como: Rotular instruções](../program-structure/how-to-label-statements.md)|Mostra como rotular uma linha de código.|
