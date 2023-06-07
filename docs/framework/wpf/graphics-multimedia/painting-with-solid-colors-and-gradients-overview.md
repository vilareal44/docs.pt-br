---
title: Visão geral da pintura com cores sólidas e gradientes
description: Saiba como usar objetos para pintar com cores sólidas, gradientes lineares e gradientes radiais no Windows Presentation Foundation (WPF).
ms.date: 03/30/2017
helpviewer_keywords:
- solid colors [WPF], painting with
- painting with gradients [WPF]
- gradients [WPF], painting with
- brushes [WPF], painting with solid colors
- brushes [WPF], painting with gradients
- painting with solid colors [WPF]
ms.assetid: f5b182f3-c5c7-4cbe-9f2f-65e690d08255
ms.openlocfilehash: 957593d758afda06db106c99f6695294d4f84f73
ms.sourcegitcommit: b6a1869f97a37f11a68c90afde1a520a6887dcbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853662"
---
# <a name="painting-with-solid-colors-and-gradients-overview"></a>Visão geral da pintura com cores sólidas e gradientes

Este tópico descreve como usar <xref:System.Windows.Media.SolidColorBrush> objetos, <xref:System.Windows.Media.LinearGradientBrush> e <xref:System.Windows.Media.RadialGradientBrush> para pintar com cores sólidas, gradientes lineares e gradientes radiais.

<a name="solidcolor"></a>

## <a name="painting-an-area-with-a-solid-color"></a>Pintar uma área com uma cor sólida

Uma das operações mais comuns em qualquer plataforma é pintar uma área com uma sólida <xref:System.Windows.Media.Color> . Para realizar essa tarefa, [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] o fornece a <xref:System.Windows.Media.SolidColorBrush> classe. As seções a seguir descrevem as diferentes maneiras de pintar com um <xref:System.Windows.Media.SolidColorBrush> .

<a name="solidcolorinxaml"></a>

### <a name="using-a-solidcolorbrush-in-xaml"></a>Usando um SolidColorBrush em "XAML"

Para pintar uma área com uma cor sólida em [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)], use uma das opções a seguir.

