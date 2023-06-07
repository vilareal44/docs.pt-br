---
title: Eventos
description: 'Saiba como os eventos F # permitem associar chamadas de função a ações do usuário, que são importantes na programação de GUI.'
ms.date: 08/15/2020
ms.openlocfilehash: 42783255412d56c6ff6729694c31d0868ed99633
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88559187"
---
# <a name="events"></a>Eventos

Eventos permitem que você associe chamadas de função a ações de usuário e são importantes na programação da GUI. Os eventos também podem ser disparados por seus aplicativos ou pelo sistema operacional.

## <a name="handling-events"></a>Tratando eventos

Quando você usa uma biblioteca de GUI como o Windows Forms ou o Windows Presentation Foundation (WPF), grande parte do código em seu aplicativo é executado em resposta a eventos que são predefinidos pela biblioteca. Esses eventos predefinidos são membros de classes da GUI como formulários e controles. Você pode adicionar comportamento personalizado a um evento preexistente, como o clique de botão, ao fazer referência ao evento de interesse nomeado específico (por exemplo, o evento `Click` da classe `Form`) e invocar o método `Add` como mostrado no código a seguir. Se você executar isso do F# Interativo, omita a chamada a `System.Windows.Forms.Application.Run(System.Windows.Forms.Form)`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3601.fs)]

O tipo do método `Add` é `('a -> unit) -> unit`. Portanto, o método do manipulador de eventos assume um parâmetro, normalmente os argumentos de evento, e retorna `unit`. O exemplo anterior mostra o manipulador de eventos como uma expressão lambda. O manipulador de eventos também pode ser um valor de função como no exemplo de código a seguir. O exemplo de código a seguir também mostra o uso de parâmetros do manipulador de eventos que fornecem informações específicas para o tipo de evento. Para um evento `MouseMove`, o sistema passa um objeto `System.Windows.Forms.MouseEventArgs` que contém a posição `X` e `Y` do ponteiro.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3602.fs)]

## <a name="creating-custom-events"></a>Criando eventos personalizados

Os eventos f # são representados pelo tipo de [evento](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpevent-1.html) f #, que implementa a interface [IEvent](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-ievent-1.html) . `IEvent` é uma interface que combina a funcionalidade de duas outras interfaces `System.IObservable<'T>` e [IDelegateEvent](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-idelegateevent-1.html). Portanto, `Event`s têm funcionalidade equivalente de delegados em outras linguagens, além da funcionalidade adicional de `IObservable` o que significa que os eventos de F# oferecem suporte à filtragem de eventos e ao uso de funções de primeira classe de F# e expressões lambda como manipuladores de eventos. Essa funcionalidade é fornecida no [módulo de evento](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-eventmodule.html).

Para criar um evento em uma classe que atue como qualquer outro evento do .NET Framework, adicione à classe uma associação `let` que defina um `Event` como um campo em uma classe. Você pode especificar o tipo de argumento de evento desejado como o argumento de tipo ou deixá-lo em branco e fazer com que o compilador deduza o tipo apropriado. Você também deve definir um membro de evento que exponha o evento como um evento de CLI. Esse membro deve ter o atributo [CLIEvent](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-clieventattribute.html) . Ele é declarado como uma propriedade e sua implementação é apenas uma chamada para a propriedade [Publish](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpevent-1.html#Publish) do evento. Os usuários de sua classe podem usar o método `Add` do evento publicado para adicionar um manipulador. O argumento para o método `Add` pode ser uma expressão lambda. Você pode usar a propriedade `Trigger` do evento para gerar o evento, passando os argumentos à função do manipulador. O exemplo de código a seguir ilustra isso. Neste exemplo, o argumento do tipo inferido para o evento é uma tupla que representa os argumentos para a expressão lambda.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3605.fs)]

A saída é a seguinte.

```console
Event1 occurred! Object data: Hello World!
```

A funcionalidade adicional fornecida pelo módulo `Event` está ilustrada aqui. O exemplo de código a seguir ilustra o uso básico de `Event.create` para criar um evento e um método de gatilho, adicionar dois manipuladores de eventos na forma de expressões lambda e disparar o evento para executar ambas as expressões lambda.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3603.fs)]

A saída do código anterior está a seguir.

```console
Event occurred.
Given a value: Event occurred.
```

## <a name="processing-event-streams"></a>Processando fluxos de eventos

Em vez de apenas adicionar um manipulador de eventos para um evento usando a função [evento. Add](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-eventmodule.html#add) , você pode usar as funções no `Event` módulo para processar fluxos de eventos de maneiras altamente personalizadas. Para fazer isso, use a barra vertical e o sinal maior que (`|>`) junto com o evento como o primeiro valor em uma série de chamadas de função e funções do módulo `Event` como chamadas de função subsequentes.

O exemplo de código a seguir mostra como configurar um evento em que o manipulador é chamado apenas em determinadas condições.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3604.fs)]

