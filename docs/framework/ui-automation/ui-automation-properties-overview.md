---
title: Visão geral das propriedades de automação da interface do usuário
description: Veja uma ampla visão geral das propriedades de automação da interface do usuário da Microsoft. Saiba mais sobre identificadores de propriedade, propriedades por categoria, localização, propriedades e eventos.
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, properties
- properties, UI Automation
ms.assetid: a6c31d7b-b33e-49b3-b5c1-31a345f9b7c8
ms.openlocfilehash: 17d780c059530be8c91890302ea4066de2d4aa73
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87163211"
---
# <a name="ui-automation-properties-overview"></a>Visão geral das propriedades de automação da interface do usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Provedores de automação de interface do usuário expõem propriedades em [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] elementos. Essas propriedades permitem que aplicativos cliente de automação da interface do usuário descubram informações sobre partes do [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] , especialmente controles, incluindo dados estáticos e dinâmicos.  
  
 Esta seção fornece uma ampla visão geral das [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] Propriedades. Informações mais específicas são fornecidas nos seguintes tópicos:  
  
- [Automação de Propriedades de Interface de Usuário para Clientes.](ui-automation-properties-for-clients.md)  
  
- [Implementação do provedor de automação de interface do usuário no lado do servidor](server-side-ui-automation-provider-implementation.md)  
  
<a name="Property_Identifiers"></a>
## <a name="property-identifiers"></a>Identificadores de propriedade  
 Cada propriedade é identificada por um número e um nome. Os nomes das propriedades são usados somente para depuração e diagnóstico. Os provedores usam as IDs numéricas para identificar as solicitações de propriedade de entrada. No entanto, os aplicativos cliente usam apenas <xref:System.Windows.Automation.AutomationProperty> o, que encapsula o número e o nome, para identificar as propriedades que desejam recuperar.  
  
 <xref:System.Windows.Automation.AutomationProperty>objetos que representam propriedades específicas estão disponíveis como campos em várias classes. Por motivos de segurança, os provedores de automação da interface do usuário obtêm esses objetos de um conjunto separado de classes contidas no Uiautomationtypes.dll.  
  
 A tabela a seguir categoriza as propriedades pelas classes que contêm as <xref:System.Windows.Automation.AutomationProperty> IDs.  
  
|Tipos de propriedades|Clientes obtêm IDs de|Os provedores obtêm IDs de|  
|-------------------------|--------------------------|----------------------------|  
|Propriedades comuns a todos os elementos (consulte as tabelas a seguir)|<xref:System.Windows.Automation.AutomationElement>|<xref:System.Windows.Automation.AutomationElementIdentifiers>|  
|Posição de uma janela de encaixe|<xref:System.Windows.Automation.DockPattern>|<xref:System.Windows.Automation.DockPatternIdentifiers>|  
|Estado de um elemento que pode ser expandido e recolhido|<xref:System.Windows.Automation.ExpandCollapsePattern>|<xref:System.Windows.Automation.ExpandCollapsePatternIdentifiers>|  
|Propriedades de um item em uma grade|<xref:System.Windows.Automation.GridItemPattern>|<xref:System.Windows.Automation.GridItemPatternIdentifiers>|  
|Propriedades de uma grade|<xref:System.Windows.Automation.GridPattern>|<xref:System.Windows.Automation.GridPatternIdentifiers>|  
|Exibição atual e com suporte de um elemento que tem várias exibições|<xref:System.Windows.Automation.MultipleViewPattern>|<xref:System.Windows.Automation.MultipleViewPatternIdentifiers>|  
|Propriedades de um elemento que se movem sobre um intervalo de valores, como um controle deslizante|<xref:System.Windows.Automation.RangeValuePattern>|<xref:System.Windows.Automation.RangeValuePatternIdentifiers>|  
|Propriedades de uma janela de rolagem|<xref:System.Windows.Automation.ScrollPattern>|<xref:System.Windows.Automation.ScrollPatternIdentifiers>|  
|Status e contêiner de um item que pode ser selecionado, como em uma lista|<xref:System.Windows.Automation.SelectionItemPattern>|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers>|  
|Propriedades de um controle que contém itens de seleção|<xref:System.Windows.Automation.SelectionPattern>|<xref:System.Windows.Automation.SelectionPatternIdentifiers>|  
|Cabeçalhos de coluna e linha de um item em uma tabela|<xref:System.Windows.Automation.TableItemPattern>|<xref:System.Windows.Automation.TableItemPatternIdentifiers>|  
|Cabeçalhos de coluna e linha e orientação de uma tabela|<xref:System.Windows.Automation.TablePattern>|<xref:System.Windows.Automation.TablePatternIdentifiers>|  
|Estado de um controle de alternância|<xref:System.Windows.Automation.TogglePattern>|<xref:System.Windows.Automation.TogglePatternIdentifiers>|  
|Recursos de um elemento que podem ser movidos, girados ou redimensionados|<xref:System.Windows.Automation.TransformPattern>|<xref:System.Windows.Automation.TransformPatternIdentifiers>|  
|Recursos de valor e leitura/gravação de um elemento que tem um valor|<xref:System.Windows.Automation.ValuePattern>|<xref:System.Windows.Automation.ValuePatternIdentifiers>|  
|Recursos e estado de uma janela|<xref:System.Windows.Automation.WindowPattern>|<xref:System.Windows.Automation.WindowPatternIdentifiers>|  
  
