---
title: Visão geral de GridView
description: Saiba mais sobre estilos e modelos para o controle Windows Presentation Foundation ListView. Modifique o ControlTemplate para dar ao controle uma aparência exclusiva.
ms.date: 03/30/2017
helpviewer_keywords:
- GridView view mode [WPF]
- ListView controls [WPF], GridView view mode
- controls [WPF], ListView
ms.assetid: b2d02267-32b3-40ce-8e9f-06972d8749d9
ms.openlocfilehash: 02a2182ef1fc893107e434f431b9fbe0b3fbcd08
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165926"
---
# <a name="gridview-overview"></a>Visão geral de GridView
<xref:System.Windows.Controls.GridView>o modo de exibição é um dos modos de exibição para um <xref:System.Windows.Controls.ListView> controle. A <xref:System.Windows.Controls.GridView> classe e suas classes de suporte permitem que você e seus usuários exibam coleções de itens em uma tabela que normalmente usa botões como cabeçalhos de coluna interativos. Este tópico apresenta a <xref:System.Windows.Controls.GridView> classe e descreve seu uso.  

<a name="DefiningaListViewthatusesGridViewView"></a>
## <a name="what-is-a-gridview-view"></a>O que é um modo de exibição GridView?  
 O <xref:System.Windows.Controls.GridView> modo de exibição exibe uma lista de itens de dados associando campos de dados a colunas e exibindo um cabeçalho de coluna para identificar o campo. O <xref:System.Windows.Controls.GridView> estilo padrão implementa botões como cabeçalhos de coluna. Usando botões para cabeçalhos de coluna, você pode implementar recursos importantes de interação do usuário; por exemplo, os usuários podem clicar no cabeçalho da coluna para classificar <xref:System.Windows.Controls.GridView> dados de acordo com o conteúdo de uma coluna específica.  
  