- Selecione um pincel de cor sólida predefinida pelo nome.  Por exemplo, você pode definir um botão <xref:System.Windows.Controls.Control.Background%2A> como "vermelho" ou "MediumBlue".  Para obter uma lista de outros pincéis de cores sólidas predefinidos, consulte as propriedades estáticas da <xref:System.Windows.Media.Brushes> classe. A seguir, é mostrado um exemplo.

  [!code-xaml[BrushOverviewExamples_snip#SolidColorBrushNamedColor1XAML](~/samples/snippets/xaml/VS_Snippets_Wpf/BrushOverviewExamples_snip/XAML/SolidColorBrushExample.xaml#solidcolorbrushnamedcolor1xaml)]

- Escolha uma cor da paleta de cores de 32 bits, especificando as quantidades de vermelho, verde e azul para combinar em uma única cor sólida.  O formato para especificar uma cor da paleta de 32 bits é "*#rrggbb*", em que *rr* é um número hexadecimal de dois dígitos que especifica a quantidade relativa de vermelho, *gg* especifica a quantidade de verde e *bb* especifica a quantidade de azul.  Além disso, a cor pode ser especificada como "#*aarrggbb*" em que *aa* especifica o valor *alfa*, ou a transparência, da cor. Essa abordagem permite que você crie cores parcialmente transparentes.  No exemplo a seguir, o <xref:System.Windows.Controls.Control.Background%2A> de a <xref:System.Windows.Controls.Button> é definido como vermelho totalmente opaco usando notação hexadecimal.

  [!code-xaml[BrushOverviewExamples_snip#SolidColorBrushHex1XAML](~/samples/snippets/xaml/VS_Snippets_Wpf/BrushOverviewExamples_snip/XAML/SolidColorBrushExample.xaml#solidcolorbrushhex1xaml)]

- Use a sintaxe da marca de propriedade para descrever um <xref:System.Windows.Media.SolidColorBrush> . Essa sintaxe é mais detalhada, mas permite que você especifique configurações adicionais, como a opacidade do pincel. No exemplo a seguir, as <xref:System.Windows.Controls.Control.Background%2A> Propriedades de dois <xref:System.Windows.Controls.Button> elementos são definidas como vermelho totalmente opaco. A primeira cor do pincel é descrita usando um nome de cor predefinido. A segunda cor do pincel é descrita usando notação hexadecimal.

  [!code-xaml[BrushOverviewExamples_snip#SolidColorBrushPropertyTag1XAML](~/samples/snippets/xaml/VS_Snippets_Wpf/BrushOverviewExamples_snip/XAML/SolidColorBrushExample.xaml#solidcolorbrushpropertytag1xaml)]

<a name="solidcolorsincode"></a>

### <a name="painting-with-a-solidcolorbrush-in-code"></a>Pintando com um SolidColorBrush no código

Para pintar uma área com uma cor sólida no código, use uma das opções a seguir.

- Use um dos pincéis predefinidos fornecidos pela <xref:System.Windows.Media.Brushes> classe. No exemplo a seguir, o <xref:System.Windows.Controls.Control.Background%2A> de a <xref:System.Windows.Controls.Button> é definido como <xref:System.Windows.Media.Brushes.Red%2A> .

  [!code-csharp[BrushOverviewExamples_snip#SolidColorBrushPredefinedBrush1CSharp](~/samples/snippets/csharp/VS_Snippets_Wpf/BrushOverviewExamples_snip/CSharp/SolidColorBrushExample.cs#solidcolorbrushpredefinedbrush1csharp)]

- Crie um <xref:System.Windows.Media.SolidColorBrush> e defina sua <xref:System.Windows.Media.SolidColorBrush.Color%2A> propriedade usando uma <xref:System.Windows.Media.Color> estrutura. Você pode usar uma cor predefinida da <xref:System.Windows.Media.Colors> classe ou pode criar um <xref:System.Windows.Media.Color> usando o método estático <xref:System.Windows.Media.Color.FromArgb%2A> .

  O exemplo a seguir mostra como definir a <xref:System.Windows.Media.SolidColorBrush.Color%2A> propriedade de um <xref:System.Windows.Media.SolidColorBrush> usando uma cor predefinida.

  [!code-csharp[BrushOverviewExamples_snip#SolidColorBrushPredefinedColor1CSharp](~/samples/snippets/csharp/VS_Snippets_Wpf/BrushOverviewExamples_snip/CSharp/SolidColorBrushExample.cs#solidcolorbrushpredefinedcolor1csharp)]

O estático <xref:System.Windows.Media.Color.FromArgb%2A> permite que você especifique os valores Alfa, vermelho, verde e azul da cor. O intervalo típico para cada um desses valores é de 0 a 255. Por exemplo, um valor alfa igual a 0 indica que uma cor é completamente transparente, enquanto um valor de 255 indica que a cor é completamente opaca. Da mesma forma, um valor de vermelho igual a 0 indica que uma cor não contém nenhum vermelho, enquanto um valor de 255 indica que uma cor tem a quantidade máxima possível de vermelho.  No exemplo a seguir, a cor do pincel é descrita especificando valores alfa, vermelho, verde e azul.

[!code-csharp[BrushOverviewExamples_snip#SolidColorBrushfromArgbExample1CSharp](~/samples/snippets/csharp/VS_Snippets_Wpf/BrushOverviewExamples_snip/CSharp/SolidColorBrushExample.cs#solidcolorbrushfromargbexample1csharp)]

Para obter outras maneiras de especificar a cor, consulte o <xref:System.Windows.Media.Color> tópico de referência.

<a name="gradient"></a>

## <a name="painting-an-area-with-a-gradient"></a>Pintando uma área com um gradiente

Um pincel de gradiente pinta uma área com várias cores que se mesclam ao longo de um eixo. Você pode usá-los para criar impressões de luz e sombra, dando uma aparência tridimensional aos seus controles. Você também pode usá-los para simular vidro, cromado, água e outras superfícies suaves.  [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]fornece dois tipos de pincéis de gradiente: <xref:System.Windows.Media.LinearGradientBrush> e <xref:System.Windows.Media.RadialGradientBrush> .

<a name="lineargradientbrush"></a>

## <a name="linear-gradients"></a>Gradientes lineares

Um <xref:System.Windows.Media.LinearGradientBrush> pinta uma área com um gradiente definido ao longo de uma linha, o *eixo gradiente*.  Você especifica as cores do gradiente e sua localização ao longo do eixo gradiente usando <xref:System.Windows.Media.GradientStop> objetos.  Você também pode modificar o eixo do gradiente, o que permite criar gradientes horizontais e verticais e inverter a direção do gradiente. O eixo de gradiente é descrito na próxima seção. Por padrão, é criado um gradiente diagonal.

O exemplo a seguir mostra o código que cria um gradiente linear com quatro cores.

[!code-xaml[GradientBrushExamples_snip#DiagonalGradient1XAML](~/samples/snippets/xaml/VS_Snippets_Wpf/GradientBrushExamples_snip/XAML/LinearGradientBrushExample.xaml#diagonalgradient1xaml)]

[!code-csharp[GradientBrushExamples_snip#DiagonalGradient1CSharp](~/samples/snippets/csharp/VS_Snippets_Wpf/GradientBrushExamples_snip/CSharp/LinearGradientBrushExample.cs#diagonalgradient1csharp)]

Esse código produz o gradiente a seguir:

![Um gradiente linear diagonal](./media/wcpsdk-graphicsmm-diaglgradient-nolabel.jpg "wcpsdk_graphicsmm_diaglgradient_nolabel")

> [!NOTE]
> Os exemplos de gradiente neste tópico usam o sistema de coordenadas padrão para definir pontos de partida e pontos de extremidade. O sistema de coordenadas padrão é relativo uma caixa delimitadora: 0 indica 0 por cento da caixa delimitadora e 1 indica 100 por cento da caixa delimitadora. Você pode alterar esse sistema de coordenadas definindo a <xref:System.Windows.Media.GradientBrush.MappingMode%2A> propriedade para o valor <xref:System.Windows.Media.BrushMappingMode.Absolute> . Um sistema de coordenadas absoluto não é relativo a uma caixa delimitadora. Os valores são interpretados diretamente no espaço local.

O <xref:System.Windows.Media.GradientStop> é o bloco de construção básico de um pincel de gradiente.  Uma parada de gradiente especifica um <xref:System.Windows.Media.GradientStop.Color%2A> em um ao <xref:System.Windows.Media.GradientStop.Offset%2A> longo do eixo de gradiente.

- A propriedade da parada do gradiente <xref:System.Windows.Media.GradientStop.Color%2A> especifica a cor da parada do gradiente. Você pode definir a cor usando uma cor predefinida (fornecida pela <xref:System.Windows.Media.Colors> classe) ou especificando ScRGB ou valores ARGB. Em [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)], você também pode usar a notação hexadecimal para descrever uma cor. Para obter mais informações, consulte a <xref:System.Windows.Media.Color> estrutura.

- A propriedade da parada do gradiente <xref:System.Windows.Media.GradientStop.Offset%2A> especifica a posição da cor da parada do gradiente no eixo do gradiente. O deslocamento é um <xref:System.Double> intervalo que varia de 0 a 1. Quanto mais próximo de 0 for o valor de deslocamento da marca de gradiente, mais próxima a cor estará do início do gradiente. Quanto mais próximo de 1 for o valor de deslocamento da marca do gradiente, mais próxima a cor estará do final do gradiente.

A cor de cada ponto entre as marcas do gradiente são linearmente interpoladas como uma combinação da cor especificada pelas duas marcas de gradiente associadas. A ilustração a seguir realça as marcas de gradiente do exemplo anterior. Os círculos marcam a posição das marcas de gradiente e uma linha tracejada mostra o eixo do gradiente.

![Interrupções de gradiente em um gradiente linear](./media/wcpsdk-graphicsmm-4gradientstops.png "wcpsdk_graphicsmm_4gradientstops")

A primeira marca de gradiente especifica a cor amarela em um deslocamento de `0.0`.  A segunda marca de gradiente especifica a cor vermelha em um deslocamento de `0.25`.  Os pontos entre essas duas marcas mudam gradualmente de amarelo para vermelho conforme você move da esquerda para a direita ao longo do eixo do gradiente.  A terceira marca de gradiente especifica a cor azul em um deslocamento de `0.75`.  Os pontos entre a segunda e a terceira marca de gradiente mudam gradualmente de vermelho para azul. A quarta marca de gradiente especifica a cor verde-limão em um deslocamento de `1.0`. Os pontos entre a terceira e a quarta marca de gradiente mudam gradualmente de azul para verde-limão.

<a name="gradientaxis"></a>

### <a name="the-gradient-axis"></a>O eixo do gradiente

Conforme mencionado anteriormente, as marcas de gradiente de um pincel de gradiente linear são posicionadas ao longo de uma linha, o eixo do gradiente. Você pode alterar a orientação e o tamanho da linha usando as <xref:System.Windows.Media.LinearGradientBrush.StartPoint%2A> Propriedades e do pincel <xref:System.Windows.Media.LinearGradientBrush.EndPoint%2A> . Ao manipular o pincel <xref:System.Windows.Media.LinearGradientBrush.StartPoint%2A> e <xref:System.Windows.Media.LinearGradientBrush.EndPoint%2A> , você pode criar gradientes horizontais e verticais, reverter a direção do gradiente, condensar a disseminação do gradiente e muito mais.

Por padrão, o pincel de gradiente linear <xref:System.Windows.Media.LinearGradientBrush.StartPoint%2A> e <xref:System.Windows.Media.LinearGradientBrush.EndPoint%2A> é relativo à área que está sendo pintada. O ponto (0,0) representa o canto superior esquerdo da área que está sendo pintada e (1,1) representa o canto inferior direito da área que está sendo pintada. O padrão <xref:System.Windows.Media.LinearGradientBrush.StartPoint%2A> de a <xref:System.Windows.Media.LinearGradientBrush> é (0, 0) e seu padrão <xref:System.Windows.Media.LinearGradientBrush.EndPoint%2A> é (1, 1), que cria um gradiente diagonal começando no canto superior esquerdo e estendendo para o canto inferior direito da área que está sendo pintada. A ilustração a seguir mostra o eixo gradiente de um pincel de gradiente linear com padrão <xref:System.Windows.Media.LinearGradientBrush.StartPoint%2A> e <xref:System.Windows.Media.LinearGradientBrush.EndPoint%2A> .

![Eixo gradiente para um gradiente linear diagonal](./media/wcpsdk-graphicsmm-diagonalgradientaxis.png "wcpsdk_graphicsmm_diagonalgradientaxis")

O exemplo a seguir mostra como criar um gradiente horizontal especificando o pincel <xref:System.Windows.Media.LinearGradientBrush.StartPoint%2A> e <xref:System.Windows.Media.LinearGradientBrush.EndPoint%2A> . Observe que as interrupções de gradiente são as mesmas dos exemplos anteriores; simplesmente alterando <xref:System.Windows.Media.LinearGradientBrush.StartPoint%2A> e <xref:System.Windows.Media.LinearGradientBrush.EndPoint%2A> , o gradiente foi alterado de diagonal para horizontal.

[!code-xaml[GradientBrushExamples_snip#HorizontalGradient1XAML](~/samples/snippets/xaml/VS_Snippets_Wpf/GradientBrushExamples_snip/XAML/LinearGradientBrushExample.xaml#horizontalgradient1xaml)]

[!code-csharp[GradientBrushExamples_snip#HorizontalGradient1CSharp](~/samples/snippets/csharp/VS_Snippets_Wpf/GradientBrushExamples_snip/CSharp/LinearGradientBrushExample.cs#horizontalgradient1csharp)]

A ilustração a seguir mostra o gradiente que é criado. O eixo do gradiente é marcado com uma linha tracejada e as marcas de gradiente são marcadas com círculos.

![Eixo gradiente para um gradiente linear horizontal](./media/wcpsdk-graphicsmm-horizontalgradient.jpg "wcpsdk_graphicsmm_horizontalgradient")

O exemplo a seguir mostra como criar um gradiente vertical.

[!code-xaml[GradientBrushExamples_snip#VerticalGradient1XAML](~/samples/snippets/xaml/VS_Snippets_Wpf/GradientBrushExamples_snip/XAML/LinearGradientBrushExample.xaml#verticalgradient1xaml)]

[!code-csharp[GradientBrushExamples_snip#VerticalGradient1CSharp](~/samples/snippets/csharp/VS_Snippets_Wpf/GradientBrushExamples_snip/CSharp/LinearGradientBrushExample.cs#verticalgradient1csharp)]

A ilustração a seguir mostra o gradiente que é criado. O eixo do gradiente é marcado com uma linha tracejada e as marcas de gradiente são marcadas com círculos.

![Eixo de gradiente para um gradiente vertical](./media/wcpsdk-graphicsmm-verticalgradient.jpg "wcpsdk_graphicsmm_verticalgradient")

<a name="radialgradients"></a>

## <a name="radial-gradients"></a>Gradientes radiais

Como um <xref:System.Windows.Media.LinearGradientBrush> , um <xref:System.Windows.Media.RadialGradientBrush> pinta uma área com cores que se misturam junto com um eixo. Os exemplos anteriores mostraram como o eixo de um pincel de gradiente linear é uma linha reta. O eixo de um pincel de gradiente radial é definido por um círculo e suas cores "irradiam" para fora de sua origem.

No exemplo a seguir, um pincel de gradiente radial é usado para pintar o interior de um retângulo.

[!code-xaml[GradientBrushExamples_snip#RadialGradient1XAML](~/samples/snippets/xaml/VS_Snippets_Wpf/GradientBrushExamples_snip/XAML/RadialGradientBrushExample.xaml#radialgradient1xaml)]

[!code-csharp[GradientBrushExamples_snip#RadialGradient1CSharp](~/samples/snippets/csharp/VS_Snippets_Wpf/GradientBrushExamples_snip/CSharp/RadialGradientBrushExample.cs#radialgradient1csharp)]

A ilustração a seguir mostra o gradiente criado no exemplo anterior. As marcas de gradiente do pincel foram realçadas. Observe que, embora os resultados sejam diferentes, as marcas de gradiente neste exemplo são idênticas às marcas de gradiente nos exemplos anteriores de pincel de gradiente linear.

![Interrupções de gradiente em um gradiente radial](./media/wcpsdk-graphicsmm-4gradientstops-rg.png "wcpsdk_graphicsmm_4gradientstops_rg")

O <xref:System.Windows.Media.RadialGradientBrush.GradientOrigin%2A> especifica o ponto inicial do eixo de gradiente de um pincel de gradiente radial. O eixo do gradiente irradia da origem do gradiente para o círculo do gradiente. O círculo de gradiente de um pincel é definido por suas <xref:System.Windows.Media.RadialGradientBrush.Center%2A> <xref:System.Windows.Media.RadialGradientBrush.RadiusX%2A> Propriedades, e <xref:System.Windows.Media.RadialGradientBrush.RadiusY%2A> .

A ilustração a seguir mostra vários gradientes radiais <xref:System.Windows.Media.RadialGradientBrush.GradientOrigin%2A> com <xref:System.Windows.Media.RadialGradientBrush.Center%2A> configurações diferentes,, <xref:System.Windows.Media.RadialGradientBrush.RadiusX%2A> e <xref:System.Windows.Media.RadialGradientBrush.RadiusY%2A> .

![Configurações de RadialGradientBrush](./media/wcpsdk-graphicsmm-originscirclesandradii.gif "wcpsdk_graphicsmm_originscirclesandradii") RadialGradientBrushes com configurações diferentes de GradientOrigin, Center, RadiusX e RadiusY.

<a name="specifyinggradientcolors"></a>

## <a name="specifying-transparent-or-partially-transparent-gradient-stops"></a>Especificando marcas de gradiente transparentes ou parcialmente transparentes

Como as paradas de gradiente não fornecem uma propriedade Opacity, você deve especificar o canal alfa de cores usando a notação hexadecimal ARGB na marcação ou usar o <xref:System.Windows.Media.Color.FromScRgb%2A?displayProperty=nameWithType> método para criar interrupções de gradiente que sejam transparentes ou parcialmente transparentes. As seções a seguir explicam como criar marcas de gradiente parcialmente transparentes em [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] e no código.

<a name="argbsyntax"></a>

### <a name="specifying-color-opacity-in-xaml"></a>Especificando a opacidade das cores em "XAML"

No [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] , você usa a notação HEXADECIMAL ARGB para especificar a opacidade de cores individuais. A notação hexadecimal ARGB usa a seguinte sintaxe:

`#` **aa** *rrggbb*

O *aa* na linha anterior representa um valor hexadecimal de dois dígitos usado para especificar a opacidade da cor. O *rr*, o *gg* e o *bb* representam um valor hexadecimal de dois dígitos usado para especificar a quantidade de vermelho, verde e azul na cor. Cada dígito hexadecimal pode ter um valor de 0 a 9 ou de A a F. 0 é o menor valor. F é o maior valor. Um valor alfa de 00 especifica uma cor que é completamente transparente, enquanto um valor alfa de FF cria uma cor que é totalmente opaca.  No exemplo a seguir, a notação ARGB do hexadecimal é usada para especificar duas cores. A primeira é parcialmente transparente (tem um valor alfa de x20), enquanto a segundo é completamente opaca.

[!code-xaml[GradientBrushExamples_snip#TransparentGradientStopExample1XAML](~/samples/snippets/xaml/VS_Snippets_Wpf/GradientBrushExamples_snip/XAML/GradientStopsExample.xaml#transparentgradientstopexample1xaml)]

<a name="fromscrgbsyntax"></a>

### <a name="specifying-color-opacity-in-code"></a>Especificando a opacidade das cores no código

Ao usar o código, o <xref:System.Windows.Media.Color.FromArgb%2A> método estático permite que você especifique um valor alfa ao criar uma cor. O método usa quatro parâmetros do tipo <xref:System.Byte> . O primeiro parâmetro especifica o canal alfa da cor e os outros três parâmetros especificam os valores de vermelho, verde e azul da cor. Cada valor deve estar entre 0 e 255, inclusivo. Um valor alfa de 0 especifica que a cor é completamente transparente, enquanto um valor alfa de 255 especifica que a cor é completamente opaca. No exemplo a seguir, o <xref:System.Windows.Media.Color.FromArgb%2A> método é usado para produzir duas cores. A primeira cor é parcialmente transparente (tem um valor alfa de 32), enquanto a segunda é completamente opaca.

[!code-csharp[GradientBrushExamples_snip#TransparentGradientStopExample1CSharp](~/samples/snippets/csharp/VS_Snippets_Wpf/GradientBrushExamples_snip/CSharp/GradientStopsExample.cs#transparentgradientstopexample1csharp)]

Como alternativa, você pode usar o <xref:System.Windows.Media.Color.FromScRgb%2A> método, que permite que você use valores de ScRGB para criar uma cor.

<a name="otherbrushes"></a>

## <a name="painting-with-images-drawings-visuals-and-patterns"></a>Pintando com imagens, desenhos, elementos visuais e padrões

<xref:System.Windows.Media.ImageBrush>as <xref:System.Windows.Media.DrawingBrush> classes, e <xref:System.Windows.Media.VisualBrush> permitem que você pinte uma área com imagens, desenhos ou visuais. Para obter informações sobre como pintar com imagens, desenhos e padrões, consulte [Pintando com imagens, desenhos e visuais](painting-with-images-drawings-and-visuals.md).

## <a name="see-also"></a>Confira também

- <xref:System.Windows.Media.Brush>
- <xref:System.Windows.Media.SolidColorBrush>
- <xref:System.Windows.Media.LinearGradientBrush>
- <xref:System.Windows.Media.RadialGradientBrush>
- [Pintando com imagens, desenhos e visuais](painting-with-images-drawings-and-visuals.md)
- [Visão geral da transformação de pincel](brush-transformation-overview.md)
- [Camadas de renderização de gráficos](../advanced/graphics-rendering-tiers.md)
