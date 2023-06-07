---
title: Visão geral da vinculação de dados
description: Saiba mais sobre as diferentes fontes de dados que você pode adicionar ao seu projeto no Windows Presentation Foundation para .NET Core. As fontes de dados podem ser associadas a elementos XAML para criar aplicativos dinâmicos.
author: adegeo
ms.date: 09/19/2019
ms.author: adegeo
dev_langs:
- csharp
- vb
ms.openlocfilehash: 829c93e97990b87e6e568614236de9708ef080d9
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325756"
---
# <a name="data-binding-overview-in-wpf"></a>Visão geral da ligação de dados no WPF

A vinculação de dados no Windows Presentation Foundation (WPF) fornece uma maneira simples e consistente para que os aplicativos apresentem e interajam com os dados. Os elementos podem ser associados a dados de uma variedade de fontes de dados na forma de objetos .NET e XML. Qualquer como <xref:System.Windows.Controls.ContentControl> <xref:System.Windows.Controls.Button> e qualquer <xref:System.Windows.Controls.ItemsControl> , como <xref:System.Windows.Controls.ListBox> e <xref:System.Windows.Controls.ListView> , têm funcionalidade interna para habilitar o estilo flexível de itens de dados individuais ou coleções de itens de dados. É possível gerar exibições com classificação, filtragem e agrupamento dos dados.

A funcionalidade de vinculação de dados no WPF tem várias vantagens em relação aos modelos tradicionais, incluindo suporte inerente à vinculação de dados por uma ampla variedade de propriedades, representação flexível da interface do usuário de dados e separação clara da lógica de negócios da interface do usuário.

Este artigo aborda primeiro os conceitos fundamentais para a ligação de dados do WPF e, em seguida, aborda o uso da <xref:System.Windows.Data.Binding> classe e de outros recursos de vinculação de dados.

[!INCLUDE [desktop guide under construction](../../../includes/desktop-guide-preview-note.md)]

## <a name="what-is-data-binding"></a>O que é a associação de dados?

A vinculação de dados é o processo que estabelece uma conexão entre a interface do usuário do aplicativo e os dados exibidos. Se a associação possui configurações corretas e os dados fornecem notificações adequadas, quando os dados mudam de valor, os elementos que são associados a dados refletem as mudanças automaticamente. A vinculação de dados também poderá significar que, se houver uma alteração de uma representação externa dos dados em um elemento, os dados subjacentes poderão ser atualizados automaticamente para refletir essa alteração. Por exemplo, se o usuário editar o valor em um `TextBox` elemento, o valor dos dados subjacentes será atualizado automaticamente para refletir essa alteração.

Um uso típico da vinculação de dados é posicionar dados de configuração de servidor ou local em formulários ou outros controles de interface do usuário. No WPF, esse conceito é expandido para incluir a associação de uma ampla variedade de propriedades a uma variedade de fontes de dados. No WPF, as propriedades de dependência de elementos podem ser associadas a objetos .NET (incluindo objetos ADO.NET ou objetos associados a serviços Web e propriedades da Web) e dados XML.

Para obter um exemplo de vinculação de dados, confira a interface do usuário do aplicativo a seguir da [demonstração de vinculação de dados][data-binding-demo], que exibe uma lista de itens de leilão.

![Captura de tela de exemplo de vinculação de dados](./media/data-binding-overview/demo.png "DataBinding_DataBindingDemo")

O aplicativo demonstra os seguintes recursos de vinculação de dados:

- O conteúdo da caixa de listagem está associado a uma coleção de objetos *AuctionItem* . Um objeto *AuctionItem* tem propriedades como *Descrição*, *StartPrice*, *StartDate*, *Category*, *SpecialFeatures*e assim por diante.

