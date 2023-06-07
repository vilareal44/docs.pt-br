---
title: Padrão de design do observador
description: Saiba mais sobre o padrão de design do observador no .NET. Esse padrão permite que um assinante registre e receba notificações de um provedor.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- IObserver(Of T) interface
- IObservable<T> interface
- IObserver<T> interface
- IObservable(Of T) interface
- observer design pattern [.NET Framework]
ms.assetid: 3680171f-f522-453c-aa4a-54f755a78f88
ms.openlocfilehash: 4edcd2645b28095f4bd18f4918b9afa5c893bd39
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662726"
---
# <a name="observer-design-pattern"></a>Padrão de design do observador

O padrão de design do observador permite a um assinante se registrar em um provedor e receber notificações dele. Ele é adequado para qualquer cenário que requer a notificação por push. O padrão define um *provedor* (também conhecido como *assunto* ou *observável*) e zero, um ou mais *observadores*. Observadores registram-se com o provedor e, sempre que uma condição, evento ou alteração de estado predefinido ocorrer, o provedor notificará automaticamente todos os observadores chamando um dos seus métodos. Nessa chamada de método, o provedor também pode fornecer informações sobre o estado atual para observadores. No .NET Framework, o padrão de design de observador é aplicado ao implementar as interfaces genéricas <xref:System.IObservable%601?displayProperty=nameWithType> e <xref:System.IObserver%601?displayProperty=nameWithType>. O parâmetro de tipo genérico representa o tipo que fornece informações de notificação.

## <a name="applying-the-pattern"></a>Aplicando o padrão

O padrão de design do observador é adequado para notificações por push distribuídas, pois oferece suporte para uma separação clara entre dois componentes diferentes ou camadas de aplicativo, como uma camada de fonte de dados (lógica de negócios) uma camada de interface do usuário (exibição). O padrão pode ser implementado sempre que um provedor usa retornos de chamada para fornecer informações atuais a seus clientes.

A implementação do padrão exige que você forneça o seguinte:

- Um provedor ou o assunto, que é o objeto que envia notificações para observadores. Um provedor é uma classe ou estrutura que implementa a interface <xref:System.IObservable%601>. O provedor deve implementar um único método, <xref:System.IObservable%601.Subscribe%2A?displayProperty=nameWithType>, que é chamado pelos observadores que desejam receber notificações do provedor.

- Um observador, que é um objeto que recebe notificações de um provedor. Um observador é uma classe ou estrutura que implementa a interface <xref:System.IObserver%601>. O observador deve implementar três métodos, que são chamados pelo provedor:

  - <xref:System.IObserver%601.OnNext%2A?displayProperty=nameWithType>, que fornece ao observador informações novas ou atuais.

  - <xref:System.IObserver%601.OnError%2A?displayProperty=nameWithType>, que informa o observador que ocorreu um erro.

  - <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType>, que indica que o provedor terminou de enviar notificações.

- Um mecanismo que permite que o provedor mantenha controle dos observadores. Normalmente, o provedor usa um objeto de contêiner, como um objeto <xref:System.Collections.Generic.List%601?displayProperty=nameWithType>, para manter as referências às implementações de <xref:System.IObserver%601> que assinaram notificações. Usar um contêiner de armazenamento para essa finalidade permite que o provedor lidar com um número ilimitado de observadores. A ordem na qual os observadores recebem notificações não está definida. O provedor está livre para usar qualquer método para determinar a ordem.

- Uma implementação de <xref:System.IDisposable> que permite que o provedor remova os observadores quando a notificação for concluída. Observadores recebem uma referência para a implementação de <xref:System.IDisposable> do método <xref:System.IObservable%601.Subscribe%2A>, portanto, eles também podem chamar o método <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> para cancelar a assinatura antes que o provedor tenha terminado de enviar notificações.

