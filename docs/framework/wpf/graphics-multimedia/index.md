---
title: Gráficos e multimídia
description: Descubra os recursos de mídia do Windows Presentation Foundation (WPF). Adicione elementos gráficos, efeitos de transição, som e vídeo aos seus aplicativos.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- media [WPF], features
- video effects [WPF]
- sound effects [WPF]
- animation [WPF], features
- graphics features [WPF]
- transition effects [WPF]
ms.assetid: 1817d9dc-3d6c-46cb-afc8-63b0bae35e37
ms.openlocfilehash: ba52e78564484f7714ab0035a5e1861766b72bbf
ms.sourcegitcommit: b6a1869f97a37f11a68c90afde1a520a6887dcbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853683"
---
# <a name="graphics-and-multimedia"></a>Gráficos e multimídia

<a name="introduction"></a>
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] fornece suporte para multimídia, gráficos vetoriais, animação e composição de conteúdo, tornando mais fácil para os desenvolvedores criar interfaces do usuário e conteúdo interessantes. Usando o Visual Studio, você pode criar gráficos vetoriais ou animações complexas e integrar mídia em seus aplicativos.

Este tópico apresenta os recursos de gráficos, animação e mídia de [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)], que permitem que você adicione gráficos, efeitos de transição, som e vídeo a seus aplicativos.

> [!NOTE]
> Não é recomendável usar tipos do WPF em um serviço Windows. Se você tentar usar tipos do WPF em um serviço Windows, será possível que o serviço não funcione conforme o esperado.

<a name="whats_new_with_graphics_and_multimedia_in_wpf_4"></a>

## <a name="whats-new-with-graphics-and-multimedia-in-wpf-4"></a>Novidades sobre gráficos e multimídia no WPF 4

Várias alterações foram feitas com relação aos gráficos e animações.

- Arredondamento de layout

  Quando a borda de um objeto está no meio de um dispositivo de pixel, o sistema de gráficos independente de DPI pode criar artefatos de renderização, como bordas desfocadas ou semitransparentes. Versões anteriores do WPF incluíam o ajuste de pixels para ajudar a lidar com esse caso. O Silverlight 2 introduziu o arredondamento de layout, que é outra maneira de mover elementos para que as bordas fiquem nos limites de pixels inteiros. O WPF agora dá suporte ao arredondamento de layout com a <xref:System.Windows.FrameworkElement.UseLayoutRounding%2A> Propriedade anexada em <xref:System.Windows.FrameworkElement> .

- Composição em cache

  Usando as novas <xref:System.Windows.Media.BitmapCache> classes e <xref:System.Windows.Media.BitmapCacheBrush> , você pode armazenar em cache uma parte complexa da árvore visual como um bitmap e melhorar muito o tempo de renderização. O bitmap permanece responsivo a entradas do usuário, como cliques do mouse e você pode pintá-lo em outros elementos, como faria com qualquer pincel.

- Suporte para o Sombreador de Pixel 3

  O WPF 4 se baseia no <xref:System.Windows.Media.Effects.ShaderEffect> suporte introduzido no WPF 3,5 SP1, permitindo que os aplicativos gravem efeitos usando o sombreador de pixel (PS) versão 3,0. O modelo de sombreador PS 3.0 é mais sofisticado do que o PS 2.0, o que permite ainda mais efeitos em hardware com suporte.

- Funções de easing

  Você pode aprimorar animações com funções de easing, que fornecem controle adicional sobre o comportamento das animações. Por exemplo, você pode aplicar um <xref:System.Windows.Media.Animation.ElasticEase> a uma animação para dar à animação um comportamento de porta. Para obter mais informações, consulte os tipos de atenuação no <xref:System.Windows.Media.Animation> namespace.

<a name="graphics_and_rendering"></a>

## <a name="graphics-and-rendering"></a>Gráficos e renderização

O WPF inclui suporte para gráficos 2D de alta qualidade. A funcionalidade inclui pincéis, geometrias, imagens, formas e transformações. Para obter mais informações, consulte [Gráficos](graphics.md). A renderização de elementos gráficos baseia-se na <xref:System.Windows.Media.Visual> classe. A estrutura de objetos visuais na tela é descrita pela árvore visual. Para obter mais informações, consulte [Visão geral de renderização de gráficos do WPF](wpf-graphics-rendering-overview.md).

### <a name="2d-shapes"></a>Formas 2D

O WPF fornece uma biblioteca de formas 2D com desenho de vetores comumente usadas, como retângulos e elipses, que a ilustração a seguir mostra.

![Diagrama mostrando reticências e retângulos.](./media/index/two-deminsional-shapes-ellipses-rectangles.png)

Essas formas intrínsecas do WPF não são apenas formas: são elementos programáveis que implementam muitos dos recursos que você espera dos controles mais comuns, que incluem entrada de teclado e mouse. O exemplo a seguir mostra como lidar com o <xref:System.Windows.UIElement.MouseUp> evento gerado clicando em um <xref:System.Windows.Shapes.Ellipse> elemento.

```xaml
<Window
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
  x:Class="Window1" >
  <Ellipse Fill="LightBlue" MouseUp="ellipseButton_MouseUp" />
</Window>
```

```csharp
public partial class Window1  : Window
{
    void ellipseButton_MouseUp(object sender, MouseButtonEventArgs e)
    {
        MessageBox.Show("You clicked the ellipse!");
    }
}
```

```vb
Partial Public Class Window1
    Inherits Window
    Private Sub ellipseButton_MouseUp(ByVal sender As Object, ByVal e As MouseButtonEventArgs)
        MessageBox.Show("You clicked the ellipse!")
    End Sub
End Class
```

