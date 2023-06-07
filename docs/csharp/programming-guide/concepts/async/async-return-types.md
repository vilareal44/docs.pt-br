---
title: Tipos de retorno assíncronos (C#)
description: Saiba mais sobre os tipos de retorno que os métodos Async podem ter em C# com exemplos de código para cada tipo e recursos adicionais.
ms.date: 04/14/2020
ms.assetid: ddb2539c-c898-48c1-ad92-245e4a996df8
ms.openlocfilehash: 954e449356819595a3a974a6dece5349e53ec88a
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925378"
---
# <a name="async-return-types-c"></a>Tipos de retorno assíncronos (C#)

Métodos assíncronos podem conter os seguintes tipos de retorno:

- <xref:System.Threading.Tasks.Task%601>, para um método assíncrono que retorna um valor.
- <xref:System.Threading.Tasks.Task>, para um método assíncrono que executa uma operação, mas não retorna nenhum valor.
- `void`, para um manipulador de eventos.
- Começando com o C# 7.0, qualquer tipo que tenha um método acessível `GetAwaiter`. O objeto retornado pelo método `GetAwaiter` deve implementar a interface <xref:System.Runtime.CompilerServices.ICriticalNotifyCompletion?displayProperty=nameWithType>.
- Começando com o C# 8,0, <xref:System.Collections.Generic.IAsyncEnumerable%601> , para um método assíncrono que retorna um *fluxo assíncrono*.

Para obter mais informações sobre os métodos assíncronos, consulte [Programação assíncrona com async e await (C#)](./index.md).  
  
## <a name="tasktresult-return-type"></a>Tipo de retorno de tarefa \<TResult\>  
O <xref:System.Threading.Tasks.Task%601> tipo de retorno é usado para um método assíncrono que contém uma instrução [Return](../../../language-reference/keywords/return.md) (C#) na qual o operando é `TResult` .  
  
No exemplo a seguir, o método assíncrono `GetLeisureHours` contém uma instrução `return` que retorna um número inteiro. Portanto, a declaração do método deve especificar um tipo de retorno de `Task<int>`.  O método assíncrono <xref:System.Threading.Tasks.Task.FromResult%2A> é um espaço reservado para uma operação que retorna uma cadeia de caracteres.
  
:::code language="csharp" source="./snippets/async-return-types/async-returns1.cs" id="SnippetFirstExample":::

Quando `GetLeisureHours` é chamado de dentro de uma expressão await no método `ShowTodaysInfo`, a expressão await recupera o valor inteiro (o valor de `leisureHours`) que está armazenado na tarefa que é retornada pelo método `GetLeisureHours`. Para obter mais informações sobre expressões await, consulte [await](../../../language-reference/operators/await.md).  
  
Você pode entender melhor como o `await` recupera o resultado de um `Task<T>` separando a chamada para `GetLeisureHours` do aplicativo do `await` , como mostra o código a seguir. Uma chamada ao método `GetLeisureHours` que não é aguardada imediatamente, retorna um `Task<int>`, como você esperaria da declaração do método. A tarefa é atribuída à variável `integerTask` no exemplo. Já que `integerTask` é um <xref:System.Threading.Tasks.Task%601>, ele contém uma propriedade <xref:System.Threading.Tasks.Task%601.Result> do tipo `TResult`. Nesse caso, `TResult` representa um tipo inteiro. Quando `await` é aplicado à `integerTask`, a expressão await é avaliada como o conteúdo da propriedade <xref:System.Threading.Tasks.Task%601.Result%2A> de `integerTask`. O valor é atribuído à variável `ret`.  
  
> [!IMPORTANT]
> A propriedade <xref:System.Threading.Tasks.Task%601.Result%2A> é uma propriedade de bloqueio. Se você tentar acessá-la antes que sua tarefa seja concluída, o thread que está ativo no momento será bloqueado até que a tarefa seja concluída e o valor esteja disponível. Na maioria dos casos, você deve acessar o valor usando `await` em vez de acessar a propriedade diretamente. <br/> O exemplo anterior recuperou o valor da propriedade <xref:System.Threading.Tasks.Task%601.Result%2A> para bloquear o thread principal, de modo que o método `ShowTodaysInfo` pudesse concluir a execução antes do encerramento do aplicativo.  

:::code language="csharp" source="./snippets/async-return-types/async-returns1a.cs" id="SnippetSecondVersion":::

## <a name="task-return-type"></a> Tipo de retorno Task  
Os métodos assíncronos que não contêm uma instrução `return` ou que contêm uma instrução `return` que não retorna um operando, normalmente têm um tipo de retorno de <xref:System.Threading.Tasks.Task>. Esses métodos retornam `void` se eles são executados de forma síncrona. Se você usar um tipo de retorno <xref:System.Threading.Tasks.Task> para um método assíncrono, um método de chamada poderá usar um operador `await` para suspender a conclusão do chamador até que o método assíncrono chamado seja concluído.  
  
No exemplo a seguir, o método assíncrono `WaitAndApologize` não contém uma instrução `return`, então o método retorna um objeto <xref:System.Threading.Tasks.Task>. Retornando uma `Task` habilitação `WaitAndApologize` a ser aguardada. O <xref:System.Threading.Tasks.Task> tipo não inclui uma `Result` propriedade porque não tem nenhum valor de retorno.  

:::code language="csharp" source="./snippets/async-return-types/async-returns2.cs" id="SnippetTaskReturn":::

O `WaitAndApologize` é aguardado, usando uma instrução await, em vez de uma expressão await, semelhante à instrução de chamada a um método síncrono de retorno void. A aplicação de um operador await, nesse caso, não produz um valor.  
  
Como no exemplo <xref:System.Threading.Tasks.Task%601> anterior, você pode separar a chamada ao `WaitAndApologize`, da aplicação de um operador await, como mostrado no código a seguir. No entanto, lembre-se que uma `Task` não tem uma propriedade `Result` e que nenhum valor será produzido quando um operador await for aplicado a uma `Task`.  
  
O código a seguir separa a chamada ao método `WaitAndApologize` da espera pela tarefa que o método retorna.  

:::code language="csharp" source="./snippets/async-return-types/async-returns2a.cs" id="SnippetAwaitTask":::

## <a name="void-return-type"></a>Tipo de retorno void

O tipo de retorno `void` é usado em manipuladores de eventos assíncronos, que exigem um tipo de retorno `void`. Para métodos diferentes de manipuladores de eventos que não retornam um valor, você deve retornar um <xref:System.Threading.Tasks.Task>, porque não é possível esperar por um método assíncrono que retorna `void`. Qualquer chamador desse método deve continuar a conclusão sem esperar que o método assíncrono chamado seja concluído. O chamador deve ser independente de quaisquer valores ou exceções geradas pelo método Async.  
  
O chamador de um método assíncrono de retorno nulo não pode capturar exceções lançadas do método, e essas exceções não tratadas provavelmente causarão a falha do aplicativo. Se um método que retorna um <xref:System.Threading.Tasks.Task> ou <xref:System.Threading.Tasks.Task%601> gera uma exceção, a exceção é armazenada na tarefa retornada. A exceção é relançada quando a tarefa é esperada. Portanto, verifique se qualquer método assíncrono que pode produzir uma exceção tem um tipo de retorno de <xref:System.Threading.Tasks.Task> ou <xref:System.Threading.Tasks.Task%601> e que as chamadas ao método são aguardadas.  
  
Para obter mais informações sobre como capturar exceções em métodos assíncronos, consulte a seção [Exceptions in Async Methods](../../../language-reference/keywords/try-catch.md#exceptions-in-async-methods) do artigo [try-catch](../../../language-reference/keywords/try-catch.md) .  
  
O exemplo a seguir mostra o comportamento de um manipulador de eventos assíncrono. No código de exemplo, um manipulador de eventos assíncrono deve permitir que o thread principal saiba quando ele for concluído. Em seguida, o thread principal pode aguardar um manipulador de eventos assíncronos ser concluído antes de sair do programa.

:::code language="csharp" source="./snippets/async-return-types/async-returns3.cs":::

## <a name="generalized-async-return-types-and-valuetasktresult"></a>Tipos de retorno assíncronos generalizados e ValueTask\<TResult\>

Começando com o C# 7.0, um método assíncrono pode retornar qualquer tipo que tenha um método `GetAwaiter` acessível.

Já que <xref:System.Threading.Tasks.Task> e <xref:System.Threading.Tasks.Task%601> são tipos de referência, a alocação de memória em caminhos críticos para o desempenho, especialmente quando alocações ocorrerem em loops estreitos, podem afetar o desempenho. Suporte para tipos de retorno generalizados significa que você pode retornar um tipo de valor leve em vez de um tipo de referência para evitar as alocações de memória adicionais.

O .NET fornece a estrutura <xref:System.Threading.Tasks.ValueTask%601?displayProperty=nameWithType> como uma implementação leve de um valor de retorno de tarefa generalizado. Para usar o tipo <xref:System.Threading.Tasks.ValueTask%601?displayProperty=nameWithType>, você deve adicionar o pacote NuGet `System.Threading.Tasks.Extensions` ao seu projeto. O exemplo a seguir usa a estrutura <xref:System.Threading.Tasks.ValueTask%601> para recuperar o valor de dois lançamentos de dados.
  
:::code language="csharp" source="./snippets/async-return-types/async-valuetask.cs":::

## <a name="async-streams-with-iasyncenumerablet"></a>Fluxos assíncronos com IAsyncEnumerable\<T\>

A partir do C# 8,0, um método assíncrono pode retornar um *fluxo assíncrono*, representado por <xref:System.Collections.Generic.IAsyncEnumerable%601> . Um fluxo assíncrono fornece uma maneira de enumerar itens lidos de um fluxo quando elementos são gerados em partes com chamadas assíncronas repetidas. O exemplo a seguir mostra um método assíncrono que gera um fluxo assíncrono:

:::code language="csharp" source="./snippets/async-return-types/AsyncStreams.cs" id="SnippetGenerateAsyncStream":::

O exemplo anterior lê linhas de uma cadeia de caracteres de forma assíncrona. Depois que cada linha é lida, o código enumera cada palavra na cadeia de caracteres. Os chamadores enumerariam cada palavra usando a `await foreach` instrução. O método aguarda quando precisa ler de forma assíncrona a próxima linha da cadeia de caracteres de origem.

## <a name="see-also"></a>Veja também

- <xref:System.Threading.Tasks.Task.FromResult%2A>
- [Passo a passo: acessando a Web usando async e await (C#)](./walkthrough-accessing-the-web-by-using-async-and-await.md)
- [Fluxo de controle em programas assíncronos (C#)](./control-flow-in-async-programs.md)
- [async](../../../language-reference/keywords/async.md)
- [await](../../../language-reference/operators/await.md)
