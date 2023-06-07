---
title: Implementando o Padrão de Controle de Transformação de Automação de IU
description: Examine as diretrizes e convenções para implementar o padrão de controle transformar na automação da interface do usuário. Conheça os membros necessários para a interface ITransformProvider.
ms.date: 03/30/2017
helpviewer_keywords:
- control patterns, Transform
- Transform control pattern
- UI Automation, Transform control pattern
ms.assetid: 5f49d843-5845-4800-9d9c-56ce0d146844
ms.openlocfilehash: da11ce4cf9da10c0ebb990f9439b0bbe3621c561
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168208"
---
# <a name="implementing-the-ui-automation-transform-control-pattern"></a>Implementando o Padrão de Controle de Transformação de Automação de IU
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico apresenta as diretrizes e convenções para implementar o <xref:System.Windows.Automation.Provider.ITransformProvider> , incluindo informações sobre propriedades, métodos e eventos. Links para referências adicionais são listados no final do tópico.  
  
 O <xref:System.Windows.Automation.TransformPattern> padrão de controle é usado para dar suporte a controles que podem ser movidos, redimensionados ou girados dentro de um espaço bidimensional. Para obter exemplos de controles que implementam esse padrão de controle, consulte [mapeamento de padrão de controle para clientes de automação da interface do usuário](control-pattern-mapping-for-ui-automation-clients.md).  
  
<a name="Implementation_Guidelines_and_Conventions"></a>
## <a name="implementation-guidelines-and-conventions"></a>Diretrizes e convenções de implementação  
 Ao implementar o padrão de controle transformar, observe as seguintes diretrizes e convenções:  
  
- O suporte para esse padrão de controle não está limitado a objetos na área de trabalho. Esse padrão de controle também deve ser suportado pelos filhos de um objeto de contêiner se os filhos puderem ser movidos, redimensionados ou girados livremente dentro dos limites do contêiner.  
  
- Um objeto não pode ser movido, redimensionado ou girado de modo que seu local de tela resultante esteja completamente fora das coordenadas de seu contêiner e, portanto, inacessível para o teclado ou mouse (por exemplo, quando uma janela de nível superior é movida fora da tela ou um objeto filho é movido para fora dos limites do visor do contêiner). Nesses casos, o objeto é colocado o mais próximo possível das coordenadas da tela solicitada com as coordenadas superior ou esquerda substituídas para estar dentro dos limites do contêiner.  
  
- Para sistemas de vários monitores, se um objeto for movido, redimensionado ou girado completamente fora das coordenadas da tela da área de trabalho combinada, o objeto será colocado no monitor primário o mais próximo possível das coordenadas solicitadas.  
  
- Todos os parâmetros e valores de propriedade são absolutos e independentes da localidade.  
  
<a name="Required_Members_for_the_IValueProvider_Interface"></a>
## <a name="required-members-for-itransformprovider"></a>Membros necessários para ITransformProvider  
 As propriedades e os métodos a seguir são necessários para implementar o <xref:System.Windows.Automation.Provider.ITransformProvider> .  
  
|Membros necessários|Tipo de membro|Anotações|  
|----------------------|-----------------|-----------|  
|<xref:System.Windows.Automation.Provider.ITransformProvider.CanMove%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.ITransformProvider.CanResize%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.ITransformProvider.CanRotate%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.ITransformProvider.Move%2A>|Método|Nenhum|  
|<xref:System.Windows.Automation.Provider.ITransformProvider.Resize%2A>|Método|Nenhum|  
|<xref:System.Windows.Automation.Provider.ITransformProvider.Rotate%2A>|Método|Nenhum|  
  
 Este padrão de controle não tem eventos associados.  
  
<a name="Exceptions"></a>
## <a name="exceptions"></a>Exceções  
 Os provedores devem lançar as seguintes exceções.  
  
|Tipo de exceção|Condição|  
|--------------------|---------------|  
|<xref:System.InvalidOperationException>|<xref:System.Windows.Automation.Provider.ITransformProvider.Move%2A><br /><br /> -Se o <xref:System.Windows.Automation.TransformPatternIdentifiers.CanMoveProperty> for false.|  
|<xref:System.InvalidOperationException>|<xref:System.Windows.Automation.Provider.ITransformProvider.Resize%2A><br /><br /> -Se o <xref:System.Windows.Automation.TransformPatternIdentifiers.CanResizeProperty> for false.|  
|<xref:System.InvalidOperationException>|<xref:System.Windows.Automation.Provider.ITransformProvider.Rotate%2A><br /><br /> -Se o <xref:System.Windows.Automation.TransformPatternIdentifiers.CanRotateProperty> for false.|  
  
## <a name="see-also"></a>Confira também

- [Visão Geral de Padrões de Controle de Automação de Interface de Usuário](ui-automation-control-patterns-overview.md)
- [Padrões de controle de suporte em um provedor de automação da interface do usuário](support-control-patterns-in-a-ui-automation-provider.md)
- [Padrões de Controle para Clientes de Automação de IU](ui-automation-control-patterns-for-clients.md)
- [Visão geral da árvore de automação de interface do usuário](ui-automation-tree-overview.md)
- [Usar armazenamento em cache em automação de interface do usuário](use-caching-in-ui-automation.md)
