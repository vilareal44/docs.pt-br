---
title: Instruções – Guia de Programação em C#
description: Saiba mais sobre as instruções na programação em C#. Veja uma lista de tipos de instrução e exiba exemplos de código e recursos adicionais.
ms.date: 07/20/2015
helpviewer_keywords:
- statements [C#], about statements
- C# language, statements
ms.assetid: 901bcde7-87de-4e15-833c-f9cfd40c8ce3
ms.openlocfilehash: 3f8ac88525c44f9572f4f647145ad251537aba57
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556742"
---
# <a name="statements-c-programming-guide"></a>Instruções (Guia de Programação em C#)

As ações que usa um programa executa são expressas em instruções. Ações comuns incluem declarar variáveis, atribuir valores, chamar métodos, fazer loops pelas coleções e ramificar para um ou para outro bloco de código, dependendo de uma determinada condição. A ordem na qual as instruções são executadas em um programa é chamada de fluxo de controle ou fluxo de execução. O fluxo de controle pode variar sempre que um programa é executado, dependendo de como o programa reage às entradas que recebe em tempo de execução.

Uma instrução pode consistir em uma única linha de código que termina em um ponto e vírgula ou uma série de instruções de uma linha em um bloco. Um bloco de instrução é colocado entre colchetes {} e pode conter blocos aninhados. O código a seguir mostra dois exemplos de instruções de linha única, bem como um bloco de instrução de várias linhas:

[!code-csharp[csProgGuideStatements#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#1)]

## <a name="types-of-statements"></a>Tipos de instruções

A tabela a seguir lista os diferentes tipos de instruções em C# e as palavras-chave associadas a elas, com links para tópicos que contêm mais informações:

|Categoria|Palavras-chave do C#/observações|
|--------------|---------------------------|
|[Instruções de declaração](#declaration-statements)|Uma declaração de instrução introduz uma nova variável ou constante. Uma declaração variável pode, opcionalmente, atribuir um valor à variável. Uma declaração constante, a atribuição é obrigatória.|
|[Instruções de expressão](#expression-statements)|Instruções de expressão que calculam um valor devem armazenar o valor em uma variável.|
|Instruções de seleção|Instruções de seleção permitem que você ramifique para diferentes seções de código, dependendo de uma ou mais condições especificadas. Para obter mais informações, consulte estes tópicos: <ul><li>[if](../../language-reference/keywords/if-else.md)</li><li>[senão](../../language-reference/keywords/if-else.md)</li><li>[switch](../../language-reference/keywords/switch.md)</li><li>[casos](../../language-reference/keywords/switch.md)</li></ul>|
|Instruções de iteração|Instruções de iteração permitem que você percorra coleções como matrizes ou execute o mesmo conjunto de instruções repetidamente até que uma determinada condição seja atendida. Para obter mais informações, consulte estes tópicos: <ul><li>[do](../../language-reference/keywords/do.md)</li><li>[for](../../language-reference/keywords/for.md)</li><li>[foreach](../../language-reference/keywords/foreach-in.md)</li><li>[no](../../language-reference/keywords/foreach-in.md)</li><li>[mesmo](../../language-reference/keywords/while.md)</li></ul>|
|Instruções de atalho|Instruções de hiperlink transferem o controle para outra seção de código. Para obter mais informações, consulte estes tópicos: <ul><li>[break](../../language-reference/keywords/break.md)</li><li>[continua](../../language-reference/keywords/continue.md)</li><li>[os](../../language-reference/keywords/switch.md)</li><li>[goto](../../language-reference/keywords/goto.md)</li><li>[exibir](../../language-reference/keywords/return.md)</li><li>[yield](../../language-reference/keywords/yield.md)</li></ul>|
|Instruções para tratamento de exceções|Instruções para tratamento de exceções permitem que você se recupere normalmente de condições excepcionais que ocorrem em tempo de execução. Para obter mais informações, consulte estes tópicos: <ul><li>[throw](../../language-reference/keywords/throw.md)</li><li>[try-catch](../../language-reference/keywords/try-catch.md)</li><li>[try-finally](../../language-reference/keywords/try-finally.md)</li><li>[try-catch-finally](../../language-reference/keywords/try-catch-finally.md)</li></ul>|
|[Marcado e desmarcado](../../language-reference/keywords/checked-and-unchecked.md)|As instruções checked e unchecked permitem que você especifique se operações numéricas podem causar um estouro quando o resultado for armazenado em uma variável que é muito pequena para conter o valor resultante. Para obter mais informações, consulte [checked](../../language-reference/keywords/checked.md) e [unchecked](../../language-reference/keywords/unchecked.md).|
|A instrução `await`|Se marcar um método com o modificador [async](../../language-reference/keywords/async.md), você poderá usar o operador [await](../../language-reference/operators/await.md) no método. Quando o controle atinge uma expressão `await` no método assíncrono, ele retorna para o chamador e o progresso no método é suspenso até a tarefa aguardada ser concluída. Quando a tarefa for concluída, a execução poderá ser retomada no método.<br /><br /> Para obter um exemplo simples, consulte a seção "Métodos assíncronos" em [Métodos](../classes-and-structs/methods.md). Para obter mais informações, consulte [Programação assíncrona com async e await](../concepts/async/index.md).|
|A instrução `yield return`|Um iterador realiza uma iteração personalizada em uma coleção, como uma lista ou uma matriz. Um iterador usa a instrução [yield return](../../language-reference/keywords/yield.md) para retornar um elemento de cada vez. Quando uma instrução `yield return` for atingida, o local atual no código será lembrado. A execução será reiniciada desse local quando o iterador for chamado na próxima vez.<br /><br /> Para obter mais informações, consulte [Iteradores](../concepts/iterators.md).|
|A instrução `fixed`|A instrução fixed impede que o coletor de lixo faça a realocação de uma variável móvel. Para obter mais informações, consulte [fixed](../../language-reference/keywords/fixed-statement.md).|
|A instrução `lock`|A instrução lock permite limitar o acesso a blocos de código a apenas um thread por vez. Para obter mais informações, consulte [lock](../../language-reference/keywords/lock-statement.md).|
|Instruções rotuladas|Você pode atribuir um rótulo a uma instrução e, em seguida, usar a palavra-chave [goto](../../language-reference/keywords/goto.md) para ir diretamente para a instrução rotulada. (Veja o exemplo na linha a seguir.)|
|A [instrução vazia](#the-empty-statement)|A instrução vazia consiste em um único ponto e vírgula. Ela não faz nada e pode ser usada em locais em que uma instrução é necessária, mas nenhuma ação precisa ser executada.|

## <a name="declaration-statements"></a>Instruções de declaração

O código a seguir mostra exemplos de declarações de variável com e sem uma atribuição inicial e uma declaração de constante com a inicialização necessária.

[!code-csharp[csProgGuideStatements#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#23)]

## <a name="expression-statements"></a>Instruções de expressão

O código a seguir mostra exemplos de instruções de expressão, incluindo a atribuição, a criação de objeto com a atribuição e a invocação de método.

[!code-csharp[csProgGuideStatements#24](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#24)]

## <a name="the-empty-statement"></a>A instrução vazia

Os exemplos a seguir mostram dois usos de uma instrução vazia:

[!code-csharp[csProgGuideStatements#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#25)]

## <a name="embedded-statements"></a>Instruções inseridas

Algumas instruções, inclusive [do](../../language-reference/keywords/do.md), [while](../../language-reference/keywords/while.md), [for](../../language-reference/keywords/for.md) e [foreach](../../language-reference/keywords/foreach-in.md), sempre têm uma instrução inserida que as segue. Essa instrução inserida pode ser uma instrução única ou várias instruções colocadas entre colchetes {} em um bloco de instrução. Até mesmo instruções inseridas de uma única linha podem ser colocadas entre colchetes {}, conforme mostrado no seguinte exemplo:

[!code-csharp[csProgGuideStatements#26](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#26)]

Uma instrução inserida que não está entre colchetes {} não pode ser uma instrução de declaração ou uma instrução rotulada. Isso é mostrado no exemplo a seguir:

[!code-csharp[csProgGuideStatements#27](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#27)]

Coloque a instrução inserida em um bloco para corrigir o erro:

[!code-csharp[csProgGuideStatements#28](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#28)]

## <a name="nested-statement-blocks"></a>Blocos de instrução aninhados

Blocos de instrução podem ser aninhados, conforme mostrado no código a seguir:

[!code-csharp[csProgGuideStatements#29](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#29)]

## <a name="unreachable-statements"></a>Instruções inacessíveis

Se o compilador determinar que o fluxo de controle nunca pode atingir uma determinada instrução em nenhuma circunstância, ele produzirá o aviso CS0162, conforme mostrado no exemplo a seguir:

[!code-csharp[csProgGuideStatements#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#22)]

## <a name="c-language-specification"></a>especificação da linguagem C#

Para saber mais, confira a seção [Instruções](~/_csharplang/spec/statements.md) da [Especificação da linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="see-also"></a>Confira também

- [Guia de programação C#](../index.md)
- [Palavras-chave da instrução](../../language-reference/keywords/statement-keywords.md)
- [Operadores e expressões C#](../../language-reference/operators/index.md)
