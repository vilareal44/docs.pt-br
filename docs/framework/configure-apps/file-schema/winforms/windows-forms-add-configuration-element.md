---
title: Windows Forms adicionar elemento de configuração
ms.date: 04/07/2017
helpviewer_keywords:
- Windows Forms Add configuration element
- configuring Windows Forms applications
ms.assetid: 3e3e04de-99d1-4658-b716-44cb669d9589
ms.openlocfilehash: 26b806f84c3e1bc44e0437a8f8806316b14897b8
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73109656"
---
# <a name="windows-forms-add-configuration-element"></a>Windows Forms adicionar elemento de configuração

O `<add>` elemento adiciona uma chave predefinida que especifica se o aplicativo do Windows Form dá suporte a recursos adicionados a Windows Forms aplicativos no .NET Framework 4,7 ou posterior.

## <a name="syntax"></a>Sintaxe

```xml
<System.Windows.Forms.ApplicationConfigurationSection>
  <add key="key-name" value="key-value" />
</System.Windows.Forms.ApplicationConfigurationSection>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

| Atributo | Descrição |
| --------- | ----------- |
| `key`     | Atributo obrigatório. Um nome de chave predefinido que corresponde a um determinado Windows Forms recurso personalizável. |
| `value`   | Atributo obrigatório. O valor a ser atribuído a `key`. |

### <a name="key-attribute-names-and-associated-values"></a>`key`nomes de atributo e valores associados

| nome de `key` | Valores | Descrição |
| ---------- | ------ | ----------- |
| "AnchorLayout.DisableSinglePassControlScaling" | "true" &#124; "false" | Indica se os controles ancorados são dimensionados em uma única passagem. "true" para desabilitar o dimensionamento de passagem única; caso contrário, false. Consulte a seção "escala de passagem única" nos [comentários](#remarks) para obter mais informações. |
| "DpiAwareness" | "PerMonitorV2" &#124; "false" | Indica se um aplicativo reconhece DPI. Defina a chave como "PerMonitorV2" para dar suporte ao reconhecimento de DPI; caso contrário, defina-o como "false". O reconhecimento de DPI é um recurso opcional; para aproveitar o suporte a alto DPI Windows Forms, você deve definir seu valor como "PerMonitorV2". Consulte a seção [comentários](#remarks) para obter mais informações. |
| "CheckedListBox. DisableHighDpiImprovements" | "true" &#124; "false" | Indica se o <xref:System.Windows.Forms.CheckedListBox> controle aproveita os aprimoramentos de dimensionamento e layout introduzidos no .NET Framework 4,7. "verdadeiro" para recusar aprimoramentos de dimensionamento e layout; caso contrário, "false". |
| "DataGridView. DisableHighDpiImprovements" | "true" &#124; "false" | Indica se o <xref:System.Windows.Forms.DataGridView> dimensionamento de controle e os aprimoramentos de layout introduzidos no .NET Framework 4,7. "true" para recusar o reconhecimento de DPI; caso contrário, "false". |
| "DisableDpiChangedMessageHandling" | "true" &#124; "false" | "true" para recusar o recebimento de mensagens relacionadas a alterações de escala de DPI; caso contrário, "false". Consulte a seção [comentários](#remarks) para obter mais informações. |
| "EnableWindowsFormsHighDpiAutoResizing" | "true" &#124; "false" | Indica se um aplicativo Windows Forms é redimensionado automaticamente devido a alterações de escala de DPI. "true" para habilitar o redimensionamento automático; caso contrário, false. |
| "Form. DisableSinglePassControlScaling" | "true" &#124; "false" | Indica se o <xref:System.Windows.Forms.Form> é dimensionado em uma única passagem. "true" para desabilitar o dimensionamento de passagem única; caso contrário, false. Consulte a seção "escala de passagem única" nos [comentários](#remarks) para obter mais informações. |
| "MonthCalendar. DisableSinglePassControlScaling" | "true" &#124; "false" | Indica se o <xref:System.Windows.Forms.MonthCalendar> controle é dimensionado em uma única passagem. "true" para desabilitar o dimensionamento de passagem única; caso contrário, false. Consulte a seção "escala de passagem única" nos [comentários](#remarks) para obter mais informações. |
| "ToolStrip. DisableHighDpiImprovements" | "true" &#124; "false" | Indica se o <xref:System.Windows.Forms.ToolStrip> controle aproveita os aprimoramentos de dimensionamento e layout introduzidos no .NET Framework 4,7. "true" para recusar o reconhecimento de DPI; caso contrário, "false". |

### <a name="child-elements"></a>Elementos filho

Nenhum.

### <a name="parent-elements"></a>Elementos pai

| Elemento | Descrição |
| ------- | ----------- |
| [`<System.Windows.Forms.ApplicationConfigurationSection>`](index.md) | Configura o suporte para novos recursos de Windows Forms aplicativo. |

## <a name="remarks"></a>Comentários

A partir do .NET Framework 4.7, o elemento `<System.Windows.Forms.ApplicationConfigurationSection>` permite configurar os aplicativos do Windows Forms para aproveitar os recursos adicionados em versões recentes do .NET Framework.

O `<System.Windows.Forms.ApplicationConfigurationSection>` elemento permite que você adicione um ou mais `<add>` elementos filho, cada um deles define uma definição de configuração específica.

Para obter uma visão geral do suporte a alto DPI Windows Forms, consulte [suporte a alto dpi no Windows Forms](../../../winforms/high-dpi-support-in-windows-forms.md).

### <a name="dpiawareness"></a>DpiAwareness

Windows Forms aplicativos executados em versões do Windows começando com o Windows 10 Creators Edition e as versões de destino do .NET Framework a partir do .NET Framework 4,7 podem ser configurados para aproveitar os aprimoramentos de DPI alto introduzidos no .NET Framework 4,7. Eles incluem:

- Suporte para cenários de DPI dinâmico em que o usuário altera o fator de escala ou DPI depois que um aplicativo de Windows Forms foi iniciado.

- Melhorias no dimensionamento e no layout de vários controles de Windows Forms, como o <xref:System.Windows.Forms.MonthCalendar> controle e o <xref:System.Windows.Forms.CheckedListBox> controle.

O reconhecimento de DPI alto é um recurso opcional; Por padrão, o valor de `DpiAwareness` é `false` . Você pode aceitar o suporte do Windows Forms para reconhecimento de DPI definindo o valor dessa chave como `PerMonitorV2` no arquivo de configuração do aplicativo. Se o reconhecimento de DPI estiver habilitado, todos os recursos individuais de DPI também estarão habilitados. Eles incluem:

- As mensagens de DPI alteradas, que são controladas pela `DisableDpiChangedMessageHandling` chave.

- Suporte a DPI dinâmico, que é controlado pela `EnableWindowsFormsHighDpiAutoResizing` chave.

- Dimensionamento de controle de passagem única, que é controlado pelo `Form.DisableSinglePassControlScaling` para <xref:System.Windows.Forms.Form> controles individuais, pela `AnchorLayout.DisableSinglePassControlScaling` chave para controles ancorados e pela `MonthCalendar.DisableSinglePassControlScaling` chave do <xref:System.Windows.Forms.MonthCalendar> controle

- Aprimoramentos de layout e dimensionamento de DPI alto, que é controlado pela `CheckListBox.DisableHighDpiImprovements` chave do <xref:System.Windows.Forms.CheckedListBox> controle, pela `DataGridView.DisableHighDpiImprovements` chave do <xref:System.Windows.Forms.DataGridView> controle e pela `Toolstrip.DisableHighDpiImprovements` chave do <xref:System.Windows.Forms.ToolStrip> controle.

A configuração de aceitação padrão única fornecida pela configuração `DpiAwareness` para `PerMonitorV2` é geralmente adequada para novos aplicativos de Windows Forms. No entanto, você pode recusar aprimoramentos individuais de DPI, adicionando a chave correspondente ao arquivo de configuração do aplicativo. Por exemplo, para aproveitar todos os novos recursos de DPI, exceto para suporte a DPI dinâmico, você adicionaria o seguinte ao arquivo de configuração do aplicativo:

```xml
<System.Windows.Forms.ApplicationConfigurationSection>
   <add key="DpiAwareness" value="PerMonitorV2" />
   <!-- Disable dynamic DPI support -->
   <add key="EnableWindowsFormsHighDpiAutoResizing" value="false" />
