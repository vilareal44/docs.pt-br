---
ms.openlocfilehash: cc3c2c2be179842f87be8892d057a6c4138086cb
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614307"
---
### <a name="accessibility-improvements-in-windows-forms-controls-for-net-472"></a>Melhorias de acessibilidade nos controles do Windows Forms para o .NET 4.7.2

#### <a name="details"></a>Detalhes

O Windows Forms Framework está melhorando sua maneira de trabalhar com tecnologias de acessibilidade para melhorar o suporte aos clientes do Windows Forms. Isso inclui as seguintes alterações:

- Alterações para melhorar a exibição durante o modo de Alto Contraste.
- Alterações para melhorar a navegação pelo teclado nos controles DataGridView e MenuStrip.
- Alterações na interação com o Narrator.

#### <a name="suggestion"></a>Sugestão

**Como aceitar ou recusar essas alterações** Para que o aplicativo se beneficie dessas alterações, ele deve ser executado no .NET Framework 4.7.2 ou posterior. O aplicativo pode se beneficiar dessas alterações de uma das seguintes maneiras:

- Ser recompilado para ser direcionado ao .NET Framework 4.7.2. Essas alterações de acessibilidade são habilitadas por padrão nos aplicativos do Windows Forms direcionados ao .NET Framework 4.7.2 ou posterior.
- Ser direcionado ao .NET Framework 4.7.1 ou a uma versão anterior e recusar os comportamentos de acessibilidade herdados, adicionando a seguinte [Opção AppContext](https://docs.microsoft.com/dotnet/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element) à seção `<runtime>` do arquivo de configuração de aplicativo e definindo-a como `false`, como mostra o exemplo a seguir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7"/>
  </startup>
  <runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true/false;key2=true/false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false" />
  </runtime>
</configuration>
```

Observe que para aceitar os recursos de acessibilidade adicionados no .NET Framework 4.7.2, você também precisa aceitar os recursos de acessibilidade do .NET Framework 4.7.1. Aplicativos direcionados para o .NET Framework 4.7.2 ou posterior e que desejam preservar o comportamento de acessibilidade herdado podem optar pelo uso de recursos de acessibilidade herdados definindo explicitamente essa opção AppContext como `true` .

**Uso de cores definidas pelo sistema operacional em temas de Alto Contraste**

- A seta suspensa do <xref:System.Windows.Forms.ToolStripDropDownButton> agora usa cores definidas pelo sistema operacional no tema de Alto Contraste.
- Os controles <xref:System.Windows.Forms.Button>, <xref:System.Windows.Forms.RadioButton> e <xref:System.Windows.Forms.CheckBox> com <xref:System.Windows.Forms.ButtonBase.FlatStyle> definido como <xref:System.Windows.Forms.FlatStyle.Flat?displayProperty=nameWithType> ou <xref:System.Windows.Forms.FlatStyle.Popup?displayProperty=nameWithType>, agora usam cores definidas pelo SO no tema de Alto Contraste quando selecionado. Anteriormente, as cores de texto e de tela de fundo não eram contrastantes e eram difíceis de ler.
- Os controles contidos em uma <xref:System.Windows.Forms.GroupBox> que tem sua propriedade <xref:System.Windows.Forms.Control.Enabled> definida como `false` agora usam cores definidas pelo SO no tema de Alto Contraste.
- Os controles <xref:System.Windows.Forms.ToolStripButton>, <xref:System.Windows.Forms.ToolStripComboBox> e <xref:System.Windows.Forms.ToolStripDropDownButton> têm uma taxa maior de contraste de luminosidade no modo de Alto Contraste.
- Por padrão, a <xref:System.Windows.Forms.DataGridViewLinkCell> usará as cores definidas pelo SO no modo de Alto Contraste para a propriedade <xref:System.Windows.Forms.DataGridViewLinkCell.LinkColor?displayProperty=nameWithType>.
Observação: o Windows 10 mudou os valores de algumas cores do sistema de Alto Contraste. A estrutura do Windows Forms é baseada na estrutura do Win32. Para obter a melhor experiência, execute na versão mais recente do Windows e aceite as alterações mais recentes do sistema operacional adicionando um arquivo app. manifest em um aplicativo de teste e removendo o comentário do seguinte código:

```xml
<!-- Windows 10 -->
<supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
```

**Melhor suporte ao Narrador**

- O Narrator agora apresenta o valor da propriedade <xref:System.Windows.Forms.ToolStripMenuItem.ShortcutKeys?displayProperty=nameWithType> quando apresenta o texto de um <xref:System.Windows.Forms.ToolStripMenuItem>.
- O Narrator agora indica quando um <xref:System.Windows.Forms.ToolStripMenuItem> tem sua propriedade <xref:System.Windows.Forms.Control.Enabled> definida como `false`.
- O Narrator agora fornece comentários sobre o estado de uma caixa de seleção quando a propriedade <xref:System.Windows.Forms.ListView.CheckBoxes?displayProperty=nameWithType> está definida como `true`.
- A ordem de foco do modo de varredura do Narrator agora é consistente com a ordem visual dos controles na janela de diálogo de download do ClickOnce.

**Melhoria no suporte à acessibilidade da DataGridView**

- As linhas em uma <xref:System.Windows.Forms.DataGridView> agora podem ser classificadas usando o teclado. Agora um usuário pode usar a tecla F3 para classificar pela coluna atual.
- Quando o <xref:System.Windows.Forms.DataGridView.SelectionMode?displayProperty=nameWithType> for definido como <xref:System.Windows.Forms.DataGridViewSelectionMode.FullRowSelect?displayProperty=nameWithType>, o cabeçalho da coluna mudará de cor para indicar a coluna atual à medida que o usuário pressionar Tab para percorrer as células na linha atual.
- A propriedade <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Parent?displayProperty=nameWithType> agora retorna o controle pai correto.

**Melhoria das indicações visuais**

- Os controles <xref:System.Windows.Forms.RadioButton> e <xref:System.Windows.Forms.CheckBox> com uma propriedade <xref:System.Windows.Forms.ButtonBase.Text> vazia agora exibirão um indicador de foco quando receberem o foco.

**Melhoria no suporte à grade de propriedade**

- Os elementos filhos do controle <xref:System.Windows.Forms.PropertyGrid> agora retornam um `true` para a propriedade <xref:System.Windows.Automation.ValuePattern.IsReadOnlyProperty> somente quando um elemento PropertyGrid está habilitado.
- Os elementos filhos do controle <xref:System.Windows.Forms.PropertyGrid> agora retornam um `false` para a propriedade <xref:System.Windows.Automation.AutomationElement.IsEnabledProperty> somente quando um elemento PropertyGrid pode ser alterado pelo usuário.
Para obter uma visão geral de automação da interface do usuário, confira a [Visão geral de automação da interface do usuário](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-overview).</p>**Melhoria na navegação por teclado**

- <xref:System.Windows.Forms.ToolStripButton>agora permite o foco quando contido em um <xref:System.Windows.Forms.ToolStripPanel> que tem a <xref:System.Windows.Forms.ToolStripPanel.TabStop> propriedade definida como `true` .

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Principal       |
| Versão | 4.7.2       |
| Type    | Redirecionando |
