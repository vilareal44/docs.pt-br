---
title: Estilos e modelos DataGrid
description: Saiba mais sobre estilos e modelos para o controle DataGrid de Windows Presentation Foundation. Modifique o ControlTemplate para dar ao controle uma aparência exclusiva.
ms.date: 03/30/2017
helpviewer_keywords:
- states [WPF], DataGrid
- ControlTemplate [WPF], DataGrid
- DataGrid [WPF], styles and templates
- templates [WPF], DataGrid
- styles [WPF], DataGrid
- parts [WPF], DataGrid
ms.assetid: 9cb31d63-f148-4d25-b079-816e73f988c7
ms.openlocfilehash: dd526ec64d5077dad58f31c004f47e63c57ec9de
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168330"
---
# <a name="datagrid-styles-and-templates"></a>Estilos e modelos DataGrid
Este tópico descreve os estilos e modelos para o <xref:System.Windows.Controls.DataGrid> controle. Você pode modificar o padrão <xref:System.Windows.Controls.ControlTemplate> para dar ao controle uma aparência exclusiva. Para obter mais informações, consulte [criar um modelo para um controle](../../../desktop-wpf/themes/how-to-create-apply-template.md).  
  
## <a name="datagrid-parts"></a>Partes de DataGrid  
 A tabela a seguir lista as partes nomeadas para o <xref:System.Windows.Controls.DataGrid> controle.  
  
|Parte|Type|Descrição|  
|-|-|-|  
|PART_ColumnHeadersPresenter|<xref:System.Windows.Controls.Primitives.DataGridColumnHeadersPresenter>|A linha que contém os cabeçalhos de coluna.|  
  
 Quando você cria um <xref:System.Windows.Controls.ControlTemplate> para o <xref:System.Windows.Controls.DataGrid> , seu modelo pode conter um <xref:System.Windows.Controls.ItemsPresenter> dentro de um <xref:System.Windows.Controls.ScrollViewer> . (O <xref:System.Windows.Controls.ItemsPresenter> exibe cada item no <xref:System.Windows.Controls.DataGrid> ; o <xref:System.Windows.Controls.ScrollViewer> habilita a rolagem dentro do controle).  Se o <xref:System.Windows.Controls.ItemsPresenter> não for o filho direto do <xref:System.Windows.Controls.ScrollViewer> , você deverá dar o <xref:System.Windows.Controls.ItemsPresenter> nome, `ItemsPresenter` .  
  
 O modelo padrão para o <xref:System.Windows.Controls.DataGrid> contém um <xref:System.Windows.Controls.ScrollViewer> controle. Para obter mais informações sobre as partes definidas pelo <xref:System.Windows.Controls.ScrollViewer> , consulte [estilos e modelos de ScrollViewer](scrollviewer-styles-and-templates.md).  
  
## <a name="datagrid-states"></a>Estados de DataGrid  
 A tabela a seguir lista os Estados visuais para o <xref:System.Windows.Controls.DataGrid> controle.  
  
|Nome do VisualState|Nome do VisualStateGroup|Descrição|  
|-|-|-|  
|Normal|CommonStates|O estado padrão.|  
|Desabilitado|CommonStates|O controle está desabilitado.|  
|InvalidFocused|ValidationStates|O controle não é válido e tem o foco.|  
|InvalidUnfocused|ValidationStates|O controle não é válido e não tem o foco.|  
|Válido|ValidationStates|O controle é válido.|  
  
## <a name="datagridcell-parts"></a>Partes de DataGridCell  
 O <xref:System.Windows.Controls.DataGridCell> elemento não tem nenhuma parte nomeada.  
  
## <a name="datagridcell-states"></a>Estados de DataGridCell  
 A tabela a seguir lista os Estados visuais para o <xref:System.Windows.Controls.DataGridCell> elemento.  
  
|Nome do VisualState|Nome do VisualStateGroup|Descrição|  
|-|-|-|  
|Normal|CommonStates|O estado padrão.|  
|MouseOver|CommonStates|O ponteiro do mouse está posicionado sobre a célula.|  
|Focalizado|FocusStates|A célula tem o foco.|  
|Sem foco|FocusStates|A célula não tem o foco|  
|Current|CurrentStates|A célula é a célula atual.|  
|Regular|CurrentStates|A célula não é a célula atual.|  
|Monitor|InteractionStates|A célula está no modo de exibição.|  
|Edição|InteractionStates|A célula está no modo de edição.|  
|Selecionado|SelectionStates|A célula está selecionada.|  
|Não selecionado|SelectionStates|A célula não está selecionada.|  
|InvalidFocused|ValidationStates|A célula não é válida e tem o foco.|  
|InvalidUnfocused|ValidationStates|A célula não é válida e não tem o foco.|  
|Válido|ValidationStates|A célula é válida.|  
  
