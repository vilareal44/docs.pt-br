---
title: Automação da Interface do Usuário e Escala da Tela
description: Leia como a automação do Windows e da interface do usuário lida com o dimensionamento de tela O DWM faz o dimensionamento padrão para todos os aplicativos, que os aplicativos cliente de automação da interface do usuário devem levar em conta.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- scaling, screens
- screens, scaling
- UI (user interface), automation
- UI Automation
ms.assetid: 4380cad7-e509-448f-b9a5-6de042605fd4
ms.openlocfilehash: 99239d7bac2e556d4da0d74f36c68916da7c688a
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164009"
---
# <a name="ui-automation-and-screen-scaling"></a>Automação da Interface do Usuário e Escala da Tela
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
A partir do Windows Vista, o Windows permite que os usuários alterem a configuração de pontos por polegada (DPI) para que a maioria dos elementos da interface do usuário na tela pareçam maiores. Embora esse recurso tenha sido muito disponível no Windows, em versões anteriores, o dimensionamento precisava ser implementado por aplicativos. A partir do Windows Vista, o Gerenciador de Janelas da Área de Trabalho executa o dimensionamento padrão para todos os aplicativos que não manipulam seu próprio dimensionamento. Os aplicativos cliente de automação da interface do usuário devem levar esse recurso em conta.  
  
<a name="Scaling_in_Windows_Vista"></a>
## <a name="scaling-in-windows-vista"></a>Dimensionamento no Windows Vista  
 A configuração padrão de DPI é 96, o que significa que 96 pixels ocupam uma largura ou altura de uma polegada de conceito. A medida exata de uma "polegada" depende do tamanho e da resolução física do monitor. Por exemplo, em um monitor de 12 polegadas de largura, com uma resolução horizontal de 1280 pixels, uma linha horizontal de 96 pixels se estende cerca de 9/10 de uma polegada.  
  
 Alterar a configuração de DPI não é o mesmo que alterar a resolução da tela. Com o ajuste de DPI, o número de pixels físicos na tela permanece o mesmo. No entanto, o dimensionamento é aplicado ao tamanho e ao local dos [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] elementos. Esse dimensionamento pode ser executado automaticamente pelo Gerenciador de Janelas da Área de Trabalho (DWM) para a área de trabalho e para aplicativos que não pedem explicitamente que não sejam dimensionados.  
  
 Em vigor, quando o usuário define o fator de escala como 120 dpi, uma polegada vertical ou horizontal na tela se torna maior em 25%. Todas as dimensões são dimensionadas de acordo. O deslocamento de uma janela de aplicativo a partir das bordas superior e esquerda da tela aumenta em 25%. Se o dimensionamento de aplicativos estiver habilitado e o aplicativo não reconhecer dpi, o tamanho da janela aumentará na mesma proporção, juntamente com os deslocamentos e tamanhos de todos os [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] elementos que ele contém.  
  
> [!NOTE]
> Por padrão, o DWM não executa o dimensionamento para aplicativos que não reconhecem dpi quando o usuário define o DPI como 120, mas o executa quando o DPI é definido como um valor personalizado de 144 ou superior. No entanto, o usuário pode substituir o comportamento padrão.  
  
 O dimensionamento de tela cria novos desafios para aplicativos que se preocupam de qualquer forma com coordenadas de tela. A tela agora contém dois sistemas de coordenadas: físico e lógico. As coordenadas físicas de um ponto são o deslocamento real em pixels da parte superior esquerda da origem. As coordenadas lógicas são os deslocamentos como seriam se os próprios pixels fossem dimensionados.  
  
 Suponha que você projete uma caixa de diálogo com um botão nas coordenadas (100, 48). Quando essa caixa de diálogo é exibida no padrão de 96 dpi, o botão está localizado em coordenadas físicas de (100, 48). Em 120 dpi, ele está localizado em coordenadas físicas de (125, 60). Mas as coordenadas lógicas são as mesmas em qualquer configuração de DPI: (100, 48).  
  
 As coordenadas lógicas são importantes, pois tornam o comportamento do sistema operacional e dos aplicativos consistentes, independentemente da configuração de DPI. Por exemplo, <xref:System.Windows.Forms.Cursor.Position%2A?displayProperty=nameWithType> normalmente retorna as coordenadas lógicas. Se você mover o cursor sobre um elemento em uma caixa de diálogo, as mesmas coordenadas serão retornadas independentemente da configuração de DPI. Se você desenhar um controle em (100, 100), ele será desenhado para essas coordenadas lógicas e ocupará a mesma posição relativa em qualquer configuração de DPI.  
  
