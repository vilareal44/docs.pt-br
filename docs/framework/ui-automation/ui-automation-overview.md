---
title: Visão geral de automação da interface do usuário
description: Leia uma visão geral da automação da interface do usuário da Microsoft, a estrutura de acessibilidade para sistemas operacionais Windows que dão suporte a Windows Presentation Foundation (WPF).
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, overview
- user interface, see UI
- accessibility, UI automation
ms.assetid: 65847654-9994-4a9e-b36d-2dd5d998770b
ms.openlocfilehash: 84b176e53f16ba0676e933efe9ed679bf425abc0
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87163256"
---
# <a name="ui-automation-overview"></a>Visão geral de automação da interface do usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)]o é a nova estrutura de acessibilidade para o Microsoft Windows, disponível em todos os sistemas operacionais que dão suporte ao [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] .  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]fornece acesso programático à maioria dos [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] elementos na área de trabalho, permitindo que os produtos de tecnologia assistencial, como leitores de tela, forneçam informações sobre os [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] usuários finais e manipulem o [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] por meio de entrada padrão. [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]também permite que os scripts de teste automatizados interajam com o [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] .  
  
> [!NOTE]
> [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]o não permite a comunicação entre os processos iniciados por usuários diferentes por meio do comando **Executar como** .  
  
 Os aplicativos cliente de automação da interface do usuário podem ser escritos com a garantia de que funcionarão em várias estruturas. O [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] núcleo mascara quaisquer diferenças nas estruturas que se basear em várias partes do [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] . Por exemplo, a `Content` propriedade de um [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] botão, a `Caption` propriedade de um botão do Win32 e a `ALT` propriedade de uma imagem HTML são mapeadas para uma única propriedade, <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.Name%2A> , na [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] exibição.  
  
A automação da interface do usuário fornece funcionalidade completa em sistemas operacionais Windows com suporte que executam o .NET Framework (Confira [.NET Framework requisitos do sistema](../get-started/system-requirements.md) ou versões do .NET Core a partir do .net Core 3,0.  
  
 Os provedores de automação da interface do usuário oferecem algum suporte para aplicativos cliente do Microsoft Acessibilidade Ativa por meio de um serviço de ponte interno.  
  
<a name="Providers_and_Clients"></a>
## <a name="providers-and-clients"></a>Provedores e clientes  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]tem quatro componentes principais, conforme mostrado na tabela a seguir.  
  
|Componente|Descrição|  
|---------------|-----------------|  
|API do provedor (UIAutomationProvider.dll e UIAutomationTypes.dll)|Um conjunto de definições de interface que são implementadas por provedores de automação de interface do usuário, objetos que fornecem informações sobre [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] elementos e respondem a entradas programáticas.|  
|API do cliente (UIAutomationClient.dll e UIAutomationTypes.dll)|Um conjunto de tipos para código gerenciado que permite que aplicativos cliente de automação da interface do usuário obtenham informações sobre o [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] e enviem entrada para controles.|  
|UiAutomationCore.dll|O código subjacente (às vezes chamado de [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] núcleo) que lida com a comunicação entre os provedores e os clientes.|  
|UIAutomationClientsideProviders.dll|Um conjunto de provedores de automação de interface do usuário para controles herdados padrão. ( [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] controles têm suporte nativo para [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] .) Esse suporte está automaticamente disponível para aplicativos cliente.|  
  
 Da perspectiva do desenvolvedor do software, há duas maneiras de usar [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] : para criar suporte para controles personalizados (usando a API do provedor) e criar aplicativos que usam o [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] núcleo para se comunicar com [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] elementos (usando a API do cliente). Dependendo do seu foco, você deve consultar diferentes partes da documentação. Você pode saber mais sobre os conceitos e obter conhecimento prático nas seções a seguir.  
  
|Seção|Assunto|Público|  
|-------------|--------------------|--------------|  
|[Conceitos básicos de automação da interface do usuário](index.md) (esta seção)|Visões gerais amplas dos conceitos.|Todos.|  
|[Provedores de automação de interface de usuário para Código Gerenciado](ui-automation-providers-for-managed-code.md)|Visões gerais e tópicos de instruções para ajudá-lo a usar a API do provedor.|Desenvolvedores de controle.|  
|[Clientes de Automação de Interface de Usuário para Código Gerenciado](ui-automation-clients-for-managed-code.md)|Visões gerais e tópicos de instruções para ajudá-lo a usar a API do cliente.|Desenvolvedores de aplicativos cliente.|  
|[Padrões de controle de automação da interface do usuário](ui-automation-control-patterns.md)|Informações sobre como os padrões de controle devem ser implementados pelos provedores e qual funcionalidade está disponível para os clientes.|Todos.|  
|[Padrão de texto de automação da interface do usuário](ui-automation-text-pattern.md)|Informações sobre como o padrão de controle de texto deve ser implementado por provedores e qual funcionalidade está disponível para os clientes.|Todos.|  
|[Tipos de controle de automação de interface do usuário](ui-automation-control-types.md)|Informações sobre as propriedades e padrões de controle com suporte de diferentes tipos de controle.|Todos.|  
  
 A tabela a seguir lista os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] namespaces, as DLLs que os contêm e o público-alvo que os utiliza.  
  
