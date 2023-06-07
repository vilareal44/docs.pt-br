---
title: Estilos e modelos de menu
description: Saiba mais sobre estilos e modelos para o Windows Presentation Foundation controle de menu. Modifique o ControlTemplate para dar ao controle uma aparência exclusiva.
ms.date: 03/30/2017
helpviewer_keywords:
- styles [WPF], Menu
- ControlTemplate [WPF], Menu
- Menu [WPF], styles and templates
- states [WPF], Menu
- templates [WPF], Menu
- parts [WPF], Menu
ms.assetid: b89da183-9b87-42c6-ac53-731a42c7b09e
ms.openlocfilehash: 5187e4a479609686e39e043808931141ca52078c
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164544"
---
# <a name="menu-styles-and-templates"></a>Estilos e modelos de menu
Este tópico descreve os estilos e modelos para o <xref:System.Windows.Controls.Menu> controle. Você pode modificar o padrão <xref:System.Windows.Controls.ControlTemplate> para dar ao controle uma aparência exclusiva. Para obter mais informações, consulte [criar um modelo para um controle](../../../desktop-wpf/themes/how-to-create-apply-template.md).  
  
## <a name="menu-parts"></a>Partes de menu  
 O <xref:System.Windows.Controls.Menu> controle não tem nenhuma parte nomeada.  
  
 Quando você cria um <xref:System.Windows.Controls.ControlTemplate> para o <xref:System.Windows.Controls.Menu> , seu modelo pode conter um <xref:System.Windows.Controls.ItemsPresenter> dentro de um <xref:System.Windows.Controls.ScrollViewer> . (O <xref:System.Windows.Controls.ItemsPresenter> exibe cada item no <xref:System.Windows.Controls.Menu> ; o <xref:System.Windows.Controls.ScrollViewer> habilita a rolagem dentro do controle).  Se o <xref:System.Windows.Controls.ItemsPresenter> não for o filho direto do <xref:System.Windows.Controls.ScrollViewer> , você deverá dar o <xref:System.Windows.Controls.ItemsPresenter> nome, `ItemsPresenter` .  
  
## <a name="menu-states"></a>Estados de menu  
 A tabela a seguir lista os Estados visuais para o <xref:System.Windows.Controls.Menu> controle.  
  
|Nome do VisualState|Nome do VisualStateGroup|Descrição|  
|-|-|-|  
|Válido|ValidationStates|O controle usa a <xref:System.Windows.Controls.Validation> classe e a <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada é `false` .|  
|InvalidFocused|ValidationStates|A <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada `true` tem o controle foco.|  
|InvalidUnfocused|ValidationStates|A <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada `true` tem o controle não tem foco.|  
  
## <a name="menuitem-parts"></a>Partes de MenuItem  
 A tabela a seguir lista as partes nomeadas para o <xref:System.Windows.Controls.Menu> controle.  
  
|Parte|Digite|Descrição|  
|-|-|-|  
|PART_Popup|<xref:System.Windows.Controls.Primitives.Popup>|A área para o submenu.|  
  
 Quando você cria um <xref:System.Windows.Controls.ControlTemplate> para o <xref:System.Windows.Controls.MenuItem> , seu modelo pode conter um <xref:System.Windows.Controls.ItemsPresenter> dentro de um <xref:System.Windows.Controls.ScrollViewer> . (O <xref:System.Windows.Controls.ItemsPresenter> exibe cada item no <xref:System.Windows.Controls.MenuItem> ; o <xref:System.Windows.Controls.ScrollViewer> habilita a rolagem dentro do controle).  Se o <xref:System.Windows.Controls.ItemsPresenter> não for o filho direto do <xref:System.Windows.Controls.ScrollViewer> , você deverá dar o <xref:System.Windows.Controls.ItemsPresenter> nome, `ItemsPresenter` .  
  
## <a name="menuitem-states"></a>Estados de MenuItem  
 A tabela a seguir lista os Estados visuais para o <xref:System.Windows.Controls.MenuItem> controle.  
  
|Nome do VisualState|Nome do VisualStateGroup|Descrição|  
|-|-|-|  
|Válido|ValidationStates|O controle usa a <xref:System.Windows.Controls.Validation> classe e a <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada é `false` .|  
|InvalidFocused|ValidationStates|A <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada `true` tem o controle foco.|  
|InvalidUnfocused|ValidationStates|A <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> Propriedade anexada `true` tem o controle não tem foco.|  
  
## <a name="menu-and-menuitem-controltemplate-example"></a>Menu e exemplo de ControlTemplate MenuItem  
 O exemplo a seguir mostra como definir um <xref:System.Windows.Controls.ControlTemplate> para o <xref:System.Windows.Controls.Menu> controle.  
  
 [!code-xaml[ControlTemplateExamples#Menu](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/menu.xaml#menu)]  
  
 O exemplo a seguir mostra como definir um <xref:System.Windows.Controls.ControlTemplate> para o <xref:System.Windows.Controls.MenuItem> controle.  
  
 [!code-xaml[ControlTemplateExamples#MenuItem](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/menu.xaml#menuitem)]  
  
 O exemplo a seguir define o `MenuScrollViewer`, que é usado no exemplo anterior.  
  
 [!code-xaml[ControlTemplateExamples#MenuScrollViewer](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/menu.xaml#menuscrollviewer)]  
  
 Os <xref:System.Windows.Controls.ControlTemplate> exemplos usam um ou mais dos recursos a seguir.  
  
 [!code-xaml[ControlTemplateExamples#Resources](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/shared.xaml#resources)]  
  
 Para ver o exemplo completo, consulte [Styling with ControlTemplates Sample (Estilos com a amostra ControlTemplates)](https://github.com/Microsoft/WPF-Samples/tree/master/Styles%20&%20Templates/IntroToStylingAndTemplating).  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.FrameworkElement.Style%2A>
- <xref:System.Windows.Controls.ControlTemplate>
- [Estilos e modelos de controle](control-styles-and-templates.md)
- [Personalização do controle](control-customization.md)
- [Estilo e modelagem](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
- [Criar um modelo para um controle](../../../desktop-wpf/themes/how-to-create-apply-template.md)