<a name="Scaling_in_UI_Automation_Clients"></a>
## <a name="scaling-in-ui-automation-clients"></a>Dimensionamento em clientes de automação da interface do usuário  
 A [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] API não usa coordenadas lógicas. Os métodos e as propriedades a seguir retornam coordenadas físicas ou as levam como parâmetros.  
  
- <xref:System.Windows.Automation.AutomationElement.GetClickablePoint%2A>  
  
- <xref:System.Windows.Automation.AutomationElement.TryGetClickablePoint%2A>  
  
- <xref:System.Windows.Automation.AutomationElement.ClickablePointProperty>  
  
- <xref:System.Windows.Automation.AutomationElement.FromPoint%2A>  
  
- <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.BoundingRectangle%2A>  
  
 Por padrão, um aplicativo cliente de automação de IU em execução em um ambiente diferente de 96 DPI não será capaz de obter os resultados corretos desses métodos e propriedades. Por exemplo, como a posição do cursor está em coordenadas lógicas, o cliente não pode simplesmente passar essas coordenadas para <xref:System.Windows.Automation.AutomationElement.FromPoint%2A> para obter o elemento que está sob o cursor. Além disso, o aplicativo não será capaz de posicionar corretamente o Windows fora de sua área de cliente.  
  
 A solução está em duas partes.  
  
1. Primeiro, torne o reconhecimento de DPI do aplicativo cliente. Para fazer isso, chame a função do Win32 `SetProcessDPIAware` na inicialização. No código gerenciado, a declaração a seguir torna essa função disponível.  
  
     [!code-csharp[Highlighter#101](../../../samples/snippets/csharp/VS_Snippets_Wpf/Highlighter/CSharp/NativeMethods.cs#101)]
     [!code-vb[Highlighter#101](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/Highlighter/VisualBasic/NativeMethods.vb#101)]  
  
     Essa função faz com que todo o processo reconheça o DPI, o que significa que todas as janelas que pertencem ao processo não são dimensionadas. No [exemplo de realce](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/Highlighter), por exemplo, as quatro janelas que compõem o retângulo de realce estão localizadas nas coordenadas físicas obtidas da automação da interface do usuário, não nas coordenadas lógicas. Se o exemplo não tiver reconhecimento de DPI, o realce seria desenhado nas coordenadas lógicas na área de trabalho, o que resultaria em um posicionamento incorreto em um ambiente diferente de 96 dpi.  
  
2. Para obter as coordenadas do cursor, chame a função do Win32 `GetPhysicalCursorPos` . O exemplo a seguir mostra como declarar e usar essa função.  
  
     [!code-csharp[UIAClient_snip#185](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#185)]
     [!code-vb[UIAClient_snip#185](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#185)]  
  
> [!CAUTION]
> Não use <xref:System.Windows.Forms.Cursor.Position%2A?displayProperty=nameWithType>. O comportamento dessa propriedade fora das janelas cliente em um ambiente dimensionado é indefinido.  
  
 Se seu aplicativo executa comunicação direta entre processos com aplicativos que não reconhecem dpi, você pode ter convertido entre coordenadas lógicas e físicas usando as funções do Win32 `PhysicalToLogicalPoint` e o `LogicalToPhysicalPoint` .  
  
## <a name="see-also"></a>Confira também

- [Exemplo de realçador](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/Highlighter)