> [!NOTE]
> Os controles de botão que o <xref:System.Windows.Controls.GridView> usa para cabeçalhos de coluna são derivados de <xref:System.Windows.Controls.Primitives.ButtonBase> .  
  
 A ilustração a seguir mostra uma <xref:System.Windows.Controls.GridView> exibição do <xref:System.Windows.Controls.ListView> conteúdo.  

 ![Captura de tela que mostra a exibição GridView do conteúdo de ListView.](./media/gridview-overview/styled-listview-content.png)  
  
 <xref:System.Windows.Controls.GridView>as colunas são representadas por <xref:System.Windows.Controls.GridViewColumn> objetos, que podem ser dimensionadas automaticamente para seu conteúdo. Opcionalmente, você pode definir explicitamente um <xref:System.Windows.Controls.GridViewColumn> para uma largura específica. Você pode redimensionar colunas arrastando a alça entre cabeçalhos de coluna. Você também pode adicionar, remover, substituir e reordenar colunas dinamicamente, pois essa funcionalidade é incorporada ao <xref:System.Windows.Controls.GridView> . No entanto, <xref:System.Windows.Controls.GridView> o não pode atualizar diretamente os dados que ele exibe.  
  
 O exemplo a seguir mostra como definir um <xref:System.Windows.Controls.GridView> que exibe dados de funcionários. Neste exemplo, <xref:System.Windows.Controls.ListView> define o `EmployeeInfoDataSource` como o <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> . As definições de propriedade de <xref:System.Windows.Controls.GridViewColumn.DisplayMemberBinding%2A> associar <xref:System.Windows.Controls.GridViewColumn> conteúdo a `EmployeeInfoDataSource` categorias de dados.  
  
 [!code-xaml[ListViewCode#ListViewEmployee](~/samples/snippets/csharp/VS_Snippets_Wpf/ListViewCode/CSharp/Window1.xaml#listviewemployee)]  
  
 A ilustração a seguir mostra a tabela que o exemplo anterior cria. O controle GridView exibe dados de um objeto ItemsSource:

 ![Captura de tela que mostra um ListView com saída GridView.](./media/gridview-overview/listview-gridview-output.jpg)  
  
<a name="GridViewLayoutandStyle"></a>
## <a name="gridview-layout-and-style"></a>Layout e estilo do GridView  
 As células de coluna e o cabeçalho de coluna de um <xref:System.Windows.Controls.GridViewColumn> têm a mesma largura. Por padrão, cada coluna redimensiona sua largura para caber seu conteúdo. Opcionalmente, você pode definir uma coluna com uma largura fixa.  
  
 Conteúdo de dados relacionado são exibidos em linhas horizontais. Por exemplo, na ilustração anterior, sobrenome, nome e número de identificação de cada funcionário são exibidos como um conjunto porque eles aparecem em uma linha horizontal.  
  
<a name="DefiningandStylingColumnsinaGridView"></a>
### <a name="defining-and-styling-columns-in-a-gridview"></a>Definindo e aplicando estilos sobre colunas em um GridView  
 Ao definir o campo de dados a ser exibido em um <xref:System.Windows.Controls.GridViewColumn> , use as <xref:System.Windows.Controls.GridViewColumn.DisplayMemberBinding%2A> <xref:System.Windows.Controls.GridViewColumn.CellTemplate%2A> Propriedades, ou <xref:System.Windows.Controls.GridViewColumn.CellTemplateSelector%2A> . A <xref:System.Windows.Controls.GridViewColumn.DisplayMemberBinding%2A> propriedade tem precedência sobre qualquer uma das propriedades do modelo.  
  
 Para especificar o alinhamento do conteúdo em uma coluna de a <xref:System.Windows.Controls.GridView> , defina um <xref:System.Windows.Controls.GridViewColumn.CellTemplate%2A> . Não use as <xref:System.Windows.Controls.Control.HorizontalContentAlignment%2A> Propriedades e <xref:System.Windows.Controls.Control.VerticalContentAlignment%2A> para o <xref:System.Windows.Controls.ListView> conteúdo que é exibido usando um <xref:System.Windows.Controls.GridView> .  
  
 Para especificar propriedades de modelo e estilo para cabeçalhos de coluna, use as <xref:System.Windows.Controls.GridView> <xref:System.Windows.Controls.GridViewColumn> classes, e <xref:System.Windows.Controls.GridViewColumnHeader> . Para obter mais informações, consulte [Visão geral de modelos e estilos de cabeçalho de coluna GridView](gridview-column-header-styles-and-templates-overview.md).  
  
<a name="AddingVisualElementstoaGridViewView"></a>
### <a name="adding-visual-elements-to-a-gridview"></a>Adicionando elementos visuais a um GridView  
 Para adicionar elementos visuais, como <xref:System.Windows.Controls.CheckBox> e <xref:System.Windows.Controls.Button> controles, a um <xref:System.Windows.Controls.GridView> modo de exibição, use modelos ou estilos.  
  
 Se você definir explicitamente um elemento visual como um item de dados, ele poderá aparecer apenas uma vez em um <xref:System.Windows.Controls.GridView> . Essa limitação existe porque um elemento pode ter apenas um pai e portanto, pode aparecer apenas uma vez na árvore visual.  
  
<a name="StylingRowsinaGridViewView"></a>
### <a name="styling-rows-in-a-gridview"></a>Aplicando estilo a linhas em um GridView  
 Use as <xref:System.Windows.Controls.GridViewRowPresenter> <xref:System.Windows.Controls.GridViewHeaderRowPresenter> classes e para formatar e exibir as linhas de um <xref:System.Windows.Controls.GridView> . Para obter um exemplo de como estilizar linhas em um <xref:System.Windows.Controls.GridView> modo de exibição, consulte [Estilizar uma linha em um ListView que implementa um GridView](how-to-style-a-row-in-a-listview-that-implements-a-gridview.md).  
  
<a name="AlignmentIssuesWhenUsingItemContainerStyle"></a>
### <a name="alignment-issues-when-you-use-itemcontainerstyle"></a>Problemas de alinhamento ao usar ItemContainerStyle  
 Para evitar problemas de alinhamento entre cabeçalhos e células de coluna, não defina uma propriedade ou especifique um modelo que afete a largura de um item em um <xref:System.Windows.Controls.ItemsControl.ItemContainerStyle%2A> . Por exemplo, não defina a <xref:System.Windows.FrameworkElement.Margin%2A> propriedade ou especifique um <xref:System.Windows.Controls.ControlTemplate> que adicione um <xref:System.Windows.Controls.CheckBox> a um <xref:System.Windows.Controls.ItemsControl.ItemContainerStyle%2A> que esteja definido em um <xref:System.Windows.Controls.ListView> controle. Em vez disso, especifique as propriedades e os modelos que afetam a largura da coluna diretamente em classes que definem um <xref:System.Windows.Controls.GridView> modo de exibição.  
  
 Por exemplo, para adicionar uma <xref:System.Windows.Controls.CheckBox> às linhas no <xref:System.Windows.Controls.GridView> modo de exibição, adicione a <xref:System.Windows.Controls.CheckBox> a a <xref:System.Windows.DataTemplate> e, em seguida, defina a <xref:System.Windows.Controls.GridViewColumn.CellTemplate%2A> propriedade para isso <xref:System.Windows.DataTemplate> .  
  
<a name="InteractingwithaGridViewControl"></a>
## <a name="user-interactions-with-a-gridview"></a>Interações do usuário com um GridView  
 Quando você usa um <xref:System.Windows.Controls.GridView> em seu aplicativo, os usuários podem interagir com e modificar a formatação do <xref:System.Windows.Controls.GridView> . Por exemplo, os usuários podem reordenar colunas, redimensionar uma coluna, selecionar itens em uma tabela e navegar pelo conteúdo. Você também pode definir um manipulador de eventos que responde quando um usuário clica no botão de cabeçalho de coluna. O manipulador de eventos pode executar operações como classificar os dados que são exibidos no de <xref:System.Windows.Controls.GridView> acordo com o conteúdo de uma coluna.  
  
 A lista a seguir aborda mais detalhadamente os recursos de uso <xref:System.Windows.Controls.GridView> para a interação do usuário:  
  
- **Reordene colunas usando o método do tipo "arrastar e soltar".**  
  
     Os usuários podem reordenar colunas em um <xref:System.Windows.Controls.GridView> pressionando o botão esquerdo do mouse enquanto ele estiver sobre um cabeçalho de coluna e, em seguida, arrastando essa coluna para uma nova posição. Enquanto o usuário arrasta o cabeçalho da coluna, uma versão flutuante do cabeçalho é exibida, bem como uma linha preta sólida que mostra onde inserir a coluna.  
  
     Se você quiser modificar o estilo padrão para a versão flutuante de um cabeçalho, especifique um <xref:System.Windows.Controls.ControlTemplate> para um <xref:System.Windows.Controls.GridViewColumnHeader> tipo que é disparado quando a <xref:System.Windows.Controls.GridViewColumnHeader.Role%2A> propriedade é definida como <xref:System.Windows.Controls.GridViewColumnHeaderRole.Floating> . Para mais informações, consulte [Criar um estilo para um cabeçalho de coluna GridView arrastado](how-to-create-a-style-for-a-dragged-gridview-column-header.md).  
  
- **Redimensionar uma coluna para seu conteúdo.**  
  
     Os usuários podem clicar duas vezes na garra à direita do cabeçalho da coluna para redimensionar uma coluna para caber seu conteúdo.  
  
    > [!NOTE]
    > Você pode definir a <xref:System.Windows.Controls.GridViewColumn.Width%2A> propriedade como `Double.NaN` para produzir o mesmo efeito.  
  
- **Selecionar os itens de linha.**  
  
     Os usuários podem selecionar um ou mais itens em um <xref:System.Windows.Controls.GridView> .  
  
     Se você quiser alterar o <xref:System.Windows.Style> de um item selecionado, consulte [usar gatilhos para estilizar itens selecionados em um ListView](how-to-use-triggers-to-style-selected-items-in-a-listview.md).  
  
- **Barra de rolagem para exibir o conteúdo que não está visível inicialmente na tela.**  
  
     Se o tamanho do <xref:System.Windows.Controls.GridView> não for grande o suficiente para exibir todos os itens, os usuários poderão rolar horizontal ou verticalmente usando barras de rolagem, que são fornecidas por um <xref:System.Windows.Controls.ScrollViewer> controle. Um <xref:System.Windows.Controls.Primitives.ScrollBar> ficará oculto se todo o conteúdo estiver visível em uma direção específica. Cabeçalhos de coluna não rolam com uma barra de rolagem vertical, mas rolam horizontalmente.  
  
- **Interagir com colunas clicando nos botões de cabeçalho de coluna.**  
  
     Quando os usuários clicam um botão de cabeçalho de coluna, eles podem classificar os dados que serão exibidos na coluna se você tiver fornecido um algoritmo de classificação.  
  
     Você pode manipular o <xref:System.Windows.Controls.Primitives.ButtonBase.Click> evento para botões de cabeçalho de coluna a fim de fornecer funcionalidade como um algoritmo de classificação. Para manipular o <xref:System.Windows.Controls.Primitives.ButtonBase.Click> evento para um único cabeçalho de coluna, defina um manipulador de eventos no <xref:System.Windows.Controls.GridViewColumnHeader> . Para definir um manipulador de eventos que manipula o <xref:System.Windows.Controls.Primitives.ButtonBase.Click> evento para todos os cabeçalhos de coluna, defina o manipulador no <xref:System.Windows.Controls.ListView> controle.  
  
<a name="Obtaining_Other_Custom_Views"></a>
## <a name="obtaining-other-custom-views"></a>Obtendo outros modos de exibição personalizados  
 A <xref:System.Windows.Controls.GridView> classe, que é derivada da <xref:System.Windows.Controls.ViewBase> classe abstract, é apenas um dos modos de exibição possíveis para a <xref:System.Windows.Controls.ListView> classe. Você pode criar outros modos de exibição personalizados para <xref:System.Windows.Controls.ListView> derivando da <xref:System.Windows.Controls.ViewBase> classe. Para obter um exemplo de um modo de exibição personalizado, consulte [Criar um modo de exibição personalizado para um ListView](how-to-create-a-custom-view-mode-for-a-listview.md).  
  
<a name="GridViewSupportingClasses"></a>
## <a name="gridview-supporting-classes"></a>Classes de apoio a GridView  
 As classes a seguir dão suporte ao <xref:System.Windows.Controls.GridView> modo de exibição.  
  
- <xref:System.Windows.Controls.GridViewColumn>  
  
- <xref:System.Windows.Controls.GridViewColumnHeader>  
  
- <xref:System.Windows.Controls.GridViewRowPresenter>  
  
- <xref:System.Windows.Controls.GridViewHeaderRowPresenter>  
  
- <xref:System.Windows.Controls.GridViewColumnCollection>  
  
- <xref:System.Windows.Controls.GridViewColumnHeaderRole>  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Controls.ListView>
- <xref:System.Windows.Controls.ListViewItem>
- <xref:System.Windows.Controls.GridViewColumn>
- <xref:System.Windows.Controls.GridViewColumnHeader>
- <xref:System.Windows.Controls.GridViewRowPresenter>
- <xref:System.Windows.Controls.GridViewHeaderRowPresenter>
- <xref:System.Windows.Controls.ViewBase>
- [Visão geral de ListView](listview-overview.md)
- [Classificar uma coluna GridView quando um cabeçalho é clicado](how-to-sort-a-gridview-column-when-a-header-is-clicked.md)
- [Tópicos explicativos](listview-how-to-topics.md)
