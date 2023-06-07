---
title: 'Suporte do .NET Framework para aplicativos da Windows Store e Windows Runtime '
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- Windows Store apps, .NET Framework support for
- Windows Runtime, .NET Framework support for
- .NET for Windows Store apps
- .NET Framework, and Windows Store apps
- .NET Framework, and Windows Runtime
ms.assetid: 6fa7d044-ae12-4c54-b8ee-50915607a565
ms.openlocfilehash: 7ca5a1259f970f2db5400837eb7d20998dd824cb
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288856"
---
# <a name="net-framework-support-for-windows-store-apps-and-windows-runtime"></a>Suporte do .NET Framework para aplicativos da Windows Store e Windows Runtime 

O .NET Framework 4,5 dá suporte a vários cenários de desenvolvimento de software com o Windows Runtime. Esses cenários se enquadram em três categorias:

- Desenvolvendo aplicativos da loja do Windows 8. x com controles XAML, conforme descrito em [roteiro para aplicativos da Windows Store usando C# ou Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10)), [como o TOS (XAML) e o](https://docs.microsoft.com/previous-versions/windows/apps/br229566(v=win.10)) [.net para aplicativos da Windows Store visão geral](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140)).

- Desenvolver bibliotecas de classes para usar nos aplicativos da loja do Windows 8. x que você cria com o .NET Framework.

- Desenvolvendo Windows Runtime componentes, empacotados em. Arquivos WinMD, que podem ser usados por qualquer linguagem de programação que dê suporte ao Windows Runtime. Por exemplo, consulte [criando componentes Windows Runtime em C# e Visual Basic](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic).

Este tópico descreve o suporte que o .NET Framework fornece para todas as três categorias, além de descrever os cenários de Windows Runtime componentes. A primeira seção inclui informações básicas sobre a relação entre o .NET Framework e o Windows Runtime e explica algumas singularidades que você pode encontrar no sistema de ajuda e no IDE. A [segunda seção](#WindowsRuntimeComponents) discute cenários para o desenvolvimento de componentes Windows Runtime.

## <a name="the-basics"></a>Noções básicas

O .NET Framework dá suporte aos três cenários de desenvolvimento listados anteriormente, fornecendo o .NET para aplicativos da loja do Windows 8. x e oferecendo suporte ao Windows Runtime em si.

- Os [namespaces .NET Framework e Windows Runtime](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140)#net-framework-and-windows-runtime-namespaces) fornecem uma visão simplificada das bibliotecas de classes de .NET Framework e incluem apenas os tipos e os membros que você pode usar para criar aplicativos de armazenamento do Windows 8. x e componentes de Windows Runtime.

  - Quando você usa o Visual Studio (Visual Studio 2012 ou posterior) para desenvolver um aplicativo da loja do Windows 8. x ou um componente Windows Runtime, um conjunto de assemblies de referência garante que você veja apenas os tipos e membros relevantes.

  - Esse conjunto de APIs simplificado é simplificado mais detalhadamente pela remoção de recursos duplicados dentro do .NET Framework ou por duplicação de recursos de Windows Runtime. Por exemplo, ele contém apenas as versões genéricas dos tipos de coleção, e o modelo de objeto de documento XML é eliminado em favor do conjunto de API XML Windows Runtime.

  - Os recursos que simplesmente encapsulam a API do sistema operacional também são removidos, porque a Windows Runtime é fácil de chamar a partir de código gerenciado.

  Para ler mais sobre o .NET para aplicativos da loja do Windows 8. x, consulte a [visão geral do .net para aplicativos da Windows Store](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140)). Para ler sobre o processo de seleção de API, consulte a entrada [.net para aplicativos estilo Metro](https://devblogs.microsoft.com/dotnet/net-for-metro-style-apps/) no blog do .net.

- O [Windows Runtime](/uwp/api/) fornece os elementos da interface do usuário para a criação de aplicativos da loja do Windows 8. x e fornece acesso aos recursos do sistema operacional. Assim como o .NET Framework, o Windows Runtime tem metadados que permitem que os compiladores C# e Visual Basic usem o Windows Runtime como eles usam as bibliotecas de classe .NET Framework. O .NET Framework torna mais fácil usar o Windows Runtime ocultando algumas diferenças:

  - Algumas diferenças nos padrões de programação entre o .NET Framework e o Windows Runtime, como o padrão para adicionar e remover manipuladores de eventos, estão ocultas. Basta usar o padrão de .NET Framework.

  - Algumas diferenças em tipos comumente usados (por exemplo, tipos primitivos e coleções) são ocultas. Você simplesmente usa o tipo de .NET Framework, conforme discutido em [diferenças que são visíveis no IDE](#DifferencesVisibleInIDE), mais adiante neste artigo.

Na maioria das vezes, .NET Framework suporte para o Windows Runtime é transparente. A próxima seção aborda algumas das diferenças aparentes entre o código gerenciado e o Windows Runtime.

<a name="AboutReferenceDocumentation"></a>

### <a name="the-net-framework-and-the-windows-runtime-reference-documentation"></a>A .NET Framework e a documentação de referência do Windows Runtime

O Windows Runtime e os conjuntos de documentação .NET Framework são separados. Se você pressionar F1 para exibir a ajuda em um tipo ou membro, a documentação de referência do conjunto apropriado será exibida. No entanto, se você navegar pela [referência de Windows Runtime](/uwp/api/) , poderá encontrar exemplos que parecem enigmático:

- Tópicos como a <xref:Windows.Foundation.Collections.IIterable%601> interface não têm sintaxe de declaração para Visual Basic ou C#. Em vez disso, uma observação aparece acima da seção de sintaxe (nesse caso, ".NET: essa interface aparece como System. Collections. Generic. IEnumerable \<T> "). Isso ocorre porque o .NET Framework e o Windows Runtime fornecem funcionalidade semelhante com interfaces diferentes. Além disso, há diferenças comportamentais: `IIterable` tem um `First` método em vez de um <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> método para retornar o enumerador. Em vez de forçá-lo a aprender uma maneira diferente de executar uma tarefa comum, o .NET Framework dá suporte ao Windows Runtime, fazendo com que seu código gerenciado pareça usar o tipo com o qual você está familiarizado. Você não verá a `IIterable` interface no IDE e, portanto, a única maneira de encontrá-la na documentação de referência do Windows Runtime é navegar diretamente pela documentação.

- A <xref:Windows.Web.Syndication.SyndicationFeed.%23ctor(System.String,System.String,Windows.Foundation.Uri)> documentação ilustra um problema bem relacionado: seus tipos de parâmetro parecem ser diferentes para idiomas diferentes. Para C# e Visual Basic, os tipos de parâmetro são <xref:System.String?displayProperty=nameWithType> e <xref:System.Uri?displayProperty=nameWithType> . Novamente, isso ocorre porque as .NET Framework têm seus próprios `String` `Uri` tipos e, para esses tipos comumente usados, não faz sentido forçar .NET Framework usuários a aprenderem uma maneira diferente de fazer as coisas. No IDE, a .NET Framework oculta os tipos de Windows Runtime correspondentes.

- Em alguns casos, como a <xref:Windows.UI.Xaml.GridLength> estrutura, o .NET Framework fornece um tipo com o mesmo nome, mas com mais funcionalidade. Por exemplo, um conjunto de tópicos de construtor e propriedade está associado a `GridLength` , mas eles têm blocos de sintaxe somente para Visual Basic e C# porque os membros estão disponíveis somente em código gerenciado. No Windows Runtime, as estruturas têm apenas campos. A estrutura de Windows Runtime requer uma classe auxiliar, <xref:Windows.UI.Xaml.GridLengthHelper> , para fornecer funcionalidade equivalente. Você não verá essa classe auxiliar no IDE quando estiver escrevendo código gerenciado.

- No IDE, os tipos de Windows Runtime parecem derivar de <xref:System.Object?displayProperty=nameWithType> . Eles parecem ter membros herdados do <xref:System.Object> , como <xref:System.Object.ToString%2A?displayProperty=nameWithType> . Esses membros operam da mesma forma que os tipos herdados <xref:System.Object> , e os tipos de Windows Runtime podem ser convertidos <xref:System.Object> . Essa funcionalidade faz parte do suporte que o .NET Framework fornece para o Windows Runtime. No entanto, se você exibir os tipos na documentação de referência do Windows Runtime, nenhum desses membros será exibido. A documentação para esses membros herdados aparentes é fornecida pela <xref:System.Object?displayProperty=nameWithType> documentação de referência.

<a name="DifferencesVisibleInIDE"></a>

### <a name="differences-that-are-visible-in-the-ide"></a>Diferenças visíveis no IDE

Em cenários de programação mais avançados, como usar um componente de Windows Runtime escrito em C# para fornecer a lógica do aplicativo para um aplicativo de armazenamento do Windows 8. x criado para Windows usando JavaScript, essas diferenças são aparentes no IDE, bem como na documentação. Quando o componente retorna um `IDictionary<int, string>` para JavaScript e você o examina no depurador do JavaScript, você verá os métodos de `IMap<int, string>` porque o JavaScript usa o tipo de Windows Runtime. Alguns tipos de coleção comumente usados que aparecem de forma diferente nos dois idiomas são mostrados na tabela a seguir:

|Tipo do Windows Runtime|Tipo de .NET Framework correspondente|
|--------------------------------------------------------------|---------------------------------------|
|`IIterable<T>`|`IEnumerable<T>`|
|`IIterator<T>`|`IEnumerator<T>`|
|`IVector<T>`|`IList<T>`|
|`IVectorView<T>`|`IReadOnlyList<T>`|
|`IMap<K, V>`|`IDictionary<TKey, TValue>`|
|`IMapView<K, V>`|`IReadOnlyDictionary<TKey, TValue>`|
|`IBindableIterable`|`IEnumerable`|
|`IBindableVector`|`IList`|
|`Windows.UI.Xaml.Data.INotifyPropertyChanged`|`System.ComponentModel.INotifyPropertyChanged`|
|`Windows.UI.Xaml.Data.PropertyChangedEventHandler`|`System.ComponentModel.PropertyChangedEventHandler`|
|`Windows.UI.Xaml.Data.PropertyChangedEventArgs`|`System.ComponentModel.PropertyChangedEventArgs`|

No Windows Runtime, `IMap<K, V>` e `IMapView<K, V>` são iterados usando `IKeyValuePair` . Quando você os transmite para código gerenciado, eles aparecem como `IDictionary<TKey, TValue>` e `IReadOnlyDictionary<TKey, TValue>`, então, naturalmente, você usa `System.Collections.Generic.KeyValuePair<TKey, TValue>` para enumerá-los.

A forma como as interfaces aparecem no código gerenciado afeta a forma como aparecem os tipos que implementam essas interfaces. Por exemplo, a classe `PropertySet` implementa `IMap<K, V>`, que aparece no código gerenciado como `IDictionary<TKey, TValue>`. `PropertySet` aparece como se ele tivesse implementado `IDictionary<TKey, TValue>` em vez de `IMap<K, V>`, por isso, no código gerenciado, ele parece ter um método `Add`, que se comporta como o método `Add` em dicionários do .NET Framework. Ela não parece ter um método `Insert`.

Para obter mais informações sobre como usar o .NET Framework para criar um componente de Windows Runtime e um passo a passos que mostra como usar esse componente com JavaScript, consulte [criando componentes Windows Runtime em C# e Visual Basic](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic).

### <a name="primitive-types"></a>Tipos primitivos

Para habilitar o uso natural do Windows Runtime no código gerenciado, .NET Framework tipos primitivos aparecem em vez de Windows Runtime tipos primitivos em seu código. No .NET Framework, os tipos primitivos como a estrutura `Int32`, têm muitas propriedades e métodos úteis, como o método `Int32.TryParse`. Por outro lado, tipos primitivos e estruturas no Windows Runtime têm apenas campos. Quando você usa primitivos em código gerenciado, eles parecem ser .NET Framework tipos e você pode usar as propriedades e métodos dos tipos de .NET Framework como faria normalmente. A lista a seguir fornece um resumo:

- Para os primitivos de Windows Runtime,,,,, `Int32` `Int64` `Single` `Double` `Boolean` `String` (uma coleção imutável de caracteres Unicode),,, `Enum` `UInt32` `UInt64` e `Guid` , use o tipo de mesmo nome no `System` namespace.

- Para `UInt8`, use `System.Byte`.

- Para `Char16`, use `System.Char`.

- Para a interface `IInspectable`, use `System.Object`.

- Para `HRESULT` , use uma estrutura com um `System.Int32` membro.

Assim como acontece com os tipos de interface, a única vez que você pode ver evidências dessa representação é quando seu projeto de .NET Framework é um componente de Windows Runtime que é usado por um aplicativo de armazenamento do Windows 8. x criado usando JavaScript.

Outros tipos de Windows Runtime básicos, comumente usados que aparecem no código gerenciado como seus equivalentes .NET Framework incluem a `Windows.Foundation.DateTime` estrutura, que aparece em código gerenciado como a <xref:System.DateTimeOffset?displayProperty=nameWithType> estrutura, e a `Windows.Foundation.TimeSpan` estrutura, que aparece como a <xref:System.TimeSpan?displayProperty=nameWithType> estrutura.

### <a name="other-differences"></a>Outras diferenças

Em alguns casos, o fato de que .NET Framework tipos aparecem em seu código em vez de Windows Runtime tipos requer ação de sua parte. Por exemplo, a <xref:Windows.Foundation.Uri?displayProperty=nameWithType> classe aparece como <xref:System.Uri?displayProperty=nameWithType> em .NET Framework código. <xref:System.Uri?displayProperty=nameWithType>permite um URI relativo, mas <xref:Windows.Foundation.Uri?displayProperty=nameWithType> requer um URI absoluto. Portanto, ao passar um URI para um método Windows Runtime, você deve garantir que ele seja absoluto. Consulte [Passando um URI para o Tempo de Execução do Windows](passing-a-uri-to-the-windows-runtime.md).

<a name="WindowsRuntimeComponents"></a>

## <a name="scenarios-for-developing-windows-runtime-components"></a>Cenários para o desenvolvimento de componentes Windows Runtime

Os cenários com suporte para componentes gerenciados Windows Runtime dependem dos seguintes princípios gerais:

- Windows Runtime componentes criados usando o .NET Framework não têm nenhuma diferença aparente de outras Runtimelibraries do Windows. Por exemplo, se você reimplementar um componente de Windows Runtime nativo usando código gerenciado, os dois componentes serão indistinguíveis de forma retroativa. O fato de que seu componente é escrito em código gerenciado é invisível para o código que o utiliza, mesmo que esse código seja código gerenciado. No entanto, internamente, o componente é um código gerenciado verdadeiro e é executado no Common Language Runtime (CLR).

- Os componentes podem conter tipos que implementam a lógica do aplicativo, os controles da interface do usuário da loja do Windows 8. x ou ambos.

  > [!NOTE]
  > É uma prática recomendada separar elementos da interface do usuário da lógica do aplicativo. Além disso, você não pode usar os controles de interface do usuário da loja do Windows 8. x em um aplicativo da loja do Windows 8. x criado para Windows usando JavaScript e HTML.

- Um componente pode ser um projeto dentro de uma solução do Visual Studio para um aplicativo da loja do Windows 8. x ou um componente reutilizável que você pode adicionar a várias soluções.

  > [!NOTE]
  > Se seu componente for usado somente com C# ou Visual Basic, não há motivo para torná-lo um componente Windows Runtime. Se você transformá-lo em uma biblioteca de classes .NET Framework comum, não precisa restringir sua superfície de API pública a tipos Windows Runtime.

- Você pode liberar versões de componentes reutilizáveis usando o <xref:Windows.Foundation.Metadata.VersionAttribute> atributo Windows Runtime para identificar quais tipos (e quais membros dentro de um tipo) foram adicionados em versões diferentes.

- Os tipos em seu componente podem derivar de tipos de Windows Runtime. Os controles podem derivar dos tipos de controle primitivos no <xref:Windows.UI.Xaml.Controls.Primitives> namespace ou de controles mais concluídos, como <xref:Windows.UI.Xaml.Controls.Button> .

  > [!IMPORTANT]
  > A partir do Windows 8 e do .NET Framework 4,5, todos os tipos públicos em um componente de Windows Runtime gerenciado devem ser lacrados. Um tipo em outro componente Windows Runtime não pode derivar deles. Se você quiser fornecer um comportamento polimórfico em seu componente, poderá criar uma interface e implementá-la nos tipos polimórficos.

- Todos os tipos de parâmetro e de retorno nos tipos públicos em seu componente devem ser Windows Runtime tipos (incluindo os tipos de Windows Runtime que seu componente define).

As seções a seguir fornecem exemplos de cenários comuns.

### <a name="application-logic-for-a-windows-8x-store-app-with-javascript"></a>Lógica do aplicativo para um aplicativo da loja do Windows 8. x com JavaScript

Ao desenvolver um aplicativo da loja do Windows 8. x para Windows usando JavaScript, você pode achar que algumas partes da lógica do aplicativo têm melhor desempenho no código gerenciado ou que são mais fáceis de desenvolver. O JavaScript não pode usar .NET Framework bibliotecas de classes diretamente, mas você pode tornar a biblioteca de classes a. Arquivo WinMD. Nesse cenário, o componente Windows Runtime é parte integrante do aplicativo, portanto, não faz sentido fornecer atributos de versão.

### <a name="reusable-windows-8x-store-ui-controls"></a>Controles de interface do usuário da loja do Windows 8. x reutilizáveis

Você pode empacotar um conjunto de controles de interface do usuário relacionados em um componente de Windows Runtime reutilizável. O componente pode ser comercializado por conta própria ou usado como um elemento nos aplicativos que você criar. Nesse cenário, faz sentido usar o <xref:Windows.Foundation.Metadata.VersionAttribute> atributo Windows Runtime para melhorar a compatibilidade.

### <a name="reusable-application-logic-from-existing-net-framework-apps"></a>Lógica de aplicativo reutilizável de aplicativos .NET Framework existentes

Você pode empacotar o código gerenciado de seus aplicativos de área de trabalho existentes como um componente Windows Runtime autônomo. Isso permite que você use o componente no Windows 8. x aplicativos da loja criados usando C++ ou JavaScript, bem como em aplicativos da loja do Windows 8. x criados usando C# ou Visual Basic. O controle de versão é uma opção se houver vários cenários de reutilização para o código.

## <a name="related-topics"></a>Tópicos relacionados

|Title|Descrição|
|-----------|-----------------|
|[Visão geral dos aplicativos .NET para Windows Store](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))|Descreve os tipos de .NET Framework e os membros que você pode usar para criar aplicativos de armazenamento do Windows 8. x e do Windows RuntimeComponents. (No centro de desenvolvimento do Windows.)|
|[Roteiro para aplicativos da Windows Store usando C# ou Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10))|Fornece recursos importantes para ajudá-lo a começar a desenvolver aplicativos da loja do Windows 8. x usando C# ou Visual Basic, incluindo muitos tópicos de início rápido, diretrizes e práticas recomendadas. (No centro de desenvolvimento do Windows.)|
|[Como o TOS (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/br229566(v=win.10))|Fornece recursos importantes para ajudá-lo a começar a desenvolver aplicativos da loja do Windows 8. x usando C# ou Visual Basic, incluindo muitos tópicos de início rápido, diretrizes e práticas recomendadas. (No centro de desenvolvimento do Windows.)|
|[Criando Componentes do Windows Runtime no C# e no Visual Basic](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic)|Descreve como criar um componente Windows Runtime usando o .NET Framework, explica como usá-lo como parte de um aplicativo da loja do Windows 8. x criado para Windows usando JavaScript e descreve como depurar a combinação com o Visual Studio. (No centro de desenvolvimento do Windows.)|
|[Referência do Windows Runtime](/uwp/api/)|A documentação de referência para o Windows Runtime. (No centro de desenvolvimento do Windows.)|
|[Passando um URI para o Windows Runtime](passing-a-uri-to-the-windows-runtime.md)|Descreve um problema que pode surgir quando você passa um URI do código gerenciado para o Windows Runtime e como evitá-lo.|
