---
title: Visão geral de elementos gráficos 3D
description: Familiarize-se com gráficos 3D no Windows Presentation Foundation (WPF) para desenhar, transformar e animar gráficos 3D na marcação e no código de procedimento.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- 3D graphics [WPF]
- graphics [WPF], 3D
ms.assetid: 67f31ed4-e36b-4b02-9889-dcce245d7afc
ms.openlocfilehash: 51da6a1ed6d5e98b99c64ee23be52f7b2385897f
ms.sourcegitcommit: b6a1869f97a37f11a68c90afde1a520a6887dcbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853870"
---
# <a name="3d-graphics-overview"></a>Visão geral de elementos gráficos 3D
<a name="introduction"></a>A funcionalidade 3D no [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] permite aos desenvolvedores desenhar, transformar e animar gráficos 3D na marcação e no código de procedimento. Os desenvolvedores podem combinar gráficos 2D e 3D para criar controles avançados, fornecer ilustrações complexas de dados ou aprimorar a experiência do usuário da interface de um aplicativo. o suporte a 3D no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] não foi projetado para fornecer uma plataforma completa de desenvolvimento de jogos. Este tópico fornece uma visão geral da funcionalidade 3D no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] sistema de gráficos.  

<a name="threed_in_2d"></a>
## <a name="3d-in-a-2d-container"></a>3D em um contêiner 2D  
 o conteúdo de gráficos 3D no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] é encapsulado em um elemento, <xref:System.Windows.Controls.Viewport3D> , que pode participar da estrutura de elementos bidimensional. O sistema de gráficos trata <xref:System.Windows.Controls.Viewport3D> como um elemento visual bidimensional como muitos outros no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] . <xref:System.Windows.Controls.Viewport3D>funciona como uma janela — um visor — em uma cena tridimensional. Mais precisamente, é uma superfície na qual uma cena 3D é projetada.  
  
 Em um aplicativo 2D convencional, use <xref:System.Windows.Controls.Viewport3D> como você faria com outro elemento de contêiner como grade ou Canvas.  Embora você possa usar <xref:System.Windows.Controls.Viewport3D> com outros objetos de desenho 2D no mesmo grafo de cena, não é possível interpenetrar objetos 2D e 3D em um <xref:System.Windows.Controls.Viewport3D> .  Este tópico se concentrará em como desenhar gráficos 3D dentro do <xref:System.Windows.Controls.Viewport3D> .  
  
<a name="coord_space"></a>
## <a name="3d-coordinate-space"></a>Espaço de coordenadas 3D  
 O [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] sistema de coordenadas para gráficos 2D localiza a origem no canto superior esquerdo da área de renderização (normalmente a tela). No sistema 2D, os valores positivos do eixo x passam para a direita e os valores positivos do eixo y prosseguem para baixo.  No sistema de coordenadas 3D, no entanto, a origem está localizada no centro da área de renderização, com valores positivos do eixo x prosseguindo para a direita, mas valores positivos do eixo y continuando para cima, e os valores positivos do eixo z continuando para fora da origem, para o visualizador.  
  
 ![Sistemas de coordenadas](./media/coordsystem-1.png "CoordSystem-1")  
Representações de sistema de coordenadas 2D e 3D convencionais  
  
 O espaço definido por esses eixos é o quadro estacionário de referência para objetos 3D no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] . Ao compilar modelos nesse espaço e criar luzes e câmeras para exibi-los, é útil distinguir esse quadro estacionário de referência, ou “espaço de mundo”, do quadro local da referência que você cria para cada modelo quando aplica transformações a ele. Lembre-se também de que objetos no espaço de mundo podem parecer totalmente diferentes ou até mesmo não serem visíveis, dependendo das configurações de câmera e de luz, mas a posição da câmera não altera o local dos objetos no espaço de mundo.  
  
