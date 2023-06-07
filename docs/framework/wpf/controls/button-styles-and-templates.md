---
title: Estilos e modelos de botão
description: Saiba mais sobre estilos e modelos para o controle botão de Windows Presentation Foundation. Modifique o ControlTemplate para dar ao controle uma aparência exclusiva.
ms.date: 03/30/2017
helpviewer_keywords:
- states [WPF], Button
- parts [WPF], Button
- styles [WPF], Button
- Button [WPF], styles and templates
- templates [WPF], Button
- ControlTemplate [WPF], Button
ms.assetid: e223c759-f8c4-4717-acfb-b1e40bdf5f3b
ms.openlocfilehash: 11509adeef397f26eb040e6e98d0edb333b2515f
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166268"
---
# <a name="button-styles-and-templates"></a>Estilos e modelos de botão
Este tópico descreve os estilos e modelos para o <xref:System.Windows.Controls.Button> controle. Você pode modificar o padrão <xref:System.Windows.Controls.ControlTemplate> para dar ao controle uma aparência exclusiva. Para obter mais informações, consulte [criar um modelo para um controle](../../../desktop-wpf/themes/how-to-create-apply-template.md).  
  
## <a name="button-parts"></a>Partes do botão  
 O <xref:System.Windows.Controls.Button> controle não tem nenhuma parte nomeada.  
  
## <a name="button-states"></a>Estados do botão  
 A tabela a seguir lista os Estados visuais para o <xref:System.Windows.Controls.Button> controle.  
  
|Nome do VisualState|Nome do VisualStateGroup|Descrição|  
|-|-|-|  
|Normal|CommonStates|O estado padrão.|  
|MouseOver|CommonStates|O ponteiro do mouse é posicionado sobre o controle.|  
|Pressionado|CommonStates|O controle é pressionado.|  
|Desabilitado|CommonStates|O controle está desabilitado.|  
|Focalizado|FocusStates|O controle tem foco.|  
|Sem foco|FocusStates|O controle não tem foco.|  
|Válido|ValidationStates|O controle usa a <xref:System.Windows.Controls.Validation> classe e a <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada é `false` .|  
|InvalidFocused|ValidationStates|A <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada é `true` e o controle tem foco.|  
|InvalidUnfocused|ValidationStates|A <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada é `true` e o controle não tem foco.|  
  
## <a name="button-controltemplate-example"></a>Exemplo de ControlTemplate de botão  
 O exemplo a seguir mostra como definir um <xref:System.Windows.Controls.ControlTemplate> para o <xref:System.Windows.Controls.Button> controle.  
  
 [!code-xaml[ControlTemplateExamples#Button](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/button.xaml#button)]  
  
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