A ilustração a seguir mostra a saída para a marcação [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] anterior e code-behind.

![Uma caixa de mensagem dizendo "você clicou na elipse!"](./media/index/messagebox-text-output.png)

Para obter mais informações, consulte [Visão geral de formas e desenho básico no WPF](shapes-and-basic-drawing-in-wpf-overview.md). Para ver um exemplo introdutório, consulte [Amostra de elementos de forma](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/ShapeElements).

### <a name="2d-geometries"></a>Geometrias 2D

Quando as formas 2D que o WPF fornece não forem suficientes, você poderá usar o suporte do WPF para geometrias e caminhos para criar o seu próprio. A ilustração a seguir mostra como você pode usar geometrias para criar formas, como um pincel de desenho e para recortar outros elementos do WPF.

![Captura de tela mostrando como você pode usar geometrias para criar formas.](./media/index/use-geometries-create-shapes.png)

Para obter mais informações, consulte [visão geral da geometria](geometry-overview.md). Para ver um exemplo introdutório, consulte [Amostra de geometrias](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/Geometry).

### <a name="2d-effects"></a>Efeitos 2D

O WPF fornece uma biblioteca de classes 2D que você pode usar para criar uma variedade de efeitos. A capacidade de renderização 2D do WPF fornece a capacidade de pintar [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] elementos que têm gradientes, bitmaps, desenhos e vídeos; e manipulá-los usando rotação, dimensionamento e inclinação. A ilustração a seguir fornece um exemplo dos muitos efeitos que você pode obter usando os pincéis do WPF.

![Ilustração mostrando os diferentes pincéis do WPF e elementos de pintura.](./media/index/brushes-paint-elements.png)

Para obter mais informações, consulte [visão geral de pincéis do WPF](wpf-brushes-overview.md). Para ver um exemplo introdutório, consulte [Amostra de pincéis](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/Brushes).

<a name="rendering"></a>

## <a name="3d-rendering"></a>Renderização 3D

O WPF fornece um conjunto de recursos de renderização 3D que se integram ao suporte a gráficos 2D no WPF para que você possa criar layouts e visualização de dados mais empolgantes [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] . Em uma extremidade do espectro, o WPF permite renderizar imagens 2D nas superfícies de formas 3D, que a ilustração a seguir demonstra.

![Captura de tela de um exemplo mostrando formas 3D com texturas diferentes.](./media/index/visual-three-dimensional-shape.png)

Para obter mais informações, consulte [visão geral de gráficos 3D](3-d-graphics-overview.md). Para obter um exemplo introdutório, consulte [exemplo de sólidos 3D](https://go.microsoft.com/fwlink/?LinkID=159964).

<a name="animation"></a>

## <a name="animation"></a>Animação

Use a animação para fazer controles e elementos crescerem, tremerem, rodarem e esmaecerem, e para criar transições de página interessantes e muito mais. Como o WPF permite que você anime a maioria das propriedades, não só é possível animar a maioria dos objetos do WPF, você também pode usar o WPF para animar objetos personalizados criados por você.

![Captura de tela de um cubo animado.](./media/index/animate-custom-objects.png)

Para obter mais informações, consulte [visão geral da animação](animation-overview.md). Para obter um exemplo introdutório, consulte [Galeria de exemplos de animação](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/AnimationExamples).

<a name="media"></a>

## <a name="media"></a>Mídia

Imagens, vídeo e áudio são formas de mídia avançadas que transmitem informações experiências do usuário.

### <a name="images"></a>Imagens

Imagens, que incluem ícones, planos de fundo e até mesmo partes de animações, são uma parte principal da maioria dos aplicativos. Como você geralmente precisa usar imagens, o WPF expõe a capacidade de trabalhar com elas de várias maneiras. A ilustração a seguir mostra apenas uma dessas formas.

![Captura de tela de exemplo de estilo](../controls/media/stylingintro-eventtriggers.png "StylingIntro_EventTriggers")

Para obter mais informações, consulte [visão geral da geração de imagens](imaging-overview.md).

### <a name="video-and-audio"></a>Áudio e vídeo

Um recurso fundamental dos recursos gráficos do WPF é fornecer suporte nativo para trabalhar com multimídia, que inclui vídeo e áudio. O exemplo a seguir mostra como inserir um player de mídia em um aplicativo.

```xaml
<MediaElement Source="media\numbers.wmv" Width="450" Height="250" />
```

<xref:System.Windows.Controls.MediaElement>é capaz de reproduzir vídeo e áudio e é extensível o suficiente para permitir a criação fácil de interfaces do cliente personalizadas.

Para obter mais informações, consulte [Visão geral de multimídia](multimedia-overview.md).

## <a name="see-also"></a>Confira também

- <xref:System.Windows.Media>
- <xref:System.Windows.Media.Animation>
- <xref:System.Windows.Media.Media3D>
- [Elementos gráficos e geração de imagens 2D](../advanced/optimizing-performance-2d-graphics-and-imaging.md)
- [Visão geral de formas e desenho básico no WPF](shapes-and-basic-drawing-in-wpf-overview.md)
- [Visão geral da pintura com cores sólidas e gradientes](painting-with-solid-colors-and-gradients-overview.md)
- [Pintando com imagens, desenhos e visuais](painting-with-images-drawings-and-visuals.md)
- [Tópicos explicativos de animação e tempo](animation-and-timing-how-to-topics.md)
- [Visão geral de elementos gráficos 3D](3-d-graphics-overview.md)
- [Visão geral de multimídia](multimedia-overview.md)