<a name="Properties_by_Category"></a>
## <a name="properties-by-category"></a>Propriedades por Categoria  
 As tabelas a seguir categorizam as propriedades cujas IDs são encontradas em <xref:System.Windows.Automation.AutomationElement> e <xref:System.Windows.Automation.AutomationElementIdentifiers> . Essas propriedades são comuns a todos os controles. Todos os poucos deles provavelmente serão estáticos durante o tempo de vida do aplicativo do provedor; a maioria das propriedades dinâmicas está associada aos padrões de controle.  
  
 A coluna **acesso à propriedade** lista quaisquer outros acessadores para cada propriedade, além <xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A> de <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A> e. Para obter mais informações sobre como obter propriedades em um aplicativo cliente, consulte [Propriedades de automação da interface do usuário para clientes](ui-automation-properties-for-clients.md).  
  
> [!NOTE]
> Para obter informações específicas sobre cada propriedade, siga o link na coluna **acesso à propriedade** .  
  
### <a name="display-characteristics"></a>Exibir Características  
  
|Identificador de propriedade|Acesso à propriedade|  
|-------------------------|---------------------|  
|<xref:System.Windows.Automation.AutomationElement.BoundingRectangleProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.BoundingRectangle%2A>|  
|<xref:System.Windows.Automation.AutomationElement.CultureProperty>|N/D|  
|<xref:System.Windows.Automation.AutomationElement.HelpTextProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.HelpText%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsOffscreenProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsOffscreen%2A>|  
|<xref:System.Windows.Automation.AutomationElement.OrientationProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.Orientation%2A>|  
  
### <a name="element-type"></a>Tipo de elemento  
  
|Identificador de propriedade|Acesso à propriedade|  
|-------------------------|---------------------|  
|<xref:System.Windows.Automation.AutomationElement.ControlTypeProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.ControlType%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsContentElementProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsContentElement%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsControlElementProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsControlElement%2A>|  
|<xref:System.Windows.Automation.AutomationElement.ItemTypeProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.ItemType%2A>|  
|<xref:System.Windows.Automation.AutomationElement.LocalizedControlTypeProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.LocalizedControlType%2A>|  
  
### <a name="identification"></a>Identificação  
  
|Identificador de propriedade|Acesso à propriedade|  
|-------------------------|---------------------|  
|<xref:System.Windows.Automation.AutomationElement.AutomationIdProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.AutomationId%2A>|  
|<xref:System.Windows.Automation.AutomationElement.ClassNameProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.ClassName%2A>|  
|<xref:System.Windows.Automation.AutomationElement.FrameworkIdProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.FrameworkId%2A>|  
|<xref:System.Windows.Automation.AutomationElement.LabeledByProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.LabeledBy%2A>|  
|<xref:System.Windows.Automation.AutomationElement.NameProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.Name%2A>|  
|<xref:System.Windows.Automation.AutomationElement.ProcessIdProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.ProcessId%2A>|  
|<xref:System.Windows.Automation.AutomationElement.RuntimeIdProperty>|<xref:System.Windows.Automation.AutomationElement.GetRuntimeId%2A>|  
|<xref:System.Windows.Automation.AutomationElement.NativeWindowHandleProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.NativeWindowHandle%2A>|  
  
### <a name="interaction"></a>Interação  
  
