---
title: Sintaxe de marcação do caminho
description: Saiba mais sobre o mini-idioma poderoso e complexo que você pode usar para especificar geometrias de caminho de compactação no Windows Presentation Foundation (WPF).
ms.date: 03/30/2017
helpviewer_keywords:
- attribute usage in XAML [WPF]
- XAML [WPF], attribute usage
- graphics [WPF], PathGeometry class
- XAML [WPF], object element usage
ms.assetid: b8586241-a02d-486e-9223-e1e98e047f41
ms.openlocfilehash: 6d2778522b622f0b0dcb59673e793a6d772a4b4f
ms.sourcegitcommit: b6a1869f97a37f11a68c90afde1a520a6887dcbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853659"
---
# <a name="path-markup-syntax"></a>Sintaxe de marcação do caminho
Os caminhos são discutidos em [formas e desenhos básicos na visão geral do WPF](shapes-and-basic-drawing-in-wpf-overview.md) e a [visão geral da geometria](geometry-overview.md), no entanto, este tópico descreve em detalhes o mini-idioma poderoso e complexo que você pode usar para especificar geometrias de caminho com mais compactação [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] .  
  
<a name="prerequisites"></a>
## <a name="prerequisites"></a>Pré-requisitos  
 Para entender este tópico, você deve estar familiarizado com os recursos básicos dos <xref:System.Windows.Media.Geometry> objetos. Para obter mais informações, consulte a [visão geral da geometria](geometry-overview.md).  
  
<a name="abouthisdocument"></a>
## <a name="streamgeometry-and-pathfigurecollection-mini-languages"></a>Mini-linguagens de PathFigureCollection e StreamGeometry  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]fornece duas classes que fornecem mini idiomas para descrever os caminhos geométricos: <xref:System.Windows.Media.StreamGeometry> e <xref:System.Windows.Media.PathFigureCollection> .  
  
