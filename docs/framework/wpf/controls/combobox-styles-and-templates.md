---
title: Estilos e modelos ComboBox
description: Saiba mais sobre estilos e modelos para o Windows Presentation Foundation controle ComboBox. Modifique o ControlTemplate para dar ao controle uma aparência exclusiva.
ms.date: 03/30/2017
helpviewer_keywords:
- ComboBox [WPF], styles and templates
- states [WPF], ComboBox
- ControlTemplate [WPF], ComboBox
- styles [WPF], ComboBox
- templates [WPF], ComboBox
- parts [WPF], ComboBox
ms.assetid: b0662fa1-16d7-4320-b26b-c1804e565a44
ms.openlocfilehash: 5e929bafeaf849b4b5682a17ca51cb0aab963613
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165917"
---
# <a name="combobox-styles-and-templates"></a>Estilos e modelos ComboBox
Este tópico descreve os estilos e modelos para o <xref:System.Windows.Controls.ComboBox> controle. Você pode modificar o padrão <xref:System.Windows.Controls.ControlTemplate> para dar ao controle uma aparência exclusiva. Para obter mais informações, consulte [criar um modelo para um controle](../../../desktop-wpf/themes/how-to-create-apply-template.md).  
  
## <a name="combobox-parts"></a>Partes de ComboBox  
 A tabela a seguir lista as partes nomeadas para o <xref:System.Windows.Controls.ComboBox> controle.  
  
|Parte|Type|Descrição|  
|-|-|-|  
|PART_EditableTextBox|<xref:System.Windows.Controls.TextBox>|Contém o texto do <xref:System.Windows.Controls.ComboBox> .|  
|PART_Popup|<xref:System.Windows.Controls.Primitives.Popup>|A lista suspensa que contém os itens na caixa de combinação.|  
  
 Quando você cria um <xref:System.Windows.Controls.ControlTemplate> para o <xref:System.Windows.Controls.ComboBox> , seu modelo pode conter um <xref:System.Windows.Controls.ItemsPresenter> dentro de um <xref:System.Windows.Controls.ScrollViewer> . (O <xref:System.Windows.Controls.ItemsPresenter> exibe cada item no <xref:System.Windows.Controls.ComboBox> ; o <xref:System.Windows.Controls.ScrollViewer> habilita a rolagem dentro do controle).  Se o <xref:System.Windows.Controls.ItemsPresenter> não for o filho direto do <xref:System.Windows.Controls.ScrollViewer> , você deverá dar o <xref:System.Windows.Controls.ItemsPresenter> nome, `ItemsPresenter` .  
  
## <a name="combobox-states"></a>Estados de ComboBox  
 A tabela a seguir lista os Estados do <xref:System.Windows.Controls.ComboBox> controle.  
  
|Nome do VisualState|Nome do VisualStateGroup|Descrição|  
|-|-|-|  
|Normal|CommonStates|O estado padrão.|  
|Desabilitado|CommonStates|O controle está desabilitado.|  
|MouseOver|CommonStates|O ponteiro do mouse está sobre o <xref:System.Windows.Controls.ComboBox> controle.|  
|Focalizado|FocusStates|O controle tem foco.|  
|Sem foco|FocusStates|O controle não tem foco.|  
|FocusedDropDown|FocusStates|A lista suspensa do <xref:System.Windows.Controls.ComboBox> tem foco.|  
|Válido|ValidationStates|O controle usa a <xref:System.Windows.Controls.Validation> classe e a <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada é `false` .|  
|InvalidFocused|ValidationStates|A <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada é `true` e o controle tem foco.|  
|InvalidUnfocused|ValidationStates|A <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada é `true` e o controle não tem foco.|  
|Editável|EditStates|A propriedade <xref:System.Windows.Controls.ComboBox.IsEditable%2A> é `true`.|  
|Não editável|EditStates|A propriedade <xref:System.Windows.Controls.ComboBox.IsEditable%2A> é `false`.|  
  
## <a name="comboboxitem-parts"></a>Partes de ComboBoxItem  
 O <xref:System.Windows.Controls.ComboBoxItem> controle não tem nenhuma parte nomeada.  
  
## <a name="comboboxitem-states"></a>Estados de ComboBoxItem  
 A tabela a seguir lista os Estados do <xref:System.Windows.Controls.ComboBoxItem> controle.  
  
|Nome do VisualState|Nome do VisualStateGroup|Descrição|  
|-|-|-|  
|Normal|CommonStates|O estado padrão.|  
|Desabilitado|CommonStates|O controle está desabilitado.|  
|MouseOver|CommonStates|O ponteiro do mouse está sobre o <xref:System.Windows.Controls.ComboBoxItem> controle.|  
|Focalizado|FocusStates|O controle tem foco.|  
|Sem foco|FocusStates|O controle não tem foco.|  
|Selecionado|SelectionStates|O item está selecionado atualmente.|  
|Não selecionado|SelectionStates|O item não está selecionado.|  
|SelectedUnfocused|SelectionStates|O item está selecionado, mas não tem o foco.|  
|Válido|ValidationStates|O controle usa a <xref:System.Windows.Controls.Validation> classe e a <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada é `false` .|  
|InvalidFocused|ValidationStates|A <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada é `true` e o controle tem foco.|  
|InvalidUnfocused|ValidationStates|A <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada é `true` e o controle não tem foco.|  
  
## <a name="combobox-controltemplate-example"></a>Exemplo de ControlTemplate de ComboBox  
 O exemplo a seguir mostra como definir um <xref:System.Windows.Controls.ControlTemplate> para o <xref:System.Windows.Controls.ComboBox> controle e os tipos associados.  
  
 [!code-xaml[ControlTemplateExamples#ComboBox](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/combobox.xaml#combobox)]  
  
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
