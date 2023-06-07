---
title: try-catch – Referência de C#
ms.date: 07/20/2015
f1_keywords:
- try
- try_CSharpKeyword
- catch
- catch_CSharpKeyword
helpviewer_keywords:
- catch keyword [C#]
- try-catch statement [C#]
ms.assetid: cb5503c7-bfa1-4610-8fc2-ddcd2e84c438
ms.openlocfilehash: 4715a27a94ac86c5e4955c0e8be95c6ee4a28507
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619696"
---
# <a name="try-catch-c-reference"></a>try-catch (Referência de C#)

A instrução try-catch consiste em um bloco `try` seguido por uma ou mais cláusulas `catch`, que especificam os manipuladores para diferentes exceções.

Quando uma exceção é lançada, o CLR (Common Language Runtime) procura a instrução `catch` que trata essa exceção. Se o método em execução no momento não contiver um bloco `catch`, o CLR procurará no método que chamou o método atual e assim por diante para cima na pilha de chamadas. Se nenhum bloco `catch` for encontrado, o CLR exibirá uma mensagem de exceção sem tratamento para o usuário e interromperá a execução do programa.

O bloco `try` contém o código protegido que pode causar a exceção. O bloco é executado até que uma exceção seja lançada ou ele seja concluído com êxito. Por exemplo, a tentativa a seguir de converter um objeto `null` gera a exceção <xref:System.NullReferenceException>:

```csharp
object o2 = null;
try
{
    int i2 = (int)o2;   // Error
}
```

Embora a cláusula `catch` possa ser usada sem argumentos para capturar qualquer tipo de exceção, esse uso não é recomendado. Em geral, você deve capturar apenas as exceções das quais você sabe se recuperar. Portanto, você sempre deve especificar um argumento de objeto derivado de <xref:System.Exception?displayProperty=nameWithType>, por exemplo:

```csharp
catch (InvalidCastException e)
{
}
```

É possível usar mais de uma cláusula `catch` específica na mesma instrução try-catch. Nesse caso, a ordem das cláusulas `catch` é importante porque as cláusulas `catch` são examinadas em ordem. Capture as exceções mais específicas antes das menos específicas. O compilador gerará um erro se você ordenar os blocos catch de forma que um bloco posterior nunca possa ser alcançado.

Usar argumentos `catch` é uma maneira de filtrar as exceções que deseja manipular.  Use também um filtro de exceção que examina melhor a exceção para decidir se ela deve ser manipulada.  Se o filtro de exceção retornar falso, a pesquisa por um manipulador continuará.

```csharp
catch (ArgumentException e) when (e.ParamName == "…")
{
}
```

Os filtros de exceção são preferíveis em relação à captura e relançamento (explicados abaixo) porque os filtros deixam a pilha intacta.  Se um manipulador posterior despeja a pilha, você pode ver de onde a exceção originalmente veio, em vez de apenas o último lugar em que ela foi relançada.  Um uso comum de expressões de filtro de exceção é o registro em log.  Crie um filtro que sempre retorna falso e que também gera um log; você pode registrar exceções conforme elas ocorrem sem precisar manipulá-las e gerá-las novamente.

Uma instrução [throw](throw.md) pode ser usada em um bloco `catch` para relançar a exceção que foi capturada pela instrução `catch`. O exemplo a seguir extrai informações de origem de uma exceção <xref:System.IO.IOException> e, em seguida, lança a exceção para o método pai.

```csharp
catch (FileNotFoundException e)
{
    // FileNotFoundExceptions are handled here.
}
catch (IOException e)
{
    // Extract some information from this exception, and then
    // throw it to the parent method.
    if (e.Source != null)
        Console.WriteLine("IOException source: {0}", e.Source);
    throw;
}
```

Você pode capturar uma exceção e lançar uma exceção diferente. Quando fizer isso, especifique a exceção capturada como a exceção interna, como mostrado no exemplo a seguir.

```csharp
catch (InvalidCastException e)
{
    // Perform some action here, and then throw a new exception.
    throw new YourCustomException("Put your error message here.", e);
}
```

Você pode também relançar uma exceção quando uma determinada condição for verdadeira, conforme mostrado no exemplo a seguir.

```csharp
catch (InvalidCastException e)
{
    if (e.Data == null)
    {
        throw;
    }
    else
    {
        // Take some action.
    }
}
```

> [!NOTE]
> Também é possível usar um filtro de exceção para obter um resultado semelhante em um modo geralmente mais limpo (além de não modificar a pilha, conforme explicado anteriormente neste documento). O exemplo a seguir tem um comportamento semelhante para chamadores como no exemplo anterior. A função gera a `InvalidCastException` novamente para o chamador quando `e.Data` é `null`.
>
> ```csharp
> catch (InvalidCastException e) when (e.Data != null)
> {
>     // Take some action.
> }
> ```

De dentro de um bloco `try`, inicialize somente as variáveis que são declaradas nele. Caso contrário, uma exceção pode ocorrer antes da conclusão da execução do bloco. Por exemplo, no exemplo de código a seguir, a variável `n` é inicializada dentro do bloco `try`. Uma tentativa de usar essa variável fora do bloco `try` na instrução `Write(n)` gerará um erro de compilador.

```csharp
static void Main()
{
    int n;
    try
    {
        // Do not initialize this variable here.
        n = 123;
    }
    catch
    {
    }
    // Error: Use of unassigned local variable 'n'.
    Console.Write(n);
}
```

Para obter mais informações sobre catch, consulte [try-catch-finally](try-catch-finally.md).

## <a name="exceptions-in-async-methods"></a>Exceções em métodos assíncronos

Um método assíncrono é marcado por um modificador [async](async.md) e geralmente contém uma ou mais expressões ou instruções await. Uma expressão await aplica o operador [await](../operators/await.md) a um <xref:System.Threading.Tasks.Task> ou <xref:System.Threading.Tasks.Task%601>.

Quando o controle atinge um `await` no método assíncrono, o progresso no método é suspenso até que a tarefa aguardada seja concluída. Quando a tarefa for concluída, a execução poderá ser retomada no método. Para obter mais informações, consulte [Programação assíncrona com async e await (C#)](../../programming-guide/concepts/async/index.md) e [Fluxo de controle em programas assíncronos](../../programming-guide/concepts/async/control-flow-in-async-programs.md).

A tarefa concluída para a qual `await` é aplicada pode estar em um estado de falha devido a uma exceção sem tratamento no método que retorna a tarefa. Aguardar a tarefa gera uma exceção. Uma tarefa também poderá terminar em um estado cancelado se o processo assíncrono que a retorna for cancelado. Aguardar uma tarefa cancelada lança um `OperationCanceledException`. Para obter mais informações sobre como cancelar um processo assíncrono, consulte [Ajuste fino de seu aplicativo assíncrono (C#)](../../programming-guide/concepts/async/fine-tuning-your-async-application.md).

Para capturar a exceção, aguarde a tarefa em um bloco `try` e capture a exceção no bloco `catch` associado. Para obter um exemplo, confira a seção [Exemplo de método assíncrono](#async-method-example).

Uma tarefa pode estar em um estado de falha porque ocorreram várias exceções no método assíncrono esperado. Por exemplo, a tarefa pode ser o resultado de uma chamada para <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType>. Quando você espera uma tarefa, somente uma das exceções é capturada e não é possível prever qual exceção será capturada. Para obter um exemplo, confira a seção [Exemplo de Task.WhenAll](#taskwhenall-example).

## <a name="example"></a>Exemplo

No exemplo a seguir, o bloco `try` contém uma chamada para o método `ProcessString` que pode causar uma exceção. A cláusula `catch` contém o manipulador de exceção que apenas exibe uma mensagem na tela. Quando instrução `throw` é chamada de dentro de `ProcessString`, o sistema procura a instrução `catch` e exibe a mensagem `Exception caught`.

[!code-csharp[csrefKeywordsExceptions#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsExceptions/CS/csrefKeywordsExceptions.cs#2)]

## <a name="two-catch-blocks-example"></a>Exemplo de dois blocos catch

No exemplo a seguir, dois blocos catch são usados e a exceção mais específica, que vem primeiro, é capturada.

Para capturar a exceção menos específica, você pode substituir a instrução throw em `ProcessString` pela seguinte instrução: `throw new Exception()`.

Se você colocar o bloco catch menos específica primeiro no exemplo, a seguinte mensagem de erro aparecerá: `A previous catch clause already catches all exceptions of this or a super type ('System.Exception')`.

[!code-csharp[csrefKeywordsExceptions#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsExceptions/CS/csrefKeywordsExceptions.cs#3)]

## <a name="async-method-example"></a>Exemplo de método assíncrono

O exemplo a seguir ilustra o tratamento de exceção para métodos assíncronos. Para capturar uma exceção que lança uma tarefa assíncrona, coloque a expressão `await` em um bloco `try` e capture a exceção em um bloco `catch`.

Remova a marca de comentário da linha `throw new Exception` no exemplo para demonstrar o tratamento de exceção. A propriedade `IsFaulted` da tarefa é definida para `True`, a propriedade `Exception.InnerException` da tarefa é definida para a exceção e a exceção é capturada em um bloco `catch`.

Remova a marca de comentário da linha `throw new OperationCanceledException` para demonstrar o que acontece quando você cancela um processo assíncrono. A propriedade `IsCanceled` da tarefa é definida para `true` e a exceção é capturada no bloco `catch`. Em algumas condições que não se aplicam a este exemplo, a propriedade `IsFaulted` da tarefa é definida para `true` e `IsCanceled` é definido para `false`.

[!code-csharp[csAsyncExceptions#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csasyncexceptions/cs/class1.cs#2)]  

## <a name="taskwhenall-example"></a>Exemplo de Task.WhenAll

O exemplo a seguir ilustra a manipulação de exceção em que várias tarefas podem resultar em várias exceções. O bloco `try` aguarda a tarefa que é retornada por uma chamada para <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType>. A tarefa é concluída quando as três tarefas às quais WhenAll se aplica são concluídas.

Cada uma das três tarefas causa uma exceção. O bloco `catch` itera por meio de exceções, que são encontradas na propriedade `Exception.InnerExceptions` da tarefa que foi retornada por <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType>.

[!code-csharp[csAsyncExceptions#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csasyncexceptions/cs/class1.cs#4)]

## <a name="c-language-specification"></a>especificação da linguagem C#

Para obter mais informações, confira a seção [A instrução try](~/_csharplang/spec/statements.md#the-try-statement) da [Especificação da linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="see-also"></a>Consulte também

- [Referência do C#](../index.md)
- [Guia de programação C#](../../programming-guide/index.md)
- [Palavras-chave do C#](index.md)
- [Instruções try, throw e catch (C++)](/cpp/cpp/try-throw-and-catch-statements-cpp)
- [throw](throw.md)
- [Experimente-finalmente](try-finally.md)
- [Como gerar exceções explicitamente](../../../standard/exceptions/how-to-explicitly-throw-exceptions.md)