- Um objeto que contém os dados que o provedor envia para seus observadores. O tipo desse objeto corresponde ao parâmetro de tipo genérico das interfaces <xref:System.IObservable%601> e <xref:System.IObserver%601>. Embora esse objeto possa ser o mesmo que a implementação de <xref:System.IObservable%601>, geralmente ele é um tipo separado.

> [!NOTE]
> Além de implementar o padrão de design do observador, pode ser interessante explorar bibliotecas que são criadas usando as interfaces <xref:System.IObservable%601> e <xref:System.IObserver%601>. Por exemplo, [Extensões Reativas para .NET (Rx)](https://docs.microsoft.com/previous-versions/dotnet/reactive-extensions/hh242985(v=vs.103)) consistem em um conjunto de métodos de extensão e operadores de sequência padrão LINQ para oferecer suporte à programação assíncrona.

## <a name="implementing-the-pattern"></a>Implementando o padrão

O exemplo a seguir usa o padrão de design do observador para implementar um sistema de informações de coleta de bagagem de aeroporto. Uma classe `BaggageInfo` fornece informações sobre voos que chegam e as esteiras onde as bagagens de cada voo estão disponíveis para retirada. Isso é mostrado no exemplo a seguir.

[!code-csharp[Conceptual.ObserverDesignPattern#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesignpattern/cs/provider.cs#1)]
[!code-vb[Conceptual.ObserverDesignPattern#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesignpattern/vb/provider.vb#1)]

Uma classe `BaggageHandler` é responsável por receber informações sobre os voos que chegam e as esteiras de coleta de bagagem. Internamente, ela mantém duas coleções:

- `observers` - Uma coleção de clientes que receberão informações atualizadas.

- `flights` - Uma coleção de voos e suas respectivas esteiras.

Ambas as coleções são representadas por objetos <xref:System.Collections.Generic.List%601> genéricos que são instanciados no construtor de classe `BaggageHandler`. O código-fonte para a classe `BaggageHandler` é mostrado no exemplo a seguir.

[!code-csharp[Conceptual.ObserverDesignPattern#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesignpattern/cs/provider.cs#2)]
[!code-vb[Conceptual.ObserverDesignPattern#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesignpattern/vb/provider.vb#2)]

Clientes que desejem receber informações atualizadas devem chamar o método `BaggageHandler.Subscribe`. Se o cliente não tiver assinado as notificações anteriormente, uma referência para a implementação <xref:System.IObserver%601> do cliente será adicionada à coleção `observers`.

O método `BaggageHandler.BaggageStatus` sobrecarregado pode ser chamado para indicar que bagagem de um voo está sendo descarregada ou que não está mais sendo descarregada. No primeiro caso, o método recebe um número de voo, o aeroporto que originou o voo e a esteira onde bagagem está sendo descarregada. No segundo caso, o método recebe apenas um número de voo. Para a bagagem que está sendo descarregada, o método verifica se as informações de `BaggageInfo` passadas para o método existem na coleção `flights`. Se não existirem, o método adicionará as informações e chamará o método `OnNext` de cada observador. Para voos cuja bagagem não está mais sendo descarregada, o método verificará se as informações sobre esse voo estão armazenadas na coleção `flights`. Se estiverem, o método chamará o método `OnNext` de cada observador e removerá o objeto `BaggageInfo` da coleção `flights`.

Quando o último voo do dia tiver aterrizado e sua bagagem tiver sido processada, o método `BaggageHandler.LastBaggageClaimed` será chamado. Esse método chama o método `OnCompleted` de cada observador para indicar que todas as notificações foram finalizadas e, em seguida, limpa a coleção `observers`.

O método <xref:System.IObservable%601.Subscribe%2A> do provedor retorna uma implementação de <xref:System.IDisposable> que permite que os observadores interrompam o recebimento de notificações antes que o método <xref:System.IObserver%601.OnCompleted%2A> seja chamado. O código-fonte para essa classe `Unsubscriber(Of BaggageInfo)` é mostrado no exemplo a seguir. Quando a classe é instanciada no método `BaggageHandler.Subscribe`, ela é passado como referência para a coleção `observers` e uma referência para o observador que é adicionado à coleção. Essas referências são atribuídas a variáveis locais. Quando o método `Dispose` do objeto é chamado, ele verifica se o observador ainda existe na coleção `observers` e, em caso afirmativo, remove o observador.

[!code-csharp[Conceptual.ObserverDesignPattern#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesignpattern/cs/provider.cs#3)]
[!code-vb[Conceptual.ObserverDesignPattern#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesignpattern/vb/provider.vb#3)]

O exemplo a seguir fornece uma implementação de <xref:System.IObserver%601> denominada `ArrivalsMonitor`, que é uma classe base que exibe informações de coleta de bagagem. As informações são exibidas em ordem alfabética, pelo nome da cidade de origem. Os métodos de `ArrivalsMonitor` são marcados como `overridable` (no Visual Basic) ou `virtual` (em C#), portanto, todos podem ser substituídos por uma classe derivada.

[!code-csharp[Conceptual.ObserverDesignPattern#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesignpattern/cs/observer.cs#4)]
[!code-vb[Conceptual.ObserverDesignPattern#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesignpattern/vb/observer.vb#4)]

A classe `ArrivalsMonitor` inclui os métodos `Subscribe` e `Unsubscribe`. O método `Subscribe` permite que a classe salve a implementação de <xref:System.IDisposable> retornada pela chamada para <xref:System.IObservable%601.Subscribe%2A> para uma variável particular. O método `Unsubscribe` permite que a classe cancele a assinatura de notificações ao chamar a implementação de <xref:System.IDisposable.Dispose%2A> do provedor. `ArrivalsMonitor` também fornece implementações dos métodos <xref:System.IObserver%601.OnNext%2A>, <xref:System.IObserver%601.OnError%2A>, e <xref:System.IObserver%601.OnCompleted%2A>. Somente a implementação de <xref:System.IObserver%601.OnNext%2A> contém uma quantidade significativa de código. O método funciona com um objeto <xref:System.Collections.Generic.List%601> particular, classificado e genérico que mantém informações sobre os aeroportos de origem dos voos que chegam e sobre as esteiras nas quais as respectivas bagagens estarão disponíveis. Se a classe `BaggageHandler` relata um novo voo chegando, a implementação do método <xref:System.IObserver%601.OnNext%2A> adiciona informações sobre esse voo à lista. Se a classe `BaggageHandler` relata que a bagagem do voo foi descarregada, o método <xref:System.IObserver%601.OnNext%2A> remove essa voo da lista. Sempre que uma alteração é feita, a lista é classificada e exibida no console.

O exemplo a seguir contém o ponto de entrada do aplicativo que instancia a classe `BaggageHandler`, bem como duas instâncias da classe `ArrivalsMonitor` e usa o método `BaggageHandler.BaggageStatus` para adicionar e remover informações sobre voos que chegam. Em cada caso, os observadores recebem atualizações e exibem corretamente as informações de coleta de bagagem.

[!code-csharp[Conceptual.ObserverDesignPattern#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesignpattern/cs/program.cs#5)]
[!code-vb[Conceptual.ObserverDesignPattern#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesignpattern/vb/module1.vb#5)]

## <a name="related-topics"></a>Tópicos relacionados

|Title|Descrição|
|-----------|-----------------|
|[Práticas recomendadas para o padrão de design do observador](observer-design-pattern-best-practices.md)|São descritas as práticas recomendadas ao desenvolver aplicativos que implementam o padrão de design do observador.|
|[Como implementar um provedor](how-to-implement-a-provider.md)|É fornecida uma implementação passo a passo de um provedor para uma aplicativo de monitoramento de temperatura.|
|[Como: implementar um observador](how-to-implement-an-observer.md)|É fornecida uma implementação passo a passo de um observador para uma aplicativo de monitoramento de temperatura.|