<a name="cameras"></a>
## <a name="cameras-and-projections"></a>Câmeras e projeções  
 Os desenvolvedores que trabalham em 2D estão acostumados a posicionar primitivas de desenho em uma tela bidimensional. Ao criar uma cena 3D, é importante lembrar que você está realmente criando uma representação 2D de objetos 3D. Como uma cena 3D tem uma aparência diferente, dependendo do ponto de vista do onlooker, você deve especificar esse ponto de vista. A <xref:System.Windows.Media.Media3D.Camera> classe permite que você especifique esse ponto de vista para uma cena 3D.  
  
 Outra maneira de entender como uma cena 3D é representada em uma superfície 2D é descrever a cena como uma projeção na superfície de exibição. O <xref:System.Windows.Media.Media3D.ProjectionCamera> permite que você especifique projeções diferentes e suas propriedades para alterar como o onlooker vê modelos 3D. Um <xref:System.Windows.Media.Media3D.PerspectiveCamera> especifica uma projeção que foreshortens a cena.  Em outras palavras, o <xref:System.Windows.Media.Media3D.PerspectiveCamera> fornece a perspectiva do ponto de fuga.  Você pode especificar a posição da câmera no espaço de coordenadas da cena, a direção e o campo de visão da câmera e um vetor que define a direção “para cima” na cena. O diagrama a seguir ilustra a <xref:System.Windows.Media.Media3D.PerspectiveCamera> projeção do.  
  
 As <xref:System.Windows.Media.Media3D.ProjectionCamera.NearPlaneDistance%2A> <xref:System.Windows.Media.Media3D.ProjectionCamera.FarPlaneDistance%2A> Propriedades e de <xref:System.Windows.Media.Media3D.ProjectionCamera> limite o intervalo da projeção da câmera. Como as câmeras podem estar localizadas em qualquer lugar na cena, é possível que a câmera esteja realmente posicionada dentro de um modelo ou muito próximo a ele, dificultando a distinção correta de objetos.  <xref:System.Windows.Media.Media3D.ProjectionCamera.NearPlaneDistance%2A>permite que você especifique uma distância mínima da câmera além da qual os objetos não serão desenhados.  Por outro lado, o <xref:System.Windows.Media.Media3D.ProjectionCamera.FarPlaneDistance%2A> permite que você especifique uma distância da câmera além da qual os objetos não serão desenhados, o que garante que os objetos muito longe de serem reconhecidos não sejam incluídos na cena.  
  
 ![Instalação da câmera](./media/coordsystem-6.png "CoordSystem-6")  
Posição da câmera  
  
 <xref:System.Windows.Media.Media3D.OrthographicCamera>Especifica uma projeção ortogonal de um modelo 3D para uma superfície visual 2D. Como outras câmeras, ela especifica uma posição, direção de visualização e direção “para cima”. Ao contrário <xref:System.Windows.Media.Media3D.PerspectiveCamera> de, no entanto, <xref:System.Windows.Media.Media3D.OrthographicCamera> descreve uma projeção que não inclui foreshortening de perspectiva. Em outras palavras, <xref:System.Windows.Media.Media3D.OrthographicCamera> descreve uma caixa de exibição cujos lados são paralelos, em vez de um cujos lados se encontram em um ponto na câmera. A imagem a seguir mostra o mesmo modelo exibido usando <xref:System.Windows.Media.Media3D.PerspectiveCamera> e <xref:System.Windows.Media.Media3D.OrthographicCamera> .  
  
 ![Projeções de perspectiva e ortográficas](./media/camera-projections4.png "Camera_projections4")  
