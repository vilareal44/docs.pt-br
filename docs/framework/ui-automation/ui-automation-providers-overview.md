---
title: Visão Geral dos Provedores de Automação de Interface do Usuário
description: Veja uma visão geral dos provedores de automação da interface do usuário, que permitem que os controles se comuniquem com os aplicativos cliente de automação Revise os tipos de provedor e conceitos de provedor.
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, providers
- providers, UI Automation
ms.assetid: 859557b8-51e1-4d15-92e8-318d2dcdb2f7
ms.openlocfilehash: 08b6a199a98fb22f197ca0cf8a0268e5439aaf41
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87163246"
---
# <a name="ui-automation-providers-overview"></a>Visão Geral dos Provedores de Automação de Interface do Usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Provedores de automação de interface do usuário permitem que controles se comuniquem com aplicativos cliente de automação de interface Em geral, cada controle ou outro elemento distinto em um [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] é representado por um provedor. O provedor expõe informações sobre o elemento e, opcionalmente, implementa padrões de controle que permitem que o aplicativo cliente interaja com o controle.  
  
 Normalmente, os aplicativos cliente não precisam trabalhar diretamente com os provedores. A maioria dos controles padrão em aplicativos que usam o Win32, Windows Forms ou [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] frameworks é exposta automaticamente ao [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] sistema. Os aplicativos que implementam controles personalizados também podem implementar [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] provedores para esses controles, e os aplicativos cliente não precisam realizar etapas especiais para obter acesso a eles.  
  
 Este tópico fornece uma visão geral de como os desenvolvedores de controle implementam [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] provedores, especialmente para controles nas janelas Windows Forms e Win32.  
  
<a name="Types_of_Providers"></a>
## <a name="types-of-providers"></a>Tipos de provedores  
 Os provedores de automação da interface do usuário se enquadram em duas categorias: provedores do lado do cliente e provedores do lado do servidor.  
  
### <a name="client-side-providers"></a>Provedores do lado do cliente  
 Os provedores do lado do cliente são implementados por clientes de automação da interface do usuário para se comunicar com um aplicativo que não dá suporte a, ou que não oferece suporte completo ao [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] . Provedores do lado do cliente geralmente se comunicam com o servidor pelo limite do processo enviando e recebendo mensagens do Windows.  
  
 Como os provedores de automação da interface do usuário para controles no Win32, Windows Forms ou [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] aplicativos são fornecidos como parte do sistema operacional, os aplicativos cliente raramente precisam implementar seus próprios provedores, e essa visão geral não os cobre.  
  
### <a name="server-side-providers"></a>Provedores do lado do servidor  
 Os provedores do lado do servidor são implementados por controles personalizados ou por aplicativos baseados em uma estrutura de interface do usuário diferente do Win32, Windows Forms ou [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] .  
  
 Os provedores do lado do servidor se comunicam com aplicativos cliente durante o limite do processo expondo interfaces ao [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] sistema principal, que, por sua vez, atende a solicitações de clientes.  
  
<a name="AutomationProviderConcepts"></a>
## <a name="ui-automation-provider-concepts"></a>Conceitos do provedor de automação de interface do usuário  
 Esta seção fornece breves explicações de alguns dos principais conceitos que você precisa entender para implementar provedores de automação de interface do usuário.  
  
### <a name="elements"></a>Elementos  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]elementos são partes do [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] que são visíveis para clientes de automação da interface do usuário. Os exemplos incluem janelas de aplicativo, painéis, botões, dicas de ferramenta, caixas de listagem e itens de lista.  
  
### <a name="navigation"></a>Navegação  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]os elementos são expostos aos clientes como uma [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore. [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]constrói a árvore navegando de um elemento para outro. A navegação é habilitada pelos provedores para cada elemento, cada um deles pode apontar para um pai, irmãos e filhos.  
  
 Para obter mais informações sobre a exibição de cliente da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore, consulte [visão geral da árvore de automação da interface do usuário](ui-automation-tree-overview.md).  
  
### <a name="views"></a>Exibições  
 Um cliente pode ver a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore em três exibições principais, conforme mostrado na tabela a seguir.  
  
|||  
|-|-|  
|Exibição bruta|Contém todos os elementos.|  
|Exibição de controle|Contém elementos que são controles.|  
|Exibição de conteúdo|Contém elementos que têm conteúdo.|  
  
 Para obter mais informações sobre as exibições de cliente da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore, consulte [visão geral da árvore de automação da interface do usuário](ui-automation-tree-overview.md).  
  
 É responsabilidade da implementação do provedor definir um elemento como um elemento de conteúdo ou um elemento de controle. Elementos de controle podem ou não ser elementos de conteúdo, mas todos os elementos de conteúdo são elementos de controle.  
  
### <a name="frameworks"></a>Estruturas  
 Uma estrutura é um componente que gerencia controles filho, testes de clique e renderização em uma área da tela. Por exemplo, uma janela do Win32, geralmente conhecida como HWND, pode servir como uma estrutura que contém vários [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] elementos, como uma barra de menus, uma barra de status e botões.  
  
 Os controles de contêiner do Win32, como caixas de listagem e exibições de árvore, são considerados estruturas, pois contêm seu próprio código para renderizar itens filho e executar testes de impacto neles. Por outro lado, uma [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] caixa de listagem não é uma estrutura, porque a renderização e o teste de impacto estão sendo tratados pela janela que a contém [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] .  
  
 O [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] em um aplicativo pode ser composto de estruturas diferentes. Por exemplo, uma janela de aplicativo HWND pode conter HTML dinâmico (DHTML) que, por sua vez, contém um componente, como uma caixa de combinação em um HWND.  
  
### <a name="fragments"></a>Fragmentos  
 Um fragmento é uma subárvore completa de elementos de uma estrutura específica. O elemento no nó raiz da subárvore é chamado de raiz do fragmento. Uma raiz de fragmento não tem um pai, mas está hospedada em alguma outra estrutura, geralmente uma janela Win32 (HWND).  
  
### <a name="hosts"></a>Hosts  
 O nó raiz de cada fragmento deve ser hospedado em um elemento, geralmente uma janela Win32 (HWND). A exceção é a área de trabalho, que não está hospedada em nenhum outro elemento. O host de um controle personalizado é o HWND do próprio controle, não a janela do aplicativo ou qualquer outra janela que possa conter grupos de controles de nível superior.  
  
 O host de um fragmento desempenha um papel importante no fornecimento de [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] serviços. Ele permite a navegação para a raiz do fragmento e fornece algumas propriedades padrão para que o provedor personalizado não precise implementá-las.  
  
## <a name="see-also"></a>Confira também

- [Implementação do provedor de automação de interface do usuário no lado do servidor](server-side-ui-automation-provider-implementation.md)
