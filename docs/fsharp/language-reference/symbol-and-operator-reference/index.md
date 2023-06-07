---
title: Referência de símbolos e operadores
description: 'Saiba mais sobre os símbolos e operadores usados na linguagem de programação F #.'
ms.date: 02/11/2019
fl_keywords:
- '|>_FS'
ms.openlocfilehash: 4c3af80e8f5a686535b7c09579d29bb3da8591a3
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87855381"
---
# <a name="symbol-and-operator-reference"></a>Referência de símbolo e operador

Este artigo inclui uma tabela de símbolos e operadores que são usados na linguagem F #.

> [!NOTE]
> A referência da API docs.microsoft.com para F # não está completa. Se você encontrar links desfeitos, consulte a [documentação da biblioteca principal F #](https://fsharp.github.io/fsharp-core-docs/) em vez disso.

## <a name="table-of-symbols-and-operators"></a>Tabela de símbolos e operadores

A tabela a seguir descreve os símbolos usados na linguagem F # e fornece uma breve descrição de alguns dos usos do símbolo e dos links para obter mais informações. Os símbolos são organizados de acordo com a ordem de conjunto de caracteres ASCII.

|Símbolo ou operador|Links|Descrição|
|------------------|-----|-----------|
|`!`|[Células de Referência](../reference-cells.md)<br /><br />[Expressões de computação](../computation-expressions.md)|<ul><li>Cancela a referência de uma célula de referência.<br /></li><li>Após uma palavra-chave, indica uma versão modificada do comportamento da palavra-chave conforme controlado por um fluxo de trabalho.<br /></li></ul>|
|`!=`||<ul><li>Não é usada em F#. Use `<>` para operações de desigualdade.<br /></li></ul>|
|`"`|[Literais](../literals.md)<br /><br />[Cadeias de caracteres](../strings.md)|<ul><li>Delimita uma cadeia de texto.<br /></li></ul>|
|`"""`|[Cadeias de caracteres](../strings.md)|Delimita uma cadeia de caracteres textuais. Difere de `@"..."` em que você pode indicar um caractere de aspas usando aspas simples na cadeia de caracteres.|
|`#`|[Diretivas de Compilador](../compiler-directives.md)<br /><br />[Tipos Flexíveis](../flexible-types.md)|<ul><li>Prefixos de uma diretiva de pré-processador ou compilador, tal como `#light`.<br /></li><li>Quando usado com um tipo, indica um *tipo flexível*, que se refere a um tipo ou a qualquer um de seus tipos derivados.<br /></li></ul>|
|`$`||<ul><li>Usado internamente para determinados nomes de variáveis e de funções gerados pelo compilador.<br /></li></ul>|
|`%`|[Operadores aritméticos](arithmetic-operators.md)<br /><br />[Citações de Código](../code-quotations.md)|<ul><li>Calcula o resto inteiro.<br /></li><li>Usado para união de expressões em cotações de códigos digitados.<br /></li></ul>|
|`%%`|[Citações de Código](../code-quotations.md)|<ul><li>Usado para união de expressões em cotações de códigos não digitados.<br /></li></ul>|
|`%?`|[Operadores Anuláveis](nullable-operators.md)|<ul><li>Calcula o resto inteiro, quando o lado direito é um tipo anulável.<br /></li></ul>|
|`&`|[Expressões de correspondência](../match-expressions.md)|<ul><li>Calcula o endereço de um valor mutável, para uso ao interoperar com outros idiomas.<br /></li><li>Usado em padrões AND.<br /></li></ul>|
|`&&`|[Operadores boolianos](boolean-operators.md)|<ul><li>Calcula a operação AND booleana.<br /></li></ul>|
|`&&&`|[Operadores bit a bit](bitwise-operators.md)|<ul><li>Calcula a operação AND bit a bit.<br /></li></ul>|
|`'`|[Literais](../literals.md)<br /><br />[Generalização Automática](../generics/automatic-generalization.md)|<ul><li>Delimita um literal de caractere único.<br /></li><li>Indica um parâmetro de tipo genérico.<br /></li></ul>|
|<code>&#96;&#96;...&#96;&#96;</code>||<ul><li>Delimita um identificador que não seria um identificador legal, como uma palavra-chave de idioma.<br /></li></ul>|
|`( )`|[Tipo Unit](../unit-type.md)|<ul><li>Representa um único valor do tipo de unidade.<br /></li></ul>|
|`(...)`|[Tuplas](../tuples.md)<br /><br />[Sobrecarga de operador](../operator-overloading.md)|<ul><li>Indica a ordem na qual as expressões são avaliadas.<br /></li><li>Delimita uma tupla.<br /></li><li>Usado em definições de operador.<br /></li></ul>|
|`(*...*)`||<ul><li>Delimita um comentário que poderia abranger várias linhas.<br /></li></ul>|
|<code>(&#124;...&#124;)</code>|[Padrões Ativos](../active-patterns.md)|<ul><li>Delimita um padrão ativo. Também chamado de *pipe ativo*.<br /></li></ul>|
|`*`|[Operadores aritméticos](arithmetic-operators.md)<br /><br />[Tuplas](../tuples.md)<br /><br />[Unidades de medida](../units-of-measure.md)|<ul><li>Quando usado como um operador binário, multiplica os lados esquerdo e direito.<br /></li><li>Em tipos, indica emparelhamento em uma tupla.<br /></li><li>Usado em unidades de tipos de medida.<br /></li></ul>|
|`*?`|[Operadores Anuláveis](nullable-operators.md)|<ul><li>Multiplica os lados esquerdo e direito, quando o lado direito for um tipo que permite valor nulo.<br /></li></ul>|
|`**`|[Operadores aritméticos](arithmetic-operators.md)|<ul><li>Calcula a operação de exponenciação (`x ** y` significa `x` à potência de `y`).<br /></li></ul>|
|`+`|[Operadores aritméticos](arithmetic-operators.md)|<ul><li>Quando usado como um operador binário, adiciona os lados esquerdo e direito.<br /></li><li>Quando usado como um operador unário, indica uma quantidade positiva. (Formalmente, ele produz o mesmo valor com o sinal inalterado).<br /></li></ul>|
|`+?`|[Operadores Anuláveis](nullable-operators.md)|<ul><li>Adiciona os lados esquerdo e direito, quando o lado direito for um tipo que permite valor nulo.<br /></li></ul>|
|`,`|[Tuplas](../tuples.md)|<ul><li>Separa os elementos de uma tupla ou parâmetros de tipo.<br /></li></ul>|
|`-`|[Operadores aritméticos](arithmetic-operators.md)|<ul><li>Quando usado como um operador binário, subtrai o lado direito do lado esquerdo.<br /></li><li>Quando usado como um operador unário, executa uma operação de negação.<br /></li></ul>|
|`-?`|[Operadores Anuláveis](nullable-operators.md)|<ul><li>Subtrai o lado direito do lado esquerdo, quando o lado direito for um tipo que permite valor nulo.<br /></li></ul>|
|`->`|[Funções](../functions/index.md)<br /><br />[Expressões de correspondência](../match-expressions.md)|<ul><li>Em tipos de função, delimita argumentos e valores de retorno.<br /></li><li>Gera uma expressão (em expressões de sequência); equivalente à palavra-chave `yield`.<br /></li><li>Usado em expressões de correspondência<br /></li></ul>|
|`.`|[Membros](../members/index.md)<br /><br />[Tipos primitivos](../basic-types.md)|<ul><li>Acessa um membro e separa nomes individuais em um nome totalmente qualificado.<br /></li><li>Especifica um ponto decimal em números de ponto flutuante.<br /></li></ul>|
|`..`|[Loops: `for...in` expressão](../loops-for-in-expression.md)|<ul><li>Especifica um intervalo.<br /></li></ul>|
|`.. ..`|[Loops: `for...in` expressão](../loops-for-in-expression.md)|<ul><li>Especifica um intervalo com um incremento.<br /></li></ul>|
|`.[...]`|[matrizes](../arrays.md)|<ul><li>Acessa um elemento de matriz.<br /></li></ul>|
|`/`|[Operadores aritméticos](arithmetic-operators.md)<br /><br />[Unidades de medida](../units-of-measure.md)|<ul><li>Divide o lado esquerdo (numerador) pelo direito (denominador).<br /></li><li>Usado em unidades de tipos de medida.<br /></li></ul>|
|`/?`|[Operadores Anuláveis](nullable-operators.md)|<ul><li>Divide o lado esquerdo pelo lado direito, quando o lado direito for um tipo que permite valor nulo.<br /></li></ul>|
|`//`||<ul><li>Indica o início de um comentário de uma linha.<br /></li></ul>|
|`///`|[Documentação XML](../xml-documentation.md)|<ul><li>Indica um comentário XML.<br /></li></ul>|
|`:`|[Funções](../functions/index.md)|<ul><li>Em uma anotação de tipo, separa um nome de parâmetro ou de membro do seu tipo.<br /></li></ul>|
|`::`|[Listas](../lists.md)<br /><br />[Expressões de correspondência](../match-expressions.md)|<ul><li>Cria uma lista. O elemento no lado esquerdo é anexado à lista no lado direito.<br /></li><li>Usado na correspondência de padrões para separar as partes de uma lista.<br /></li></ul>|
|`:=`|[Células de Referência](../reference-cells.md)|<ul><li>Atribui um valor a uma célula de referência.<br /></li></ul>|
|`:>`|[Conversões cast e conversões](../casting-and-conversions.md)|<ul><li>Converte um tipo em um tipo que seja superior na hierarquia.<br /></li></ul>|
|`:?`|[Expressões de correspondência](../match-expressions.md)|<ul><li>Retorna `true` se o valor corresponde ao tipo especificado (incluindo se for um subtipo); caso contrário, retorna `false` (operador de teste de tipo).<br /></li></ul>|
|`:?>`|[Conversões cast e conversões](../casting-and-conversions.md)|<ul><li>Converte um tipo em um tipo que seja inferior na hierarquia.<br /></li></ul>|
|`;`|[Sintaxe Detalhada](../verbose-syntax.md)<br /><br />[Listas](../lists.md)<br /><br />[Grava](../records.md)|<ul><li>Separa expressões (usadas principalmente na sintaxe detalhada).<br /></li><li>Separa elementos de uma lista.<br /></li><li>Separa campos de um registro.<br /></li></ul>|
|`<`|[Operadores aritméticos](arithmetic-operators.md)|<ul><li>Calcula a operação menor que.<br /></li></ul>|
|`<?`|[Operadores Anuláveis](nullable-operators.md)|Calcula a operação menor que, quando o lado direito for um tipo que permite valor nulo.|
|`<<`|[Funções](../functions/index.md)|<ul><li>Compõe duas funções em ordem inversa; a segunda é executada primeiro (operador de composição invertida).<br /></li></ul>|
|`<<<`|[Operadores bit a bit](bitwise-operators.md)|<ul><li>Desloca bits na quantidade no lado esquerdo para a esquerda pelo número de bits especificado no lado direito.<br /></li></ul>|
|`<-`|[Valores](../values/index.md)|<ul><li>Atribui um valor a uma variável.<br /></li></ul>|
|`<...>`|[Generalização Automática](../generics/automatic-generalization.md)|<ul><li>Delimita parâmetros de tipo.<br /></li></ul>|
|`<>`|[Operadores aritméticos](arithmetic-operators.md)|<ul><li>Retorna `true` se o lado esquerdo não for igual ao direito; caso contrário, retorna false.<br /></li></ul>|
|`<>?`|[Operadores Anuláveis](nullable-operators.md)|<ul><li>Calcula a operação "diferente de", quando o lado direito for um tipo que permite valor nulo.<br /></li></ul>|
|`<=`|[Operadores aritméticos](arithmetic-operators.md)|<ul><li>Retorna `true` se o lado esquerdo for menor ou igual ao lado direito; caso contrário, retorna `false`.<br /></li></ul>|
|`<=?`|[Operadores Anuláveis](nullable-operators.md)|<ul><li>Calcula a operação "menor ou igual a", quando o lado direito for um tipo que permite valor nulo.<br /></li></ul>|
|<code>&lt;&#124;</code>|[Funções](../functions/index.md)|<ul><li>Passa o resultado da expressão no lado direito para a função no lado esquerdo (operador barra vertical invertida).<br /></li></ul>|
|<code>&lt;&#124;&#124;</code>|[Operadores.&#40; &#60;&#124;&#124; &#41;&#60;'T1,'T2,'U&#62; Função](https://msdn.microsoft.com/visualfsharpdocs/conceptual/operators.%5b-%5bhh-%5d%5b%27t1%2c%27t2%2c%27u%5d-function-%5bfsharp%5d)|<ul><li>Passa a tupla de dois argumentos no lado direito para a função no lado esquerdo.<br /></li></ul>|
|<code>&lt;&#124;&#124;&#124;</code>|[Operadores.&#40; &#60;&#124;&#124;&#124; &#41;&#60;'T1,'T2,'T3,'U&#62; Função](https://msdn.microsoft.com/visualfsharpdocs/conceptual/operators.%5b-%5bhhh-%5d%5b%27t1%2c%27t2%2c%27t3%2c%27u%5d-function-%5bfsharp%5d)|<ul><li>Passa a tupla de três argumentos no lado direito para a função no lado esquerdo.<br /></li></ul>|
|`<@...@>`|[Citações de Código](../code-quotations.md)|<ul><li>Delimita uma cotação de código digitado.<br /></li></ul>|
|`<@@...@@>`|[Citações de Código](../code-quotations.md)|<ul><li>Delimita uma cotação de código não digitado.<br /></li></ul>|
|`=`|[Operadores aritméticos](arithmetic-operators.md)|<ul><li>Retorna `true` se o lado esquerdo for igual ao lado direito; caso contrário, retorna `false`.<br /></li></ul>|
|`=?`|[Operadores Anuláveis](nullable-operators.md)|<ul><li>Calcula a operação "igual", quando o lado direito for um tipo que permite valor nulo.<br /></li></ul>|
|`==`||<ul><li>Não é usada em F#. Use `=` para operações de igualdade.<br /></li></ul>|
|`>`|[Operadores aritméticos](arithmetic-operators.md)|<ul><li>Retorna `true` se o lado esquerdo for maior que o lado direito; caso contrário, retorna `false`.<br /></li></ul>|
|`>?`|[Operadores Anuláveis](nullable-operators.md)|<ul><li>Computa a operação "maior que" quando o lado direito é um tipo anulável.<br /></li></ul>|
|`>>`|[Funções](../functions/index.md)|<ul><li>Compõe duas funções (operador de composição direta).<br /></li></ul>|
|`>>>`|[Operadores bit a bit](bitwise-operators.md)|<ul><li>Desloca bits na quantidade no lado esquerdo para a direita pelo número de casas especificado no lado direito.<br /></li></ul>|
|`>=`|[Operadores aritméticos](arithmetic-operators.md)|<ul><li>Retorna `true` se o lado esquerdo é maior ou igual ao lado direito; caso contrário, retorna `false` .<br /></li></ul>|
|`>=?`|[Operadores Anuláveis](nullable-operators.md)|<ul><li>Calcula a operação "maior ou igual a", quando o lado direito for um tipo que permite valor nulo.<br /></li></ul>|
|`?`|[Parâmetros e Argumentos](../parameters-and-arguments.md)|<ul><li>Especifica um argumento opcional.<br /></li><li>Usado como um operador de chamadas de método e propriedade dinâmicas. Você deve fornecer sua própria implementação.<br /></li></ul>|
|`? ... <- ...`||<ul><li>Usado como um operador de propriedades de configuração dinâmica. Você deve fornecer sua própria implementação.<br /></li></ul>|
|`?>=`, `?>`, `?<=`, `?<`, `?=`, `?<>`, `?+`, `?-`, `?*`, `?/`|[Operadores Anuláveis](nullable-operators.md)|<ul><li>Equivalente aos operadores correspondentes sem o sufixo ? em que um tipo que permite valor nulo está à esquerda.<br /></li></ul>|
|`>=?`, `>?`, `<=?`, `<?`, `=?`, `<>?`, `+?`, `-?`, `*?`, `/?`|[Operadores Anuláveis](nullable-operators.md)|<ul><li>Equivalente aos operadores correspondentes sem o sufixo ? em que um tipo que permite valor nulo está à direita.<br /></li></ul>|
|`?>=?`, `?>?`, `?<=?`, `?<?`, `?=?`, `?<>?`, `?+?`, `?-?`, `?*?`, `?/?`|[Operadores Anuláveis](nullable-operators.md)|<ul><li>Equivalente aos operadores correspondentes sem pontos de interrogação adjacentes, em que os dois lados são tipos que permitem valor nulo.<br /></li></ul>|
|`@`|[Listas](../lists.md)<br /><br />[Cadeias de caracteres](../strings.md)|<ul><li>Concatena duas listas.<br /></li><li>Quando colocado antes de uma sequência literal, indica que a sequência deve ser interpretada textualmente, sem nenhuma interpretação de caracteres de escape.<br /></li></ul>|
|`[...]`|[Listas](../lists.md)|<ul><li>Delimita os elementos de uma lista.<br /></li></ul>|
|<code>[&#124;...&#124;]</code>|[matrizes](../arrays.md)|<ul><li>Delimita os elementos de uma matriz.<br /></li></ul>|
|`[<...>]`|[Atributos](../attributes.md)|<ul><li>Delimita um atributo.<br /></li></ul>|
|`\`|[Cadeias de caracteres](../strings.md)|<ul><li>Pula o próximo caractere; usado em literal de caractere ou em sequência literal.<br /></li></ul>|
|`^`|[Parâmetros de tipo resolvidos estaticamente](../generics/statically-resolved-type-parameters.md)<br /><br />[Cadeias de caracteres](../strings.md)|<ul><li>Especifica os parâmetros de tipo que devem ser resolvidos no tempo de compilação, não no runtime.<br /></li><li>Concatena sequências.<br /></li></ul>|
|`^^^`|[Operadores bit a bit](bitwise-operators.md)|<ul><li>Calcula a operação OR exclusiva bit a bit.<br /></li></ul>|
|`_`|[Expressões de correspondência](../match-expressions.md)<br /><br />[Genéricos](../generics/index.md)|<ul><li>Indica um padrão de curinga.<br /></li><li>Especifica um parâmetro genérico anônimo.<br /></li></ul>|
|<code>&#96;</code>|[Generalização Automática](../generics/automatic-generalization.md)|<ul><li>Usado internamente para indicar um parâmetro de tipo genérico.<br /></li></ul>|
|`{...}`|[Sequências](../sequences.md)<br /><br />[Grava](../records.md)|<ul><li>Delimita expressões de sequência e expressões de computação.<br /></li><li>Usado em definições de registro.<br /></li></ul>|
|<code>&#124;</code>|[Expressões de correspondência](../match-expressions.md)|<ul><li>Delimita casos de correspondência individuais, casos de união discriminada individuais e valores de enumeração.<br /></li></ul>|
|<code>&#124;&#124;</code>|[Operadores boolianos](boolean-operators.md)|<ul><li>Calcula a operação OR booleana.<br /></li></ul>|
|<code>&#124;&#124;&#124;</code>|[Operadores bit a bit](bitwise-operators.md)|<ul><li>Calcula a operação OR bit a bit.<br /></li></ul>|
|<code>&#124;></code>|[Funções](../functions/index.md)|<ul><li>Passa o resultado do lado esquerdo para a função no lado direito (operador barra vertical direta).<br /></li></ul>|
|<code>&#124;&#124;></code>|[Operadores.&#40; &#124;&#124;&#62; &#41;&#60;'T1,'T2,'U&#62; Função](https://msdn.microsoft.com/visualfsharpdocs/conceptual/operators.%5b-hh%5d-%5d%5b%27t1%2c%27t2%2c%27u%5d-function-%5bfsharp%5d)|<ul><li>Passa a tupla de dois argumentos no lado esquerdo para a função no lado direito.<br /></li></ul>|
|<code>&#124;&#124;&#124;></code>|[Operadores.&#40; &#124;&#124;&#124;&#62; &#41;&#60;'T1,'T2,'T3,'U&#62; Função](https://msdn.microsoft.com/visualfsharpdocs/conceptual/operators.%5b-hhh%5d-%5d%5b%27t1%2c%27t2%2c%27t3%2c%27u%5d-function-%5bfsharp%5d)|<ul><li>Passa a tupla de três argumentos no lado esquerdo para a função no lado direito.<br /></li></ul>|
|`~~`|[Sobrecarga de operador](../operator-overloading.md)|<ul><li>Usado para declarar uma sobrecarga para o operador de negação unário.<br /></li></ul>|
|`~~~`|[Operadores bit a bit](bitwise-operators.md)|<ul><li>Calcula a operação NOT bit a bit.<br /></li></ul>|
|`~-`|[Sobrecarga de operador](../operator-overloading.md)|<ul><li>Usado para declarar uma sobrecarga para o operador de subtração unário.<br /></li></ul>|
|`~+`|[Sobrecarga de operador](../operator-overloading.md)|<ul><li>Usado para declarar uma sobrecarga para o operador de adição unário.<br /></li></ul>|

## <a name="operator-precedence"></a>Precedência do operador

A tabela a seguir mostra a ordem de precedência de operadores e outras palavras-chave da expressão na linguagem F#, na ordem de precedência mais baixa para a mais alta. Também é listada a capacidade de associação, se aplicável.

|Operador|Capacidade de associação|
|--------|-------------|
|`as`|Direita|
|`when`|Direita|
|<code>&#124;</code>conexão|Esquerda|
|`;`|Direita|
|`let`|Não associativo|
|`function`, `fun`, `match`, `try`|Não associativo|
|`if`|Não associativo|
|`not`|Direita|
|`->`|Direita|
|`:=`|Direita|
|`,`|Não associativo|
|`or`, <code>&#124;&#124;</code>|Esquerda|
|`&`, `&&`|Esquerda|
|`:>`, `:?>`|Direita|
|`<`*op*, `>` *op*, `=` , <code>&#124;</code> *op*, `&` *op*,`&`<br /><br />(incluindo `<<<` , `>>>` , <code>&#124;&#124;&#124;</code> , `&&&` )|Esquerda|
|`^`*parar*<br /><br />(incluindo `^^^`)|Direita|
|`::`|Direita|
|`:?`|Não associativo|
|`-`*op*, `+` *op*|Aplica-se a utilizações de infixo desses símbolos|
|`*`*op*, `/` *op*, `%` *op*|Esquerda|
|`**`*parar*|Direita|
|`f x` (aplicativo de função)|Esquerda|
|<code>&#124;</code> (correspondência padrão)|Direita|
|operadores de prefixo ( `+` *op*, `-` *op*,,,, `%` `%%` `&` `&&` , `!` *op*, `~` *op*)|Esquerda|
|`.`|Esquerda|
|`f(x)`|Esquerda|
|`f<`*digita*`>`|Esquerda|

F# oferece suporte à sobrecarga de operador personalizado. Isso significa que você pode definir seus próprio operadores. Na tabela anterior, *op* pode ser qualquer sequência (possivelmente vazia) válida de caracteres de operador, internos ou definidos pelo usuário. Dessa forma, é possível usar esta tabela para determinar qual sequência de caracteres usar para que um operador personalizado atinja o nível desejado de precedência. Os caracteres `.` à esquerda são ignorados quando o compilador determina a precedência.

## <a name="see-also"></a>Confira também

- [Referência de linguagem F #](../index.md)
- [Sobrecarga de operador](../operator-overloading.md)
