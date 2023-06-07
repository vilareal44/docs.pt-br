---
title: Implementando o Padrão Controle de Value de Automação de Interface de Usuário
description: Examine as diretrizes e convenções para implementar o padrão de controle de valor na automação da interface do usuário. Conheça os membros necessários para a interface IValueProvider.
ms.date: 03/30/2017
helpviewer_keywords:
- control patterns, Value
- UI Automation, Value control pattern
- Value control pattern
ms.assetid: b0fcdd87-3add-4345-bca9-e891205e02ba
ms.openlocfilehash: a15c0b50996e2c0dfdc937bc9565d5f9ba20c992
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168196"
---
# <a name="implementing-the-ui-automation-value-control-pattern"></a>Implementando o Padrão Controle de Value de Automação de Interface de Usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico apresenta diretrizes e convenções para implementação <xref:System.Windows.Automation.Provider.IValueProvider> , incluindo informações sobre eventos e propriedades. Links para referências adicionais são listados no final do tópico.  
  
 O <xref:System.Windows.Automation.ValuePattern> padrão de controle é usado para dar suporte a controles que têm um valor intrínseco que não abrange um intervalo e que pode ser representado como uma cadeia de caracteres. Essa cadeia de caracteres pode ser editável, dependendo do controle e de suas configurações. Para obter exemplos de controles que implementam esse padrão, consulte [mapeamento de padrão de controle para clientes de automação da interface do usuário](control-pattern-mapping-for-ui-automation-clients.md).  
  
<a name="Implementation_Guidelines_and_Conventions"></a>
## <a name="implementation-guidelines-and-conventions"></a>Diretrizes e convenções de implementação  
 Ao implementar o padrão de controle de valor, observe as seguintes diretrizes e convenções:  
  
- Controles como <xref:System.Windows.Automation.ControlType.ListItem> e <xref:System.Windows.Automation.ControlType.TreeItem> devem oferecer suporte <xref:System.Windows.Automation.ValuePattern> se o valor de qualquer um dos itens for editável, independentemente do modo de edição atual do controle. O controle pai também deve oferecer suporte <xref:System.Windows.Automation.ValuePattern> se os itens filho forem editáveis.  
  
 ![Item de lista editável.](./media/uia-valuepattern-editable-listitem.PNG "UIA_ValuePattern_Editable_ListItem")  
Exemplo de um item de lista editável  
  
- Os controles de edição de linha única dão suporte ao acesso programático a seu conteúdo implementando <xref:System.Windows.Automation.Provider.IValueProvider> . No entanto, os controles de edição de várias linhas não são implementados <xref:System.Windows.Automation.Provider.IValueProvider> ; em vez disso, eles fornecem acesso ao seu conteúdo implementando <xref:System.Windows.Automation.Provider.ITextProvider> .  
  
- Para recuperar o conteúdo textual de um controle de edição de várias linhas, o controle deve implementar <xref:System.Windows.Automation.Provider.ITextProvider> . No entanto, <xref:System.Windows.Automation.Provider.ITextProvider> o não oferece suporte à definição do valor de um controle.  
  
- <xref:System.Windows.Automation.Provider.IValueProvider>o não oferece suporte à recuperação de informações de formatação ou valores de subcadeias. Implemente <xref:System.Windows.Automation.Provider.ITextProvider> nesses cenários.  
  
- <xref:System.Windows.Automation.Provider.IValueProvider>deve ser implementado por controles como o controle de seleção do **seletor de cores** do Microsoft Word (ilustrado abaixo), que oferece suporte ao mapeamento de cadeia de caracteres entre um valor de cor (por exemplo, "amarelo") e uma estrutura RGB interna equivalente.  
  
 ![Seletor de cores com amarelo realçado.](./media/uia-valuepattern-colorpicker.png "UIA_ValuePattern_ColorPicker")  
Exemplo de mapeamento de cadeia de caracteres de amostra de cor  
  
- Um controle deve ter seu <xref:System.Windows.Automation.AutomationElement.IsEnabledProperty> definido como `true` e seu <xref:System.Windows.Automation.ValuePattern.IsReadOnlyProperty> definido como `false` antes de permitir uma chamada para <xref:System.Windows.Automation.Provider.IValueProvider.SetValue%2A> .  
  
<a name="Required_Members_for_the_IValueProvider_Interface"></a>
## <a name="required-members-for-ivalueprovider"></a>Membros necessários para IValueProvider  
 As propriedades e os métodos a seguir são necessários para implementar o <xref:System.Windows.Automation.Provider.IValueProvider> .  
  
|Membros necessários|Tipo de membro|Anotações|  
|----------------------|-----------------|-----------|  
|<xref:System.Windows.Automation.ValuePattern.IsReadOnlyProperty>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.ValuePattern.ValueProperty>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.ValuePattern.SetValue%2A>|Método|Nenhum|  
  
<a name="Exceptions"></a>
## <a name="exceptions"></a>Exceções  
 Os provedores devem lançar as seguintes exceções.  
  
|Tipo de exceção|Condição|  
|--------------------|---------------|  
|<xref:System.InvalidOperationException>|<xref:System.Windows.Automation.ValuePattern.SetValue%2A><br /><br /> -Se informações específicas de localidade forem passadas para um controle em um formato incorreto, como uma data formatada incorretamente.|  
|<xref:System.ArgumentException>|<xref:System.Windows.Automation.ValuePattern.SetValue%2A><br /><br /> -Se um novo valor não puder ser convertido de uma cadeia de caracteres em um formato reconhecido pelo controle.|  
|<xref:System.Windows.Automation.ElementNotEnabledException>|<xref:System.Windows.Automation.ValuePattern.SetValue%2A><br /><br /> -Quando é feita uma tentativa de manipular um controle que não está habilitado.|  
  
## <a name="see-also"></a>Confira também

- [Visão Geral de Padrões de Controle de Automação de Interface de Usuário](ui-automation-control-patterns-overview.md)
- [Padrões de controle de suporte em um provedor de automação da interface do usuário](support-control-patterns-in-a-ui-automation-provider.md)
- [Padrões de Controle para Clientes de Automação de IU](ui-automation-control-patterns-for-clients.md)
- [Exemplo de inserção de texto do ValuePattern](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/InsertText)
- [Visão geral da árvore de automação de interface do usuário](ui-automation-tree-overview.md)
- [Usar armazenamento em cache em automação de interface do usuário](use-caching-in-ui-automation.md)
