---
title: Definir recursos XAML
description: Saiba mais sobre os recursos XAML no WPF para .NET Core. Entenda os tipos de recursos XAML e saiba como definir recursos XAML.
author: adegeo
ms.author: adegeo
ms.date: 08/21/2019
ms.openlocfilehash: f8eaf3fd931aa6804b0b9a9c19c6bcc042678ebf
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325711"
---
# <a name="overview-of-xaml-resources"></a>Visão geral dos recursos XAML

Um recurso é um objeto que pode ser reutilizado em locais diferentes em seu aplicativo. Exemplos de recursos incluem pincéis e estilos. Esta visão geral descreve como usar os recursos no Extensible Application Markup Language (XAML). Você também pode criar e acessar recursos usando código.

> [!NOTE]
> Os recursos XAML descritos neste artigo são diferentes dos *recursos de aplicativo* que geralmente são arquivos adicionados a um aplicativo, como conteúdo, dados ou arquivos inseridos.

<!-- TODO: File redirect from docs\framework\wpf\advanced\xaml-resources.md -->

[!INCLUDE [desktop guide under construction](../../../includes/desktop-guide-preview-note.md)]

## <a name="using-resources-in-xaml"></a>Usando recursos em XAML

O exemplo a seguir define um <xref:System.Windows.Media.SolidColorBrush> como um recurso no elemento raiz de uma página. O exemplo faz referência ao recurso e o usa para definir as propriedades de vários elementos filho, incluindo um <xref:System.Windows.Shapes.Ellipse> , um <xref:System.Windows.Controls.TextBlock> e um <xref:System.Windows.Controls.Button> .