## <a name="datagridrow-parts"></a>Partes da DataGridRow  
 O <xref:System.Windows.Controls.DataGridRow> elemento não tem nenhuma parte nomeada.  
  
## <a name="datagridrow-states"></a>Estados de DataGridRow  
 A tabela a seguir lista os Estados visuais para o <xref:System.Windows.Controls.DataGridRow> elemento.  
  
|Nome do VisualState|Nome do VisualStateGroup|Descrição|  
|-|-|-|  
|Normal|CommonStates|O estado padrão.|  
|MouseOver|CommonStates|O ponteiro do mouse está posicionado sobre a linha.|  
|MouseOver_Editing|CommonStates|O ponteiro do mouse é posicionado sobre a linha e a linha está no modo de edição.|  
|MouseOver_Selected|CommonStates|O ponteiro do mouse está posicionado sobre a linha e a linha está selecionada.|  
|MouseOver_Unfocused_Editing|CommonStates|O ponteiro do mouse está posicionado sobre a linha, a linha está no modo de edição e não tem o foco.|  
|MouseOver_Unfocused_Selected|CommonStates|O ponteiro do mouse está posicionado sobre a linha, a linha está selecionada e não tem o foco.|  
|Normal_AlternatingRow|CommonStates|A linha é uma linha alternada.|  
|Normal_Editing|CommonStates|A linha está no modo de edição.|  
|Normal_Selected|CommonStates|A linha está selecionada.|  
|Unfocused_Editing|CommonStates|A linha está no modo de edição e não tem o foco.|  
|Unfocused_Selected|CommonStates|A linha está selecionada e não tem o foco.|  
|InvalidFocused|ValidationStates|O controle não é válido e tem o foco.|  
|InvalidUnfocused|ValidationStates|O controle não é válido e não tem o foco.|  
|Válido|ValidationStates|O controle é válido.|  
  
## <a name="datagridrowheader-parts"></a>Partes de DataGridRowHeader  
 A tabela a seguir lista as partes nomeadas para o <xref:System.Windows.Controls.Primitives.DataGridRowHeader> elemento.  
  
|Parte|Type|Descrição|  
|-|-|-|  
|PART_TopHeaderGripper|<xref:System.Windows.Controls.Primitives.Thumb>|O elemento usado para redimensionar o cabeçalho de linha partindo da parte superior.|  
|PART_BottomHeaderGripper|<xref:System.Windows.Controls.Primitives.Thumb>|O elemento usado para redimensionar o cabeçalho de linha partindo da parte inferior.|  
  
## <a name="datagridrowheader-states"></a>Estados de DataGridRowHeader  
 A tabela a seguir lista os Estados visuais para o <xref:System.Windows.Controls.Primitives.DataGridRowHeader> elemento.  
  
|Nome do VisualState|Nome do VisualStateGroup|Descrição|  
|-|-|-|  
|Normal|CommonStates|O estado padrão.|  
|MouseOver|CommonStates|O ponteiro do mouse está posicionado sobre a linha.|  
|MouseOver_CurrentRow|CommonStates|O ponteiro do mouse está posicionado sobre a linha e a linha é na linha atual.|  
|MouseOver_CurrentRow_Selected|CommonStates|O ponteiro do mouse está posicionado sobre a linha e a linha é a atual e está selecionada.|  
|MouseOver_EditingRow|CommonStates|O ponteiro do mouse é posicionado sobre a linha e a linha está no modo de edição.|  
|MouseOver_Selected|CommonStates|O ponteiro do mouse está posicionado sobre a linha e a linha está selecionada.|  
|MouseOver_Unfocused_CurrentRow_Selected|CommonStates|O ponteiro do mouse está posicionado sobre a linha, a linha é a atual e está selecionada e não tem o foco.|  
|MouseOver_Unfocused_EditingRow|CommonStates|O ponteiro do mouse está posicionado sobre a linha, a linha está no modo de edição e não tem o foco.|  
|MouseOver_Unfocused_Selected|CommonStates|O ponteiro do mouse está posicionado sobre a linha, a linha está selecionada e não tem o foco.|  
|Normal_CurrentRow|CommonStates|A linha é a linha atual.|  
|Normal_CurrentRow_Selected|CommonStates|A linha é a linha atual e está selecionada.|  
|Normal_EditingRow|CommonStates|A linha está no modo de edição.|  
|Normal_Selected|CommonStates|A linha está selecionada.|  
|Unfocused_CurrentRow_Selected|CommonStates|A linha é a linha atual, está selecionada e não tem o foco.|  
|Unfocused_EditingRow|CommonStates|A linha está no modo de edição e não tem o foco.|  
|Unfocused_Selected|CommonStates|A linha está selecionada e não tem o foco.|  
|InvalidFocused|ValidationStates|O controle não é válido e tem o foco.|  
|InvalidUnfocused|ValidationStates|O controle não é válido e não tem o foco.|  
|Válido|ValidationStates|O controle é válido.|  
  
