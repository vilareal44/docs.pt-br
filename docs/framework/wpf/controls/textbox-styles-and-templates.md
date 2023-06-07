---
title: Estilos e modelos TextBox
description: Saiba mais sobre estilos e modelos para o controle caixa de texto Windows Presentation Foundation. Modifique o ControlTemplate para dar ao controle uma aparência exclusiva.
ms.date: 03/30/2017
helpviewer_keywords:
- ControlTemplate [WPF], TextBox
- parts [WPF], TextBox
- states [WPF], TextBox
- styles [WPF], TextBox
- templates [WPF], TextBox
- TextBox [WPF], styles and templates
ms.assetid: aa99130c-43a1-450f-9b46-c40ae0db0cca
ms.openlocfilehash: 0e15fd40f5590ee46da49cc6c0d5fb30e051f7e4
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164732"
---
# <a name="textbox-styles-and-templates"></a>Estilos e modelos TextBox
Este tópico descreve os estilos e modelos para o <xref:System.Windows.Controls.TextBox> controle. Você pode modificar o padrão <xref:System.Windows.Controls.ControlTemplate> para dar ao controle uma aparência exclusiva. Para obter mais informações, consulte [criar um modelo para um controle](../../../desktop-wpf/themes/how-to-create-apply-template.md).  
  
## <a name="textbox-parts"></a>Partes da caixa de texto  
 A tabela a seguir lista as partes nomeadas para o <xref:System.Windows.Controls.TextBox> controle.  
  
|Parte|Type|Descrição|  
|-|-|-|  
|PART_ContentHost|<xref:System.Windows.FrameworkElement>|Um elemento visual que pode conter um <xref:System.Windows.FrameworkElement> . O texto do <xref:System.Windows.Controls.TextBox> é exibido neste elemento.|  
  
## <a name="textbox-states"></a>Estados da caixa de texto  
 A tabela a seguir lista os Estados visuais para o <xref:System.Windows.Controls.TextBox> controle.  
  
|Nome do VisualState|Nome do VisualStateGroup|Descrição|  
|----------------------|---------------------------|-----------------|  
|Normal|CommonStates|O estado padrão.|  
|MouseOver|CommonStates|O ponteiro do mouse é posicionado sobre o controle.|  
|Desabilitado|CommonStates|O controle está desabilitado.|  
|ReadOnly|CommonStates|O usuário não pode alterar o texto no <xref:System.Windows.Controls.TextBox> .|  
|Focalizado|FocusStates|O controle tem foco.|  
|Sem foco|FocusStates|O controle não tem foco.|  
|Válido|ValidationStates|O controle usa a <xref:System.Windows.Controls.Validation> classe e a <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada é `false` .|  
|InvalidFocused|ValidationStates|A <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada `true` tem o controle foco.|  
|InvalidUnfocused|ValidationStates|A <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada `true` tem o controle não tem foco.|  
  
## <a name="textbox-controltemplate-example"></a>Exemplo de ControlTemplate de TextBox  
 O exemplo a seguir mostra como definir um <xref:System.Windows.Controls.ControlTemplate> para o <xref:System.Windows.Controls.TextBox> controle.  
  
 [!code-xaml[ControlTemplateExamples#TextBox](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/textbox.xaml#textbox)]  
  
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