[!code-xaml[FEResourceSH_snip#XAML](~/samples/snippets/csharp/VS_Snippets_Wpf/FEResourceSH_snip/CS/page1.xaml#xaml)]

Cada elemento de nível de estrutura ( <xref:System.Windows.FrameworkElement> ou <xref:System.Windows.FrameworkContentElement> ) tem uma <xref:System.Windows.FrameworkElement.Resources%2A> propriedade, que é um <xref:System.Windows.ResourceDictionary> tipo que contém recursos definidos. Você pode definir recursos em qualquer elemento, como um <xref:System.Windows.Controls.Button> . No entanto, os recursos são definidos com mais frequência no elemento raiz, que está <xref:System.Windows.Controls.Page> no exemplo.

Cada recurso em um dicionário de recursos deve ter uma chave exclusiva. Ao definir recursos na marcação, você atribui a chave exclusiva por meio da [diretiva x:Key](../xaml-services/xkey-directive.md). Normalmente, a chave é uma cadeia de caracteres; no entanto, você pode também a definir com outros tipos de objeto usando as extensões de marcação apropriadas. Chaves de não cadeia de caracteres para recursos são usadas por determinadas áreas de recursos no WPF, especialmente para estilos, recursos de componente e estilo de dados.

Você pode usar um recurso definido com a sintaxe de extensão de marcação de recurso que especifica o nome da chave do recurso. Por exemplo, use o recurso como o valor de uma propriedade em outro elemento.

[!code-xaml[FEResourceSH_snip#KeyNameUsage](~/samples/snippets/csharp/VS_Snippets_Wpf/FEResourceSH_snip/CS/page2.xaml#keynameusage)]

No exemplo anterior, quando o carregador XAML processa o valor da `{StaticResource MyBrush}` <xref:System.Windows.Controls.Control.Background%2A> Propriedade on <xref:System.Windows.Controls.Button> , a lógica de pesquisa de recurso primeiro verifica o dicionário de recursos para o <xref:System.Windows.Controls.Button> elemento. Se <xref:System.Windows.Controls.Button> não tiver uma definição da chave de recurso `MyBrush` (nesse exemplo, ela não é; sua coleção de recursos está vazia), a pesquisa em seguida verifica o elemento pai de <xref:System.Windows.Controls.Button> , que é <xref:System.Windows.Controls.Page> . Se você definir um recurso no <xref:System.Windows.Controls.Page> elemento raiz, todos os elementos na árvore lógica do <xref:System.Windows.Controls.Page> poderão acessá-lo. E você pode reutilizar o mesmo recurso para definir o valor de qualquer propriedade que aceite o mesmo tipo que o recurso representa. No exemplo anterior, o mesmo `MyBrush` recurso define duas propriedades diferentes: o <xref:System.Windows.Controls.Control.Background%2A> de a <xref:System.Windows.Controls.Button> e o de a <xref:System.Windows.Shapes.Shape.Fill%2A> <xref:System.Windows.Shapes.Rectangle> .

## <a name="static-and-dynamic-resources"></a>Recursos estáticos e dinâmicos

Um recurso pode ser referenciado como estático ou dinâmico. As referências são criadas usando a [extensão de marcação StaticResource](../../framework/wpf/advanced/staticresource-markup-extension.md) ou a [extensão de marcação DynamicResource](../../framework/wpf/advanced/dynamicresource-markup-extension.md). Uma extensão de marcação é um recurso XAML que permite especificar uma referência de objeto fazendo com que a extensão de marcação processe a cadeia de caracteres de atributo e retorne o objeto para um carregador XAML. Para obter mais informações sobre o comportamento de extensão de marcação, consulte [Markup Extensions e WPF XAML](../../framework/wpf/advanced/markup-extensions-and-wpf-xaml.md).

Quando você usa uma extensão de marcação, normalmente fornece um ou mais parâmetros no formulário de cadeia de caracteres que são processados por essa extensão de marcação específica. A [extensão de marcação StaticResource](../../framework/wpf/advanced/staticresource-markup-extension.md) processa uma chave procurando o valor dessa chave em todos os dicionários de recursos disponíveis. O processamento ocorre durante o carregamento, que é quando o processo de carregamento precisa atribuir o valor da propriedade. Em vez disso, a [extensão de marcação DynamicResource](../../framework/wpf/advanced/dynamicresource-markup-extension.md) processa uma chave criando uma expressão, e essa expressão permanece não avaliada até que o aplicativo seja executado, no momento em que a expressão é avaliada e fornece um valor.

Quando você referencia um recurso, as seguintes considerações podem influenciar se uma referência de recurso estático ou dinâmico será usada:

- Ao determinar o design geral de como você cria os recursos para seu aplicativo (por página, no aplicativo, em XAML flexível ou em um assembly somente de recursos), considere o seguinte:

- A funcionalidade do aplicativo. Está atualizando os recursos em tempo real de seus requisitos de aplicativo?
- O respectivo comportamento de pesquisa desse tipo de referência de recurso.
- A propriedade ou o tipo de recurso específico e o comportamento nativo desses tipos.

## <a name="static-resources"></a>Recursos estáticos

As referências de recursos estáticos funcionam melhor nas seguintes circunstâncias:

- Seu design de aplicativo concentra a maioria de seus recursos em dicionários de recursos em nível de página ou aplicativo. As referências a recursos estáticos não são reavaliadas com base em comportamentos de tempo de execução, como recarregar uma página. Portanto, pode haver algum benefício de desempenho para evitar um grande número de referências de recursos dinâmicos quando elas não são necessárias com base no seu recurso e no design de aplicativos.

- Você está definindo o valor de uma propriedade que não está em um <xref:System.Windows.DependencyObject> ou um <xref:System.Windows.Freezable> .

- Você está criando um dicionário de recursos que será compilado em uma DLL e empacotado como parte do aplicativo ou compartilhado entre aplicativos.

- Você está criando um tema para um controle personalizado e está definindo recursos que são usados dentro dos temas. Para esse caso, você normalmente não deseja o comportamento de pesquisa de referência de recurso dinâmico; em vez disso, você deseja o comportamento de referência de recurso estático para que a pesquisa seja previsível e independente para o tema. Com uma referência de recurso dinâmico, até mesmo uma referência dentro de um tema é deixada não avaliada até o tempo de execução. e há uma chance de que, quando o tema for aplicado, algum elemento local redefinirá uma chave que seu tema está tentando referenciar, e o elemento local cairá antes do próprio tema na pesquisa. Se isso acontecer, seu tema não se comportará conforme o esperado.

- Você está usando recursos para definir um grande número de propriedades de dependência. As propriedades de dependência têm o cache de valor efetivo como habilitado pelo sistema de propriedades, portanto, se você fornecer um valor para uma propriedade de dependência que possa ser avaliada no tempo de carregamento, a propriedade de dependência não precisará verificar se há uma expressão reavaliada e retornar o último valor efetivo. Essa técnica pode ser uma vantagem de desempenho.

- Você deseja alterar o recurso subjacente para todos os consumidores ou deseja manter instâncias graváveis separadas para cada consumidor usando o [atributo x:Shared](../xaml-services/xshared-attribute.md).

### <a name="static-resource-lookup-behavior"></a>Comportamento de pesquisa de recursos estáticos

O seguinte descreve o processo de pesquisa que ocorre automaticamente quando um recurso estático é referenciado por uma propriedade ou um elemento:

01. O processo de pesquisa verifica a chave solicitada no dicionário de recursos definido pelo elemento que define a propriedade.

01. O processo de pesquisa então percorre a árvore lógica para cima até o elemento pai e seu dicionário de recursos. Esse processo continua até que o elemento raiz seja atingido.

01. Os recursos do aplicativo são verificados. Os recursos de aplicativo são aqueles no dicionário de recursos que é definido pelo <xref:System.Windows.Application> objeto para seu aplicativo do WPF.

As referências de recursos estáticos em um dicionário de recursos devem referenciar um recurso que já foi definido lexicalmente antes da referência ao recurso. As referências de encaminhamento não podem ser resolvidas por uma referência de recursos estáticos. Por esse motivo, crie sua estrutura de dicionário de recursos, de modo que os recursos sejam definidos no início de cada dicionário de recursos ou próximo dele.

A pesquisa de recursos estáticos pode se estender a temas ou a recursos do sistema, mas essa pesquisa só tem suporte porque o carregador XAML adia a solicitação. O adiamento é necessário para que o tema de tempo de execução no momento em que a página é carregada se aplique corretamente ao aplicativo. No entanto, as referências a recursos estáticos para chaves que são conhecidas apenas em temas ou como recursos do sistema não são recomendadas, pois essas referências não são reavaliadas se o tema é alterado pelo usuário em tempo real. Uma referência de recurso dinâmico é mais confiável quando você solicita recursos do sistema ou de tema. A exceção é quando um próprio elemento de tema solicita outro recurso. Essas referências devem ser referências de recursos estáticos, pelas razões mencionadas anteriormente.

O comportamento da exceção se uma referência de recurso estático não for encontrada varia. Se o recurso foi adiado, a exceção ocorre em runtime. Se o recurso não foi adiado, a exceção ocorre em tempo de carregamento.

## <a name="dynamic-resources"></a>Recursos dinâmicos

Os recursos dinâmicos funcionam melhor quando:

- O valor do recurso, incluindo recursos do sistema ou recursos que, de outra forma, o usuário configurável, depende de condições que não são conhecidas até o tempo de execução. Por exemplo, você pode criar valores setter que se referem às propriedades do sistema como expostas por <xref:System.Windows.SystemColors> , <xref:System.Windows.SystemFonts> ou <xref:System.Windows.SystemParameters> . Esses valores são realmente dinâmicos, pois, basicamente, são obtidos do ambiente em runtime do usuário e do sistema operacional. Você também pode ter temas em nível do aplicativo que podem ser alterados, em que o acesso aos recursos em nível de página também deve captar a alteração.

- Você está criando ou referenciando estilos de tema para um controle personalizado.

- Você pretende ajustar o conteúdo de um <xref:System.Windows.ResourceDictionary> durante um tempo de vida do aplicativo.

- Você tem uma estrutura de recursos complicada que tem interdependências, em que uma referência de encaminhamento pode ser necessária. As referências de recursos estáticos não dão suporte a referências de encaminhamento, mas as referências a recursos dinâmicos dão suporte a elas porque o recurso não precisa ser avaliado até o tempo de execução e as referências de encaminhamento não são, portanto, um conceito relevante.

- Você está fazendo referência a um recurso que é grande da perspectiva de um conjunto de trabalho ou de compilação, e o recurso pode não ser usado imediatamente quando a página é carregada. As referências a recursos estáticos sempre são carregadas do XAML quando a página é carregada. No entanto, uma referência de recurso dinâmico não é carregada até que seja usada.

- Você está criando um estilo em que os valores setter podem vir de outros valores que são influenciados por temas ou outras configurações de usuário.

- Você está aplicando recursos a elementos que podem ser repais na árvore lógica durante o tempo de vida do aplicativo. Potencialmente, a alteração do pai também altera o escopo de pesquisa de recursos; portanto, se você desejar que o recurso de um elemento reassociado seja reavaliado com base no novo escopo, sempre use uma referência de recursos dinâmicos.

### <a name="dynamic-resource-lookup-behavior"></a>Comportamento de pesquisa de recursos dinâmicos

O comportamento de pesquisa de recurso para uma referência de recurso dinâmico paraleliza o comportamento de pesquisa em seu código se você chamar <xref:System.Windows.FrameworkElement.FindResource%2A> ou <xref:System.Windows.FrameworkElement.SetResourceReference%2A> :

01. A pesquisa verifica a chave solicitada dentro do dicionário de recursos definido pelo elemento que define a propriedade:

    - Se o elemento definir uma <xref:System.Windows.FrameworkElement.Style%2A> propriedade, o <xref:System.Windows.FrameworkElement.Style?displayProperty=fullName> do elemento terá seu <xref:System.Windows.Style.Resources> dicionário marcado.

    - Se o elemento definir uma <xref:System.Windows.Controls.Control.Template%2A> propriedade, o <xref:System.Windows.FrameworkTemplate.Resources?displayProperty=fullName> dicionário do elemento será verificado.

01. A pesquisa percorre a árvore lógica para cima até o elemento pai e seu dicionário de recursos. Esse processo continua até que o elemento raiz seja atingido.

01. Os recursos do aplicativo são verificados. Os recursos de aplicativo são aqueles no dicionário de recursos que são definidos pelo <xref:System.Windows.Application> objeto para seu aplicativo do WPF.

01. O dicionário de recursos do tema é verificado para o tema ativo no momento. Se o tema for alterado em runtime, o valor será reavaliado.

01. Os recursos do sistema são verificados.

O comportamento de exceção (se houver) varia:

- Se um recurso foi solicitado por uma <xref:System.Windows.FrameworkElement.FindResource%2A> chamada e não foi encontrado, uma exceção será lançada.

- Se um recurso foi solicitado por uma <xref:System.Windows.FrameworkElement.TryFindResource%2A> chamada e não foi encontrado, nenhuma exceção será lançada e o valor retornado será `null` . Se a propriedade que está sendo definida não aceitar `null` , ainda será possível que uma exceção mais profunda seja gerada, dependendo da propriedade individual que está sendo definida.

- Se um recurso foi solicitado por uma referência de recurso dinâmico em XAML e não foi encontrado, o comportamento dependerá do sistema de propriedades geral. O comportamento geral é como se nenhuma operação de configuração de propriedade ocorreu no nível em que o recurso existe. Por exemplo, se você tentar definir o plano de fundo em um elemento de botão individual usando um recurso que não pôde ser avaliado, nenhum valor definido resultará, mas o valor efetivo ainda poderá vir de outros participantes no sistema de propriedades e precedência de valor. Por exemplo, o valor de plano de fundo ainda pode vir de um estilo de botão definido localmente ou do estilo de tema. Para propriedades que não são definidas por estilos de tema, o valor efetivo após uma avaliação de recurso com falha pode vir do valor padrão nos metadados de propriedade.

### <a name="restrictions"></a>Restrições

As referências de recursos dinâmicos têm algumas restrições importantes. Pelo menos uma das seguintes condições deve ser verdadeira:

- A propriedade que está sendo definida deve ser uma propriedade em um <xref:System.Windows.FrameworkElement> ou <xref:System.Windows.FrameworkContentElement> . Essa propriedade deve ser apoiada por um <xref:System.Windows.DependencyProperty> .

- A referência é para um valor dentro de um `StyleSetter` .

- A propriedade que está sendo definida deve ser uma propriedade em um <xref:System.Windows.Freezable> que é fornecida como um valor de <xref:System.Windows.FrameworkElement> uma <xref:System.Windows.FrameworkContentElement> propriedade ou, ou um <xref:System.Windows.Setter> valor.

Como a propriedade que está sendo definida deve ser uma <xref:System.Windows.DependencyProperty> <xref:System.Windows.Freezable> propriedade ou, a maioria das alterações de propriedade pode ser propagada para a interface do usuário porque uma alteração de propriedade (o valor de recurso dinâmico alterado) é confirmada pelo sistema de propriedades. A maioria dos controles inclui lógica que forçará outro layout de um controle se uma <xref:System.Windows.DependencyProperty> alteração e essa propriedade puderem afetar o layout. No entanto, nem todas as propriedades que têm uma [extensão de marcação DynamicResource](../../framework/wpf/advanced/dynamicresource-markup-extension.md) como seu valor têm a garantia de fornecer atualizações em tempo real na interface do usuário. Essa funcionalidade ainda pode variar dependendo da propriedade, bem como dependendo do tipo que possui a propriedade, ou até mesmo da estrutura lógica do seu aplicativo.

## <a name="styles-datatemplates-and-implicit-keys"></a>Estilos, DataTemplates e chaves implícitas

Embora todos os itens em um <xref:System.Windows.ResourceDictionary> devam ter uma chave, isso não significa que todos os recursos devem ter um explícito `x:Key` . Vários tipos de objeto dão suporte a uma chave implícita quando definidos como um recurso, em que o valor da chave é vinculado ao valor de outra propriedade. Esse tipo de chave é conhecido como uma chave implícita, enquanto que um `x:Key` atributo é uma chave explícita. Você pode substituir uma chave implícita especificando uma chave explícita.

Um cenário importante para os recursos é quando você define um <xref:System.Windows.Style> . Na verdade, um <xref:System.Windows.Style> é quase sempre definido como uma entrada em um dicionário de recursos, porque os estilos são inerentemente destinados à reutilização. Para obter mais informações sobre estilos, consulte [estilização e modelagem](styles-templates-overview.md).

Os estilos de controles podem ser criados com uma chave implícita e referenciados com ela. Os estilos de tema que definem a aparência padrão de um controle se baseiam nessa chave implícita. Do ponto de vista da solicitação, a chave implícita é o <xref:System.Type> do próprio controle. Do ponto de vista da definição dos recursos, a chave implícita é o <xref:System.Windows.Style.TargetType%2A> do estilo. Portanto, se você estiver criando temas para controles personalizados ou criando estilos que interagem com estilos de tema existentes, não será necessário especificar uma [diretiva x:Key](../xaml-services/xkey-directive.md) para isso <xref:System.Windows.Style> . Além disso, você desejar usar os estilos de tema, não precisará especificar nenhum estilo. Por exemplo, a definição de estilo a seguir funciona, embora o <xref:System.Windows.Style> recurso não pareça ter uma chave:

[!code-xaml[FEResourceSH_snip#ImplicitStyle](~/samples/snippets/csharp/VS_Snippets_Wpf/FEResourceSH_snip/CS/page2.xaml#implicitstyle)]

Esse estilo realmente tem uma chave: a chave implícita `typeof(System.Windows.Controls.Button)` . Na marcação, você pode especificar um <xref:System.Windows.Style.TargetType%2A> diretamente como o nome do tipo (ou opcionalmente, você pode usar [{x:Type...}](../xaml-services/xtype-markup-extension.md) para retornar um <xref:System.Type> .

Por meio dos mecanismos de estilo de tema padrão usados pelo WPF, esse estilo é aplicado como o estilo de tempo de execução de a <xref:System.Windows.Controls.Button> na página, mesmo que a <xref:System.Windows.Controls.Button> si não tente especificar sua <xref:System.Windows.FrameworkElement.Style%2A> propriedade ou uma referência de recurso específica ao estilo. Seu estilo definido na página é encontrado anteriormente na sequência de pesquisa do que o estilo de dicionário do tema, usando a mesma chave que o estilo do dicionário do tema. Você poderia apenas especificar `<Button>Hello</Button>` qualquer lugar na página, e o estilo que você definiu a <xref:System.Windows.Style.TargetType%2A> `Button` ser aplicado a esse botão. Se desejar, você ainda poderá chavear explicitamente o estilo com o mesmo valor de tipo <xref:System.Windows.Style.TargetType%2A> para fins de clareza na sua marcação, mas isso é opcional.

As chaves implícitas para estilos não se aplicam a um controle se <xref:System.Windows.FrameworkElement.OverridesDefaultStyle%2A> for `true` . (Observe também que <xref:System.Windows.FrameworkElement.OverridesDefaultStyle%2A> pode ser definido como parte do comportamento nativo da classe Control, em vez de explicitamente em uma instância do controle.) Além disso, para dar suporte a chaves implícitas para cenários de classe derivada, o controle deve substituir <xref:System.Windows.FrameworkElement.DefaultStyleKey%2A> (todos os controles existentes fornecidos como parte do WPF incluem essa substituição). Para obter mais informações sobre estilos, temas e design de controle, consulte [diretrizes para criar controles stylable](../../framework/wpf/controls/guidelines-for-designing-stylable-controls.md).

<xref:System.Windows.DataTemplate>também tem uma chave implícita. A chave implícita para um <xref:System.Windows.DataTemplate> é o <xref:System.Windows.DataTemplate.DataType%2A> valor da propriedade. <xref:System.Windows.DataTemplate.DataType%2A>também pode ser especificado como o nome do tipo em vez de explicitamente usar [{x:Type...}](../xaml-services/xtype-markup-extension.md). Para obter detalhes, consulte [visão geral de modelagem de dados](../../framework/wpf/data/data-templating-overview.md).

## <a name="see-also"></a>Veja também

- <xref:System.Windows.ResourceDictionary>
- [Recursos do aplicativo](../../framework/wpf/advanced/optimizing-performance-application-resources.md)
- [Recursos e código](../../framework/wpf/advanced/resources-and-code.md)
- [Definir e referenciar um recurso](../../framework/wpf/advanced/how-to-define-and-reference-a-resource.md)
- [Visão geral do gerenciamento de aplicativos](../../framework/wpf/app-development/application-management-overview.md)
- [extensão de marcação x:Type](../xaml-services/xtype-markup-extension.md)
- [Extensão de marcação StaticResource](../../framework/wpf/advanced/staticresource-markup-extension.md)
- [Extensão de marcação DynamicResource](../../framework/wpf/advanced/dynamicresource-markup-extension.md)
