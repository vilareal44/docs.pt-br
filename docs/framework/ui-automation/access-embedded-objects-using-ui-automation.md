---
title: Acessar objetos inseridos usando automação de interface de usuário
description: Consulte como acessar objetos incorporados usando a automação da interface do usuário dentro do conteúdo de controle de texto. Os objetos inseridos são considerados filhos do provedor de texto de automação da interface do usuário.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- embedded objects, accessing
- accessing embedded objects
- UI Automation, accessing embedded objects
ms.assetid: a5b513ec-7fa6-4460-869f-c18ff04f7cf2
ms.openlocfilehash: 031d9c90318eec59ad2b77d611e0ed0d5a3ae719
ms.sourcegitcommit: b4f8849c47c1a7145eb26ce68bc9f9976e0dbec3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87516964"
---
# <a name="access-embedded-objects-using-ui-automation"></a>Acessar objetos inseridos usando automação de interface de usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico mostra como o [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] pode ser usado para expor objetos inseridos no conteúdo de um controle de texto.  
  
> [!NOTE]
> Os objetos inseridos podem incluir imagens, hiperlinks, botões, tabelas ou controles ActiveX.  
  
 Os objetos inseridos são considerados filhos do [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] provedor de texto. Isso permite que eles sejam expostos por meio da mesma estrutura de árvore de automação da interface do usuário que todos os outros [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] elementos. A funcionalidade, por sua vez, é exposta por meio dos padrões de controle normalmente exigidos pelo tipo de controle objetos inseridos (por exemplo, já que os hiperlinks são baseados em texto, eles terão suporte <xref:System.Windows.Automation.TextPattern> ).  
  
 ![Objetos inseridos em um contêiner de texto.](./media/uia-textpattern-embeddedobjects.PNG "UIA_TextPattern_EmbeddedObjects")  
Um documento de exemplo com conteúdo textual ("você sabia?" ...) e dois objetos incorporados (uma imagem de um baixo e um hiperlink de texto), usados como um destino para os exemplos de código.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir demonstra como recuperar uma coleção de objetos inseridos de dentro de um [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] provedor de texto. Para o documento de exemplo fornecido na introdução, dois objetos seriam retornados (um elemento Image e um elemento Text).  
  
> [!NOTE]
> O elemento Image deve ter algum texto intrínseco associado a ele, que descreve a imagem, normalmente em seu <xref:System.Windows.Automation.AutomationElement.NameProperty> (por exemplo, "um" texto azul ".). No entanto, quando um intervalo de texto que abrange o objeto de imagem é obtido, nem a imagem nem esse texto descritivo é retornado no fluxo de texto.  
  
[!code-csharp[FindText#StartApp](../../../samples/snippets/csharp/VS_Snippets_Wpf/FindText/CSharp/SearchWindow.cs#startapp)]
[!code-vb[FindText#StartApp](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/FindText/VisualBasic/SearchWindow.vb#startapp)]  
[!code-csharp[FindText#FindTextProvider](../../../samples/snippets/csharp/VS_Snippets_Wpf/FindText/CSharp/SearchWindow.cs#findtextprovider)]
[!code-vb[FindText#FindTextProvider](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/FindText/VisualBasic/SearchWindow.vb#findtextprovider)]  
[!code-csharp[FindText#GetChildren](../../../samples/snippets/csharp/VS_Snippets_Wpf/FindText/CSharp/SearchWindow.cs#getchildren)]
[!code-vb[FindText#GetChildren](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/FindText/VisualBasic/SearchWindow.vb#getchildren)]  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir demonstra como obter um intervalo de texto de um objeto inserido dentro de um [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] provedor de texto. O intervalo de texto recuperado é um intervalo vazio no qual o ponto de extremidade inicial segue "... oceano. (espaço) "e o ponto de extremidade final precede o". "de fechamento que representa o hiperlink inserido (conforme mostrado pela imagem fornecida na introdução). Embora este seja um intervalo vazio, ele não é considerado um intervalo de degeneração porque tem um Span diferente de zero.  
  
> [!NOTE]
> <xref:System.Windows.Automation.TextPattern>pode recuperar um objeto inserido baseado em texto, como um hiperlink; no entanto, um secundário precisará <xref:System.Windows.Automation.TextPattern> ser obtido do objeto incorporado para expor sua funcionalidade completa.  
  
 [!code-csharp[UIATextPattern_snip#GetRangeFromChild](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIATextPattern_snip/CSharp/SearchWindow.cs#getrangefromchild)]
 [!code-vb[UIATextPattern_snip#GetRangeFromChild](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIATextPattern_snip/VisualBasic/SearchWindow.vb#getrangefromchild)]  
  
## <a name="see-also"></a>Consulte também

- [Visão geral de TextPattern de automação da interface do usuário](ui-automation-textpattern-overview.md)
- [Visão Geral de Padrões de Controle de Automação de Interface de Usuário](ui-automation-control-patterns-overview.md)
- [Padrões de Controle para Clientes de Automação de IU](ui-automation-control-patterns-for-clients.md)
- [Acrescentar Conteúdo a um Text Box Utilizando Automação de IU](add-content-to-a-text-box-using-ui-automation.md)
- [Encontre e destaque texto usando automação de interface de usuário](find-and-highlight-text-using-ui-automation.md)