O [módulo observável](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-observablemodule.html) contém funções semelhantes que operam em objetos observáveis. Objetos observáveis são semelhantes a eventos, mas apenas assinam eventos ativamente se eles mesmos estiverem sendo assinados.

## <a name="implementing-an-interface-event"></a>Implementando um evento de interface

À medida em que desenvolve componentes de interface do usuário, você frequentemente começa ao criar um novo formulário ou um novo controle que herde de um formulário ou controle existente. Eventos são frequentemente definidos em uma interface e, nesse caso, você deve implementar a interface para implementar o evento. A interface `System.ComponentModel.INotifyPropertyChanged` define um único evento `System.ComponentModel.INotifyPropertyChanged.PropertyChanged`. O código a seguir ilustra como implementar o evento que essa interface herdada definiu:

```fsharp
module CustomForm

open System.Windows.Forms
open System.ComponentModel

type AppForm() as this =
    inherit Form()

    // Define the propertyChanged event.
    let propertyChanged = Event<PropertyChangedEventHandler, PropertyChangedEventArgs>()
    let mutable underlyingValue = "text0"

    // Set up a click event to change the properties.
    do
        this.Click |> Event.add(fun evArgs ->
            this.Property1 <- "text2"
            this.Property2 <- "text3")

    // This property does not have the property-changed event set.
    member val Property1 : string = "text" with get, set

    // This property has the property-changed event set.
    member this.Property2
        with get() = underlyingValue
        and set(newValue) =
            underlyingValue <- newValue
            propertyChanged.Trigger(this, new PropertyChangedEventArgs("Property2"))

    // Expose the PropertyChanged event as a first class .NET event.
    [<CLIEvent>]
    member this.PropertyChanged = propertyChanged.Publish

    // Define the add and remove methods to implement this interface.
    interface INotifyPropertyChanged with
        member this.add_PropertyChanged(handler) = propertyChanged.Publish.AddHandler(handler)
        member this.remove_PropertyChanged(handler) = propertyChanged.Publish.RemoveHandler(handler)

    // This is the event-handler method.
    member this.OnPropertyChanged(args : PropertyChangedEventArgs) =
        let newProperty = this.GetType().GetProperty(args.PropertyName)
        let newValue = newProperty.GetValue(this :> obj) :?> string
        printfn "Property %s changed its value to %s" args.PropertyName newValue

// Create a form, hook up the event handler, and start the application.
let appForm = new AppForm()
let inpc = appForm :> INotifyPropertyChanged
inpc.PropertyChanged.Add(appForm.OnPropertyChanged)
Application.Run(appForm)
```

Se você desejar conectar o evento no construtor, o código será um pouco mais complicado porque a conexão do evento deverá estar em um bloco `then` em um construtor adicional como no exemplo a seguir:

```fsharp
module CustomForm

open System.Windows.Forms
open System.ComponentModel

// Create a private constructor with a dummy argument so that the public
// constructor can have no arguments.
type AppForm private (dummy) as this =
    inherit Form()

    // Define the propertyChanged event.
    let propertyChanged = Event<PropertyChangedEventHandler, PropertyChangedEventArgs>()
    let mutable underlyingValue = "text0"

    // Set up a click event to change the properties.
    do
        this.Click |> Event.add(fun evArgs ->
            this.Property1 <- "text2"
            this.Property2 <- "text3")

    // This property does not have the property changed event set.
    member val Property1 : string = "text" with get, set

    // This property has the property changed event set.
    member this.Property2
        with get() = underlyingValue
        and set(newValue) =
            underlyingValue <- newValue
            propertyChanged.Trigger(this, new PropertyChangedEventArgs("Property2"))

    [<CLIEvent>]
    member this.PropertyChanged = propertyChanged.Publish

    // Define the add and remove methods to implement this interface.
    interface INotifyPropertyChanged with
        member this.add_PropertyChanged(handler) = this.PropertyChanged.AddHandler(handler)
        member this.remove_PropertyChanged(handler) = this.PropertyChanged.RemoveHandler(handler)

    // This is the event handler method.
    member this.OnPropertyChanged(args : PropertyChangedEventArgs) =
        let newProperty = this.GetType().GetProperty(args.PropertyName)
        let newValue = newProperty.GetValue(this :> obj) :?> string
        printfn "Property %s changed its value to %s" args.PropertyName newValue

    new() as this =
        new AppForm(0)
        then
            let inpc = this :> INotifyPropertyChanged
            inpc.PropertyChanged.Add(this.OnPropertyChanged)

// Create a form, hook up the event handler, and start the application.
let appForm = new AppForm()
Application.Run(appForm)
```

## <a name="see-also"></a>Confira também

- [Membros](index.md)
- [Manipulando e gerando eventos](../../../standard/events/index.md)
- [Expressões lambda: a `fun` palavra-chave](../functions/lambda-expressions-the-fun-keyword.md)