|Identificador de propriedade|Acesso à propriedade|  
|-------------------------|---------------------|  
|<xref:System.Windows.Automation.AutomationElement.AcceleratorKeyProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.AcceleratorKey%2A>|  
|<xref:System.Windows.Automation.AutomationElement.AccessKeyProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.AccessKey%2A>|  
|<xref:System.Windows.Automation.AutomationElement.ClickablePointProperty>|<xref:System.Windows.Automation.AutomationElement.GetClickablePoint%2A>|  
|<xref:System.Windows.Automation.AutomationElement.HasKeyboardFocusProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.HasKeyboardFocus%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsEnabledProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsEnabled%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsKeyboardFocusableProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsKeyboardFocusable%2A>|  
  
### <a name="support-for-patterns"></a>Suporte para padrões  
  
|Identificador de propriedade|Acesso à propriedade|  
|-------------------------|---------------------|  
|<xref:System.Windows.Automation.AutomationElement.IsDockPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsExpandCollapsePatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsGridItemPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsGridPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsInvokePatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsMultipleViewPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsRangeValuePatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsScrollItemPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsScrollPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsSelectionItemPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsSelectionPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsTableItemPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsTablePatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsTextPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsTogglePatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsTransformPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsValuePatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsWindowPatternAvailableProperty>|<xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>|  
  
### <a name="miscellaneous"></a>Diversos  
  
|Identificador de propriedade|Acesso à propriedade|  
|-------------------------|---------------------|  
|<xref:System.Windows.Automation.AutomationElement.IsRequiredForFormProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsRequiredForForm%2A>|  
|<xref:System.Windows.Automation.AutomationElement.IsPasswordProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsPassword%2A>|  
|<xref:System.Windows.Automation.AutomationElement.ItemStatusProperty>|<xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.ItemStatus%2A>|  
  
<a name="Localization"></a>
## <a name="localization"></a>Localização  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]os provedores devem apresentar as seguintes propriedades no idioma do sistema operacional:  
  
- <xref:System.Windows.Automation.AutomationElementIdentifiers.AcceleratorKeyProperty>  
  
- <xref:System.Windows.Automation.AutomationElementIdentifiers.AccessKeyProperty>  
  
- <xref:System.Windows.Automation.AutomationElementIdentifiers.HelpTextProperty>  
  
- <xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>  
  
- <xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>  
  
<a name="Properties_and_Events"></a>
## <a name="properties-and-events"></a>Propriedades e eventos  
 Fortemente vinculados com as propriedades no [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] é o conceito de eventos de alteração de propriedade. Para propriedades dinâmicas, o aplicativo cliente precisa de uma maneira de saber que um valor de propriedade foi alterado, para que ele possa atualizar seu cache de informações ou reagir às novas informações de alguma outra maneira.  
  
 Provedores geram eventos quando algo nas [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] alterações. Por exemplo, se uma caixa de seleção for marcada ou desmarcada, um evento de alteração de propriedade será gerado pela implementação do provedor do padrão de alternância. Os provedores podem gerar eventos seletivamente, dependendo se algum cliente está ouvindo eventos ou escutando eventos específicos.  
  
 Nem todas as alterações de propriedade geram eventos; Isso é inteiramente a implementação do provedor de automação da interface do usuário para o elemento. Por exemplo, os provedores de proxy padrão para caixas de listagem não geram um evento quando as <xref:System.Windows.Automation.SelectionPattern.SelectionProperty> alterações. Nesse caso, o aplicativo deve escutar um <xref:System.Windows.Automation.SelectionItemPattern.ElementSelectedEvent> .  
  
 Os clientes escutam eventos assinando-os. Assinar eventos significa criar métodos delegados que possam manipular os eventos e, em seguida, passar os métodos para [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] junto com os eventos específicos que serão tratados nesses métodos. Para eventos de propriedade alterada em particular, os clientes devem implementar <xref:System.Windows.Automation.AutomationPropertyChangedEventHandler> .  
  
## <a name="see-also"></a>Confira também

- [Armazenando em cache em clientes de automação de interface do usuário](caching-in-ui-automation-clients.md)
- [Automação de Propriedades de Interface de Usuário para Clientes.](ui-automation-properties-for-clients.md)
- [Implementação do provedor de automação de interface do usuário no lado do servidor](server-side-ui-automation-provider-implementation.md)
- [Localizar um elemento de automação de interface do usuário com base na condição de uma propriedade](find-a-ui-automation-element-based-on-a-property-condition.md)
- [Retornando Propriedades de um Provedor de Automação de IU](return-properties-from-a-ui-automation-provider.md)
- [Disparar Eventos de um Provedor de Automação UI](raise-events-from-a-ui-automation-provider.md)
