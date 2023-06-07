---
title: Implementação do provedor de automação de interface do usuário no lado do servidor
description: Entenda como implementar um provedor de automação da interface do usuário do lado do servidor para um controle personalizado no .NET. A implementação de elementos WPF e não WPF é diferente.
ms.date: 03/30/2017
helpviewer_keywords:
- server-side UI Automation provider implementation
- UI Automation, server-side provider implementation
- provider implementation, UI Automation
ms.assetid: 6acc6d08-bd67-4e2e-915c-9c1d34eb86fe
ms.openlocfilehash: ea1b5e668e29d854233d4dde4c0e6152d591da97
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84903890"
---
# <a name="server-side-ui-automation-provider-implementation"></a>Implementação do provedor de automação de interface do usuário no lado do servidor

> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).

Esta seção descreve como implementar um provedor de automação da interface do usuário do lado do servidor para um controle personalizado.

A implementação para elementos de Windows Presentation Foundation (WPF) e elementos não-WPF (como os projetados para Windows Forms) é fundamentalmente diferente. Os elementos do WPF oferecem suporte ao [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] por meio de uma classe derivada de <xref:System.Windows.Automation.Peers.AutomationPeer> . Elementos não-WPF oferecem suporte por meio de implementações de interfaces de provedor.

<a name="Security_Considerations"></a>

## <a name="security-considerations"></a>Considerações de segurança

Os provedores devem ser escritos para que possam trabalhar em um ambiente de confiança parcial. Como UIAutomationClient.dll não está configurado para ser executado sob confiança parcial, o código do provedor não deve fazer referência a esse assembly. Se isso ocorrer, o código poderá ser executado em um ambiente de confiança total, mas, em seguida, falhar em um ambiente parcialmente confiável.

Em particular, não use campos de classes em UIAutomationClient.dll como as do <xref:System.Windows.Automation.AutomationElement> . Em vez disso, use os campos equivalentes de classes no UIAutomationTypes.dll, como <xref:System.Windows.Automation.AutomationElementIdentifiers> .

<a name="Provider_Implementation_by_WPF_Elements"></a>

## <a name="provider-implementation-by-windows-presentation-foundation-elements"></a>Implementação de provedor por elementos de Windows Presentation Foundation

Para obter mais informações sobre este tópico, consulte [automação da interface do usuário de um controle personalizado do WPF](../wpf/controls/ui-automation-of-a-wpf-custom-control.md).

<a name="Provider_Implementation_by_non_WPF_Elements"></a>

## <a name="provider-implementation-by-non-wpf-elements"></a>Implementação de provedor por elementos não-WPF

Controles personalizados que não fazem parte da estrutura do WPF, mas que são escritos em código gerenciado (geralmente são Windows Forms controles), oferecem suporte ao [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] implementando interfaces. Cada elemento deve implementar pelo menos uma das interfaces listadas na primeira tabela na próxima seção. Além disso, se o elemento oferecer suporte a um ou mais padrões de controle, ele deverá implementar a interface apropriada para cada padrão de controle.

Seu [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] projeto de provedor deve referenciar os seguintes assemblies:

- UIAutomationProviders.dll

- UIAutomationTypes.dll

- WindowsBase.dll

<a name="Provider_Interfaces"></a>

### <a name="provider-interfaces"></a>Interfaces de provedor

Cada [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] provedor deve implementar uma das seguintes interfaces.

|Interface|Descrição|
|---------------|-----------------|
|<xref:System.Windows.Automation.Provider.IRawElementProviderSimple>|Fornece funcionalidade para um controle simples hospedado em uma janela, incluindo suporte para padrões de controle e propriedades.|
|<xref:System.Windows.Automation.Provider.IRawElementProviderFragment>|Herdada de <xref:System.Windows.Automation.Provider.IRawElementProviderSimple>. Adiciona funcionalidade para um elemento em um controle complexo, incluindo navegação dentro do fragmento, configuração de foco e retorno do retângulo delimitador do elemento.|
|<xref:System.Windows.Automation.Provider.IRawElementProviderFragmentRoot>|Herdada de <xref:System.Windows.Automation.Provider.IRawElementProviderFragment>. Adiciona a funcionalidade para o elemento raiz em um controle complexo, incluindo a localização de um elemento filho em coordenadas especificadas e a definição do estado de foco para todo o controle.|

