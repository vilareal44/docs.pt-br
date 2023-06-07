---
title: Criar um modelo no WPF-.NET desktop
description: Saiba como criar e referenciar um modelo de controle no Windows Presentation Foundation e no .NET Core.
author: adegeo
ms.author: adegeo
ms.date: 11/15/2019
no-loc:
- <Window>
- <ControlTemplate>
- <Ellipse>
- <ContentPresenter>
- <Trigger>
- <Setter>
- <PropertyTrigger>
- <Grid>
- <VisualStateManager.VisualStateGroups>
- <VisualStateGroup>
- <VisualState>
- <Storyboard>
- SizeToContent
- MinWidth
- TargetType
- Title
helpviewer_keywords:
- control contract [WPF]
- controls [WPF], visual structure changes
- ControlTemplate [WPF], customizing for existing controls
- skinning controls [WPF]
- controls [WPF], appearance specified by state
- templates [WPF], custom for existing controls
ms.openlocfilehash: c372659676b450cde789c96e45c7ec5de2aea194
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325731"
---
# <a name="create-a-template-for-a-control"></a>Criar um modelo para um controle

Com o Windows Presentation Foundation (WPF), você pode personalizar uma estrutura visual do controle existente e um comportamento com seu próprio modelo reutilizável. Os modelos podem ser aplicados globalmente ao seu aplicativo, janelas e páginas ou diretamente aos controles. A maioria dos cenários que exigem a criação de um novo controle pode ser abordada em vez de criar um novo modelo para um controle existente.

[!INCLUDE [desktop guide under construction](../../../includes/desktop-guide-preview-note.md)]

Neste artigo, você vai explorar a criação de uma nova <xref:System.Windows.Controls.ControlTemplate> para o <xref:System.Windows.Controls.Button> controle.

## <a name="when-to-create-a-controltemplate"></a>Quando criar um ControlTemplate

Os controles têm muitas propriedades, como <xref:System.Windows.Controls.Border.Background%2A> , <xref:System.Windows.Controls.Control.Foreground%2A> e <xref:System.Windows.Controls.Control.FontFamily%2A> . Essas propriedades controlam diferentes aspectos da aparência do controle, mas as alterações que você pode fazer definindo essas propriedades são limitadas. Por exemplo, você pode definir a <xref:System.Windows.Controls.Control.Foreground%2A> propriedade como azul e <xref:System.Windows.Controls.Control.FontStyle%2A> itálico em um <xref:System.Windows.Controls.CheckBox> . Quando você quiser personalizar a aparência do controle além do que definir as outras propriedades no controle pode fazer, crie um <xref:System.Windows.Controls.ControlTemplate> .

Na maioria das interfaces de usuário, um botão tem a mesma aparência geral: um retângulo com algum texto. Se você quisesse criar um botão arredondado, poderia criar um novo controle herdado do botão ou recriar a funcionalidade do botão. Além disso, o novo controle de usuário forneceria o Visual circular.

Você pode evitar a criação de novos controles Personalizando o layout visual de um controle existente. Com um botão arredondado, você cria um <xref:System.Windows.Controls.ControlTemplate> com o layout visual desejado.

Por outro lado, se você precisar de um controle com novas funcionalidades, propriedades diferentes e novas configurações, você criará um novo <xref:System.Windows.Controls.UserControl> .

## <a name="prerequisites"></a>Pré-requisitos

Crie um novo aplicativo WPF e, em *MainWindow. XAML* (ou outra janela de sua escolha) defina as seguintes propriedades no **\<Window>** elemento:

|     |     |
| --- | --- |
| **Title**         | `Template Intro Sample` |
| **SizeToContent** | `WidthAndHeight` |
| **MinWidth**      | `250` |

Defina o conteúdo do **\<Window>** elemento como o XAML a seguir:

