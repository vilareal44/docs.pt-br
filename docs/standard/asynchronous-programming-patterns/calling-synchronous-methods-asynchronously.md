---
title: Chamando métodos síncronos de forma assíncrona
description: Saiba como chamar métodos síncronos de forma assíncrona no .NET, usando os métodos BeginInvoke e EndInvoke.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- asynchronous programming, delegates
- asynchronous delegates
- AsyncWaitHandle property
- callback methods
- calling synchronous methods in asynchronous manner
- WaitHandle class, code examples
- asynchronous programming, status polling
- polling asynchronous operation status
- delegates [.NET Framework], asynchronous
- synchronous calling in asynchronous manner
- waiting for asynchronous calls
- status information [.NET Framework], asynchronous operations
ms.assetid: 41972034-92ed-450a-9664-ab93fcc6f1fb
ms.openlocfilehash: ff2d30c00e7b6becb0c3ff910d825c2e9d6f78e3
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662635"
---
# <a name="calling-synchronous-methods-asynchronously"></a>Chamando métodos síncronos de forma assíncrona

O .NET Framework permite que você chame qualquer método de forma assíncrona. Para fazer isso, defina um delegado com a mesma assinatura que o método que você deseja chamar; o common language runtime definirá automaticamente os métodos `BeginInvoke` e `EndInvoke` para este delegado, com as assinaturas apropriadas.

> [!NOTE]
> Chamadas assíncronas de delegado, especificamente os métodos `BeginInvoke` e `EndInvoke`, não têm suporte no .NET Compact Framework.

O método `BeginInvoke` inicia a chamada assíncrona. Ele tem os mesmos parâmetros que o método que você deseja executar de forma assíncrona, além de outros dois parâmetros opcionais. O primeiro parâmetro é um delegado <xref:System.AsyncCallback> que faz referência a um método a ser chamado quando a chamada assíncrona é concluída. O segundo parâmetro é um objeto definido pelo usuário que passa informações para o método de retorno de chamada. `BeginInvoke` retorna imediatamente e não espera a conclusão da chamada assíncrona. `BeginInvoke` retorna um <xref:System.IAsyncResult>, que pode ser usado para monitorar o andamento da chamada assíncrona.

O método `EndInvoke` recupera os resultados da chamada assíncrona. Ele pode ser chamado a qualquer momento após `BeginInvoke`. Se a chamada assíncrona não for concluída, `EndInvoke` bloqueará o thread de chamada até que seja concluída. Os parâmetros de `EndInvoke` incluem os `out` `ref` parâmetros e ( `<Out>` `ByRef` e `ByRef` em Visual Basic) do método que você deseja executar de forma assíncrona, além do <xref:System.IAsyncResult> retornado por `BeginInvoke` .

> [!NOTE]
> O recurso IntelliSense no Visual Studio exibe os parâmetros de `BeginInvoke` e `EndInvoke`. Se você não estiver usando o Visual Studio ou uma ferramenta semelhante ou se estiver usando C# com Visual Studio, confira [APM (Modelo de Programação Assíncrona)](asynchronous-programming-model-apm.md) para obter uma descrição dos parâmetros definidos para esses métodos.

Os exemplos de código neste tópico demonstram quatro formas comuns de usar `BeginInvoke` e `EndInvoke` para fazer chamadas assíncronas. Após chamar `BeginInvoke`, você pode fazer o seguinte:

- Trabalhe um pouco e depois chame `EndInvoke` para bloquear até que a chamada seja concluída.

- Obtenha um <xref:System.Threading.WaitHandle> usando a propriedade <xref:System.IAsyncResult.AsyncWaitHandle%2A?displayProperty=nameWithType>, use seu método <xref:System.Threading.WaitHandle.WaitOne%2A> para impedir a execução até que o <xref:System.Threading.WaitHandle> seja sinalizado e, depois, chame `EndInvoke`.

- Sonde o <xref:System.IAsyncResult> retornado por `BeginInvoke` para determinar quando a chamada assíncrona for concluída e, em seguida, chame `EndInvoke`.

- Passe um delegado para um método de retorno de chamada para `BeginInvoke`. O método será executado em um thread <xref:System.Threading.ThreadPool> quando a chamada assíncrona for concluída. O método de retorno de chamada chama `EndInvoke`.

