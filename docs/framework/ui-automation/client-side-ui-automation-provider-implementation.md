---
title: Implementação de Provedor de Automação de Interface de Usuário do Lado do Cliente
description: Entenda a implementação do provedor de automação da interface do usuário do lado do cliente. Saiba como distribuir, registrar e configurar provedores do lado do cliente.
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, client-side provider implementation
- client-side UI Automation provider, implementation
- provider implementation, UI Automation
ms.assetid: 3584c0a1-9cd0-4968-8b63-b06390890ef6
ms.openlocfilehash: 867293c00d0724e27f5163f3ae8be43aca30cfe8
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164396"
---
# <a name="client-side-ui-automation-provider-implementation"></a>Implementação de Provedor de Automação de Interface de Usuário do Lado do Cliente
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Várias [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] estruturas diferentes estão em uso em sistemas operacionais da Microsoft, incluindo Win32, Windows Forms e [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] . [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)]expõe informações sobre elementos de interface do usuário para clientes. No entanto, [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ele não tem conhecimento dos diferentes tipos de controles existentes nessas estruturas e nas técnicas necessárias para extrair informações delas. Em vez disso, ele deixa essa tarefa para objetos chamados provedores. Um provedor extrai informações de um controle específico e passa essas informações para [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] o, que os apresenta ao cliente de maneira consistente.  
  
 Os provedores podem existir no lado do servidor ou no lado do cliente. Um provedor do lado do servidor é implementado pelo próprio controle. [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)]os elementos implementam provedores, assim como qualquer outro controle de terceiros escrito com [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] em mente.  
  
 No entanto, controles mais antigos, como aqueles no Win32 e Windows Forms, não dão suporte diretamente a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] . Esses controles são servidos em vez de provedores que existem no processo do cliente e obtêm informações sobre controles usando a comunicação entre processos; por exemplo, monitorando mensagens do Windows de e para os controles. Esses provedores do lado do cliente são chamados também de proxies.  
  
 O Windows Vista fornece provedores para controles padrão do Win32 e do Windows Forms. Além disso, um provedor de fallback fornece [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] suporte parcial a qualquer controle que não seja atendido por outro provedor ou proxy do lado do servidor, mas tenha uma implementação do Microsoft acessibilidade ativa. Todos esses provedores são carregados e disponibilizados automaticamente para aplicativos cliente.  
  
 Para obter mais informações sobre o suporte para controles Win32 e Windows Forms, consulte [suporte de automação da interface do usuário para controles padrão](ui-automation-support-for-standard-controls.md).  
  
 Os aplicativos também podem registrar outros provedores do lado do cliente.  
  
<a name="Distributing_Client-Side_Providers"></a>
## <a name="distributing-client-side-providers"></a>Distribuindo provedores do lado do cliente  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]espera encontrar provedores do lado do cliente em um assembly de código gerenciado. O namespace neste assembly deve ter o mesmo nome que o assembly. Por exemplo, um assembly chamado ContosoProxies.dll conteria o namespace ContosoProxies. No namespace, crie uma <xref:UIAutomationClientsideProviders.UIAutomationClientSideProviders> classe. Na implementação do <xref:UIAutomationClientsideProviders.UIAutomationClientSideProviders.ClientSideProviderDescriptionTable> campo estático, crie uma matriz de <xref:System.Windows.Automation.ClientSideProviderDescription> estruturas que descreve os provedores.  
  
<a name="Registering_and_Configuring_Client-Side_Providers"></a>
## <a name="registering-and-configuring-client-side-providers"></a>Registrando e configurando provedores do lado do cliente  
 Os provedores do lado do cliente em uma DLL (biblioteca de vínculo dinâmico) são carregados chamando <xref:System.Windows.Automation.ClientSettings.RegisterClientSideProviderAssembly%2A> . Nenhuma ação adicional é exigida por um aplicativo cliente para fazer uso dos provedores.  
  
 Os provedores implementados no código próprio do cliente são registrados usando o <xref:System.Windows.Automation.ClientSettings.RegisterClientSideProviders%2A> . Esse método usa como um argumento uma matriz de <xref:System.Windows.Automation.ClientSideProviderDescription> estruturas, cada uma delas especifica as seguintes propriedades:  
  
- Uma função de retorno de chamada que cria o objeto do provedor.  
  
- O nome da classe dos controles que o provedor vai fornecer.  
  
- O nome da imagem do aplicativo (geralmente o nome completo do arquivo executável) que o provedor irá fornecer.  
  
- Sinalizadores que regem como o nome da classe é correspondido em relação às classes de janela encontradas no aplicativo de destino.  
  
 Os dois últimos parâmetros são opcionais. O cliente pode especificar o nome da imagem do aplicativo de destino quando quiser usar provedores diferentes para aplicativos diferentes. Por exemplo, o cliente pode usar um provedor para um controle de exibição de lista do Win32 em um aplicativo conhecido que dá suporte ao padrão de várias exibições e outro para um controle semelhante em outro aplicativo conhecido que não tem.  
  
## <a name="see-also"></a>Confira também

- [Criar um Provedor de Automação de Interface de Usuário do Lado do Cliente](create-a-client-side-ui-automation-provider.md)
- [Implementar provedores de automação de interface do usuário em um aplicativo cliente](implement-ui-automation-providers-in-a-client-application.md)