[!code-xaml[Initial](~/samples/snippets/desktop-guide/wpf/styles-templates-create-apply-template/csharp/Window1.xaml#Initial)]

No final, o arquivo *MainWindow. XAML* deve ser semelhante ao seguinte:

[!code-xaml[InitialWhole](~/samples/snippets/desktop-guide/wpf/styles-templates-create-apply-template/csharp/Window1.xaml#InitialWhole)]

Se você executar o aplicativo, ele será semelhante ao seguinte:

![Janela do WPF com dois botões sem estilo](media/create-apply-template/unstyled-button.png)

## <a name="create-a-controltemplate"></a>Criar um ControlTemplate

A maneira mais comum de declarar um <xref:System.Windows.Controls.ControlTemplate> é como um recurso na `Resources` seção em um arquivo XAML. Como os modelos são recursos, eles obedecem às mesmas regras de escopo que se aplicam a todos os recursos. Em suma, o local em que você declara um modelo afeta o local em que o modelo pode ser aplicado. Por exemplo, se você declarar o modelo no elemento raiz do arquivo XAML de definição de aplicativo, o modelo poderá ser usado em qualquer lugar em seu aplicativo. Se você definir o modelo em uma janela, somente os controles nessa janela poderão usar o modelo.

Para começar, adicione um `Window.Resources` elemento ao seu arquivo *MainWindow. XAML* :

[!code-xaml[WindowResStart](~/samples/snippets/desktop-guide/wpf/styles-templates-create-apply-template/csharp/Window2.xaml#WindowResStart)]

Crie um novo **\<ControlTemplate>** com as seguintes propriedades definidas:

|     |     |
| --- | --- |
| **x:Key**         | `roundbutton` |
| **TargetType**    | `Button` |

Este modelo de controle será simples:

- um elemento raiz para o controle, um<xref:System.Windows.Controls.Grid>
- um <xref:System.Windows.Shapes.Ellipse> para desenhar a aparência arredondada do botão
- um <xref:System.Windows.Controls.ContentPresenter> para exibir o conteúdo do botão especificado pelo usuário

[!code-xaml[ControlTemplate](~/samples/snippets/desktop-guide/wpf/styles-templates-create-apply-template/csharp/Window3.xaml#ControlTemplate)]

### <a name="templatebinding"></a>TemplateBinding

Quando você cria um novo <xref:System.Windows.Controls.ControlTemplate> , ainda convém usar as propriedades públicas para alterar a aparência do controle. A extensão de marcação [TemplateBinding](../../framework/wpf/advanced/templatebinding-markup-extension.md) associa uma propriedade de um elemento que está no <xref:System.Windows.Controls.ControlTemplate> a uma propriedade pública que é definida pelo controle. Quando você usa um [TemplateBinding](../../framework/wpf/advanced/templatebinding-markup-extension.md), você habilita as propriedades no controle para atuar como parâmetros para o modelo. Ou seja, quando uma propriedade em um controle é definida, esse valor é passado para o elemento que contém a [TemplateBinding](../../framework/wpf/advanced/templatebinding-markup-extension.md).

### <a name="ellipse"></a>Elipse

Observe que as **:::no-loc text="Fill":::** **:::no-loc text="Stroke":::** Propriedades e do **\<Ellipse>** elemento estão associadas às propriedades e ao controle <xref:System.Windows.Controls.Control.Foreground> <xref:System.Windows.Controls.Control.Background> .

### <a name="contentpresenter"></a>ContentPresenter

Um [\<ContentPresenter>](xref:System.Windows.Controls.ContentPresenter) elemento também é adicionado ao modelo. Como esse modelo foi projetado para um botão, leve em consideração que o botão é herdado de <xref:System.Windows.Controls.ContentControl> . O botão apresenta o conteúdo do elemento. Você pode definir qualquer coisa dentro do botão, como texto sem formatação ou até mesmo outro controle. Os dois itens a seguir são botões válidos:

```xaml
<Button>My Text</Button>

<!-- and -->

<Button>
    <CheckBox>Checkbox in a button</CheckBox>
</Button>
```

Nos dois exemplos anteriores, o texto e a caixa de seleção são definidos como a propriedade [Button. Content](xref:System.Windows.Controls.ContentControl.Content) . O que for definido como o conteúdo pode ser apresentado por meio de um **\<ContentPresenter>** , que é o que o modelo faz.

Se o <xref:System.Windows.Controls.ControlTemplate> for aplicado a um <xref:System.Windows.Controls.ContentControl> tipo, como um `Button` , um <xref:System.Windows.Controls.ContentPresenter> será procurado na árvore de elementos. Se `ContentPresenter` for encontrado, o modelo associará automaticamente a propriedade do controle <xref:System.Windows.Controls.ContentControl.Content> ao `ContentPresenter` .

## <a name="use-the-template"></a>Usar o modelo

Localize os botões que foram declarados no início deste artigo.

[!code-xaml[Initial](~/samples/snippets/desktop-guide/wpf/styles-templates-create-apply-template/csharp/Window1.xaml#Initial)]

Defina a propriedade do segundo botão <xref:System.Windows.Controls.Control.Template> para o `roundbutton` recurso:

[!code-xaml[StyledButton](~/samples/snippets/desktop-guide/wpf/styles-templates-create-apply-template/csharp/Window3.xaml#StyledButton)]

Se você executar o projeto e examinar o resultado, verá que o botão tem um plano de fundo arredondado.

![Janela do WPF com um botão de modelo oval](media/create-apply-template/styled-button.png)

Você deve ter notado que o botão não é um círculo, mas está distorcido. Devido à maneira como o **\<Ellipse>** elemento funciona, ele sempre se expande para preencher o espaço disponível. Torne o círculo uniforme alterando as **:::no-loc text="width":::** Propriedades e do botão **:::no-loc text="height":::** para o mesmo valor:

[!code-xaml[StyledButtonSize](~/samples/snippets/desktop-guide/wpf/styles-templates-create-apply-template/csharp/Window3.xaml#StyledButtonSize)]

![Janela do WPF com um botão circular de modelo](media/create-apply-template/styled-uniform-button.png)

## <a name="add-a-trigger"></a>Adicionar um gatilho

Embora um botão com um modelo aplicado pareça diferente, ele comporta-se como qualquer outro botão. Se você pressionar o botão, o <xref:System.Windows.Controls.Primitives.ButtonBase.Click> evento será acionado. No entanto, você pode ter notado que quando você move o mouse sobre o botão, os visuais do botão não são alterados. Essas interações visuais são todas definidas pelo modelo.

Com o evento dinâmico e os sistemas de propriedades fornecidos pelo WPF, você pode observar uma propriedade específica para um valor e reestilizar o modelo quando apropriado. Neste exemplo, você observará a propriedade do botão <xref:System.Windows.UIElement.IsMouseOver> . Quando o mouse estiver sobre o controle, estilizar o **\<Ellipse>** com uma nova cor. Esse tipo de gatilho é conhecido como um *PropertyTrigger*.

Para que isso funcione, você precisará adicionar um nome ao **\<Ellipse>** que você possa referenciar. Dê a ele o nome de **backgroundelement**.

[!code-xaml[EllipseName](~/samples/snippets/desktop-guide/wpf/styles-templates-create-apply-template/csharp/Window4.xaml#EllipseName)]

Em seguida, adicione um novo <xref:System.Windows.Trigger> à coleção [ControlTemplate. Triggers](xref:System.Windows.Controls.ControlTemplate.Triggers) . O gatilho observará o `IsMouseOver` evento para o valor `true` .

[!code-xaml[ControlTemplate](~/samples/snippets/desktop-guide/wpf/styles-templates-create-apply-template/csharp/Window4.xaml?name=ControlTemplate&highlight=6-10)]

Em seguida, adicione um **\<Setter>** ao **\<Trigger>** que altera a propriedade **Fill** de **\<Ellipse>** para uma nova cor.

[!code-xaml[MouseOver](~/samples/snippets/desktop-guide/wpf/styles-templates-create-apply-template/csharp/Window5.xaml#MouseOver)]

Execute o projeto. Observe que, quando você move o mouse sobre o botão, a cor das **\<Ellipse>** alterações.

![o mouse se move sobre o botão do WPF para alterar a cor do preenchimento](media/create-apply-template/mouse-move-over-button.gif)

## <a name="use-a-visualstate"></a>Usar um VisualState

Os Estados visuais são definidos e disparados por um controle. Por exemplo, quando o mouse é movido sobre o controle, o `CommonStates.MouseOver` estado é disparado. Você pode animar as alterações de propriedade com base no estado atual do controle. Na seção anterior, um **\<PropertyTrigger>** foi usado para alterar o primeiro plano do botão para `AliceBlue` quando a `IsMouseOver` Propriedade era `true` . Em vez disso, crie um estado visual que anime a alteração dessa cor, fornecendo uma transição suave. Para obter mais informações sobre *VisualStates*, consulte [estilos e modelos no WPF](../fundamentals/styles-templates-overview.md#visual-states).

Para converter o **\<PropertyTrigger>** em um estado visual animado, primeiro remova o **\<ControlTemplate.Triggers>** elemento do modelo.

[!code-xaml[CleanTemplate](~/samples/snippets/desktop-guide/wpf/styles-templates-create-apply-template/csharp/Window5.xaml#CleanTemplate)]

Em seguida, na **\<Grid>** raiz do modelo de controle, adicione o **\<VisualStateManager.VisualStateGroups>** elemento com um **\<VisualStateGroup>** para `CommonStates` . Defina dois Estados `Normal` e `MouseOver` .

[!code-xaml[VisualState](~/samples/snippets/desktop-guide/wpf/styles-templates-create-apply-template/csharp/Window6.xaml#VisualState)]

Todas as animações definidas em um **\<VisualState>** são aplicadas quando esse estado é disparado. Crie animações para cada Estado. As animações são colocadas dentro de um **\<Storyboard>** elemento. Para obter mais informações sobre storyboards, consulte [visão geral de storyboards](../../framework/wpf/graphics-multimedia/storyboards-overview.md).

- Normal

  Esse estado anima o preenchimento da elipse, restaurando-o para a `Background` cor do controle.

  [!code-xaml[NormalState](~/samples/snippets/desktop-guide/wpf/styles-templates-create-apply-template/csharp/Window6.xaml#NormalState)]

- MouseOver

  Esse estado anima a cor da elipse `Background` com uma nova cor: `Yellow` .

  [!code-xaml[MouseOverState](~/samples/snippets/desktop-guide/wpf/styles-templates-create-apply-template/csharp/Window6.xaml#MouseOverState)]

Agora, o **\<ControlTemplate>** deve se parecer com o seguinte.

[!code-xaml[FinalTemplate](~/samples/snippets/desktop-guide/wpf/styles-templates-create-apply-template/csharp/Window7.xaml#FinalTemplate)]

Execute o projeto. Observe que, quando você move o mouse sobre o botão, a cor das **\<Ellipse>** animações.

![o mouse se move sobre o botão do WPF para alterar a cor do preenchimento](media/create-apply-template/mouse-move-over-button-visualstate.gif)

## <a name="next-steps"></a>Próximas etapas

- [Criar um estilo para um controle no WPF](../fundamentals/styles-templates-create-apply-style.md)
- [Estilos e modelos no WPF](../fundamentals/styles-templates-overview.md)
- [Visão geral dos recursos XAML](../fundamentals/xaml-resources-define.md)