- Os dados (objetos*AuctionItem* ) exibidos no `ListBox` são modelados de forma que a descrição e o preço atual sejam mostrados para cada item. O modelo é criado usando um <xref:System.Windows.DataTemplate> . Além disso, a aparência de cada item depende do valor de *SpecialFeatures* do *AuctionItem* sendo exibido. Se o valor *SpecialFeatures* do *AuctionItem* for *Color*, o item terá uma borda azul. Se o valor for *Highlight*, o item terá uma borda laranja e uma estrela. A seção [Modelos de dados](#data-templating) fornece informações sobre modelagem de dados.

- O usuário pode agrupar, filtrar ou classificar os dados usando o `CheckBoxes` fornecido. Na imagem acima, a **categoria agrupar por** e **classificar por categoria e data** `CheckBoxes` são selecionados. Você talvez tenha observado que os dados estão agrupados com base na categoria do produto e que o nome da categoria está em ordem alfabética. É difícil perceber isto na imagem, mas os itens também são classificados por data de início dentro de cada categoria. A classificação é feita usando uma *exibição de coleção*. A seção [associando a coleções](#binding-to-collections) discute exibições de coleção.

- Quando o usuário seleciona um item, <xref:System.Windows.Controls.ContentControl> exibe os detalhes do item selecionado. Essa experiência é chamada de *cenário de detalhes mestre*. A seção [cenário de detalhes mestre](#master-detail-binding-scenario) fornece informações sobre esse tipo de associação.

- O tipo da propriedade *StartDate* é <xref:System.DateTime> , que retorna uma data que inclui a hora para o milissegundo. Nesse aplicativo, um conversor personalizado foi usado para que uma cadeia de caracteres de data mais curta seja exibida. A seção [conversão de dados](#data-conversion) fornece informações sobre conversores.

Quando o usuário seleciona o botão *Adicionar produto* , o formulário a seguir é exibido.

![Página Adicionar Listagem de produtos](./media/data-binding-overview/demo-addproductlisting.png "DataBinding_Demo_AddProductListing")

O usuário pode editar os campos no formulário, Visualizar a listagem de produtos usando os painéis de visualização curtos ou detalhados e selecionar `Submit` para adicionar a nova listagem de produtos. Todas as configurações existentes de agrupamento, filtragem e classificação serão aplicadas à nova entrada. Neste caso em particular, o item inserido na imagem acima será exibido como o segundo item dentro da categoria *Computador*.

Não mostrada nesta imagem é a lógica de validação fornecida na *data de início* <xref:System.Windows.Controls.TextBox> . Se o usuário inserir uma data inválida (formatação inválida ou uma data anterior), o usuário será notificado com um <xref:System.Windows.Controls.ToolTip> e um ponto de exclamação vermelho ao lado do <xref:System.Windows.Controls.TextBox> . A seção [Validação de dados](#data-validation) discute como criar lógica de validação.

Antes de entrar nos diferentes recursos de ligação de dados descritos acima, discutiremos primeiro os conceitos fundamentais que são essenciais para entender a ligação de dados do WPF.

## <a name="basic-data-binding-concepts"></a>Conceitos básicos de vinculação de dados

Independentemente do elemento que você está associando e da natureza de sua fonte de dados, cada associação sempre segue o modelo ilustrado pela figura a seguir.

![Diagrama que mostra o modelo básico de ligação de dados.](./media/data-binding-overview/basic-data-binding-diagram.png)

Como mostra a figura, a vinculação de dados é essencialmente a ponte entre seu destino de associação e sua fonte de associação. A figura demonstra os seguintes conceitos fundamentais de ligação de dados do WPF:

- Normalmente, cada associação tem quatro componentes:

  - Um objeto de destino de associação.
  - Uma propriedade de destino.
  - Uma origem de associação.
  - Um caminho para o valor na origem da associação a ser usado.
  
  > Por exemplo, se você deseja associar o conteúdo de um `TextBox` à `Employee.Name` propriedade, o objeto de destino é o `TextBox` , a propriedade de destino é a <xref:System.Windows.Controls.TextBox.Text%2A> propriedade, o valor a ser usado é *nome*e o objeto de origem é o objeto *Employee* .

- A propriedade de destino deve ser uma propriedade de dependência. A maioria das <xref:System.Windows.UIElement> Propriedades são propriedades de dependência e a maioria das propriedades de dependência, exceto somente leitura, dá suporte à associação de dados por padrão. (Somente tipos derivados de <xref:System.Windows.DependencyObject> podem definir propriedades de dependência; e todos os <xref:System.Windows.UIElement> tipos derivam de `DependencyObject` .)

- Embora não seja mostrado na figura, deve-se observar que o objeto de origem da associação não está restrito a ser um objeto .NET personalizado. A ligação de dados do WPF dá suporte a dados na forma de objetos .NET e XML. Para fornecer alguns exemplos, sua fonte de associação pode ser um <xref:System.Windows.UIElement> , qualquer objeto de lista, um ADO.net ou um objeto de serviços Web, ou um XmlNode que contém os dados XML. Para obter mais informações, consulte [visão geral de fontes de associação](../../framework/wpf/data/binding-sources-overview.md).

É importante lembrar que quando você estiver estabelecendo uma associação, você está associando um destino de associação *a* uma fonte de associação. Por exemplo, se você estiver exibindo alguns dados XML subjacentes em um <xref:System.Windows.Controls.ListBox> usando Associação de dados, você está ligando seus `ListBox` para os dados XML.

Para estabelecer uma associação, use o <xref:System.Windows.Data.Binding> objeto. O restante deste artigo aborda muitos dos conceitos associados a e algumas das propriedades e do uso do `Binding` objeto.

### <a name="direction-of-the-data-flow"></a>Direção do fluxo de dados

Conforme indicado na seta na figura anterior, o fluxo de dados de uma associação pode ir do destino de associação para a fonte de associação (por exemplo, o valor de origem é alterado quando um usuário edita o valor de a `TextBox` ) e/ou da origem da Associação para o destino de associação (por exemplo, seu `TextBox` conteúdo é atualizado com alterações na origem da associação) se a origem da Associação fornecer as notificações adequadas.

Talvez você queira que seu aplicativo permita que os usuários alterem os dados e os propaguem de volta para o objeto de origem. Ou talvez você não queira permitir que os usuários atualizem os dados de origem. Você pode controlar o fluxo de dados definindo o <xref:System.Windows.Data.Binding.Mode?displayProperty=nameWithType> .

Esta figura ilustra os diferentes tipos de fluxo de dados:

![Fluxo de dados de vinculação de dados](./media/data-binding-overview/databinding-dataflow.png "DataBinding_DataFlow")

- <xref:System.Windows.Data.BindingMode.OneWay>a associação faz com que as alterações na propriedade de origem atualizem automaticamente a propriedade de destino, mas as alterações na propriedade de destino não são propagadas de volta para a propriedade de origem. Esse tipo de associação será apropriado se o controle associado for somente leitura de forma implícita. Por exemplo, você pode associar a uma fonte, como uma cotação de ações, ou talvez a sua propriedade de destino não tenha nenhuma interface de controle fornecida para fazer alterações, como uma cor de plano de fundo associada a dados de uma tabela. Se não houver necessidade de monitorar as alterações da propriedade de destino, o uso do modo de associação <xref:System.Windows.Data.BindingMode.OneWay> evitará a sobrecarga do modo de associação <xref:System.Windows.Data.BindingMode.TwoWay>.

- <xref:System.Windows.Data.BindingMode.TwoWay>a associação faz com que as alterações sejam feitas na propriedade Source ou na propriedade de destino para atualizar automaticamente a outra. Esse tipo de associação é apropriado para formulários editáveis ou outros cenários de interface do usuário totalmente interativas. A maioria das propriedades assume a associação como padrão <xref:System.Windows.Data.BindingMode.OneWay> , mas algumas propriedades de dependência (normalmente Propriedades de controles editáveis pelo usuário, como a <xref:System.Windows.Controls.TextBox.Text?displayProperty=nameWithType> caixa de [seleção e. ischeckd](xref:System.Windows.Controls.Primitives.ToggleButton.IsChecked) default para <xref:System.Windows.Data.BindingMode.TwoWay> Binding. Uma maneira programática de determinar se uma propriedade de dependência associa uma ou duas vias por padrão é obter os metadados de propriedade com <xref:System.Windows.DependencyProperty.GetMetadata%2A?displayProperty=nameWithType> e, em seguida, verificar o valor booliano da <xref:System.Windows.FrameworkPropertyMetadata.BindsTwoWayByDefault%2A?displayProperty=nameWithType> propriedade.

- <xref:System.Windows.Data.BindingMode.OneWayToSource>é o inverso da <xref:System.Windows.Data.BindingMode.OneWay> Associação; ele atualiza a propriedade Source quando a propriedade de destino é alterada. Um cenário de exemplo é se você só precisa reavaliar o valor de origem da interface do usuário.

- Não ilustrado na figura a <xref:System.Windows.Data.BindingMode.OneTime> associação, que faz com que a propriedade Source inicialize a propriedade de destino, mas não propaga as alterações subsequentes. Se o contexto de dados for alterado ou o objeto no contexto de dados for alterado, a alteração *não* será refletida na propriedade de destino. Esse tipo de associação será apropriado se um instantâneo do estado atual for apropriado ou se os dados forem verdadeiramente estáticos. Esse tipo de associação também é útil se você deseja inicializar a propriedade de destino com um valor de uma propriedade de origem e o contexto de dados não é conhecido com antecedência. Esse modo é essencialmente uma forma mais simples de <xref:System.Windows.Data.BindingMode.OneWay> associação que fornece melhor desempenho em casos em que o valor de origem não é alterado.

Para detectar alterações de origem (aplicáveis <xref:System.Windows.Data.BindingMode.OneWay> às <xref:System.Windows.Data.BindingMode.TwoWay> associações e), a origem deve implementar um mecanismo de notificação de alteração de propriedade adequado, como <xref:System.ComponentModel.INotifyPropertyChanged> . Consulte [como implementar a notificação de alteração de propriedade](../../framework/wpf/data/how-to-implement-property-change-notification.md) para obter um exemplo de uma <xref:System.ComponentModel.INotifyPropertyChanged> implementação.

A <xref:System.Windows.Data.Binding.Mode?displayProperty=nameWithType> propriedade fornece mais informações sobre modos de associação e um exemplo de como especificar a direção de uma associação.

### <a name="what-triggers-source-updates"></a>O que dispara as atualizações de origem

Associações que são <xref:System.Windows.Data.BindingMode.TwoWay> ou <xref:System.Windows.Data.BindingMode.OneWayToSource> escutam alterações na propriedade de destino e as propagam de volta para a origem, conhecidas como atualização da origem. Por exemplo, você pode editar o texto de uma caixa de texto para alterar o valor da origem subjacente.

No entanto, seu valor de origem é atualizado enquanto você está editando o texto ou depois de terminar de editar o texto e o controle perde o foco? A <xref:System.Windows.Data.Binding.UpdateSourceTrigger?displayProperty=nameWithType> propriedade determina o que dispara a atualização da origem. Os pontos das setas à direita na figura a seguir ilustram a função da <xref:System.Windows.Data.Binding.UpdateSourceTrigger?displayProperty=nameWithType> propriedade.

![Diagrama que mostra a função da propriedade UpdateSourceTrigger.](./media/data-binding-overview/data-binding-updatesource-trigger.png)

Se o `UpdateSourceTrigger` valor for <xref:System.Windows.Data.UpdateSourceTrigger.PropertyChanged?displayProperty=nameWithType> , o valor apontado pela seta para a direita <xref:System.Windows.Data.BindingMode.TwoWay> ou pelas <xref:System.Windows.Data.BindingMode.OneWayToSource> associações será atualizado assim que a propriedade de destino for alterada. No entanto, se o `UpdateSourceTrigger` valor for <xref:System.Windows.Data.UpdateSourceTrigger.LostFocus> , esse valor somente será atualizado com o novo valor quando a propriedade de destino perder o foco.

Semelhante à <xref:System.Windows.Data.Binding.Mode%2A> propriedade, diferentes propriedades de dependência têm valores padrão diferentes <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> . O valor padrão para a maioria das propriedades de dependência é <xref:System.Windows.Data.UpdateSourceTrigger.PropertyChanged>, enquanto a propriedade `TextBox.Text` tem um valor padrão de <xref:System.Windows.Data.UpdateSourceTrigger.LostFocus>. `PropertyChanged`significa que as atualizações de origem geralmente ocorrem sempre que a propriedade de destino é alterada. Alterações instantâneas são bem para caixas de seleção e outros controles simples. No entanto, para campos de texto, a atualização após cada pressionamento de tecla pode diminuir o desempenho e nega ao usuário a oportunidade usual de Backspace e corrigir erros de digitação antes de confirmar o novo valor.

Consulte a <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> página de propriedades para obter informações sobre como localizar o valor padrão de uma propriedade de dependência.

A tabela a seguir fornece um cenário de exemplo para cada <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> valor usando o <xref:System.Windows.Controls.TextBox> como um exemplo.

| Valor de UpdateSourceTrigger | Quando o valor de origem é atualizado | Cenário de exemplo para TextBox |
| ------------------------- | ---------------------------------- | ---------------------------- |
| `LostFocus`(padrão para <xref:System.Windows.Controls.TextBox.Text%2A?displayProperty=nameWithType> ) | Quando o controle TextBox perde o foco. | Uma caixa de texto associada à lógica de validação (consulte a [validação de dados](#data-validation) abaixo). |
| `PropertyChanged` | Conforme você digita no <xref:System.Windows.Controls.TextBox> . | Controles de caixa de texto em uma janela de sala de chat. |
| `Explicit` | Quando o aplicativo chama <xref:System.Windows.Data.BindingExpression.UpdateSource%2A> . | Os controles de caixa de texto em um formulário editável (atualiza os valores de origem somente quando o usuário clica no botão enviar). |

Para obter um exemplo, consulte [como controlar quando o texto de TextBox atualiza a origem](../../framework/wpf/data/how-to-control-when-the-textbox-text-updates-the-source.md).

## <a name="creating-a-binding"></a>Criando uma associação

Para redefinir alguns dos conceitos discutidos nas seções anteriores, você estabelece uma associação usando o <xref:System.Windows.Data.Binding> objeto e cada associação geralmente tem quatro componentes: um destino de associação, uma propriedade de destino, uma fonte de associação e um caminho para o valor de origem a ser usado. Esta seção discute como configurar uma associação.

Considere o exemplo a seguir, no qual o objeto de origem da associação é uma classe chamada *MyData* que é definida no namespace *SDKSample*. Para fins de demonstração, *MyData* tem uma propriedade de cadeia de caracteres chamada *colorname* cujo valor é definido como "Red". Assim, esse exemplo gera um botão com uma tela de fundo vermelha.

[!code-xaml[BindNonTextProperty](~/samples/snippets/desktop-guide/wpf/data-binding-overview/csharp/AutoConvertPropertyToColor.xaml#BindAutoConvertColor)]

Para obter mais informações sobre a sintaxe de declaração de associação e exemplos de como configurar uma associação no código, consulte [visão geral de declarações de associação](../../framework/wpf/data/binding-declarations-overview.md).

Se nós aplicarmos esse exemplo a nosso diagrama básico, a figura resultante será semelhante à exibida a seguir. Esta figura descreve uma <xref:System.Windows.Data.BindingMode.OneWay> Associação porque a propriedade Background dá suporte à <xref:System.Windows.Data.BindingMode.OneWay> associação por padrão.

![Diagrama que mostra a propriedade de plano de fundo de ligação de dados.](./media/data-binding-overview/data-binding-button-background-example.png)

Você pode imaginar por que essa associação funciona, embora a propriedade *colorname* seja do tipo cadeia de caracteres enquanto a <xref:System.Windows.Controls.Control.Background%2A> propriedade for do tipo <xref:System.Windows.Media.Brush> . Essa associação usa a conversão de tipo padrão, que é discutida na seção de [conversão de dados](#data-conversion) .

### <a name="specifying-the-binding-source"></a>Especificando a origem da Associação

Observe que, no exemplo anterior, a origem da associação é especificada definindo a propriedade [DockPanel. DataContext](xref:System.Windows.FrameworkElement.DataContext) . <xref:System.Windows.Controls.Button>Em seguida, o herda o <xref:System.Windows.FrameworkElement.DataContext%2A> valor de <xref:System.Windows.Controls.DockPanel> , que é seu elemento pai. Para reiterar, o objeto de origem da associação é um dos quatro componentes necessários de uma associação. Portanto, sem o objeto de origem de associação que está sendo especificado, a associação não faria nada.

Há várias maneiras para especificar o objeto de origem de associação. O uso da <xref:System.Windows.FrameworkElement.DataContext%2A> propriedade em um elemento pai é útil quando você está associando várias propriedades à mesma fonte. No entanto, às vezes pode ser mais apropriado especificar a origem da associação em declarações de associação individuais. Para o exemplo anterior, em vez de usar a <xref:System.Windows.FrameworkElement.DataContext%2A> propriedade, você pode especificar a origem da Associação definindo a <xref:System.Windows.Data.Binding.Source%2A?displayProperty=nameWithType> propriedade diretamente na declaração de associação do botão, como no exemplo a seguir.

[!code-xaml[BindNonTextPropertyCompactBinding](~/samples/snippets/desktop-guide/wpf/data-binding-overview/csharp/AutoConvertPropertyToColor.xaml#BindAutoConvertColorCompactBinding)]

Além de definir a <xref:System.Windows.FrameworkElement.DataContext%2A> propriedade em um elemento diretamente, herdar o <xref:System.Windows.FrameworkElement.DataContext%2A> valor de um ancestral (como o botão no primeiro exemplo) e especificar explicitamente a origem da associação, definindo a <xref:System.Windows.Data.Binding.Source%2A?displayProperty=nameWithType> Propriedade na associação (como o botão último exemplo), você também pode usar a <xref:System.Windows.Data.Binding.ElementName?displayProperty=nameWithType> propriedade ou a <xref:System.Windows.Data.Binding.RelativeSource?displayProperty=nameWithType> propriedade para especificar a origem da associação. A <xref:System.Windows.Data.Binding.ElementName%2A> propriedade é útil quando você está ligando a outros elementos em seu aplicativo, como quando você está usando um controle deslizante para ajustar a largura de um botão. A <xref:System.Windows.Data.Binding.RelativeSource%2A> propriedade é útil quando a associação é especificada em um <xref:System.Windows.Controls.ControlTemplate> ou um <xref:System.Windows.Style> . Para obter mais informações, consulte [como especificar a origem da Associação](../../framework/wpf/data/how-to-specify-the-binding-source.md).

### <a name="specifying-the-path-to-the-value"></a>Especificando o caminho para o valor

Se sua fonte de associação for um objeto, você usará a <xref:System.Windows.Data.Binding.Path?displayProperty=nameWithType> propriedade para especificar o valor a ser usado para sua associação. Se você estiver ligando a dados XML, use a <xref:System.Windows.Data.Binding.XPath?displayProperty=nameWithType> propriedade para especificar o valor. Em alguns casos, pode ser aplicável usar a <xref:System.Windows.Data.Binding.Path%2A> Propriedade mesmo quando seus dados são XML. Por exemplo, se você quiser acessar a propriedade Name de um XmlNode retornado (como resultado de uma consulta XPath), deverá usar a <xref:System.Windows.Data.Binding.Path%2A> Propriedade além da <xref:System.Windows.Data.Binding.XPath%2A> propriedade.

Para obter mais informações, consulte <xref:System.Windows.Data.Binding.Path%2A> as <xref:System.Windows.Data.Binding.XPath%2A> Propriedades e.

Embora tenhamos enfatizado que o <xref:System.Windows.Data.Binding.Path%2A> para o valor a ser usado é um dos quatro componentes necessários de uma associação, nos cenários que você deseja associar a um objeto inteiro, o valor a ser usado seria o mesmo que o objeto de origem da associação. Nesses casos, é aplicável não especificar um <xref:System.Windows.Data.Binding.Path%2A> . Considere o exemplo a seguir.

[!code-xaml[EmptyBinding](~/samples/snippets/desktop-guide/wpf/data-binding-overview/csharp/EmptyBinding.xaml#EmptyBinding)]

O exemplo acima usa a sintaxe de associação vazia: {Binding}. Nesse caso, o <xref:System.Windows.Controls.ListBox> herda o DataContext de um elemento DockPanel pai (não mostrado neste exemplo). Quando o caminho não é especificado, o padrão é associar ao objeto inteiro. Em outras palavras, neste exemplo, o caminho saiu porque estamos associando a <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> propriedade ao objeto inteiro. (Consulte a seção [associação às coleções](#binding-to-collections) para uma discussão aprofundada.)

Além de ao associar a uma coleção, este cenário também é útil quando você deseja associar a um objeto inteiro em vez de apenas uma única propriedade de um objeto. Por exemplo, se o objeto de origem for do tipo <xref:System.String> , talvez você queira simplesmente associar à própria cadeia de caracteres. Outro cenário comum é quando você deseja associar um elemento a um objeto com várias propriedades.

Talvez seja necessário aplicar a lógica personalizada para que os dados sejam significativos para a propriedade de destino associada. A lógica personalizada pode estar na forma de um conversor personalizado se a conversão de tipo padrão não existir. Consulte [conversão de dados](#data-conversion) para obter informações sobre conversores.

### <a name="binding-and-bindingexpression"></a>Associação e BindingExpression

Antes de entrar em outros recursos e usos da vinculação de dados, é útil introduzir a <xref:System.Windows.Data.BindingExpression> classe. Como você viu nas seções anteriores, a <xref:System.Windows.Data.Binding> classe é a classe de alto nível para a declaração de uma associação; ela fornece muitas propriedades que permitem que você especifique as características de uma associação. Uma classe relacionada, <xref:System.Windows.Data.BindingExpression> , é o objeto subjacente que mantém a conexão entre a origem e o destino. Uma associação contém todas as informações que podem ser compartilhadas entre várias expressões de associação. Um <xref:System.Windows.Data.BindingExpression> é uma expressão de instância que não pode ser compartilhada e contém todas as informações de instância do <xref:System.Windows.Data.Binding> .

Considere o exemplo a seguir, em que `myDataObject` é uma instância da `MyData` classe, `myBinding` é o <xref:System.Windows.Data.Binding> objeto de origem e `MyData` é uma classe definida que contém uma propriedade de cadeia de caracteres denominada `MyDataProperty` . Este exemplo associa o conteúdo de texto de `myText` , uma instância do <xref:System.Windows.Controls.TextBlock> , ao `MyDataProperty` .

[!code-csharp[CodeOnlyBinding](~/samples/snippets/desktop-guide/wpf/data-binding-overview/csharp/ManualBinding.cs#CodeOnlyBinding)]
[!code-vb[CodeOnlyBinding](~/samples/snippets/desktop-guide/wpf/data-binding-overview/vb/ManualBinding.vb#CodeOnlyBinding)]

Você pode usar o mesmo objeto *myBinding* para criar outras associações. Por exemplo, você pode usar o objeto *Myassociation* para associar o conteúdo de texto de uma caixa de seleção a *MyDataProperty*. Nesse cenário, haverá duas instâncias de <xref:System.Windows.Data.BindingExpression> compartilhamento do objeto *myBinding* .

Um <xref:System.Windows.Data.BindingExpression> objeto é retornado chamando <xref:System.Windows.Data.BindingOperations.GetBindingExpression%2A> -se em um objeto associado a dados. Os artigos a seguir demonstram alguns dos usos da <xref:System.Windows.Data.BindingExpression> classe:

- [Obter o objeto de associação de uma propriedade de destino associada](../../framework/wpf/data/how-to-get-the-binding-object-from-a-bound-target-property.md)

- [Controlar quando o texto de TextBox atualizará a origem](../../framework/wpf/data/how-to-control-when-the-textbox-text-updates-the-source.md)

## <a name="data-conversion"></a>Conversão de dados

Na seção [criando uma associação](#creating-a-binding) , o botão é vermelho porque sua <xref:System.Windows.Controls.Control.Background%2A> propriedade está associada a uma propriedade de cadeia de caracteres com o valor "vermelho". Esse valor de cadeia de caracteres funciona porque um conversor de tipo está presente no <xref:System.Windows.Media.Brush> tipo para converter o valor da cadeia de caracteres em um <xref:System.Windows.Media.Brush> .

A adição dessas informações à figura na seção [criando uma associação](#creating-a-binding) é parecida com esta.

![Diagrama que mostra a propriedade padrão de associação de dados.](./media/data-binding-overview/data-binding-button-default-conversion.png)

No entanto, e se, em vez de ter uma propriedade do tipo String, seu objeto de origem de associação tiver uma propriedade *Color* do tipo <xref:System.Windows.Media.Color> ? Nesse caso, para que a ligação funcione, você precisaria primeiro transformar o valor da propriedade *Color* em algo que a <xref:System.Windows.Controls.Control.Background%2A> Propriedade aceite. Você precisaria criar um conversor personalizado implementando a <xref:System.Windows.Data.IValueConverter> interface, como no exemplo a seguir.

[!code-csharp[CodeOnlyBinding](~/samples/snippets/desktop-guide/wpf/data-binding-overview/csharp/ColorBrushConverter.cs#ColorBrushConverter)]
[!code-vb[CodeOnlyBinding](~/samples/snippets/desktop-guide/wpf/data-binding-overview/vb/ColorBrushConverter.vb#ColorBrushConverter)]

Consulte <xref:System.Windows.Data.IValueConverter> para obter mais informações.

Agora, o conversor personalizado é usado em vez da conversão padrão, e nosso diagrama é semelhante a este.

![Diagrama que mostra o conversor personalizado de vinculação de dados.](./media/data-binding-overview/data-binding-converter-color-example.png)

Para reiterar, conversões padrão podem estar disponíveis devido a conversores de tipo presentes no tipo para o qual a associação está sendo realizada. Esse comportamento dependerá de quais conversores de tipo estão disponíveis no destino. Em caso de dúvida, crie seu próprio conversor.

A seguir estão alguns cenários típicos em que faz sentido implementar um conversor de dados:

- Os dados devem ser exibidos de forma diferente, dependendo da cultura. Por exemplo, talvez você queira implementar um conversor de moeda ou um conversor de data/hora do calendário com base nas convenções usadas em uma cultura específica.

- Os dados que está sendo usados não são necessariamente destinados a alterar o valor de texto de uma propriedade, mas sim destinados a alterar alguns outros valores como a origem de uma imagem ou então a cor ou o estilo do texto. Conversores podem ser usados nesta instância convertendo a associação de uma propriedade que pode não parecer apropriada, assim como a associação de um campo de texto à propriedade Background de uma célula de tabela.

- Mais de um controle ou várias propriedades de controles são associados aos mesmos dados. Nesse caso, a associação primária pode simplesmente exibir o texto, enquanto outras associações lidam com questões específicas de exibição, mas ainda usam a mesma associação como informações da origem.

- Uma propriedade de destino tem uma coleção de associações, que é chamada <xref:System.Windows.Data.MultiBinding> . Para o <xref:System.Windows.Data.MultiBinding> , você usa um personalizado <xref:System.Windows.Data.IMultiValueConverter> para produzir um valor final dos valores das associações. Por exemplo, a cor pode ser computada de valores vermelhos, azuis e verdes, que podem ser valores dos mesmos ou de diferentes objetos de origem da associação. Consulte <xref:System.Windows.Data.MultiBinding> para obter exemplos e informações.

## <a name="binding-to-collections"></a>Associando a coleções

Um objeto de origem de associação pode ser tratado como um único objeto cujas propriedades contêm dados ou como uma coleção de dados de objetos polimórficos que geralmente são agrupados juntos (como o resultado de uma consulta a um banco de dado). Até agora, discutimos apenas a ligação a objetos individuais. No entanto, a associação a uma coleção de dados é um cenário comum. Por exemplo, um cenário comum é usar um <xref:System.Windows.Controls.ItemsControl> <xref:System.Windows.Controls.ListBox> , <xref:System.Windows.Controls.ListView> ou <xref:System.Windows.Controls.TreeView> para exibir uma coleção de dados, como no aplicativo mostrado na seção [o que é Associação de dados](#what-is-data-binding) .

Felizmente, nosso diagrama básico ainda se aplica. Se você estiver associando um <xref:System.Windows.Controls.ItemsControl> a uma coleção, o diagrama terá esta aparência.

![Diagrama que mostra o objeto ItemsControl de vinculação de dados.](./media/data-binding-overview/data-binding-itemscontrol.png)

Conforme mostrado neste diagrama, para associar um <xref:System.Windows.Controls.ItemsControl> a um objeto de coleção, <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A?displayProperty=nameWithType> a propriedade é a propriedade a ser usada. Você pode considerar `ItemsSource` como o conteúdo do <xref:System.Windows.Controls.ItemsControl> . A associação é <xref:System.Windows.Data.BindingMode.OneWay> porque a `ItemsSource` Propriedade dá suporte à `OneWay` associação por padrão.

### <a name="how-to-implement-collections"></a>Como implementar coleções

Você pode enumerar em qualquer coleção que implemente a <xref:System.Collections.IEnumerable> interface. No entanto, para configurar associações dinâmicas para que as inserções ou exclusões na coleção atualizem a interface do usuário automaticamente, a coleção deve implementar a <xref:System.Collections.Specialized.INotifyCollectionChanged> interface. Essa interface expõe um evento que deve ser acionado sempre que a coleção subjacente for alterada.

O WPF fornece a <xref:System.Collections.ObjectModel.ObservableCollection%601> classe, que é uma implementação interna de uma coleção de dados que expõe a <xref:System.Collections.Specialized.INotifyCollectionChanged> interface. Para oferecer suporte total à transferência de valores de dados de objetos de origem para destinos, cada objeto em sua coleção que dá suporte a propriedades vinculáveis também deve implementar a <xref:System.ComponentModel.INotifyPropertyChanged> interface. Para obter mais informações, consulte [visão geral de fontes de associação](../../framework/wpf/data/binding-sources-overview.md).

Antes de implementar sua própria coleção, considere usar <xref:System.Collections.ObjectModel.ObservableCollection%601> ou uma das classes de coleção existentes, como <xref:System.Collections.Generic.List%601> , <xref:System.Collections.ObjectModel.Collection%601> e <xref:System.ComponentModel.BindingList%601> , entre muitas outras. Se você tiver um cenário avançado e quiser implementar sua própria coleção, considere usar o <xref:System.Collections.IList> , que fornece uma coleção não genérica de objetos que podem ser acessados individualmente pelo índice e, portanto, fornece o melhor desempenho.

### <a name="collection-views"></a>Exibições de coleção

Quando o <xref:System.Windows.Controls.ItemsControl> estiver associado a uma coleção de dados, talvez você queira classificar, filtrar ou agrupar os dados. Para fazer isso, você usa exibições de coleção, que são classes que implementam a <xref:System.ComponentModel.ICollectionView> interface.

#### <a name="what-are-collection-views"></a>O que são exibições de coleção?

Uma exibição de coleção é uma camada sobre uma coleção de origem da associação que permite a você navegar e exibir a coleção de origem com base em consultas de classificação, filtragem e agrupamento, sem precisar alterar a coleção de origem subjacente. Uma exibição de coleção também mantém um ponteiro para o item atual na coleção. Se a coleção de origem implementar a <xref:System.Collections.Specialized.INotifyCollectionChanged> interface, as alterações geradas pelo <xref:System.Collections.Specialized.INotifyCollectionChanged.CollectionChanged> evento serão propagadas para as exibições.

Já que as exibições não alteram as coleções de origem subjacentes, cada coleção de origem pode ter várias exibições associadas a ela. Por exemplo, você pode ter uma coleção de objetos *Task*. Com o uso de exibições, você pode exibir esses mesmos dados de maneiras diferentes. Por exemplo, no lado esquerdo da página, talvez você queira mostrar as tarefas ordenadas por prioridade e, no lado direito, agrupadas por área.

#### <a name="how-to-create-a-view"></a>Como criar uma exibição

Uma maneira de criar e usar uma exibição é instanciar o objeto de exibição diretamente e então usá-lo como a origem da associação. Por exemplo, considere o aplicativo de [demonstração de vinculação de dados][data-binding-demo] mostrado na seção [o que é Associação de dados](#what-is-data-binding) . O aplicativo é implementado de modo que o seja <xref:System.Windows.Controls.ListBox> associado a uma exibição sobre a coleta de dados em vez da coleta de dados diretamente. O exemplo a seguir é extraído do aplicativo de [demonstração de vinculação de dados][data-binding-demo] . A <xref:System.Windows.Data.CollectionViewSource> classe é o proxy XAML de uma classe herdada de <xref:System.Windows.Data.CollectionView> . Neste exemplo específico, o <xref:System.Windows.Data.CollectionViewSource.Source%2A> da exibição está associado à coleção *AuctionItems* (do tipo <xref:System.Collections.ObjectModel.ObservableCollection%601> ) do objeto de aplicativo atual.

[!code-xaml[CollectionView](~/samples/snippets/desktop-guide/wpf/data-binding-overview/csharp/CollectionView.xaml#CollectionView)]

O recurso *listingDataView* , em seguida, serve como a fonte de associação para elementos no aplicativo, como o <xref:System.Windows.Controls.ListBox> .

[!code-xaml[ListBoxCollectionView](~/samples/snippets/desktop-guide/wpf/data-binding-overview/csharp/CollectionView.xaml#ListBoxCollectionView)]

Para criar outra exibição para a mesma coleção, você pode criar outra <xref:System.Windows.Data.CollectionViewSource> instância e dar a ela um `x:Key` nome diferente.

A tabela a seguir mostra quais tipos de dados de exibição são criados como a exibição de coleção padrão ou <xref:System.Windows.Data.CollectionViewSource> com base no tipo de coleção de origem.

| Tipo de coleção de origem                    | Tipo de exibição de coleção | Observações |
| ----------------------------------------- | -------------------- | ----- |
| <xref:System.Collections.IEnumerable>     | Um tipo interno baseado em<xref:System.Windows.Data.CollectionView> | Não é possível agrupar itens. |
| <xref:System.Collections.IList>           | <xref:System.Windows.Data.ListCollectionView> | Mais rápido. |
| <xref:System.ComponentModel.IBindingList> | <xref:System.Windows.Data.BindingListCollectionView> | |

#### <a name="using-a-default-view"></a>Usando uma exibição padrão

Especificar uma exibição de coleção como uma origem da associação é uma maneira de criar e usar uma exibição de coleção. O WPF também cria uma exibição de coleção padrão para cada coleção usada como uma origem da associação. Se você associar diretamente a uma coleção, o WPF será associado à sua exibição padrão. Essa exibição padrão é compartilhada por todas as associações para a mesma coleção, portanto, uma alteração feita em um modo de exibição padrão por um controle ou código associado (como classificação ou uma alteração no ponteiro do item atual, discutida posteriormente) é refletida em todas as outras associações para a mesma coleção.

Para obter a exibição padrão, use o <xref:System.Windows.Data.CollectionViewSource.GetDefaultView%2A> método. Para obter um exemplo, consulte [obter a exibição padrão de uma coleção de dados](../../framework/wpf/data/how-to-get-the-default-view-of-a-data-collection.md).

#### <a name="collection-views-with-adonet-datatables"></a>Exibições de coleção com DataTables ADO.NET

Para melhorar o desempenho, as exibições de coleção para ADO.NET <xref:System.Data.DataTable> ou <xref:System.Data.DataView> objetos delegam classificação e filtragem para o <xref:System.Data.DataView> , que faz com que a classificação e a filtragem sejam compartilhadas entre todas as exibições de coleção da fonte de dados. Para habilitar cada exibição de coleção para classificar e filtrar de forma independente, inicialize cada modo de exibição de coleção com seu próprio <xref:System.Data.DataView> objeto.

#### <a name="sorting"></a>Classificação

Conforme mencionado anteriormente, as exibições podem aplicar uma ordem de classificação para uma coleção. Já que eles existem na coleção subjacente, seus dados podem ter ou não uma ordem inerente e relevante. A exibição da coleção permite impor uma ordem ou alterar a ordem padrão, com base em critérios de comparação fornecidos por você. Já que essa é uma exibição dos dados baseada no cliente, um cenário comum é que o usuário deseje classificar colunas de dados tabulares segundo o valor ao qual a coluna corresponde. Usando exibições, essa classificação controlada pelo usuário pode ser aplicada, novamente sem necessidade de alterações na coleção subjacente nem mesmo precisar repetir a consulta do conteúdo da coleção. Para obter um exemplo, consulte [classificar uma coluna GridView quando um cabeçalho é clicado](../../framework/wpf/controls/how-to-sort-a-gridview-column-when-a-header-is-clicked.md).

O exemplo a seguir mostra a lógica de classificação de "classificar por categoria e data" <xref:System.Windows.Controls.CheckBox> da interface do usuário do aplicativo na seção [o que é Associação de dados](#what-is-data-binding) .

[!code-csharp[AddSortChecked](~/samples/snippets/desktop-guide/wpf/data-binding-overview/csharp/CollectionView.xaml.cs#AddSortChecked)]
[!code-vb[AddSortChecked](~/samples/snippets/desktop-guide/wpf/data-binding-overview/vb/CollectionView.xaml.vb#AddSortChecked)]

#### <a name="filtering"></a>Filtragem

As exibições também podem aplicar um filtro a uma coleção, de modo que a exibição mostre apenas um determinado subconjunto da coleção completa. Você pode filtrar por uma condição nos dados. Por exemplo, como é feito pelo aplicativo na seção [o que é Associação de dados](#what-is-data-binding) , o "mostrar apenas os dados" <xref:System.Windows.Controls.CheckBox> contém a lógica para filtrar os itens que custam $25 ou mais. O código a seguir é executado para definir *ShowOnlyBargainsFilter* como o <xref:System.Windows.Data.CollectionViewSource.Filter> manipulador de eventos quando <xref:System.Windows.Controls.CheckBox> selecionado.

[!code-csharp[ListingViewFilter](~/samples/snippets/desktop-guide/wpf/data-binding-overview/csharp/CollectionView.xaml.cs#ListingViewFilter)]
[!code-vb[ListingViewFilter](~/samples/snippets/desktop-guide/wpf/data-binding-overview/vb/CollectionView.xaml.vb#ListingViewFilter)]

O manipulador de eventos *ShowOnlyBargainsFilter* tem a implementação a seguir.

[!code-csharp[FilterEvent](~/samples/snippets/desktop-guide/wpf/data-binding-overview/csharp/CollectionView.xaml.cs#FilterEvent)]
[!code-vb[FilterEvent](~/samples/snippets/desktop-guide/wpf/data-binding-overview/vb/CollectionView.xaml.vb#FilterEvent)]

Se você estiver usando uma das <xref:System.Windows.Data.CollectionView> classes diretamente em vez de <xref:System.Windows.Data.CollectionViewSource> , você usaria a <xref:System.Windows.Data.CollectionView.Filter%2A> propriedade para especificar um retorno de chamada. Para obter um exemplo, consulte [Filtrar dados em uma exibição](../../framework/wpf/data/how-to-filter-data-in-a-view.md).

#### <a name="grouping"></a>Agrupamento

Exceto para a classe interna que exibe uma <xref:System.Collections.IEnumerable> coleção, todas as exibições de coleção dão suporte ao *agrupamento*, o que permite ao usuário particionar a coleção no modo de exibição de coleção em grupos lógicos. Os grupos podem ser explícitos (caso em que o usuário fornece uma lista de grupos) ou então implícitos (caso em que os grupos são gerados dinamicamente, dependendo dos dados).

O exemplo a seguir mostra a lógica do "Agrupar por categoria" <xref:System.Windows.Controls.CheckBox> .

[!code-csharp[ListingGroupCheck](~/samples/snippets/desktop-guide/wpf/data-binding-overview/csharp/CollectionView.xaml.cs#ListingGroupCheck)]
[!code-vb[ListingGroupCheck](~/samples/snippets/desktop-guide/wpf/data-binding-overview/vb/CollectionView.xaml.vb#ListingGroupCheck)]

Para obter outro exemplo de agrupamento, consulte [Agrupar itens em uma ListView que implementa uma GridView](../../framework/wpf/controls/how-to-group-items-in-a-listview-that-implements-a-gridview.md).

#### <a name="current-item-pointers"></a>Ponteiros do item atual

As exibições também dão suporte à noção de um item atual. Você pode navegar pelos objetos em uma exibição de coleção. Enquanto você navega, você está movendo um ponteiro de item que permite que você recupere o objeto existente nesse local específico na coleção. Para obter um exemplo, consulte [navegar pelos objetos em um CollectionView de dados](../../framework/wpf/data/how-to-navigate-through-the-objects-in-a-data-collectionview.md).

Já que o WPF associa a uma coleção usando uma exibição (uma exibição que você especificar ou uma exibição padrão da coleção), todas as associações a coleções têm um ponteiro de item atual. Ao associar a um modo de exibição, o caractere de barra ("/") em um valor `Path` atribui o item atual da exibição. No exemplo a seguir, o contexto de dados é uma exibição de coleção. A primeira linha é associada à coleção. A segunda linha é associada ao item atual na coleção. A terceira linha é associada à propriedade `Description` do item atual na coleção.

```xaml
<Button Content="{Binding }" />
<Button Content="{Binding Path=/}" />
<Button Content="{Binding Path=/Description}" />
```

A sintaxe de barra e de propriedade também pode ser empilhada para transpor uma hierarquia de coleções. O exemplo a seguir associa o item atual de uma coleção chamada `Offices`, que é uma propriedade do item atual da coleção de origem.

```xaml
<Button Content="{Binding /Offices/}" />
```

O ponteiro do item atual pode ser afetado por qualquer classificação ou filtragem aplicada à coleção. A classificação preserva o ponteiro do item atual no último item selecionado, mas a exibição de coleção está agora reestruturada em torno dele. (Talvez o item selecionado estivesse no início da lista antes, mas agora o item selecionado pode estar em algum lugar no meio.) A filtragem preserva o item selecionado se essa seleção permanecer no modo de exibição após a filtragem. Caso contrário, o ponteiro do item atual será definido como o primeiro item da exibição de coleção filtrada.

#### <a name="master-detail-binding-scenario"></a>Cenário de associação mestre-detalhes

A noção de um item atual é útil não somente para navegação por itens em uma coleção, mas também para o cenário de associação mestre/detalhes. Considere a interface do usuário do aplicativo na seção [o que é Associação de dados](#what-is-data-binding) novamente. Nesse aplicativo, a seleção dentro de <xref:System.Windows.Controls.ListBox> determina o conteúdo mostrado no <xref:System.Windows.Controls.ContentControl> . Para colocá-lo de outra maneira, quando um <xref:System.Windows.Controls.ListBox> item é selecionado, o <xref:System.Windows.Controls.ContentControl> mostra os detalhes do item selecionado.

Você pode implementar o cenário mestre/detalhes simplesmente associando dois ou mais controles à mesma exibição. O exemplo a seguir da [demonstração de vinculação de dados][data-binding-demo] mostra a marcação do <xref:System.Windows.Controls.ListBox> e o <xref:System.Windows.Controls.ContentControl> que você vê na interface do usuário do aplicativo na seção [o que é Associação de dados](#what-is-data-binding) .

[!code-xaml[ListBoxContentControl](~/samples/snippets/desktop-guide/wpf/data-binding-overview/csharp/CollectionView.xaml#ListBoxContentControl)]

Observe que os dois controles estão associados à mesma origem, o recurso estático *listingDataView* (consulte a definição desse recurso na [seção como criar uma exibição](#how-to-create-a-view)). Essa associação funciona porque, quando um objeto singleton ( <xref:System.Windows.Controls.ContentControl> neste caso) está associado a uma exibição de coleção, ele automaticamente se vincula ao <xref:System.Windows.Data.CollectionView.CurrentItem%2A> da exibição. Os <xref:System.Windows.Data.CollectionViewSource> objetos sincronizam automaticamente a moeda e a seleção. Se o controle de lista não estiver associado a um <xref:System.Windows.Data.CollectionViewSource> objeto como neste exemplo, você precisará definir sua <xref:System.Windows.Controls.Primitives.Selector.IsSynchronizedWithCurrentItem%2A> propriedade como para `true` que isso funcione.

Para obter outros exemplos, consulte [associar a uma coleção e exibir informações com base na seleção](../../framework/wpf/data/how-to-bind-to-a-collection-and-display-information-based-on-selection.md) e [usar o padrão de detalhes mestre com dados hierárquicos](../../framework/wpf/data/how-to-use-the-master-detail-pattern-with-hierarchical-data.md).

Talvez você tenha observado que o exemplo anterior usa um modelo. Na verdade, os dados não seriam exibidos da maneira que desejamos sem o uso de modelos (aquele usado explicitamente pelo <xref:System.Windows.Controls.ContentControl> e aquele usado implicitamente pelo <xref:System.Windows.Controls.ListBox> ). Agora, na próxima seção, nos ocuparemos da modelagem de dados.

## <a name="data-templating"></a>Modelagem de dados

Sem o uso de modelos de dados, nossa interface do usuário do aplicativo na seção [o que é Associação de dados](#what-is-data-binding) seria semelhante ao seguinte.

![Demonstração de vinculação de dados sem modelos de dados](./media/data-binding-overview/demo-no-template.png)

Conforme mostrado no exemplo na seção anterior, o <xref:System.Windows.Controls.ListBox> controle e o <xref:System.Windows.Controls.ContentControl> são vinculados a todo o objeto da coleção (ou mais especificamente, a exibição sobre o objeto de coleção) de s *AuctionItem*. Sem instruções específicas de como exibir a coleta de dados, o <xref:System.Windows.Controls.ListBox> exibe a representação de cadeia de caracteres de cada objeto na coleção subjacente e <xref:System.Windows.Controls.ContentControl> exibe a representação de cadeia de caracteres do objeto ao qual está associado.

Para resolver esse problema, o aplicativo define <xref:System.Windows.DataTemplate?text=DataTemplates> . Conforme mostrado no exemplo na seção anterior, o <xref:System.Windows.Controls.ContentControl> usa explicitamente o modelo de dados *detailsProductListingTemplate* . O <xref:System.Windows.Controls.ListBox> controle usa implicitamente o seguinte modelo de dados ao exibir os objetos *AuctionItem* na coleção.

[!code-xaml[AuctionItemDataTemplate](~/samples/snippets/desktop-guide/wpf/data-binding-overview/csharp/CollectionView.xaml#AuctionItemDataTemplate)]

Com o uso desses dois modelos de dados, a interface do usuário resultante é a mostrada na seção [o que é a associação de dados](#what-is-data-binding) . Como você pode ver nessa captura de tela, além de permitir que você coloque dados em seus controles, os DataTemplates permitem que você defina visuais atraentes para seus dados. Por exemplo, <xref:System.Windows.DataTrigger> s são usados no acima <xref:System.Windows.DataTemplate> para que os s de *AuctionItem*com o valor *SpecialFeatures* de *realce* sejam exibidos com uma borda laranja e uma estrela.

Para obter mais informações sobre modelos de dados, consulte [visão geral de modelagem de dados](../../framework/wpf/data/data-templating-overview.md).

## <a name="data-validation"></a>Validação de dados

A maioria dos aplicativos que usam a entrada do usuário precisa ter lógica de validação para garantir que o usuário tenha inserido as informações esperadas. As verificações de validação podem ser baseadas no tipo, no intervalo, no formato ou em outros requisitos específicos do aplicativo. Esta seção discute como a validação de dados funciona no WPF.

### <a name="associating-validation-rules-with-a-binding"></a>Associando regras de validação a uma associação

O modelo de ligação de dados do WPF permite que você associe <xref:System.Windows.Data.Binding.ValidationRules%2A> com o <xref:System.Windows.Data.Binding> objeto. Por exemplo, o exemplo a seguir associa um <xref:System.Windows.Controls.TextBox> a uma propriedade chamada `StartPrice` e adiciona um <xref:System.Windows.Controls.ExceptionValidationRule> objeto à <xref:System.Windows.Data.Binding.ValidationRules%2A?displayProperty=nameWithType> propriedade.

[!code-xaml[TextboxStartPrice](~/samples/snippets/desktop-guide/wpf/data-binding-overview/csharp/DataValidation.xaml#TextboxStartPrice)]

Um <xref:System.Windows.Controls.ValidationRule> objeto verifica se o valor de uma propriedade é válido. O WPF tem dois tipos de objetos internos <xref:System.Windows.Controls.ValidationRule> :

- Uma <xref:System.Windows.Controls.ExceptionValidationRule> verificação de exceções geradas durante a atualização da propriedade de origem da associação. No exemplo anterior, `StartPrice` é do tipo inteiro. Quando o usuário insere um valor que não pode ser convertido em um número inteiro, é gerada uma exceção que faz com que a associação seja marcada como inválida. Uma sintaxe alternativa para definir <xref:System.Windows.Controls.ExceptionValidationRule> explicitamente é definir a <xref:System.Windows.Data.Binding.ValidatesOnExceptions%2A> propriedade como `true` no seu <xref:System.Windows.Data.Binding> <xref:System.Windows.Data.MultiBinding> objeto or.

- Um <xref:System.Windows.Controls.DataErrorValidationRule> objeto verifica se há erros que são gerados por objetos que implementam a <xref:System.ComponentModel.IDataErrorInfo> interface. Para obter um exemplo de como usar essa regra de validação, consulte <xref:System.Windows.Controls.DataErrorValidationRule> . Uma sintaxe alternativa para definir <xref:System.Windows.Controls.DataErrorValidationRule> explicitamente é definir a <xref:System.Windows.Data.Binding.ValidatesOnDataErrors%2A> propriedade como `true` no seu <xref:System.Windows.Data.Binding> <xref:System.Windows.Data.MultiBinding> objeto or.

Você também pode criar sua própria regra de validação derivando da <xref:System.Windows.Controls.ValidationRule> classe e implementando o <xref:System.Windows.Controls.ValidationRule.Validate%2A> método. O exemplo a seguir mostra a regra usada pela seção "data de início" da *listagem de produtos de adição* <xref:System.Windows.Controls.TextBox> . [What is data binding](#what-is-data-binding)

[!code-csharp[FutureDateRule](~/samples/snippets/desktop-guide/wpf/data-binding-overview/csharp/FutureDateRule.cs#FutureDateRule)]
[!code-vb[FutureDateRule](~/samples/snippets/desktop-guide/wpf/data-binding-overview/vb/FutureDateRule.vb#FutureDateRule)]

O *StartDateEntryForm* <xref:System.Windows.Controls.TextBox> usa esse *FutureDateRule*, conforme mostrado no exemplo a seguir.

[!code-xaml[TextboxStartDate](~/samples/snippets/desktop-guide/wpf/data-binding-overview/csharp/DataValidation.xaml#TextboxStartDate)]

Como o <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> valor é <xref:System.Windows.Data.UpdateSourceTrigger.PropertyChanged> , o mecanismo de associação atualiza o valor de origem em cada pressionamento de tecla, o que significa que ele também verifica cada regra na <xref:System.Windows.Data.Binding.ValidationRules%2A> coleção em cada pressionamento de tecla. Discutiremos isso de modo mais aprofundado na seção Processo de validação.

### <a name="providing-visual-feedback"></a>Fornecendo comentários visuais

Se o usuário inserir um valor inválido, talvez você queira fornecer alguns comentários sobre o erro na interface do usuário do aplicativo. Uma maneira de fornecer esses comentários é definir a <xref:System.Windows.Controls.Validation.ErrorTemplate%2A?displayProperty=nameWithType> Propriedade anexada como um personalizado <xref:System.Windows.Controls.ControlTemplate> . Conforme mostrado na subseção anterior, o *StartDateEntryForm* <xref:System.Windows.Controls.TextBox> usa um <xref:System.Windows.Controls.Validation.ErrorTemplate%2A> chamado *validationTemplate*. O exemplo a seguir mostra a definição de *validationTemplate*.

[!code-xaml[ControlTemplate](~/samples/snippets/desktop-guide/wpf/data-binding-overview/csharp/DataValidation.xaml#ControlTemplate)]

O <xref:System.Windows.Controls.AdornedElementPlaceholder> elemento especifica onde o controle que está sendo adornado deve ser colocado.

Além disso, você também pode usar um <xref:System.Windows.Controls.ToolTip> para exibir a mensagem de erro. O *StartDateEntryForm* e o *StartPriceEntryForm* <xref:System.Windows.Controls.TextBox> es usam o estilo *textStyleTextBox*, que cria um <xref:System.Windows.Controls.ToolTip> que exibe a mensagem de erro. O exemplo a seguir mostra a definição de *textStyleTextBox*. A propriedade anexada <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> é `true` quando uma ou mais das associações nas propriedades do elemento associado estão com erro.

[!code-xaml[TextBoxStyle](~/samples/snippets/desktop-guide/wpf/data-binding-overview/csharp/DataValidation.xaml#TextBoxStyle)]

Com o personalizado <xref:System.Windows.Controls.Validation.ErrorTemplate%2A> e o <xref:System.Windows.Controls.ToolTip> , o *StartDateEntryForm* é <xref:System.Windows.Controls.TextBox> semelhante ao seguinte quando há um erro de validação.

![Erro de validação de associação de dados](./media/data-binding-overview/demo-validation-date.png "DataBindingDemo_Validation")

Se o <xref:System.Windows.Data.Binding> tiver regras de validação associadas, mas você não especificar um <xref:System.Windows.Controls.Validation.ErrorTemplate%2A> no controle ligado, um padrão <xref:System.Windows.Controls.Validation.ErrorTemplate%2A> será usado para notificar os usuários quando houver um erro de validação. O padrão <xref:System.Windows.Controls.Validation.ErrorTemplate%2A> é um modelo de controle que define uma borda vermelha na camada de adorno. Com o padrão <xref:System.Windows.Controls.Validation.ErrorTemplate%2A> e o <xref:System.Windows.Controls.ToolTip> , a interface do usuário do *StartPriceEntryForm* é <xref:System.Windows.Controls.TextBox> semelhante ao seguinte quando há um erro de validação.

![Erro de validação de associação de dados](./media/data-binding-overview/demo-validation-price.png "DataBindingDemo_ValidationDefault")

Para obter um exemplo de como fornecer lógica para validar todos os controles em uma caixa de diálogo, consulte a seção caixas de diálogo personalizadas na [visão geral das caixas de diálogo](../../framework/wpf/app-development/dialog-boxes-overview.md).

### <a name="validation-process"></a>Processo de validação

Geralmente, a validação ocorre quando o valor de um destino é transferido para a propriedade de origem da associação. Essa transferência ocorre em <xref:System.Windows.Data.BindingMode.TwoWay> <xref:System.Windows.Data.BindingMode.OneWayToSource> associações e. Para reiterar, o que causa uma atualização de origem depende do valor da <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> propriedade, conforme descrito na seção [o que dispara as atualizações de origem](#what-triggers-source-updates) .

Os itens a seguir descrevem o processo de *validação* . Se um erro de validação ou outro tipo de erro ocorrer a qualquer momento durante esse processo, o processo será interrompido:

1. O mecanismo de associação verifica se há objetos personalizados <xref:System.Windows.Controls.ValidationRule> definidos cujo <xref:System.Windows.Controls.ValidationRule.ValidationStep%2A> conjunto está definido como <xref:System.Windows.Controls.ValidationStep.RawProposedValue> para isso <xref:System.Windows.Data.Binding> ; nesse caso, ele chama o <xref:System.Windows.Controls.ValidationRule.Validate%2A> método em cada <xref:System.Windows.Controls.ValidationRule> um até que um deles seja executado em um erro ou até que todos eles passem.

2. O mecanismo de associação então chamará o conversor, se houver.

3. Se o conversor for bem-sucedido, o mecanismo de associação verificará se há algum <xref:System.Windows.Controls.ValidationRule> objeto personalizado definido, cujo <xref:System.Windows.Controls.ValidationRule.ValidationStep%2A> está definido como <xref:System.Windows.Controls.ValidationStep.ConvertedProposedValue> para isso <xref:System.Windows.Data.Binding> . nesse caso, ele chamará o <xref:System.Windows.Controls.ValidationRule.Validate%2A> método em cada <xref:System.Windows.Controls.ValidationRule> um que tenha <xref:System.Windows.Controls.ValidationRule.ValidationStep%2A> definido como <xref:System.Windows.Controls.ValidationStep.ConvertedProposedValue> até que um deles tenha um erro ou até que todos eles passem.

4. O mecanismo de associação define a propriedade de origem.

5. O mecanismo de associação verifica se há objetos personalizados <xref:System.Windows.Controls.ValidationRule> definidos cujo <xref:System.Windows.Controls.ValidationRule.ValidationStep%2A> conjunto está definido como <xref:System.Windows.Controls.ValidationStep.UpdatedValue> para isso. <xref:System.Windows.Data.Binding> nesse caso, ele chama o <xref:System.Windows.Controls.ValidationRule.Validate%2A> método em cada <xref:System.Windows.Controls.ValidationRule> um que tenha <xref:System.Windows.Controls.ValidationRule.ValidationStep%2A> definido como até que <xref:System.Windows.Controls.ValidationStep.UpdatedValue> um deles tenha um erro ou até que todos eles passem. Se um <xref:System.Windows.Controls.DataErrorValidationRule> estiver associado a uma associação e seu <xref:System.Windows.Controls.ValidationRule.ValidationStep%2A> estiver definido como o padrão, <xref:System.Windows.Controls.ValidationStep.UpdatedValue> o <xref:System.Windows.Controls.DataErrorValidationRule> será verificado neste ponto. Neste ponto, qualquer associação que tenha o <xref:System.Windows.Data.Binding.ValidatesOnDataErrors%2A> conjunto definido como `true` está marcada.

6. O mecanismo de associação verifica se há objetos personalizados <xref:System.Windows.Controls.ValidationRule> definidos cujo <xref:System.Windows.Controls.ValidationRule.ValidationStep%2A> conjunto está definido como <xref:System.Windows.Controls.ValidationStep.CommittedValue> para isso. <xref:System.Windows.Data.Binding> nesse caso, ele chama o <xref:System.Windows.Controls.ValidationRule.Validate%2A> método em cada <xref:System.Windows.Controls.ValidationRule> um que tenha <xref:System.Windows.Controls.ValidationRule.ValidationStep%2A> definido como até que <xref:System.Windows.Controls.ValidationStep.CommittedValue> um deles tenha um erro ou até que todos eles passem.

Se um não <xref:System.Windows.Controls.ValidationRule> passar algum tempo durante todo esse processo, o mecanismo de associação criará um <xref:System.Windows.Controls.ValidationError> objeto e o adicionará à <xref:System.Windows.Controls.Validation.Errors%2A?displayProperty=nameWithType> coleção do elemento associado. Antes que o mecanismo de associação execute os <xref:System.Windows.Controls.ValidationRule> objetos em qualquer etapa determinada, ele remove qualquer <xref:System.Windows.Controls.ValidationError> que foi adicionado à <xref:System.Windows.Controls.Validation.Errors%2A?displayProperty=nameWithType> Propriedade anexada do elemento associado durante essa etapa. Por exemplo, se um <xref:System.Windows.Controls.ValidationRule> cujo <xref:System.Windows.Controls.ValidationRule.ValidationStep%2A> estiver definido como <xref:System.Windows.Controls.ValidationStep.UpdatedValue> falha, na próxima vez que o processo de validação ocorrer, o mecanismo de associação removerá isso <xref:System.Windows.Controls.ValidationError> imediatamente antes de chamar qualquer um <xref:System.Windows.Controls.ValidationRule> que tenha sido <xref:System.Windows.Controls.ValidationRule.ValidationStep%2A> definido como <xref:System.Windows.Controls.ValidationStep.UpdatedValue> .

Quando <xref:System.Windows.Controls.Validation.Errors%2A?displayProperty=nameWithType> não está vazio, a <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada do elemento é definida como `true` . Além disso, se a <xref:System.Windows.Data.Binding.NotifyOnValidationError%2A> propriedade de <xref:System.Windows.Data.Binding> for definida como `true` , o mecanismo de associação gerará o <xref:System.Windows.Controls.Validation.Error?displayProperty=nameWithType> evento anexado no elemento.

Observe também que uma transferência de valor válida em qualquer direção (destino para origem ou origem para destino) limpa a <xref:System.Windows.Controls.Validation.Errors%2A?displayProperty=nameWithType> Propriedade anexada.

Se a associação tiver um <xref:System.Windows.Controls.ExceptionValidationRule> associado a ela ou se a <xref:System.Windows.Data.Binding.ValidatesOnExceptions%2A> propriedade estiver definida como `true` e uma exceção for lançada quando o mecanismo de associação definir a origem, o mecanismo de associação verificará se há um <xref:System.Windows.Data.Binding.UpdateSourceExceptionFilter%2A> . Você tem a opção de usar o <xref:System.Windows.Data.Binding.UpdateSourceExceptionFilter%2A> retorno de chamada para fornecer um manipulador personalizado para lidar com exceções. Se um <xref:System.Windows.Data.Binding.UpdateSourceExceptionFilter%2A> não for especificado no <xref:System.Windows.Data.Binding> , o mecanismo de associação criará um <xref:System.Windows.Controls.ValidationError> com a exceção e o adicionará à <xref:System.Windows.Controls.Validation.Errors%2A?displayProperty=nameWithType> coleção do elemento associado.

## <a name="debugging-mechanism"></a>Mecanismo de depuração

Você pode definir a propriedade anexada <xref:System.Diagnostics.PresentationTraceSources.TraceLevel%2A?displayProperty=nameWithType> em um objeto relacionado à associação para receber informações sobre o status de uma associação específica.

## <a name="see-also"></a>Veja também

- <xref:System.Windows.Controls.DataErrorValidationRule>
- [Associar aos resultados de uma consulta LINQ](../../framework/wpf/data/how-to-bind-to-the-results-of-a-linq-query.md)
- [Associação de dados](../../framework/wpf/advanced/optimizing-performance-data-binding.md)
- [Demonstração de vinculação de dados][data-binding-demo]
- [Artigos de instruções](../../framework/wpf/data/data-binding-how-to-topics.md)
- [Associar a uma fonte de dados ADO.NET](../../framework/wpf/data/how-to-bind-to-an-ado-net-data-source.md)

[data-binding-demo]: https://github.com/microsoft/WPF-Samples/tree/master/Sample%20Applications/DataBindingDemo "aplicativo de demonstração de vinculação de dados"