- Você usa o <xref:System.Windows.Media.StreamGeometry> Mini-idioma ao definir uma propriedade do tipo <xref:System.Windows.Media.Geometry> , como a <xref:System.Windows.UIElement.Clip%2A> propriedade de uma <xref:System.Windows.UIElement> ou a <xref:System.Windows.Shapes.Path.Data%2A> propriedade de um <xref:System.Windows.Shapes.Path> elemento. O exemplo a seguir usa a sintaxe de atributo para criar um <xref:System.Windows.Media.StreamGeometry> .  
  
     [!code-xaml[GeometrySample_snip_XAML#GraphicsMMStreamGeometryAttributeSyntaxInline](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometrySample_snip_XAML/CS/MiniLanguageExample.xaml#graphicsmmstreamgeometryattributesyntaxinline)]  
  
- Você usa o <xref:System.Windows.Media.PathFigureCollection> Mini-idioma ao definir a <xref:System.Windows.Media.PathGeometry.Figures%2A> propriedade de um <xref:System.Windows.Media.PathGeometry> . O exemplo a seguir usa uma sintaxe de atributo para criar um <xref:System.Windows.Media.PathFigureCollection> para um <xref:System.Windows.Media.PathGeometry> .  
  
     [!code-xaml[GeometrySample_snip_XAML#GraphicsMMPathFigureCollectionAttributeSyntaxInline](~/samples/snippets/csharp/VS_Snippets_Wpf/GeometrySample_snip_XAML/CS/MiniLanguageExample.xaml#graphicsmmpathfigurecollectionattributesyntaxinline)]  
  
 Como você pode ver nos exemplos anteriores, as duas mini-linguagens são muito semelhantes. Sempre é possível usar um <xref:System.Windows.Media.PathGeometry> em qualquer situação em que você possa usar um <xref:System.Windows.Media.StreamGeometry> ; portanto, qual deles você deve usar? Use um <xref:System.Windows.Media.StreamGeometry> quando você não precisar modificar o caminho depois de criá-lo; use um <xref:System.Windows.Media.PathGeometry> se você precisar modificar o caminho.  
  
 Para obter mais informações sobre as diferenças <xref:System.Windows.Media.PathGeometry> entre <xref:System.Windows.Media.StreamGeometry> objetos e, consulte a [visão geral da geometria](geometry-overview.md).  
  
### <a name="a-note-about-white-space"></a>Uma observação sobre o espaço em branco  
 Para resumir, um único espaço é mostrado na seção de sintaxe que segue, mas vários espaços também são aceitos sempre que um único espaço é mostrado.  
  
 Na verdade, dois números não precisam ser separados por uma vírgula ou um espaço em branco, mas isso só pode ser feito quando a cadeia de caracteres resultante não é ambígua. Por exemplo, `2..3` é, na verdade, dois números: "2." E ".3". Da mesma forma, `2-3` é "2" e "-3". Espaços não são necessários antes ou depois de comandos.  
  
### <a name="syntax"></a>Syntax  
 A [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] sintaxe de uso de atributo para um <xref:System.Windows.Media.StreamGeometry> é composta por um <xref:System.Windows.Media.FillRule> valor opcional e uma ou mais descrições de figura.  
  
|Uso do atributo StreamGeometry XAML|  
|-----------------------------------------|  
|`<`*object* *Propriedade* do objeto `="` [ `fillRule` ] `figureDescription` [ `figureDescription` ] *`" ... />`|  
  
 A [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] sintaxe de uso de atributo para um <xref:System.Windows.Media.PathFigureCollection> é composta de uma ou mais descrições de figura.  
  
|Uso do atributo PathFigureCollection XAML|  
|-----------------------------------------------|  
|`<`*object* *Propriedade* do objeto `="` `figureDescription` [ `figureDescription` ] *`" ... />`|  
  
|Termo|Descrição|  
|----------|-----------------|  
|*fillRule*|<xref:System.Windows.Media.FillRule?displayProperty=nameWithType><br /><br /> Especifica se o <xref:System.Windows.Media.StreamGeometry> usa o <xref:System.Windows.Media.FillRule.EvenOdd> ou o <xref:System.Windows.Media.FillRule.Nonzero> <xref:System.Windows.Media.PathGeometry.FillRule%2A> .<br /><br /> -   `F0`Especifica a <xref:System.Windows.Media.FillRule.EvenOdd> regra de preenchimento.<br />-   `F1`Especifica a <xref:System.Windows.Media.FillRule.Nonzero> regra de preenchimento.<br /><br /> Se você omitir esse comando, o subcaminho usará o comportamento padrão, que é <xref:System.Windows.Media.FillRule.EvenOdd> . Se você especificar este comando, coloque-o primeiro.|  
|*figureDescription*|Uma figura é composta de um comando mover, desenhar e um comando opcional para fechar.<br /><br /> `moveCommand` `drawCommands`  `[` `closeCommand` `]`|  
|*moveCommand*|Um comando de mover que especifica o ponto inicial da figura. Consulte a seção de [comando mover](#themovecommand) .|  
|*drawCommands*|Um ou mais comandos de desenho que descrevem o conteúdo da figura. Consulte a seção [comandos de desenho](#drawcommands) .|  
|*closeCommand*|Um comando Fechar opcional que fecha a figura. Consulte a seção de [comando fechar](#closecommand) .|  
  
<a name="themovecommand"></a>
## <a name="move-command"></a>Comando Mover  
 Especifica o ponto de início de uma nova figura.  
  
|Syntax|  
|------------|  
|`M`*StartPoint*<br /><br /> - ou -<br /><br /> `m`*StartPoint*|  
  
|Termo|Descrição|  
|----------|-----------------|  
|*startPoint*|<xref:System.Windows.Point?displayProperty=nameWithType><br /><br /> O ponto inicial da nova figura.|  
  
 Uma letra maiúscula `M` indica que `startPoint` é um valor absoluto; uma letra minúscula `m` indica que `startPoint` é um deslocamento para o ponto anterior ou (0, 0) se não houver nenhum. Se você listar vários pontos após o comando Mover, uma linha será desenhada nestes pontos nos quais você especificou a linha de comando.  
  
<a name="drawcommands"></a>
## <a name="draw-commands"></a>Comandos Desenhar  
 Um comando de desenho pode consistir em vários comandos de formas. Os comandos de forma a seguir estão disponíveis: linha, linha horizontal, linha vertical, curva de Bézier cúbica, curva de Bezier quadrática, curva de Bézier cúbica suave, curva de Bezier quadrática suave e arco elíptico.  
  
 Insira cada comando usando uma letra maiúscula ou uma letra minúscula: letras maiúsculas denotam valores absolutos e letras minúsculas denotam valores relativos: os pontos de controle para aquele segmento são relativos ao ponto final do exemplo anterior. Ao inserir sequencialmente mais de um comando do mesmo tipo, você pode omitir a entrada de comando duplicada; por exemplo, `L 100,200 300,400` é equivalente a `L 100,200 L 300,400` . A tabela a seguir descreve os comandos **mover** e **desenhar** .  
  
### <a name="line-command"></a>Comando de Linha  
 Cria uma linha reta entre o ponto atual e o ponto final especificado. `l 20 30`e `L 20,30` são exemplos de comandos de **linha** válidos.  
  
|Syntax|  
|------------|  
|`L`*ponto de extremidade*<br /><br /> - ou -<br /><br /> `l`*ponto de extremidade*|  
  
|Termo|Descrição|  
|----------|-----------------|  
|*Extremidade*|<xref:System.Windows.Point?displayProperty=nameWithType><br /><br /> O ponto de extremidade da linha.|  

Uma letra maiúscula `L` indica que `endPoint` é um valor absoluto; uma letra minúscula `l` indica que `endPoint` é um deslocamento para o ponto anterior ou (0, 0) se não houver nenhum.

### <a name="horizontal-line-command"></a>Comando de linha horizontal  
 Cria uma linha horizontal entre o ponto atual e a coordenada X especificada. `H 90` é um exemplo de um comando de linha horizontal válido.

|Syntax|  
|------------|  
|`H`  *w.x.y.*<br /><br /> - ou -<br /><br /> `h`  *w.x.y.*|  
  
|Termo|Descrição|  
|----------|-----------------|  
|*x*|<xref:System.Double?displayProperty=nameWithType><br /><br /> A coordenada X do ponto final da linha.|  
  
Uma letra maiúscula `H` indica que `x` é um valor absoluto; uma letra minúscula `h` indica que `x` é um deslocamento para o ponto anterior ou (0, 0) se não houver nenhum.
  
### <a name="vertical-line-command"></a>Comando de linha vertical  
 Cria uma linha vertical entre o ponto atual e a coordenada y especificada. `v 90` é um exemplo de um comando de linha vertical válido.

|Syntax|  
|------------|  
|`V`  *Iar*<br /><br /> - ou -<br /><br /> `v`  *Iar*|  
  
|Termo|Descrição|  
|----------|-----------------|  
|*Iar*|<xref:System.Double?displayProperty=nameWithType><br /><br /> A coordenada y do ponto final da linha.|  

Uma letra maiúscula `V` indica que `y` é um valor absoluto; uma letra minúscula `v` indica que `y` é um deslocamento para o ponto anterior ou (0, 0) se não houver nenhum.  

### <a name="cubic-bezier-curve-command"></a>Comando de curva de Bézier cúbica  
 Cria uma curva de Bézier cúbica entre o ponto atual e o ponto de extremidade especificado usando os dois pontos de controle especificados ( `controlPoint` 1 e `controlPoint` 2). `C 100,200 200,400 300,200` é um exemplo de um comando de curva válido.  
  
|Syntax|  
|------------|  
|`C``controlPoint`1 `controlPoint` 2`endPoint`<br /><br /> - ou -<br /><br /> `c``controlPoint`1 `controlPoint` 2`endPoint`|  
  
|Termo|Descrição|  
|----------|-----------------|  
|`controlPoint`1|<xref:System.Windows.Point?displayProperty=nameWithType><br /><br /> O primeiro ponto de controle da curva, que determina o início da tangente da curva.|  
|`controlPoint`2|<xref:System.Windows.Point?displayProperty=nameWithType><br /><br /> O segundo ponto de controle da curva, que determina o final da tangente da curva.|  
|`endPoint`|<xref:System.Windows.Point?displayProperty=nameWithType><br /><br /> O ponto em que a curva é desenhada.|  
  
### <a name="quadratic-bezier-curve-command"></a>Comando de curva de Bezier quadrática  
 Cria uma curva de Bezier quadrática entre o ponto atual e o ponto de extremidade especificado usando o ponto de controle especificado ( `controlPoint` ). `q 100,200 300,200` é um exemplo de um comando de curva de Bezier quadrática válido.  
  
|Syntax|  
|------------|  
|`Q` `controlPoint` `endPoint`<br /><br /> - ou -<br /><br /> `q` `controlPoint` `endPoint`|  
  
|Termo|Descrição|  
|----------|-----------------|  
|`controlPoint`|<xref:System.Windows.Point?displayProperty=nameWithType><br /><br /> O ponto de controle da curva, que determina o início e final da tangente da curva.|  
|`endPoint`|<xref:System.Windows.Point?displayProperty=nameWithType><br /><br /> O ponto em que a curva é desenhada.|  
  
### <a name="smooth-cubic-bezier-curve-command"></a>Comando de Curva de Bézier cúbica suave  
 Cria uma curva de Bézier cúbica entre o ponto atual e o ponto final especificado. O primeiro ponto de controle é assumido ser a reflexão do segundo ponto de controle do comando anterior relativo ao ponto atual. Se não houver nenhum comando anterior ou se o comando anterior não era um comando de curva de Bézier cúbico ou um comando de curva de Bézier cúbico suave, suponha que o primeiro ponto de controle é coincidente com o ponto atual. O segundo ponto de controle, o ponto de controle para o final da curva, é especificado por `controlPoint` 2. Por exemplo, `S 100,200 200,300` é um comando de curva de Bézier cúbica suave válido.  
  
|Syntax|  
|------------|  
|`S` `controlPoint`2`endPoint`<br /><br /> - ou -<br /><br /> `s` `controlPoint`2`endPoint`|  
  
|Termo|Descrição|  
|----------|-----------------|  
|`controlPoint`2|<xref:System.Windows.Point?displayProperty=nameWithType><br /><br /> O ponto de controle da curva, que determina o final da tangente da curva.|  
|`endPoint`|<xref:System.Windows.Point?displayProperty=nameWithType><br /><br /> O ponto em que a curva é desenhada.|  
  
### <a name="smooth-quadratic-bezier-curve-command"></a>Curva de Bezier quadrática suave comando  
 Cria uma curva de Bezier quadrática entre o ponto atual e o ponto final especificado. O ponto de controle é assumido ser a reflexão do ponto de controle do comando anterior relativo ao ponto atual. Se não houver nenhum comando anterior ou se o comando anterior não era um comando de curva de Bezier quadrática ou um comando de curva de Bezier quadrático suave, o ponto de controle será coincidente com o ponto atual.  
  
|Syntax|  
|------------|  
|`T` `controlPoint` `endPoint`<br /><br /> - ou -<br /><br /> `t` `controlPoint` `endPoint`|  
  
|Termo|Descrição|  
|----------|-----------------|  
|`controlPoint`|<xref:System.Windows.Point?displayProperty=nameWithType><br /><br /> O ponto de controle da curva, que determina o início e tangente da curva.|  
|`endPoint`|<xref:System.Windows.Point?displayProperty=nameWithType><br /><br /> O ponto em que a curva é desenhada.|  
  
### <a name="elliptical-arc-command"></a>Comando de arco elíptico  
 Cria um arco elíptico entre o ponto atual e o ponto final especificado.  
  
|Syntax|  
|------------|  
|`A` `size` `rotationAngle` `isLargeArcFlag` `sweepDirectionFlag` `endPoint`<br /><br /> - ou -<br /><br /> `a` `size` `rotationAngle` `isLargeArcFlag` `sweepDirectionFlag` `endPoint`|  
  
|Termo|Descrição|  
|----------|-----------------|  
|`size`|<xref:System.Windows.Size?displayProperty=nameWithType><br /><br /> Os raios x e y do arco.|  
|`rotationAngle`|<xref:System.Double?displayProperty=nameWithType><br /><br /> A rotação da elipse, em graus.|  
|`isLargeArcFlag`|Definido como 1 se o ângulo do arco deva ser 180 graus ou maior. Caso contrário, defina como 0.|  
|`sweepDirectionFlag`|Definido como 1 se o arco é desenhado na direção ângulo-positiva. Caso contrário, defina como 0.|  
|`endPoint`|<xref:System.Windows.Point?displayProperty=nameWithType><br /><br /> O ponto em que o arco é desenhado.|  
  
<a name="closecommand"></a>
## <a name="the-close-command"></a>O comando Fechar  
 Termina a figura atual e cria uma linha que conecta o ponto atual ao ponto de partida da figura. Este comando cria uma linha-junção (canto) entre o último segmento e o primeiro segmento da figura.  
  
|Syntax|  
|------------|  
|`Z`<br /><br /> - ou -<br /><br /> `z`|  

<a name="pointsyntax"></a>
## <a name="point-syntax"></a>Sintaxe de ponto  
 Descreve as coordenadas x e y de um ponto em que (0, 0) é o canto superior esquerdo.
  
|Syntax|  
|------------|  
|`x` `,` `y`<br /><br /> - ou -<br /><br /> `x` `y`|  
  
|Termo|Descrição|  
|----------|-----------------|  
|`x`|<xref:System.Double?displayProperty=nameWithType><br /><br /> A coordenada X do ponto.|  
|`y`|<xref:System.Double?displayProperty=nameWithType><br /><br /> A coordenada Y do ponto.|  
  
<a name="specialvalues"></a>
## <a name="special-values"></a>Valores especiais  
 Em vez de um valor numérico padrão, você também pode usar os seguintes valores especiais. Esses valores diferenciam maiúsculas de minúsculas.  
  
 Infinity  
 Representa <xref:System.Double.PositiveInfinity?displayProperty=nameWithType> .  
  
 -Infinito  
 Representa <xref:System.Double.NegativeInfinity?displayProperty=nameWithType> .  
  
 NaN  
 Representa <xref:System.Double.NaN?displayProperty=nameWithType> .  
  
 Você também pode usar notação científica. Por exemplo, `+1.e17` é um valor válido.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Shapes.Path>
- <xref:System.Windows.Media.StreamGeometry>
- <xref:System.Windows.Media.PathGeometry>
- <xref:System.Windows.Media.PathFigureCollection>
- [Visão geral de formas e desenho básico no WPF](shapes-and-basic-drawing-in-wpf-overview.md)
- [Visão geral da geometria](geometry-overview.md)
- [Tópicos explicativos](geometries-how-to-topics.md)
