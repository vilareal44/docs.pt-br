---
title: Implementando o padrão de controle de encaixe da automação de interface do usuário
description: Aprenda a implementar o padrão de controle de encaixe de automação da interface do usuário. Use o padrão de controle DockPattern para expor as propriedades Dock de um controle. Implemente IDockProvider.
ms.date: 03/30/2017
helpviewer_keywords:
- control patterns, dock
- dock control pattern
- UI Automation, dock control pattern
ms.assetid: ea3d2212-7c8e-4dd7-bf08-73141ca2d4fb
ms.openlocfilehash: 8080d78c7bded3cb884f92948eb1259cda5544dc
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165905"
---
# <a name="implementing-the-ui-automation-dock-control-pattern"></a>Implementando o padrão de controle de encaixe da automação de interface do usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico apresenta diretrizes e convenções para implementação <xref:System.Windows.Automation.Provider.IDockProvider> , incluindo informações sobre propriedades. Links para referências adicionais são listados no final do tópico.  
  
 O <xref:System.Windows.Automation.DockPattern> padrão de controle é usado para expor as propriedades de encaixe de um controle dentro de um contêiner de encaixe. Um contêiner de encaixe é um controle que permite que você organize elementos filho horizontal e verticalmente, em relação um ao outro. Para obter exemplos de controles que implementam esse padrão de controle, consulte [mapeamento de padrão de controle para clientes de automação da interface do usuário](control-pattern-mapping-for-ui-automation-clients.md).  
  
 ![Contêiner de encaixe com dois filhos encaixados.](./media/uia-dockpattern-dockingexample.PNG "UIA_DockPattern_DockingExample")  
Exemplo de encaixe do Visual Studio onde a janela "Modo de Exibição de Classe" é DockPosition. direita e a janela "Lista de Erros" é DockPosition. Bottom  
  
<a name="Implementation_Guidelines_and_Conventions"></a>
## <a name="implementation-guidelines-and-conventions"></a>Diretrizes e convenções de implementação  
 Ao implementar o padrão de controle Dock, observe as seguintes diretrizes e convenções:  
  
- <xref:System.Windows.Automation.Provider.IDockProvider>Não expõe nenhuma Propriedade do contêiner de encaixe ou quaisquer propriedades de controles encaixadas adjacentes ao controle atual dentro do contêiner de encaixe.  
  
- Os controles são encaixados em relação uns aos outros com base em sua ordem z atual; Quanto mais alto o posicionamento de ordem z, mais distante eles serão colocados da borda especificada do contêiner de encaixe.  
  
- Se o contêiner de encaixe for redimensionado, todos os controles encaixados dentro do contêiner serão liberados para a mesma borda para a qual eles foram originalmente encaixados. Os controles encaixados também serão redimensionados para preencher qualquer espaço dentro do contêiner de acordo com o comportamento de encaixe de seus <xref:System.Windows.Automation.DockPosition> . Por exemplo, se <xref:System.Windows.Automation.DockPosition.Top> for especificado, os lados esquerdo e direito do controle serão expandidos para preencher qualquer espaço disponível. Se <xref:System.Windows.Automation.DockPosition.Fill> for especificado, todos os quatro lados do controle serão expandidos para preencher qualquer espaço disponível.  
  
- Em um sistema de vários monitores, os controles devem ser encaixados no lado esquerdo ou direito do monitor atual. Se isso não for possível, eles deverão encaixar no lado esquerdo do monitor mais à esquerda ou no lado direito do monitor mais à direita.  
  
<a name="Required_Members_for_IDockProvider"></a>
## <a name="required-members-for-idockprovider"></a>Membros necessários para IDockProvider  
 As propriedades e os métodos a seguir são necessários para implementar a interface IDockProvider.  
  
|Membros necessários|Tipo de membro|Observações|  
|----------------------|-----------------|-----------|  
|<xref:System.Windows.Automation.Provider.IDockProvider.DockPosition%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.IDockProvider.SetDockPosition%2A>|Método|Nenhum|  
  
 Este padrão de controle não tem eventos associados.  
  
<a name="Exceptions"></a>
## <a name="exceptions"></a>Exceções  
 Os provedores devem lançar as seguintes exceções.  
  
|Tipo de exceção|Condição|  
|--------------------|---------------|  
|<xref:System.InvalidOperationException>|<xref:System.Windows.Automation.Provider.IDockProvider.SetDockPosition%2A><br /><br /> -Quando um controle não é capaz de executar o estilo de encaixe solicitado.|  
  
## <a name="see-also"></a>Confira também

- [Visão Geral de Padrões de Controle de Automação de Interface de Usuário](ui-automation-control-patterns-overview.md)
- [Padrões de controle de suporte em um provedor de automação da interface do usuário](support-control-patterns-in-a-ui-automation-provider.md)
- [Padrões de Controle para Clientes de Automação de IU](ui-automation-control-patterns-for-clients.md)
- [Visão geral da árvore de automação de interface do usuário](ui-automation-tree-overview.md)
- [Usar armazenamento em cache em automação de interface do usuário](use-caching-in-ui-automation.md)