## <a name="datagridcolumnheaderspresenter-parts"></a>Partes de DataGridColumnHeadersPresenter  
 A tabela a seguir lista as partes nomeadas para o <xref:System.Windows.Controls.Primitives.DataGridColumnHeadersPresenter> elemento.  
  
|Parte|Type|Descrição|  
|-|-|-|  
|PART_FillerColumnHeader|<xref:System.Windows.Controls.Primitives.DataGridColumnHeader>|O espaço reservado para cabeçalhos de coluna.|  
  
## <a name="datagridcolumnheaderspresenter-states"></a>Estados de DataGridColumnHeadersPresenter  
 A tabela a seguir lista os Estados visuais para o <xref:System.Windows.Controls.Primitives.DataGridColumnHeadersPresenter> elemento.  
  
|Nome do VisualState|Nome do VisualStateGroup|Descrição|  
|-|-|-|  
|InvalidFocused|ValidationStates|A célula não é válida e tem o foco.|  
|InvalidUnfocused|ValidationStates|A célula não é válida e não tem o foco.|  
|Válido|ValidationStates|A célula é válida.|  
  
## <a name="datagridcolumnheader-parts"></a>Partes de DataGridColumnHeader  
 A tabela a seguir lista as partes nomeadas para o <xref:System.Windows.Controls.Primitives.DataGridColumnHeader> elemento.  
  
|Parte|Type|Descrição|  
|-|-|-|  
|PART_LeftHeaderGripper|<xref:System.Windows.Controls.Primitives.Thumb>|O elemento usado para redimensionar o cabeçalho de coluna partindo da esquerda.|  
|PART_RightHeaderGripper|<xref:System.Windows.Controls.Primitives.Thumb>|O elemento usado para redimensionar o cabeçalho de coluna partindo da direita.|  
  
## <a name="datagridcolumnheader-states"></a>Estados de DataGridColumnHeader  
 A tabela a seguir lista os Estados visuais para o <xref:System.Windows.Controls.Primitives.DataGridColumnHeader> elemento.  
  
|Nome do VisualState|Nome do VisualStateGroup|Descrição|  
|-|-|-|  
|Normal|CommonStates|O estado padrão.|  
|MouseOver|CommonStates|O ponteiro do mouse é posicionado sobre o controle.|  
|Pressionado|CommonStates|O controle é pressionado.|  
|SortAscending|SortStates|A coluna é classificada em ordem crescente.|  
|SortDescending|SortStates|A coluna é classificada em ordem decrescente.|  
|Não Classificado|SortStates|A coluna não é classificada.|  
|InvalidFocused|ValidationStates|O controle não é válido e tem o foco.|  
|InvalidUnfocused|ValidationStates|O controle não é válido e não tem o foco.|  
|Válido|ValidationStates|O controle é válido.|  
  
## <a name="datagrid-controltemplate-example"></a>Exemplo de ControlTemplate de DataGrid  
 O exemplo a seguir mostra como definir um <xref:System.Windows.Controls.ControlTemplate> para o <xref:System.Windows.Controls.DataGrid> controle e seus tipos associados.  
  
 [!code-xaml[ControlTemplateExamples#DataGrid](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/datagrid.xaml#datagrid)]  
  
 O exemplo anterior usa um ou mais dos seguintes recursos.  
  
 [!code-xaml[ControlTemplateExamples#Resources](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/shared.xaml#resources)]  
  
 Para ver o exemplo completo, consulte [Styling with ControlTemplates Sample (Estilos com a amostra ControlTemplates)](https://github.com/Microsoft/WPF-Samples/tree/master/Styles%20&%20Templates/IntroToStylingAndTemplating).  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.FrameworkElement.Style%2A>
- <xref:System.Windows.Controls.ControlTemplate>
- [Estilos e modelos de controle](control-styles-and-templates.md)
- [Personalização do controle](control-customization.md)
- [Estilo e modelagem](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
- [Criar um modelo para um controle](../../../desktop-wpf/themes/how-to-create-apply-template.md)