|Namespace|DLLs referenciadas|Público|  
|---------------|---------------------|--------------|  
|<xref:System.Windows.Automation>|UIAutomationClientUIAutomationTypes|Desenvolvedores de cliente de automação da interface do usuário; usado para localizar <xref:System.Windows.Automation.AutomationElement> objetos, registrar para [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] eventos e trabalhar com [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] padrões de controle.|  
|<xref:System.Windows.Automation.Provider>|UIAutomationProviderUIAutomationTypes|Desenvolvedores de provedores de automação de interface do usuário para estruturas diferentes de [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] .|  
|<xref:System.Windows.Automation.Text>|UIAutomationClientUIAutomationTypes|Desenvolvedores de provedores de automação de interface do usuário para estruturas diferentes de [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] ; usados para implementar o padrão de controle TextPattern.|  
|<xref:System.Windows.Automation.Peers>|PresentationFramework|Desenvolvedores de provedores de automação de interface do usuário para o [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] .|  
  
<a name="UI_Automation_Model"></a>
## <a name="ui-automation-model"></a>Modelo de automação da interface do usuário  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]expõe cada parte do [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] para aplicativos cliente como um <xref:System.Windows.Automation.AutomationElement> . Os elementos estão contidos em uma estrutura de árvore, com a área de trabalho como o elemento raiz. Os clientes podem filtrar a exibição bruta da árvore como uma exibição de controle ou de conteúdo. Os aplicativos também podem criar exibições personalizadas.  
  
 <xref:System.Windows.Automation.AutomationElement>os objetos expõem as propriedades comuns dos [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] elementos que representam. Uma dessas propriedades é o tipo de controle, que define sua aparência e funcionalidade básicas como uma única entidade reconhecível: por exemplo, um botão ou caixa de seleção.  
  
 Além disso, os elementos expõem padrões de controle que fornecem propriedades específicas para seus tipos de controle. Os padrões de controle também expõem métodos que permitem que os clientes obtenham mais informações sobre o elemento e forneçam entrada.  
  
> [!NOTE]
> Não há uma correspondência um-para-um entre tipos de controle e padrões de controle. Um padrão de controle pode ser suportado por vários tipos de controle, e um controle pode dar suporte a vários padrões de controle, cada um dos quais expõe diferentes aspectos de seu comportamento. Por exemplo, uma caixa de combinação tem pelo menos dois padrões de controle: um que representa sua capacidade de expandir e recolher e outro que representa o mecanismo de seleção. Para obter informações específicas, consulte [tipos de controle de automação da interface do usuário](ui-automation-control-types.md).  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]também fornece informações para aplicativos cliente por meio de eventos. Ao contrário de WinEvents, os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] eventos não são baseados em um mecanismo de difusão. [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Os clientes se registram para notificações de eventos específicas e podem solicitar que [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades específicas e informações de padrão de controle sejam passadas para seus manipuladores de eventos. Além disso, um [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] evento contém uma referência ao elemento que o gerou. Os provedores podem melhorar o desempenho gerando eventos de forma seletiva, dependendo se algum cliente está ouvindo.  
  
## <a name="see-also"></a>Confira também

- [Visão geral da árvore de automação de interface do usuário](ui-automation-tree-overview.md)
- [Visão Geral de Padrões de Controle de Automação de Interface de Usuário](ui-automation-control-patterns-overview.md)
- [Visão geral das propriedades de automação da interface do usuário](ui-automation-properties-overview.md)
- [Visão geral sobre eventos de automação de interface do usuário](ui-automation-events-overview.md)
- [Visão geral de segurança da automação de interface do usuário](ui-automation-security-overview.md)
