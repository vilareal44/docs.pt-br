---
title: Estilos e modelos
description: Saiba mais sobre os recursos XAML no Windows Presentation Foundation (WPF) para .NET Core. Entenda os tipos de recursos XAML relacionados a estilos e temas.
author: adegeo
ms.author: adegeo
ms.date: 09/09/2019
dev_langs:
- csharp
- vb
ms.openlocfilehash: faa54e0a3c827717114ca6ca4f033c1c4c3acfa8
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325776"
---
# <a name="styles-and-templates-in-wpf"></a>Estilos e modelos no WPF

O estilo e a modelagem do Windows Presentation Foundation (WPF) referem-se a um conjunto de recursos que permitem que desenvolvedores e designers criem efeitos visualmente atraentes e uma aparência consistente para seus produtos. Ao personalizar a aparência de um aplicativo, você deseja um modelo forte de estilo e modelagem que permite a manutenção e o compartilhamento da aparência dentro e entre os aplicativos. O WPF fornece esse modelo.

Outro recurso do modelo de estilo do WPF é a separação da apresentação e da lógica. Os designers podem trabalhar na aparência de um aplicativo usando apenas XAML ao mesmo tempo que os desenvolvedores trabalham na lógica de programação usando C# ou Visual Basic.

Esta visão geral concentra-se nos aspectos de estilo e modelagem do aplicativo e não aborda nenhum dos conceitos de ligação de dados. Para obter informações sobre associação de dados, consulte [Visão geral de vinculação de dados](../data/data-binding-overview.md).

É importante entender os recursos, que são o que permite que os estilos e modelos sejam reutilizados. Para obter mais informações sobre recursos, consulte [Recursos de XAML](xaml-resources-define.md).

[!INCLUDE [desktop guide under construction](../../../includes/desktop-guide-preview-note.md)]

## <a name="sample"></a>Amostra