As interfaces a seguir fornecem funcionalidade adicional, mas não precisam ser implementadas.

|Interface|Descrição|
|---------------|-----------------|
|<xref:System.Windows.Automation.Provider.IRawElementProviderAdviseEvents>|Permite que o provedor rastreie solicitações de eventos.|
|<xref:System.Windows.Automation.Provider.IRawElementProviderHwndOverride>|Habilita a reposicionamento de elementos baseados em janela dentro da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore de um fragmento.|

Todas as outras interfaces no <xref:System.Windows.Automation.Provider> namespace são para suporte ao padrão de controle.

<a name="Requirements_for_Non_WPF_Providers"></a>

### <a name="requirements-for-non-wpf-providers"></a>Requisitos para provedores não WPF

Para se comunicar com o [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , seu controle deve implementar as seguintes áreas principais de funcionalidade:

|Funcionalidade|Implementação|
|-------------------|--------------------|
|Expor o provedor para[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]|Em resposta a uma mensagem de WM_GETOBJECT enviada para a janela de controle, retorne o objeto que implementa <xref:System.Windows.Automation.Provider.IRawElementProviderSimple> (ou uma interface derivada). Para fragmentos, esse deve ser o provedor para a raiz do fragmento.|
|Fornecer valores de propriedade|Implemente <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.GetPropertyValue%2A> para fornecer ou substituir valores.|
|Habilitar o cliente a interagir com o controle|Implemente interfaces que dão suporte a padrões de controle, como <xref:System.Windows.Automation.Provider.IInvokeProvider> . Retorne esses provedores de padrões na sua implementação do <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.GetPatternProvider%2A> .|
|Gerar eventos|Chame um dos métodos estáticos de <xref:System.Windows.Automation.Provider.AutomationInteropProvider> para gerar um evento que um cliente possa escutar.|
|Habilitar navegação e foco em um fragmento|Implemente <xref:System.Windows.Automation.Provider.IRawElementProviderFragment> para cada elemento dentro do fragmento. (Não é necessário para elementos que não fazem parte de um fragmento.)|
|Habilitar foco e localização do elemento filho em um fragmento|Implementar <xref:System.Windows.Automation.Provider.IRawElementProviderFragmentRoot>. (Não é necessário para elementos que não sejam raízes de fragmento.)|

<a name="Property_Values_in_Non_WPF_Providers"></a>

### <a name="property-values-in-non-wpf-providers"></a>Valores de propriedade em provedores não WPF

[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]os provedores de controles personalizados devem dar suporte a determinadas propriedades que podem ser usadas pelo sistema de automação, bem como por aplicativos cliente. Para elementos que são hospedados no Windows (HWNDs), o [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] pode recuperar algumas propriedades do provedor de janela padrão, mas deve obter outras do provedor personalizado.

Os provedores de controles baseados em HWND normalmente não precisam fornecer as seguintes propriedades (identificadas por valores de campo):

- <xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.ProcessIdProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.ClassNameProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.HasKeyboardFocusProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.IsPasswordProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.RuntimeIdProperty>

> [!NOTE]
> O <xref:System.Windows.Automation.AutomationElementIdentifiers.RuntimeIdProperty> de um elemento simples ou raiz de fragmento hospedado em uma janela é obtido na janela; no entanto, os elementos de fragmento abaixo da raiz (como itens de lista em uma caixa de listagem) devem fornecer seus próprios identificadores. Para obter mais informações, consulte <xref:System.Windows.Automation.Provider.IRawElementProviderFragment.GetRuntimeId%2A>.
>
> O <xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty> deve ser retornado para provedores hospedados em um controle de Windows Forms. Nesse caso, o provedor de janela padrão pode não conseguir recuperar o valor correto.
>
> O <xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty> geralmente é fornecido pelo provedor de host. Por exemplo, se um controle personalizado for derivado de <xref:System.Windows.Forms.Control> , o nome será derivado da `Text` Propriedade do controle.

Para obter um exemplo de código, consulte [Propriedades de retorno de um provedor de automação de interface do usuário](return-properties-from-a-ui-automation-provider.md).

<a name="Events_in_Non_WPF_Providers"></a>

### <a name="events-in-non-wpf-providers"></a>Eventos em provedores que não são do WPF

[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]os provedores devem gerar eventos para notificar aplicativos cliente sobre alterações no estado da interface do usuário. Os métodos a seguir são usados para gerar eventos.

|Método|Description|
|------------|-----------------|
|<xref:System.Windows.Automation.Provider.AutomationInteropProvider.RaiseAutomationEvent%2A>|Gera vários eventos, incluindo eventos disparados por padrões de controle.|
|<xref:System.Windows.Automation.Provider.AutomationInteropProvider.RaiseAutomationPropertyChangedEvent%2A>|Gera um evento quando uma propriedade [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] foi alterada.|
|<xref:System.Windows.Automation.Provider.AutomationInteropProvider.RaiseStructureChangedEvent%2A>|Gera um evento quando a estrutura da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore é alterada; por exemplo, pela remoção ou adição de um elemento.|

A finalidade de um evento é notificar o cliente sobre algo que está ocorrendo no [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] , se a atividade é ou não disparada pelo [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] próprio sistema. Por exemplo, o evento identificado por <xref:System.Windows.Automation.InvokePatternIdentifiers.InvokedEvent> deve ser gerado sempre que o controle é invocado, seja por meio de entrada direta do usuário ou pela chamada do aplicativo cliente <xref:System.Windows.Automation.InvokePattern.Invoke%2A> .

Para otimizar o desempenho, um provedor poderá acionar eventos seletivamente ou não gerar nenhum evento se nenhum aplicativo cliente estiver registrado para recebê-los. Os métodos a seguir são usados para otimização.

|Método|Description|
|------------|-----------------|
|<xref:System.Windows.Automation.Provider.AutomationInteropProvider.ClientsAreListening%2A>|Essa propriedade estática especifica se qualquer aplicativo cliente assinou [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] eventos.|
|<xref:System.Windows.Automation.Provider.IRawElementProviderAdviseEvents>|A implementação do provedor dessa interface em uma raiz de fragmento permite que ela seja avisada quando os clientes registram e cancelam o registro de manipuladores de eventos para eventos no fragmento.|

<a name="Non_WPF_Provider_Navigation"></a>

### <a name="non-wpf-provider-navigation"></a>Navegação de provedor não WPF

Provedores para controles simples, como um botão personalizado hospedado em uma janela (HWND), não precisam dar suporte à navegação na [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore. A navegação de e para o elemento é manipulada pelo provedor padrão para a janela do host, que é especificada na implementação de <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.HostRawElementProvider%2A> . No entanto, quando você implementa um provedor para um controle personalizado complexo, deve dar suporte à navegação entre o nó raiz do fragmento e seus descendentes e entre nós irmãos.

> [!NOTE]
> Elementos de um fragmento diferente da raiz devem retornar uma `null` referência de <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.HostRawElementProvider%2A> , porque eles não são hospedados diretamente em uma janela, e nenhum provedor padrão pode dar suporte à navegação de e para eles.

A estrutura do fragmento é determinada pela sua implementação de <xref:System.Windows.Automation.Provider.IRawElementProviderFragment.Navigate%2A> . Para cada direção possível de cada fragmento, esse método retorna o objeto do provedor para o elemento nessa direção. Se não houver nenhum elemento nessa direção, o método retornará uma `null` referência.

A raiz do fragmento dá suporte à navegação somente para elementos filho. Por exemplo, uma caixa de listagem retorna o primeiro item da lista quando a direção é <xref:System.Windows.Automation.Provider.NavigateDirection.FirstChild> , e o último item quando a direção é <xref:System.Windows.Automation.Provider.NavigateDirection.LastChild> . A raiz do fragmento não dá suporte à navegação para pais ou irmãos; Isso é tratado pelo provedor de janela do host.

Elementos de um fragmento que não são a raiz devem dar suporte à navegação para o pai e a quaisquer irmãos e filhos que eles têm.

<a name="Non_WPF_Provider_Reparenting"></a>

### <a name="non-wpf-provider-reparenting"></a>Repai do provedor não WPF

As janelas pop-up são, na verdade, janelas de nível superior e, por padrão, aparecem na [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore como filhas da área de trabalho. Em muitos casos, no entanto, janelas pop-up são logicamente filhos de algum outro controle. Por exemplo, a lista suspensa de uma caixa de combinação é logicamente um filho da caixa de combinação. Da mesma forma, uma janela pop-up de menu é logicamente um filho do menu. [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]fornece suporte para reinicializar janelas pop-up para que pareçam ser filhos do controle associado.

Para recriar o pai de uma janela pop-up:

1. Crie um provedor para a janela pop-up. Isso requer que a classe da janela pop-up seja conhecida com antecedência.

2. Implemente todas as propriedades e padrões como de costume para esse pop-up, como se fosse um controle por si só.

3. Implemente a <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.HostRawElementProvider%2A> propriedade para que ela retorne o valor obtido de <xref:System.Windows.Automation.Provider.AutomationInteropProvider.HostProviderFromHandle%2A> , onde o parâmetro é o identificador de janela da janela pop-up.

4. Implemente <xref:System.Windows.Automation.Provider.IRawElementProviderFragment.Navigate%2A> para a janela pop-up e seu pai para que a navegação seja manipulada corretamente do pai lógico para os filhos lógicos e entre filhos irmãos.

Quando [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] o encontra a janela pop-up, ele reconhece que a navegação está sendo substituída do padrão e ignora a janela pop-up quando ela é encontrada como um filho da área de trabalho. Em vez disso, o nó só poderá ser acessado por meio do fragmento.

A repai não é adequada para casos em que um controle pode hospedar uma janela de qualquer classe. Por exemplo, um rebar pode hospedar qualquer tipo de HWND em suas bandas. Para lidar com esses casos, [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] o dá suporte a uma forma alternativa de realocação de HWND, conforme descrito na próxima seção.

<a name="Non_WPF_Provider_Repositioning"></a>

### <a name="non-wpf-provider-repositioning"></a>Reposicionamento de provedor não WPF

[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]os fragmentos podem conter dois ou mais elementos que estão contidos em uma janela (HWND). Como cada HWND tem seu próprio provedor padrão que considera o HWND como um filho de um HWND recipiente, a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore, por padrão, mostrará os HWNDs no fragmento como filhos da janela pai. Na maioria dos casos, esse comportamento é desejável, mas às vezes pode levar à confusão porque não corresponde à estrutura lógica do [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] .

Um bom exemplo disso é um controle rebar. Um rebar contém bandas, e cada uma delas pode, por sua vez, conter um controle baseado em HWND, como uma barra de ferramentas, uma caixa de edição ou uma caixa de combinação. O provedor de janela padrão para o HWND de rebar vê os HWNDs de controle de banda como filhos, e o provedor de rebar vê as bandas como filhos. Como o provedor de HWND e o provedor de rebar estão trabalhando em tandem e combinando seus filhos, as faixas e os controles baseados em HWND aparecem como filhos do rebar. No entanto, logicamente, somente as faixas devem aparecer como filhos do rebar, e cada provedor de banda deve ser acoplado ao provedor HWND padrão para o controle que ele contém.

Para fazer isso, o provedor raiz do fragmento do rebar expõe um conjunto de filhos representando as faixas. Cada banda tem um único provedor que pode expor propriedades e padrões. Em sua implementação de <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.HostRawElementProvider%2A> , o provedor de banda retorna o provedor de janela padrão para o HWND de controle, que é obtido chamando <xref:System.Windows.Automation.Provider.AutomationInteropProvider.HostProviderFromHandle%2A> , passando o identificador de janela do controle. Por fim, o provedor raiz do fragmento para o rebar implementa a <xref:System.Windows.Automation.Provider.IRawElementProviderHwndOverride> interface e, em sua implementação, <xref:System.Windows.Automation.Provider.IRawElementProviderHwndOverride.GetOverrideProviderForHwnd%2A> retorna o provedor de banda apropriado para o controle contido no HWND especificado.

## <a name="see-also"></a>Veja também

- [Visão Geral dos Provedores de Automação de Interface do Usuário](ui-automation-providers-overview.md)
- [Expor um provedor de automação de interface do usuário do lado do servidor](expose-a-server-side-ui-automation-provider.md)
- [Retornando Propriedades de um Provedor de Automação de IU](return-properties-from-a-ui-automation-provider.md)
- [Disparar Eventos de um Provedor de Automação UI](raise-events-from-a-ui-automation-provider.md)
- [Permitir navegação em um fragmento por um provedor de automação de interface do usuário](enable-navigation-in-a-ui-automation-fragment-provider.md)
- [Padrões de controle de suporte em um provedor de automação da interface do usuário](support-control-patterns-in-a-ui-automation-provider.md)