Projeções de perspectiva e ortográficas  
  
 O código a seguir mostra algumas configurações típicas da câmera.  
  
 [!code-csharp[3dgallery_procedural_snip#Basic3DShapeCodeExampleInline1](~/samples/snippets/csharp/VS_Snippets_Wpf/3DGallery_procedural_snip/CSharp/Basic3DShapeExample.cs#basic3dshapecodeexampleinline1)]
 [!code-vb[3dgallery_procedural_snip#Basic3DShapeCodeExampleInline1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/3DGallery_procedural_snip/visualbasic/basic3dshapeexample.vb#basic3dshapecodeexampleinline1)]  
  
<a name="models_meshes"></a>
## <a name="model-and-mesh-primitives"></a>Modelo e primitivos de malha  
  
 <xref:System.Windows.Media.Media3D.Model3D>é a classe base abstrata que representa um objeto 3D genérico. Para criar uma cena 3D, você precisa de alguns objetos para exibir e os objetos que compõem o grafo de cena derivam de <xref:System.Windows.Media.Media3D.Model3D> . Atualmente, o [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] oferece suporte a geometrias de modelagem com <xref:System.Windows.Media.Media3D.GeometryModel3D> . A <xref:System.Windows.Media.Media3D.GeometryModel3D.Geometry%2A> propriedade desse modelo usa uma malha primitiva.  
  
 Para compilar um modelo, comece criando um primitivo, ou uma malha. Um primitivo 3D é uma coleção de vértices que forma uma única entidade 3D. A maioria dos sistemas 3D fornece primitivos modelados na figura mais simples fechada: um triângulo definido por três vértices.  Como os três pontos de um triângulo são coplanares, você pode continuar adicionando triângulos para modelar formas mais complexas, chamadas malhas.  
  
 O [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] sistema 3D atualmente fornece a <xref:System.Windows.Media.Media3D.MeshGeometry3D> classe, que permite que você especifique qualquer geometria; atualmente, ela não dá suporte a primitivos 3D predefinidos como, por exemplo, para os formatos cúbicos e formas cúbicas. Comece a criar um <xref:System.Windows.Media.Media3D.MeshGeometry3D> especificando uma lista de vértices de triângulo como sua <xref:System.Windows.Media.Media3D.MeshGeometry3D.Positions%2A> propriedade. Cada vértice é especificado como um <xref:System.Windows.Media.Media3D.Point3D> .  (No [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] , especifique essa propriedade como uma lista de números agrupados em três que representam as coordenadas de cada vértice.) Dependendo de sua geometria, sua malha pode ser composta por muitos triângulos, algumas das quais compartilham os mesmos cantos (vértices). Para desenhar a malha corretamente, o [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] precisa de informações sobre quais vértices são compartilhados por quais triângulos. Você fornece essas informações especificando uma lista de índices de triângulo com a <xref:System.Windows.Media.Media3D.MeshGeometry3D.TriangleIndices%2A> propriedade. Esta lista especifica a ordem na qual os pontos especificados na <xref:System.Windows.Media.Media3D.MeshGeometry3D.Positions%2A> lista determinarão um triângulo.  
  
 [!code-xaml[basic3d#Basic3DXAML3DN3](~/samples/snippets/xaml/VS_Snippets_Wpf/Basic3D/XAML/Window1.xaml#basic3dxaml3dn3)]  
  
 No exemplo anterior, a <xref:System.Windows.Media.Media3D.MeshGeometry3D.Positions%2A> lista especifica oito vértices para definir uma malha em formato de cubo. A <xref:System.Windows.Media.Media3D.MeshGeometry3D.TriangleIndices%2A> propriedade especifica uma lista de doze grupos de três índices.  Cada número na lista se refere a um deslocamento na <xref:System.Windows.Media.Media3D.MeshGeometry3D.Positions%2A> lista.  Por exemplo, os três primeiros vértices especificados pela <xref:System.Windows.Media.Media3D.MeshGeometry3D.Positions%2A> lista são (1, 1, 0), (0, 1, 0) e (0, 0, 0). Os três primeiros índices especificados pela <xref:System.Windows.Media.Media3D.MeshGeometry3D.TriangleIndices%2A> lista são 0, 2 e 1, que correspondem ao primeiro, terceiro e segundo pontos na <xref:System.Windows.Media.Media3D.MeshGeometry3D.Positions%2A> lista. Como resultado, o primeiro triângulo que compõe o modelo de cubo será composto de (1,1,0) para (0,1,0) e (0, 0,0) e os onze triângulos restantes serão determinados da mesma forma.  
  
 Você pode continuar definindo o modelo especificando valores para as <xref:System.Windows.Media.Media3D.MeshGeometry3D.Normals%2A> Propriedades e <xref:System.Windows.Media.Media3D.MeshGeometry3D.TextureCoordinates%2A> .  Para renderizar a superfície do modelo, o sistema de elementos gráficos precisa de informações sobre para qual direção a superfície está voltada em um triângulo específico. Ele usa essas informações para fazer cálculos de iluminação para o modelo: as superfícies voltadas diretamente para uma fonte de luz parecem mais brilhantes do que aquelas que têm ângulos distantes da luz. Embora o [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] possa determinar vetores normais padrão usando as coordenadas de posição, também é possível especificar vetores normais diferentes para aproximar a aparência de superfícies curvas.  
  
 A <xref:System.Windows.Media.Media3D.MeshGeometry3D.TextureCoordinates%2A> propriedade especifica uma coleção de <xref:System.Windows.Point> s que informa ao sistema de gráficos como mapear as coordenadas que determinam como uma textura é desenhada para os vértices da malha. <xref:System.Windows.Media.Media3D.MeshGeometry3D.TextureCoordinates%2A>são especificados como um valor entre zero e 1, inclusive.  Assim como acontece com a <xref:System.Windows.Media.Media3D.MeshGeometry3D.Normals%2A> propriedade, o sistema de gráficos pode calcular as coordenadas de textura padrão, mas você pode optar por definir diferentes coordenadas de textura para controlar o mapeamento de uma textura que inclui parte de um padrão de repetição, por exemplo. Encontre mais informações sobre coordenadas de textura nos próximos tópicos ou no SDK do Direct3D Gerenciado.  
  
 O exemplo a seguir mostra como criar uma face do modelo de cubo no código de procedimento. Você pode desenhar o cubo inteiro como um único GeometryModel3D; Este exemplo desenha a face do cubo como um modelo distinto a fim de aplicar texturas separadas a cada face posteriormente.  
  
 [!code-csharp[3doverview#3DOverview3DN6](~/samples/snippets/csharp/VS_Snippets_Wpf/3DOverview/CSharp/Window1.xaml.cs#3doverview3dn6)]
 [!code-vb[3doverview#3DOverview3DN6](~/samples/snippets/visualbasic/VS_Snippets_Wpf/3DOverview/visualbasic/window1.xaml.vb#3doverview3dn6)]  
  
 [!code-csharp[3doverview#3DOverview3DN7](~/samples/snippets/csharp/VS_Snippets_Wpf/3DOverview/CSharp/Window1.xaml.cs#3doverview3dn7)]
 [!code-vb[3doverview#3DOverview3DN7](~/samples/snippets/visualbasic/VS_Snippets_Wpf/3DOverview/visualbasic/window1.xaml.vb#3doverview3dn7)]  
  
<a name="materials"></a>'
## <a name="applying-materials-to-the-model"></a>Aplicando materiais ao modelo  
  
 Para que uma malha tenha a aparência de um objeto tridimensional, ela deve ter uma textura aplicada para cobrir a superfície definida por seus vértices e triângulos, de forma que ela possa ser iluminada e projetada pela câmera. Em 2D, você usa a <xref:System.Windows.Media.Brush> classe para aplicar cores, padrões, gradientes ou outro conteúdo visual a áreas da tela.  No entanto, a aparência de objetos 3D é uma função do modelo de iluminação, não apenas da cor ou padrão aplicado a eles. Objetos do mundo real refletem a luz de maneira diferente dependendo da qualidade de suas superfícies: superfícies brilhantes não têm a mesma aparência que superfícies ásperas ou foscas, e alguns objetos parecem absorver luz, enquanto outros brilham. Você pode aplicar todos os mesmos pincéis a objetos 3D que podem ser aplicados a objetos 2D, mas não pode aplicá-los diretamente.  
  
 Para definir as características da superfície de um modelo, [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] o usa a <xref:System.Windows.Media.Media3D.Material> classe abstract. As subclasses concretas de Material determinam algumas das características de aparência da superfície do modelo, e cada uma delas também fornece uma propriedade Brush para a qual você pode passar um SolidColorBrush, TileBrush ou VisualBrush.  
  
- <xref:System.Windows.Media.Media3D.DiffuseMaterial>Especifica que o pincel será aplicado ao modelo como se esse modelo estivesse aceso difusamente. Usar o DiffuseMaterial mais se assemelha ao uso de pincéis diretamente em modelos 2D; as superfícies de modelo não refletem a luz como se fosse brilhante.  
  
- <xref:System.Windows.Media.Media3D.SpecularMaterial>Especifica que o pincel será aplicado ao modelo como se a superfície do modelo fosse rígida ou brilhante, capaz de refletir os destaques. Você pode definir o grau para o qual a textura irá sugerir essa qualidade reflexiva, ou "brilhar", especificando um valor para a <xref:System.Windows.Media.Media3D.SpecularMaterial.SpecularPower%2A> propriedade.  
  
- <xref:System.Windows.Media.Media3D.EmissiveMaterial>permite que você especifique que a textura será aplicada como se o modelo estivesse emitindo uma luz igual à cor do pincel. Isso não faz do modelo uma luz; no entanto, ele participará de maneira diferente do sombreamento comparado ao que ocorreria caso fosse texturizado com DiffuseMaterial ou SpecularMaterial.  
  
 Para obter um melhor desempenho, os bastidores de um <xref:System.Windows.Media.Media3D.GeometryModel3D> (que estão fora de exibição porque estão no lado oposto do modelo da câmera) são escolhidos da cena.  Para especificar um <xref:System.Windows.Media.Media3D.Material> a ser aplicado ao backface de um modelo como um plano, defina a propriedade do modelo <xref:System.Windows.Media.Media3D.GeometryModel3D.BackMaterial%2A> .  
  
 Para obter algumas qualidades de superfície, como efeitos brilhantes ou reflexivos, talvez você deseje aplicar vários pincéis diferentes a um modelo em sucessão. Você pode aplicar e reutilizar vários materiais usando a <xref:System.Windows.Media.Media3D.MaterialGroup> classe. Os filhos do MaterialGroup são aplicados do primeiro ao último em várias aplicações de renderização.  
  
 Os exemplos de código a seguir mostram como aplicar uma cor sólida e um desenho como pincéis a modelos 3D.  
  
 [!code-xaml[basic3d#Basic3DXAML3DN5](~/samples/snippets/xaml/VS_Snippets_Wpf/Basic3D/XAML/Window1.xaml#basic3dxaml3dn5)]  
  ' [!code-xaml[3doverview#3DOverview3DN9](~/samples/snippets/csharp/VS_Snippets_Wpf/3DOverview/CSharp/app.xaml#3doverview3dn9)]  
 ' [!code-csharp[3doverview#3DOverview3DN8](~/samples/snippets/csharp/VS_Snippets_Wpf/3DOverview/CSharp/Window1.xaml.cs#3doverview3dn8)]
  [!code-vb[3doverview#3DOverview3DN8](~/samples/snippets/visualbasic/VS_Snippets_Wpf/3DOverview/visualbasic/window1.xaml.vb#3doverview3dn8)]  
  
<a name="lights"></a>
## <a name="illuminating-the-scene"></a>Iluminando a cena  
 As luzes em gráficos 3D fazem o que as luzes fazem no mundo real: tornam as superfícies visíveis. Mais especificamente, as luzes determinam qual parte de uma cena será incluída na projeção. Os objetos de luz do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] criam uma variedade de efeitos de luz e sombra e são modelados com o comportamento de várias luzes do mundo real. Inclua pelo menos uma luz em sua cena ou nenhum modelo estará visível.  
  
 As luzes a seguir derivam da classe base <xref:System.Windows.Media.Media3D.Light> :  
  
- <xref:System.Windows.Media.Media3D.AmbientLight>: Fornece a iluminação ambiente que ilumina todos os objetos uniformemente, independentemente de sua localização ou orientação.  
  
- <xref:System.Windows.Media.Media3D.DirectionalLight>: Ilumina como uma fonte de luz distante.  Luzes direcionais têm um <xref:System.Windows.Media.Media3D.DirectionalLight.Direction%2A> especificado como um Vector3D, mas nenhum local especificado.  
  
- <xref:System.Windows.Media.Media3D.PointLight>: Ilumina como uma fonte de luz próxima. As PointLights têm uma posição e lançam uma luz a dessa posição. Os objetos na cena são iluminados dependendo de sua posição e distância com relação à luz. <xref:System.Windows.Media.Media3D.PointLightBase>expõe uma <xref:System.Windows.Media.Media3D.PointLightBase.Range%2A> propriedade, que determina uma distância além da qual os modelos não serão iluminados pela luz. PointLight também expõe propriedades de atenuação, que determinam como a intensidade da luz diminui à distância. Você pode especificar interpolações constantes, lineares ou quadráticas para a atenuação da luz.  
  
- <xref:System.Windows.Media.Media3D.SpotLight>: Herda de <xref:System.Windows.Media.Media3D.PointLight> . Os destaques iluminam como PointLight e têm posição e direção. Elas projetam claros em uma área em forma de cone definida por <xref:System.Windows.Media.Media3D.SpotLight.InnerConeAngle%2A> e <xref:System.Windows.Media.Media3D.SpotLight.OuterConeAngle%2A> Propriedades, especificadas em graus.  
  
 As luzes são <xref:System.Windows.Media.Media3D.Model3D> objetos, de modo que você pode transformar e animar propriedades claras, incluindo posição, cor, direção e intervalo.  
  
 [!code-xaml[hittest3d#HitTest3D3DN6](~/samples/snippets/csharp/VS_Snippets_Wpf/HitTest3D/CSharp/Window1.xaml#hittest3d3dn6)]  
  
 [!code-csharp[basic3d#Basic3D3DN11](~/samples/snippets/csharp/VS_Snippets_Wpf/Basic3D/CSharp/Window1.xaml.cs#basic3d3dn11)]
 [!code-vb[basic3d#Basic3D3DN11](~/samples/snippets/visualbasic/VS_Snippets_Wpf/Basic3D/visualbasic/window1.xaml.vb#basic3d3dn11)]  
  
 [!code-csharp[basic3d#Basic3D3DN12](~/samples/snippets/csharp/VS_Snippets_Wpf/Basic3D/CSharp/Window1.xaml.cs#basic3d3dn12)]
 [!code-vb[basic3d#Basic3D3DN12](~/samples/snippets/visualbasic/VS_Snippets_Wpf/Basic3D/visualbasic/window1.xaml.vb#basic3d3dn12)]  
  
 [!code-csharp[basic3d#Basic3D3DN13](~/samples/snippets/csharp/VS_Snippets_Wpf/Basic3D/CSharp/Window1.xaml.cs#basic3d3dn13)]
 [!code-vb[basic3d#Basic3D3DN13](~/samples/snippets/visualbasic/VS_Snippets_Wpf/Basic3D/visualbasic/window1.xaml.vb#basic3d3dn13)]  
  
<a name="transforms"></a>
## <a name="transforming-models"></a>Transformando modelos  
 Quando você cria modelos, eles têm uma localização específica na cena. Para mover esses modelos na cena, girá-los ou alterar seus tamanhos, não é prático alterar os vértices que definem os próprios modelos.  Em vez disso, assim como em 2D, você aplica transformações a modelos.  
  
 Cada objeto de modelo tem uma <xref:System.Windows.Media.Media3D.Model3D.Transform%2A> propriedade com a qual você pode mover, reorientar ou redimensionar o modelo.  Ao aplicar uma transformação, você efetivamente desloca todos os pontos do modelo por meio de qualquer vetor ou valor especificado pela transformação. Em outras palavras, você transformou o espaço de coordenadas no qual o modelo está definido (“espaço do modelo”), mas não alterou os valores que compõem a geometria do modelo no sistema de coordenadas de toda a cena (“espaço de mundo”).  
  
 Para obter mais informações sobre como transformar modelos, consulte [visão geral de transformações 3D](3-d-transformations-overview.md).  
  
<a name="animations"></a>
## <a name="animating-models"></a>Animando modelos  
 A [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] implementação 3D participa do mesmo sistema de tempo e animação que os gráficos 2D. Em outras palavras, para animar uma cena 3D, anime as propriedades de seus modelos. É possível animar propriedades de primitivas diretamente, mas é geralmente mais fácil animar transformações que alteram a posição ou a aparência de modelos. Como as transformações podem ser aplicadas a <xref:System.Windows.Media.Media3D.Model3DGroup> objetos, bem como modelos individuais, é possível aplicar um conjunto de animações a um filho de um Model3DGroup e outro conjunto de animações a um grupo de objetos filho. Você também pode obter uma variedade de efeitos visuais animando as propriedades de iluminação da cena. Por fim, é possível optar por animar a própria projeção animando a posição ou o campo de visão da câmera. Para obter informações básicas sobre o sistema de animação e tempo do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)], consulte os tópicos [Visão geral da animação](animation-overview.md), [Visão geral dos storyboards](storyboards-overview.md) e [Visão geral dos objetos congeláveis](../advanced/freezable-objects-overview.md).  
  
 Para animar um objeto no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)], você cria uma linha do tempo, define uma animação (que é, na verdade, uma alteração em alguns valores da propriedade ao longo do tempo) e especifica a propriedade à qual aplicar a animação. Como todos os objetos em uma cena 3D são filhos de <xref:System.Windows.Controls.Viewport3D> , as propriedades direcionadas a qualquer animação que você deseja aplicar à cena são propriedades de Viewport3D.  
  
 Suponha que você deseje fazer com que um modelo pareça se mexer no lugar. Você pode optar por aplicar um <xref:System.Windows.Media.Media3D.RotateTransform3D> ao modelo e animar o eixo de sua rotação de um vetor para outro. O exemplo de código a seguir demonstra a aplicação de uma Vector3DAnimation à propriedade do Eixo da Rotation3D da transformação, supondo que a RotateTransform3D seja uma das várias transformações aplicadas ao modelo com um TransformGroup.  
  
 [!code-csharp[3doverview#3DOverview3DN1](~/samples/snippets/csharp/VS_Snippets_Wpf/3DOverview/CSharp/Window1.xaml.cs#3doverview3dn1)]
 [!code-vb[3doverview#3DOverview3DN1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/3DOverview/visualbasic/window1.xaml.vb#3doverview3dn1)]  
  
 [!code-csharp[3doverview#3DOverview3DN3](~/samples/snippets/csharp/VS_Snippets_Wpf/3DOverview/CSharp/Window1.xaml.cs#3doverview3dn3)]
 [!code-vb[3doverview#3DOverview3DN3](~/samples/snippets/visualbasic/VS_Snippets_Wpf/3DOverview/visualbasic/window1.xaml.vb#3doverview3dn3)]  
  
 [!code-csharp[3doverview#3DOverview3DN4](~/samples/snippets/csharp/VS_Snippets_Wpf/3DOverview/CSharp/Window1.xaml.cs#3doverview3dn4)]
 [!code-vb[3doverview#3DOverview3DN4](~/samples/snippets/visualbasic/VS_Snippets_Wpf/3DOverview/visualbasic/window1.xaml.vb#3doverview3dn4)]  
  
 [!code-csharp[3doverview#3DOverview3DN5](~/samples/snippets/csharp/VS_Snippets_Wpf/3DOverview/CSharp/Window1.xaml.cs#3doverview3dn5)]
 [!code-vb[3doverview#3DOverview3DN5](~/samples/snippets/visualbasic/VS_Snippets_Wpf/3DOverview/visualbasic/window1.xaml.vb#3doverview3dn5)]  
  
<a name="animations1"></a>
## <a name="add-3d-content-to-the-window"></a>Adicionar conteúdo 3D à janela  
 Para renderizar a cena, adicione modelos e luzes a um <xref:System.Windows.Media.Media3D.Model3DGroup> e, em seguida, defina o <xref:System.Windows.Media.Media3D.Model3DGroup> como <xref:System.Windows.Media.Media3D.ModelVisual3D.Content%2A> de um <xref:System.Windows.Media.Media3D.ModelVisual3D> . Adicione o <xref:System.Windows.Media.Media3D.ModelVisual3D> à <xref:System.Windows.Controls.Viewport3D.Children%2A> coleção do <xref:System.Windows.Controls.Viewport3D> . Adicione câmeras ao <xref:System.Windows.Controls.Viewport3D> definindo sua <xref:System.Windows.Controls.Viewport3D.Camera%2A> propriedade.  
  
 Por fim, adicione o <xref:System.Windows.Controls.Viewport3D> à janela. Quando o <xref:System.Windows.Controls.Viewport3D> é incluído como o conteúdo de um elemento de layout como Canvas, especifique o tamanho do Viewport3D definindo suas <xref:System.Windows.FrameworkElement.Height%2A> Propriedades e <xref:System.Windows.FrameworkElement.Width%2A> (herdadas de <xref:System.Windows.FrameworkElement> ).  
  
 [!code-xaml[hostingwpfusercontrolinwf#1](~/samples/snippets/csharp/VS_Snippets_Wpf/HostingWpfUserControlInWf/CSharp/HostingWpfUserControlInWf/ConeControl.xaml#1)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Controls.Viewport3D>
- <xref:System.Windows.Media.Media3D.PerspectiveCamera>
- <xref:System.Windows.Media.Media3D.DirectionalLight>
- <xref:System.Windows.Media.Media3D.Material>
- [Visão geral de transformações 3D](3-d-transformations-overview.md)
- [Maximizar desempenho 3D do WPF](maximize-wpf-3d-performance.md)
- [Tópicos explicativos](3-d-graphics-how-to-topics.md)
- [Visão geral de formas e desenho básico no WPF](shapes-and-basic-drawing-in-wpf-overview.md)
- [Pintando com imagens, desenhos e visuais](painting-with-images-drawings-and-visuals.md)