O código de exemplo fornecido nesta visão geral é baseado em um [aplicativo simples de navegação de fotos](https://github.com/Microsoft/WPF-Samples/tree/master/Styles%20&%20Templates/IntroToStylingAndTemplating) mostrado na ilustração a seguir.

![ListView com estilo](./media/styles-and-templates-overview/stylingintro-triggers.png "StylingIntro_triggers")

Essa amostra de foto simples usa estilo e modelagem para criar uma experiência de usuário visualmente atraente. O exemplo tem dois <xref:System.Windows.Controls.TextBlock> elementos e um <xref:System.Windows.Controls.ListBox> controle que está associado a uma lista de imagens.

Para ver a amostra completa, consulte [Introdução à amostra de estilo e modelagem](https://github.com/Microsoft/WPF-Samples/tree/master/Styles%20&%20Templates/IntroToStylingAndTemplating).

## <a name="styles"></a>Estilos

Você pode considerar um <xref:System.Windows.Style> como uma maneira conveniente de aplicar um conjunto de valores de propriedade a vários elementos. Você pode usar um estilo em qualquer elemento que derive de <xref:System.Windows.FrameworkElement> ou <xref:System.Windows.FrameworkContentElement> como um <xref:System.Windows.Window> ou um <xref:System.Windows.Controls.Button> .

A maneira mais comum de declarar um estilo é como um recurso na `Resources` seção em um arquivo XAML. Como os estilos são recursos, eles obedecem as mesmas regras de escopo que se aplicam a todos os recursos. Coloque simplesmente, em que você declara um estilo que afeta o local em que o estilo pode ser aplicado. Por exemplo, se você declarar o estilo no elemento raiz do arquivo XAML de definição de aplicativo, o estilo poderá ser usado em qualquer lugar em seu aplicativo.

Por exemplo, o código XAML a seguir declara dois estilos para um `TextBlock` , um aplicado automaticamente a todos os `TextBlock` elementos e outro que deve ser explicitamente referenciado.

[!code-xaml[SnippetDefaultTextBlockStyleBasedOn](~/samples/snippets/desktop-guide/wpf/styles-and-templates-intro/csharp/Window2.xaml#SnippetDefaultTextBlockStyleBasedOn)]

Aqui está um exemplo dos estilos declarados acima que estão sendo usados.

[!code-xaml[SnippetTextBlocksExplicit](~/samples/snippets/desktop-guide/wpf/styles-and-templates-intro/csharp/Window2.xaml#SnippetTextBlocksExplicit)]

![Bloco de Textcom estilo](./media/styles-and-templates-overview/stylingintro-textblocks.png)

Para obter mais informações, consulte [criar um estilo para um controle](styles-templates-create-apply-style.md).

## <a name="controltemplates"></a>ControlTemplates

No WPF, o <xref:System.Windows.Controls.ControlTemplate> de um controle define a aparência do controle. Você pode alterar a estrutura e a aparência de um controle definindo um novo <xref:System.Windows.Controls.ControlTemplate> e atribuindo-o a um controle. Em muitos casos, os modelos fornecem flexibilidade suficiente para que você não precise escrever seus próprios controles personalizados.

Cada controle tem um modelo padrão atribuído à propriedade [Control. Template](xref:System.Windows.Controls.Control.Template) . O modelo conecta a apresentação visual do controle com os recursos do controle. Como você define um modelo em XAML, você pode alterar a aparência do controle sem escrever nenhum código. Cada modelo é projetado para um controle específico, como um <xref:System.Windows.Controls.Button> .

Normalmente, você declara um modelo como um recurso na `Resources` seção de um arquivo XAML. Assim como acontece com todos os recursos, as regras de escopo se aplicam.

Os modelos de controle são muito mais envolvidos do que um estilo. Isso ocorre porque o modelo de controle reescreve a aparência visual do controle inteiro, enquanto um estilo simplesmente aplica alterações de propriedade ao controle existente. No entanto, como o modelo de um controle é aplicado definindo a propriedade [Control. Template](xref:System.Windows.Controls.Control.Template) , você pode usar um estilo para definir ou definir um modelo.

Os designers geralmente permitem que você crie uma cópia de um modelo existente e modifique-o. Por exemplo, no designer do WPF do Visual Studio, selecione um `CheckBox` controle e clique com o botão direito do mouse e selecione **Editar modelo**  >  **criar uma cópia**. Esse comando gera um *estilo que define um modelo*.

```xaml
<Style x:Key="CheckBoxStyle1" TargetType="{x:Type CheckBox}">
    <Setter Property="FocusVisualStyle" Value="{StaticResource FocusVisual1}"/>
    <Setter Property="Background" Value="{StaticResource OptionMark.Static.Background1}"/>
    <Setter Property="BorderBrush" Value="{StaticResource OptionMark.Static.Border1}"/>
    <Setter Property="Foreground" Value="{DynamicResource {x:Static SystemColors.ControlTextBrushKey}}"/>
    <Setter Property="BorderThickness" Value="1"/>
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="{x:Type CheckBox}">
                <Grid x:Name="templateRoot" Background="Transparent" SnapsToDevicePixels="True">
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto"/>
                        <ColumnDefinition Width="*"/>
                    </Grid.ColumnDefinitions>
                    <Border x:Name="checkBoxBorder" Background="{TemplateBinding Background}" BorderThickness="{TemplateBinding BorderThickness}" BorderBrush="{TemplateBinding BorderBrush}" HorizontalAlignment="{TemplateBinding HorizontalContentAlignment}" Margin="1" VerticalAlignment="{TemplateBinding VerticalContentAlignment}">
                        <Grid x:Name="markGrid">
                            <Path x:Name="optionMark" Data="F1 M 9.97498,1.22334L 4.6983,9.09834L 4.52164,9.09834L 0,5.19331L 1.27664,3.52165L 4.255,6.08833L 8.33331,1.52588e-005L 9.97498,1.22334 Z " Fill="{StaticResource OptionMark.Static.Glyph1}" Margin="1" Opacity="0" Stretch="None"/>
                            <Rectangle x:Name="indeterminateMark" Fill="{StaticResource OptionMark.Static.Glyph1}" Margin="2" Opacity="0"/>
                        </Grid>
                    </Border>
                    <ContentPresenter x:Name="contentPresenter" Grid.Column="1" Focusable="False" HorizontalAlignment="{TemplateBinding HorizontalContentAlignment}" Margin="{TemplateBinding Padding}" RecognizesAccessKey="True" SnapsToDevicePixels="{TemplateBinding SnapsToDevicePixels}" VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
                </Grid>
                <ControlTemplate.Triggers>
                    <Trigger Property="HasContent" Value="true">
                        <Setter Property="FocusVisualStyle" Value="{StaticResource OptionMarkFocusVisual1}"/>
                        <Setter Property="Padding" Value="4,-1,0,0"/>

... content removed to save space ...
```

Editar uma cópia de um modelo é uma ótima maneira de aprender como os modelos funcionam. Em vez de criar um novo modelo em branco, é mais fácil editar uma cópia e alterar alguns aspectos da apresentação visual.

Para obter um exemplo, consulte [criar um modelo para um controle](../themes/how-to-create-apply-template.md).

### <a name="templatebinding"></a>TemplateBinding

Talvez você tenha notado que o recurso de modelo definido na seção anterior usa a [extensão de marcação TemplateBinding](../../framework/wpf/advanced/templatebinding-markup-extension.md). Uma `TemplateBinding` é uma forma otimizada de uma associação para cenários de modelo, análoga a uma associação construída com `{Binding RelativeSource={RelativeSource TemplatedParent}}` . `TemplateBinding`é útil para associar partes do modelo a propriedades do controle. Por exemplo, cada controle tem uma <xref:System.Windows.Controls.Control.BorderThickness> propriedade. Use um `TemplateBinding` para gerenciar qual elemento no modelo é afetado por essa configuração de controle.

### <a name="contentcontrol-and-itemscontrol"></a>ContentControl e ItemsControl

Se um <xref:System.Windows.Controls.ContentPresenter> for declarado no <xref:System.Windows.Controls.ControlTemplate> de a <xref:System.Windows.Controls.ContentControl> , o se <xref:System.Windows.Controls.ContentPresenter> associará automaticamente às <xref:System.Windows.Controls.ContentControl.ContentTemplate%2A> Propriedades e <xref:System.Windows.Controls.ContentControl.Content%2A> . Da mesma forma, um <xref:System.Windows.Controls.ItemsPresenter> que está no <xref:System.Windows.Controls.ControlTemplate> de um <xref:System.Windows.Controls.ItemsControl> será automaticamente associado às <xref:System.Windows.Controls.ItemsControl.ItemTemplate%2A> <xref:System.Windows.Controls.ItemsControl.Items%2A> Propriedades e.

## <a name="datatemplates"></a>DataTemplates

Neste aplicativo de exemplo, há um <xref:System.Windows.Controls.ListBox> controle associado a uma lista de fotos.

[!code-xaml[ListBox](~/samples/snippets/desktop-guide/wpf/styles-and-templates-intro/csharp/Window3.xaml#SnippetListBox)]

Atualmente, isso <xref:System.Windows.Controls.ListBox> é semelhante ao seguinte.

![ListBox antes de aplicar o modelo](./media/styles-and-templates-overview/stylingintro-listboxbefore.png "StylingIntro_ListBoxBefore")

A maioria dos controles tem algum tipo de conteúdo, que, frequentemente, vem dos dados aos quais você está se associando. Neste exemplo, os dados estão na lista de fotos. No WPF, você usa um <xref:System.Windows.DataTemplate> para definir a representação visual dos dados. Basicamente, o que você coloca em um <xref:System.Windows.DataTemplate> determina a aparência dos dados no aplicativo renderizado.

Em nosso aplicativo de exemplo, cada `Photo` objeto personalizado tem uma `Source` Propriedade do tipo cadeia de caracteres que especifica o caminho do arquivo da imagem. Atualmente, os objetos de fotos aparecem como caminhos de arquivo.

[!code-csharp[PhotoClass](~/samples/snippets/desktop-guide/wpf/styles-and-templates-intro/csharp/Photo.cs#PhotoClass)]
[!code-vb[PhotoClass](~/samples/snippets/desktop-guide/wpf/styles-and-templates-intro/vb/Photo.vb#PhotoClass)]

Para que as fotos apareçam como imagens, você cria um <xref:System.Windows.DataTemplate> como um recurso.

[!code-xaml[DataTemplate](~/samples/snippets/desktop-guide/wpf/styles-and-templates-intro/csharp/Window4.xaml#SnippetDataTemplate)]

Observe que a <xref:System.Windows.DataTemplate.DataType%2A> propriedade é semelhante à <xref:System.Windows.Style.TargetType%2A> Propriedade do <xref:System.Windows.Style> . Se o <xref:System.Windows.DataTemplate> estiver na seção de recursos, quando você especificar a <xref:System.Windows.DataTemplate.DataType%2A> propriedade para um tipo e omitir um `x:Key` , o <xref:System.Windows.DataTemplate> será aplicado sempre que esse tipo for exibido. Você sempre tem a opção de atribuir o <xref:System.Windows.DataTemplate> com um `x:Key` e, em seguida, defini-lo como um `StaticResource` para propriedades que usam <xref:System.Windows.DataTemplate> tipos, como a <xref:System.Windows.Controls.ItemsControl.ItemTemplate%2A> propriedade ou a <xref:System.Windows.Controls.ContentControl.ContentTemplate%2A> propriedade.

Essencialmente, o <xref:System.Windows.DataTemplate> no exemplo acima define que sempre que houver um `Photo` objeto, ele deve aparecer como um <xref:System.Windows.Controls.Image> dentro de um <xref:System.Windows.Controls.Border> . Com isso <xref:System.Windows.DataTemplate> , nosso aplicativo agora tem esta aparência.

![Imagem de foto](./media/styles-and-templates-overview/stylingintro-photosasimages.png "StylingIntro_PhotosAsImages")

O modelo de modelagem de dados fornece outros recursos. Por exemplo, se você estiver exibindo dados de coleção que contêm outras coleções usando um <xref:System.Windows.Controls.HeaderedItemsControl> tipo como <xref:System.Windows.Controls.Menu> ou <xref:System.Windows.Controls.TreeView> , há o <xref:System.Windows.HierarchicalDataTemplate> . Outro recurso de modelagem de dados é o <xref:System.Windows.Controls.DataTemplateSelector> , que permite que você escolha um <xref:System.Windows.DataTemplate> para usar com base na lógica personalizada. Para obter mais informações, consulte [Visão geral de modelagem dos dados](../../framework/wpf/data/data-templating-overview.md), que oferece uma discussão mais detalhada sobre os diferentes recursos de modelagem de dados.

## <a name="triggers"></a>Gatilhos

Um gatilho define propriedades ou inicia ações, como uma animação, quando um valor da propriedade for alterado ou quando um evento for gerado. <xref:System.Windows.Style>, <xref:System.Windows.Controls.ControlTemplate> e <xref:System.Windows.DataTemplate> All têm uma `Triggers` propriedade que pode conter um conjunto de gatilhos. Há vários tipos de gatilhos.

### <a name="propertytriggers"></a>PropertyTriggers

Um <xref:System.Windows.Trigger> que define valores de propriedade ou inicia ações com base no valor de uma propriedade é chamado de gatilho de propriedade.

Para demonstrar como usar gatilhos de propriedade, você pode fazer cada um <xref:System.Windows.Controls.ListBoxItem> parcialmente transparente, a menos que esteja selecionado. O estilo a seguir define o <xref:System.Windows.UIElement.Opacity%2A> valor de a <xref:System.Windows.Controls.ListBoxItem> para `0.5` . <xref:System.Windows.Controls.ListBoxItem.IsSelected%2A> `true` No entanto, quando a propriedade é <xref:System.Windows.UIElement.Opacity%2A> definida como `1.0` .

[!code-xaml[PropertyTrigger](~/samples/snippets/desktop-guide/wpf/styles-and-templates-intro/csharp/Window5.xaml#SnippetPropertyTrigger)]

Este exemplo usa um <xref:System.Windows.Trigger> para definir um valor de propriedade, mas observe que a <xref:System.Windows.Trigger> classe também tem <xref:System.Windows.TriggerBase.EnterActions%2A> as <xref:System.Windows.TriggerBase.ExitActions%2A> Propriedades e que permitem que um gatilho execute ações.

Observe que a <xref:System.Windows.FrameworkElement.MaxHeight%2A> propriedade de <xref:System.Windows.Controls.ListBoxItem> é definida como `75` . Na ilustração a seguir, o terceiro item é o item selecionado.

![ListView com estilo](./media/styles-and-templates-overview/stylingintro-triggers.png "StylingIntro_triggers")

### <a name="eventtriggers-and-storyboards"></a>EventTriggers e storyboards

Outro tipo de gatilho é o <xref:System.Windows.EventTrigger> , que inicia um conjunto de ações com base na ocorrência de um evento. Por exemplo, os objetos a seguir <xref:System.Windows.EventTrigger> especificam que quando o ponteiro do mouse entra no <xref:System.Windows.Controls.ListBoxItem> , a <xref:System.Windows.FrameworkElement.MaxHeight%2A> propriedade é animada para um valor de `90` um `0.2` segundo período. Quando o mouse se afasta do item, a propriedade retorna para o valor original durante um período de `1` segundos. Observe como não é necessário especificar um <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> valor para a <xref:System.Windows.ContentElement.MouseLeave> animação. Isso ocorre porque a animação é capaz de controlar o valor original.

[!code-xaml[StyleEventTriggers](~/samples/snippets/desktop-guide/wpf/styles-and-templates-intro/csharp/Window6.xaml#SnippetStyleEventTriggers)]

Para obter mais informações, consulte a [visão geral dos storyboards](../../framework/wpf/graphics-multimedia/storyboards-overview.md).

Na ilustração a seguir, o mouse está apontando para o terceiro item.

![Captura de tela de exemplo de estilo](./media/styles-and-templates-overview/stylingintro-eventtriggers.png "StylingIntro_EventTriggers")

### <a name="multitriggers-datatriggers-and-multidatatriggers"></a>MultiTriggers, DataTriggers e MultiDataTriggers

Além de <xref:System.Windows.Trigger> e <xref:System.Windows.EventTrigger> , há outros tipos de gatilhos. <xref:System.Windows.MultiTrigger>permite que você defina valores de propriedade com base em várias condições. Você usa <xref:System.Windows.DataTrigger> e <xref:System.Windows.MultiDataTrigger> quando a propriedade da condição é associada a dados.

## <a name="visual-states"></a>Estados visuais

Os controles estão sempre em um **estado**específico. Por exemplo, quando o mouse se move sobre a superfície de um controle, o controle é considerado em um Estado comum de `MouseOver` . Um controle sem um estado específico é considerado no `Normal` estado comum. Os Estados são divididos em grupos e os Estados mencionados anteriormente fazem parte do grupo de Estados `CommonStates` . A maioria dos controles tem dois grupos `CommonStates` de Estado: e `FocusStates` . De cada grupo de Estados aplicado a um controle, um controle sempre está em um estado de cada grupo, como `CommonStates.MouseOver` e `FocusStates.Unfocused` . No entanto, um controle não pode estar em dois Estados diferentes dentro do mesmo grupo, como `CommonStates.Normal` e `CommonStates.Disabled` . Aqui está uma tabela de Estados que a maioria dos controles reconhece e usa.

| Nome do VisualState | Nome do VisualStateGroup | Descrição |
| ---------------- | --------------------- | ----------- |
| Normal           | CommonStates          | O estado padrão. |
| MouseOver        | CommonStates          | O ponteiro do mouse é posicionado sobre o controle. |
| Pressionado          | CommonStates          | O controle é pressionado. |
| Desabilitado         | CommonStates          | O controle está desabilitado. |
| Focalizado          | FocusStates           | O controle tem foco. |
| Sem foco        | FocusStates           | O controle não tem foco. |

Ao definir um <xref:System.Windows.VisualStateManager?displayProperty=fullName> no elemento raiz de um modelo de controle, você pode disparar animações quando um controle entra em um estado específico. O `VisualStateManager` declara quais combinações de <xref:System.Windows.VisualStateGroup> e <xref:System.Windows.VisualState> observar. Quando o controle entra em um estado observado, a animação definida pelo `VisaulStateManager` é iniciada.

Por exemplo, o código XAML a seguir observa o `CommonStates.MouseOver` estado para animar a cor de preenchimento do elemento chamado `backgroundElement` . Quando o controle retorna para o `CommonStates.Normal` estado, a cor de preenchimento do elemento chamado `backgroundElement` é restaurada.

```xaml
<ControlTemplate x:Key="roundbutton" TargetType="Button">
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup Name="CommonStates">
                <VisualState Name="Normal">
                    <ColorAnimation Storyboard.TargetName="backgroundElement"
                                    Storyboard.TargetProperty="(Shape.Fill).(SolidColorBrush.Color)"
                                    To="{TemplateBinding Background}"
                                    Duration="0:0:0.3"/>
                </VisualState>
                <VisualState Name="MouseOver">
                    <ColorAnimation Storyboard.TargetName="backgroundElement"
                                    Storyboard.TargetProperty="(Shape.Fill).(SolidColorBrush.Color)"
                                    To="Yellow"
                                    Duration="0:0:0.3"/>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        ...
```

Para obter mais informações sobre storyboards, consulte [visão geral de storyboards](../../framework/wpf/graphics-multimedia/storyboards-overview.md).

## <a name="shared-resources-and-themes"></a>Temas e recursos compartilhados

Um aplicativo WPF típico pode ter vários recursos de interface do usuário que são aplicados em todo o aplicativo. Coletivamente, esse conjunto de recursos pode ser considerado o tema para o aplicativo. O WPF dá suporte para empacotamento de recursos de interface do usuário como um tema usando um dicionário de recursos que é encapsulado como a <xref:System.Windows.ResourceDictionary> classe.

Os temas do WPF são definidos usando o mecanismo de modelagem e estilo que o WPF expõe para personalizar os elementos visuais de qualquer elemento.

Os recursos de tema do WPF são armazenados em dicionários de recursos inseridos. Esses dicionários de recursos devem ser inseridos em um assembly assinado; podem ser inseridos no mesmo assembly, como o próprio código, ou em um assembly lado a lado. Por PresentationFramework.dll, o assembly que contém controles WPF, recursos de tema estão em uma série de assemblies lado a lado.

O tema torna-se o último local para procurar o estilo de um elemento. Normalmente, a pesquisa começará orientando a árvore de elementos procurando um recurso apropriado e, em seguida, examinará a coleção de recursos de aplicativo e, finalmente, consultará o sistema. Isso dá aos desenvolvedores de aplicativos a oportunidade de redefinir o estilo de qualquer objeto no nível da árvore ou do aplicativo antes de alcançar o tema.

Você pode definir dicionários de recursos como arquivos individuais que permitem reutilizar um tema em vários aplicativos. Você também pode criar temas permutáveis definindo vários dicionários de recursos que fornecem os mesmos tipos de recursos, mas com valores diferentes. A redefinição desses estilos ou de outros recursos no nível do aplicativo é a abordagem recomendada para a capa de um aplicativo.

Para compartilhar um conjunto de recursos, incluindo estilos e modelos, entre aplicativos, você pode criar um arquivo XAML e definir um <xref:System.Windows.ResourceDictionary> que inclua referência a um `shared.xaml` arquivo.

```xaml
<ResourceDictionary.MergedDictionaries>
  <ResourceDictionary Source="Shared.xaml" />
</ResourceDictionary.MergedDictionaries>
```

É o compartilhamento de `shared.xaml` , que, por sua vez, define um <xref:System.Windows.ResourceDictionary> que contém um conjunto de recursos de estilo e pincel, que permite que os controles em um aplicativo tenham uma aparência consistente.

Para obter mais informações, consulte [dicionários de Recursos mesclados](../../framework/wpf/advanced/merged-resource-dictionaries.md).

Se você estiver criando um tema para seu controle personalizado, consulte a seção **definindo recursos no nível do tema** da [visão geral de criação de controles](../../framework/wpf/controls/control-authoring-overview.md#defining-resources-at-the-theme-level).

## <a name="see-also"></a>Veja também

- [URIs "pack://" no WPF](../../framework/wpf/app-development/pack-uris-in-wpf.md)
- [Como: localizar elementos gerados por ControlTemplate](../../framework/wpf/controls/how-to-find-controltemplate-generated-elements.md)
- [Localizar elementos gerados pelo DataTemplate](../../framework/wpf/data/how-to-find-datatemplate-generated-elements.md)
