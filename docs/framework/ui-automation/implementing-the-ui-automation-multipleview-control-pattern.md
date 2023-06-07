---
title: Implementando o padrão de controle MultipleView de interface de usuário
description: Examine as diretrizes e convenções para implementar o padrão de controle MultipleView na automação da interface do usuário. Consulte membros necessários para a interface IMultipleViewProvider.
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, MultipleView control pattern
- MultipleView control pattern
- control patterns, MultipleView
ms.assetid: 5bf1b248-ffee-48c8-9613-0b134bbe9f6a
ms.openlocfilehash: 0d65d57637891fcb1307f5ee83a417941ff323fb
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168232"
---
# <a name="implementing-the-ui-automation-multipleview-control-pattern"></a>Implementando o padrão de controle MultipleView de interface de usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico apresenta diretrizes e convenções para implementação <xref:System.Windows.Automation.Provider.IMultipleViewProvider> , incluindo informações sobre eventos e propriedades. Links para referências adicionais são listados no final do tópico.  
  
 O <xref:System.Windows.Automation.MultipleViewPattern> padrão de controle é usado para dar suporte a controles que fornecem e são capazes de alternar entre, várias representações do mesmo conjunto de informações ou controles filho.  
  
 Exemplos de controles que podem apresentar várias exibições incluem o modo de exibição de lista (que pode mostrar seu conteúdo como miniaturas, blocos, ícones ou detalhes), gráficos do Microsoft Excel (pizza, linha, barra, valor de célula com uma fórmula), documentos do Microsoft Word (normal, de layout da Web, layout de impressão, layout de leitura, estrutura de tópicos), calendário do Microsoft Outlook (ano, mês, semana, dia) e capas do Microsoft Windows Media Player. As exibições com suporte são determinadas pelo desenvolvedor de controle e são específicas para cada controle.  
  
<a name="Implementation_Guidelines_and_Conventions"></a>
## <a name="implementation-guidelines-and-conventions"></a>Diretrizes e convenções de implementação  
 Ao implementar o padrão de controle Multiple View, observe as seguintes diretrizes e convenções:  
  
- <xref:System.Windows.Automation.Provider.IMultipleViewProvider>também deve ser implementado em um contêiner que gerencia a exibição atual se ele for diferente de um controle que fornece a exibição atual. Por exemplo, o Windows Explorer contém um controle de lista para o conteúdo da pasta atual, enquanto a exibição do controle é gerenciada por meio do aplicativo do Windows Explorer.  
  
- Um controle que é capaz de classificar seu conteúdo não é considerado para dar suporte a várias exibições.  
  
- A coleção de exibições deve ser idêntica entre instâncias.  
  
- Os nomes de exibição devem ser adequados para uso em Conversão de Texto em Fala, Braille e outros aplicativos legíveis.  
  
<a name="Required_Members_for_IMultipleViewProvider"></a>
## <a name="required-members-for-imultipleviewprovider"></a>Membros necessários para IMultipleViewProvider  
 As propriedades e os métodos a seguir são necessários para implementar IMultipleViewProvider.  
  
|Membros necessários|Tipo de membro|Anotações|  
|----------------------|-----------------|-----------|  
|<xref:System.Windows.Automation.Provider.IMultipleViewProvider.CurrentView%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.IMultipleViewProvider.GetSupportedViews%2A>|Método|Nenhum|  
|<xref:System.Windows.Automation.Provider.IMultipleViewProvider.GetViewName%2A>|Método|Nenhum|  
|<xref:System.Windows.Automation.Provider.IMultipleViewProvider.SetCurrentView%2A>|Método|Nenhum|  
  
 Não há eventos associados a este padrão de controle.  
  
<a name="Exceptions"></a>
## <a name="exceptions"></a>Exceções  
 O provedor deve lançar as seguintes exceções.  
  
|Tipo de exceção|Condição|  
|--------------------|---------------|  
|<xref:System.ArgumentException>|Quando <xref:System.Windows.Automation.Provider.IMultipleViewProvider.SetCurrentView%2A> ou <xref:System.Windows.Automation.Provider.IMultipleViewProvider.GetViewName%2A> é chamado com um parâmetro que não é membro da coleção views com suporte.|  
  
## <a name="see-also"></a>Confira também

- [Visão Geral de Padrões de Controle de Automação de Interface de Usuário](ui-automation-control-patterns-overview.md)
- [Padrões de controle de suporte em um provedor de automação da interface do usuário](support-control-patterns-in-a-ui-automation-provider.md)
- [Padrões de Controle para Clientes de Automação de IU](ui-automation-control-patterns-for-clients.md)
- [Visão geral da árvore de automação de interface do usuário](ui-automation-tree-overview.md)
- [Usar armazenamento em cache em automação de interface do usuário](use-caching-in-ui-automation.md)