> [!IMPORTANT]
> Não importa qual técnica você usa, sempre chame `EndInvoke` para completar a chamada assíncrona.

## <a name="defining-the-test-method-and-asynchronous-delegate"></a>Definir o Método de teste e o Delegado assíncrono
 Os exemplos de código a seguir demonstram várias maneiras de chamar o mesmo método de longa execução, `TestMethod`, de forma assíncrona. O método `TestMethod` exibe uma mensagem de console para mostrar que começou a processar, dorme por alguns segundos e, depois, termina. `TestMethod` tem um parâmetro `out` para demonstrar a maneira como esses parâmetros são adicionados às assinaturas de `BeginInvoke` e `EndInvoke`. Você pode tratar os parâmetros `ref` da mesma forma.

 O exemplo de código a seguir mostra a definição de `TestMethod` e o delegado chamado `AsyncMethodCaller` que pode ser usado para chamar `TestMethod` de forma assíncrona. Para compilar os exemplos de código, você deve incluir as definições de `TestMethod` e o delegado `AsyncMethodCaller`.

 [!code-cpp[AsyncDelegateExamples#1](../../../samples/snippets/cpp/VS_Snippets_CLR/AsyncDelegateExamples/cpp/TestMethod.cpp#1)]
 [!code-csharp[AsyncDelegateExamples#1](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDelegateExamples/CS/TestMethod.cs#1)]
 [!code-vb[AsyncDelegateExamples#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDelegateExamples/VB/TestMethod.vb#1)]

## <a name="waiting-for-an-asynchronous-call-with-endinvoke"></a>Aguardar uma chamada assíncrona com EndInvoke
 A maneira mais simples de executar um método de forma assíncrona é começar a execução do método chamando o método `BeginInvoke` do delegado, trabalhar um pouco no thread principal e, depois, chamar o método `EndInvoke` do delegado. `EndInvoke` pode bloquear o thread de chamada, pois não retorna até que a chamada assíncrona seja concluída. Essa é uma técnica válida para usar com operações de arquivo ou de rede.

> [!IMPORTANT]
> Como `EndInvoke` pode bloquear, nunca chame-o a partir de threads que atendem à interface do usuário.

 [!code-cpp[AsyncDelegateExamples#2](../../../samples/snippets/cpp/VS_Snippets_CLR/AsyncDelegateExamples/cpp/EndInvoke.cpp#2)]
 [!code-csharp[AsyncDelegateExamples#2](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDelegateExamples/CS/EndInvoke.cs#2)]
 [!code-vb[AsyncDelegateExamples#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDelegateExamples/VB/EndInvoke.vb#2)]

## <a name="waiting-for-an-asynchronous-call-with-waithandle"></a>Aguardar uma chamada assíncrona com WaitHandle
 Você pode obter um <xref:System.Threading.WaitHandle> usando a propriedade <xref:System.IAsyncResult.AsyncWaitHandle%2A> do <xref:System.IAsyncResult> retornado por `BeginInvoke`. O <xref:System.Threading.WaitHandle> é sinalizado quando a chamada assíncrona é concluída, e você pode esperar por ele chamando o método <xref:System.Threading.WaitHandle.WaitOne%2A>.

 Se você usar um <xref:System.Threading.WaitHandle>, poderá executar um processamento adicional antes ou depois da conclusão da chamada assíncrona, mas antes de chamar `EndInvoke` para recuperar os resultados.

> [!NOTE]
> O identificador de espera não é fechado automaticamente quando você chama `EndInvoke`. Se você liberar todas as referências para o identificador de espera, os recursos de sistema serão liberados quando a coleta de lixo recuperar o identificador de espera. Para liberar os recursos do sistema assim que você terminar de usar o identificador de espera, descarte-o chamando o método <xref:System.Threading.WaitHandle.Close%2A?displayProperty=nameWithType>. A coleta de lixo funciona com mais eficiência quando objetos descartáveis são explicitamente descartados.

 [!code-cpp[AsyncDelegateExamples#3](../../../samples/snippets/cpp/VS_Snippets_CLR/AsyncDelegateExamples/cpp/waithandle.cpp#3)]
 [!code-csharp[AsyncDelegateExamples#3](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDelegateExamples/CS/waithandle.cs#3)]
 [!code-vb[AsyncDelegateExamples#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDelegateExamples/VB/WaitHandle.vb#3)]

## <a name="polling-for-asynchronous-call-completion"></a>Sondar a conclusão da chamada assíncrona
 Você pode usar a propriedade <xref:System.IAsyncResult.IsCompleted%2A> do <xref:System.IAsyncResult> retornado por `BeginInvoke` para descobrir quando a chamada assíncrona for concluída. Você pode fazer isso ao fazer a chamada assíncrona de um thread que atenda à interface do usuário. A sondagem da conclusão permite que o thread de chamada continue em execução enquanto a chamada assíncrona é executada em um thread <xref:System.Threading.ThreadPool>.

 [!code-cpp[AsyncDelegateExamples#4](../../../samples/snippets/cpp/VS_Snippets_CLR/AsyncDelegateExamples/cpp/polling.cpp#4)]
 [!code-csharp[AsyncDelegateExamples#4](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDelegateExamples/CS/polling.cs#4)]
 [!code-vb[AsyncDelegateExamples#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDelegateExamples/VB/polling.vb#4)]

## <a name="executing-a-callback-method-when-an-asynchronous-call-completes"></a>Executar um método de retorno de chamada quando uma chamada assíncrona for concluída
 Se o thread que inicia a chamada assíncrona não precisar ser o thread que processa os resultados, você poderá executar um método de retorno de chamada quando a chamada for concluída. O método de retorno de chamada é executado em um thread <xref:System.Threading.ThreadPool>.

 Para usar um método de retorno de chamada, você deve passar a `BeginInvoke` um delegado <xref:System.AsyncCallback> que representa o método de retorno de chamada. Você também pode passar um objeto que contém informações a serem usadas pelo método de retorno de chamada. No método de retorno de chamada, você pode converter o <xref:System.IAsyncResult>, que é o único parâmetro do método de retorno de chamada, em um objeto <xref:System.Runtime.Remoting.Messaging.AsyncResult>. Depois, você pode usar a propriedade <xref:System.Runtime.Remoting.Messaging.AsyncResult.AsyncDelegate%2A?displayProperty=nameWithType> para obter o delegado que foi usado para iniciar a chamada para que você possa chamar `EndInvoke`.

 Observações sobre o exemplo:

- O `threadId` parâmetro de `TestMethod` é um `out` parâmetro ([ `<Out>` `ByRef` em Visual Basic), portanto, seu valor de entrada nunca é usado pelo `TestMethod` . Uma variável fictícia é passada para a chamada `BeginInvoke`. Se o parâmetro `threadId` fosse um parâmetro `ref` (`ByRef` no Visual Basic), a variável precisaria ser um campo de nível de classe para que pudesse ser passada para `BeginInvoke` e `EndInvoke`.

- As informações de estado que são passadas para `BeginInvoke` são uma cadeia de caracteres de formato, usada pelo método de retorno de chamada para formatar uma mensagem de saída. Como são passadas como o tipo <xref:System.Object>, as informações de estado devem ser convertidas para o tipo correto antes de poderem ser usadas.

- O retorno de chamada é feito em um thread <xref:System.Threading.ThreadPool>. Threads <xref:System.Threading.ThreadPool> são threads em segundo plano, os quais não mantêm o aplicativo em execução se o thread principal terminar. Portanto, o thread principal do exemplo precisa ficar suspenso durante um tempo suficiente para o retorno de chamada ser concluído.

 [!code-cpp[AsyncDelegateExamples#5](../../../samples/snippets/cpp/VS_Snippets_CLR/AsyncDelegateExamples/cpp/callback.cpp#5)]
 [!code-csharp[AsyncDelegateExamples#5](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDelegateExamples/CS/callback.cs#5)]
 [!code-vb[AsyncDelegateExamples#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDelegateExamples/VB/callback.vb#5)]

## <a name="see-also"></a>Confira também

- <xref:System.Delegate>
- [EAP (Padrão Assíncrono baseado em Evento)](event-based-asynchronous-pattern-eap.md)