</System.Windows.Forms.ApplicationConfigurationSection>
```

Normalmente, você recusa um recurso específico porque optou por tratá-lo programaticamente.

Para obter mais informações sobre como aproveitar o suporte de DPI alto em aplicativos Windows Forms, consulte [suporte a alto dpi no Windows Forms](../../../winforms/high-dpi-support-in-windows-forms.md).

### <a name="disabledpichangedmessagehandling"></a>DisableDpiChangedMessageHandling

A partir do .NET Framework 4,7, Windows Forms controles geram vários eventos relacionados a alterações no dimensionamento de DPI. Isso inclui os <xref:System.Windows.Forms.Control.DpiChangedAfterParent> <xref:System.Windows.Forms.Control.DpiChangedBeforeParent> eventos, e <xref:System.Windows.Forms.Form.DpiChanged> . O valor da `DisableDpiChangedMessageHandling` chave determina se esses eventos são gerados em um aplicativo Windows Forms.

### <a name="single-pass-scaling"></a>Dimensionamento de passagem única

O dimensionamento único ou de várias passagens influencia a capacidade de resposta percebida da interface do usuário e a aparência visual dos elementos da interface do usuário conforme eles são dimensionados. A partir do .NET Framework 4,7, Windows Forms usa o dimensionamento de passagem única. Nas versões anteriores do .NET Framework, o dimensionamento foi realizado por meio de várias passagens, o que fazia com que alguns controles fosse escalado mais do que o necessário. O dimensionamento de passagem única só deverá ser desabilitado se seu aplicativo depender do comportamento antigo.

## <a name="see-also"></a>Confira também

- [Seção de configuração do Windows Forms](index.md)
- [Suporte a alto DPI no Windows Forms](../../../winforms/high-dpi-support-in-windows-forms.md)
