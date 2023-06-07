---
title: Novidades na acessibilidade do .NET Framework
description: Veja o que há de novo na acessibilidade do .NET, começando com o .NET Framework 4.7.1. Os recursos de acessibilidade permitem que um aplicativo forneça a experiência certa para os usuários de tecnologia assistencial.
ms.date: 04/18/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- what's new [.NET Framework]
ms.openlocfilehash: df9188c4f7c2af77f5dc87309880a41724254c5c
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558953"
---
# <a name="whats-new-in-accessibility-in-the-net-framework"></a>Novidades na acessibilidade do .NET Framework

O .NET Framework pretende tornar os aplicativos mais acessíveis para os usuários. As funcionalidades de acessibilidade permitem que um aplicativo forneça uma experiência apropriada para os usuários da Tecnologia Adaptativa. A partir do .NET Framework 4.7.1, o .NET Framework inclui diversas melhorias de acessibilidade que permitem aos desenvolvedores criar aplicativos acessíveis.

## <a name="accessibility-switches"></a>Opções de acessibilidade

Configure seu aplicativo para aceitar os recursos de acessibilidade se ele for direcionado ao .NET Framework 4.7 ou a uma versão anterior, mas estiver em execução no .NET Framework 4.7.1 ou posterior. Além disso, configure seu aplicativo para usar recursos herdados (e não aproveitar os recursos de acessibilidade) se ele for direcionado ao .NET Framework 4.7.1 ou posterior. Cada versão do .NET Framework que inclui recursos de acessibilidade tem um comutador de acessibilidade específico à versão, que você adiciona ao [`<AppContextSwitchOverrides>`](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) elemento na [`<runtime>`](../configure-apps/file-schema/runtime/index.md) seção do arquivo de configuração do aplicativo. Estas são as opções compatíveis:

|Versão|Alternar|
|---|---|
|.NET Framework 4.7.1|"Switch.UseLegacyAccessibilityFeatures"|
|.NET Framework 4.7.2|"Switch.UseLegacyAccessibilityFeatures.2"|
|.NET Framework 4.8|"Switch.UseLegacyAccessibilityFeatures.3"|

### <a name="taking-advantage-of-accessibility-enhancements"></a>Aproveitando as vantagens das melhorias de acessibilidade

As novas funcionalidades de acessibilidade são habilitadas por padrão para os aplicativos direcionados ao .NET Framework 4.7.1 ou posterior. Além disso, os aplicativos direcionados a uma versão anterior do .NET Framework mas que estão em execução no .NET Framework 4.7.1 ou posterior podem recusar comportamentos de acessibilidade herdados (e, portanto, aproveitar as melhorias de acessibilidade) adicionando opções ao [`<AppContextSwitchOverrides>`](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) elemento na [`<runtime>`](../configure-apps/file-schema/runtime/index.md) seção do arquivo de configuração do aplicativo e definindo seu valor como `false` . O seguinte exemplo mostra como aceitar as melhorias de acessibilidade introduzidas no .NET Framework 4.7.1:

```xml
<runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true|false;key2=true|false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false" />
</runtime>
```

Se você optar por aceitar os recursos de acessibilidade em uma versão posterior do .NET Framework, deverá também aceitar explicitamente os recursos de versões anteriores do .NET Framework. Configurar seu aplicativo para aproveitar as melhorias de acessibilidade no .NET Framework 4.7.1 e no 4.7.2 requer o seguinte [`<AppContextSwitchOverrides>`](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) elemento:

```xml
<runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true|false;key2=true|false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false" />
</runtime>
```

Configurar seu aplicativo para aproveitar as melhorias de acessibilidade no .NET Framework 4.7.1, 4.7.2 e 4,8 requer o seguinte [`<AppContextSwitchOverrides>`](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) elemento:

```xml
<runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true|false;key2=true|false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false" />
</runtime>
```

### <a name="restoring-legacy-behavior"></a>Restaurando o comportamento herdado

Os aplicativos que visam versões do .NET Framework a partir do 4.7.1 podem desabilitar os recursos de acessibilidade adicionando opções ao [`<AppContextSwitchOverrides>`](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) elemento na [`<runtime>`](../configure-apps/file-schema/runtime/index.md) seção do arquivo de configuração do aplicativo e definindo seu valor como `true` . Por exemplo, a seguinte configuração recusa os recursos de acessibilidade introduzidos no .NET Framework 4.7.2:

```xml
<runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true|false;key2=true|false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures.2=true" />
</runtime>
```

## <a name="whats-new-in-accessibility-in-net-framework-48"></a>Novidades na acessibilidade do .NET Framework 4.8

O .NET Framework 4.8 inclui novos recursos de acessibilidade nas seguintes áreas:

- [Windows Forms](#winforms48)

- [WPF (Windows Presentation Foundation)](#wpf48)

- [Designer de fluxo de trabalho do WF (Windows Workflow Foundation)](#wf48)

<a name="winforms48"></a>

### <a name="windows-forms"></a>Windows Forms

No .NET Framework 4.8, o Windows Forms adiciona suporte ao LiveRegions e Eventos de Notificação para muitos controles usados. Ele também adiciona suporte para Dicas de Ferramentas quando um usuário navega para um controle usando o teclado.

**Suporte para UIA LiveRegions em rótulos e StatusStrips**

UIA LiveRegions permitem que os desenvolvedores de aplicativos notifiquem os leitores de tela de uma alteração de texto em um controle que está localizado longe do local em que o usuário está trabalhando. Isso é útil, por exemplo, para um controle <xref:System.Windows.Forms.StatusStrip> que mostra um status de conexão. Se a conexão cair e o status mudar, o desenvolvedor deverá notificar o leitor de tela.

A partir do .NET Framework 4.8, o Windows Forms implementa UIA LiveRegions nos controles <xref:System.Windows.Forms.Label> e <xref:System.Windows.Forms.StatusStrip>. Por exemplo, o seguinte código usa o para LiveRegion em um controle <xref:System.Windows.Forms.Label> chamado `label1`:

```csharp
public Form1()
{
   InitializeComponent();
   label1.AutomationLiveSetting = AutomationLiveSetting.Polite;
}

…
Label1.Text = “Ready!”;
```

O narrador anuncia “Pronto” independentemente de onde o usuário esteja interagindo com o aplicativo.

Você também pode implementar seu <xref:System.Windows.Forms.UserControl> como uma LiveRegion:

```csharp
using System;
using System.Windows.Forms;
using System.Windows.Forms.Automation;

namespace WindowsFormsApplication
{
   public partial class UserControl1 : UserControl, IAutomationLiveRegion
   {
      public UserControl1()
      {
         InitializeComponent();
      }

      public AutomationLiveSetting AutomationLiveSetting { get; set; }
      private AutomationLiveSetting IAutomationLiveRegion.GetLiveSetting()
      {
         return this.AutomationLiveSetting;
      }

      protected override void OnTextChanged(EventArgs e)
      {
         base.OnTextChanged(e);
         AutomationNotifications.UiaRaiseLiveRegionChangedEvent(this.AccessibilityObject);
      }
   }
}
```

**Eventos de notificação de UIA**

O evento de notificação de UIA, apresentado no Windows 10 Fall Creators Update, permite que seu aplicativo gere um evento UIA, que leva o Narrador a simplesmente fazer um anúncio com base no texto que você forneceu com o evento, sem a necessidade de ter um controle correspondente na interface do usuário. Em alguns cenários, isso é uma maneira direta de melhorar consideravelmente a acessibilidade de seu aplicativo. Também pode ser útil para notificar sobre o progresso de algum processo que pode levar muito tempo. Para obter mais informações sobre eventos de Notificação de UIA, consulte [Seu aplicativo da área de trabalho pode utilizar o novo evento de Notificação de UIA?](https://docs.microsoft.com/archive/blogs/winuiautomation/can-your-desktop-app-leverage-the-new-uia-notification-event-in-order-to-have-narrator-say-exactly-what-your-customers-need).

O exemplo a seguir gera o [Evento de notificação](xref:System.Windows.Forms.AccessibleObject.RaiseAutomationNotification%2A):

```csharp
MethodInfo raiseMethod = typeof(AccessibleObject).GetMethod("RaiseAutomationNotification");
if (raiseMethod != null) {
   raiseMethod.Invoke(progressBar1.AccessibilityObject, new object[3] {/*Other*/ 4, /*All*/ 2, "The progress is 50%." });
}
```

**Dicas de ferramentas sobre o acesso ao teclado**

Em aplicativos destinados ao .NET Framework 4.7.2 e versões anteriores, uma [dica de ferramenta](xref:System.Windows.Forms.ToolTip) de controle só pode ser acionada para exibição ao mover o ponteiro do mouse no controle. A partir do .NET Framework 4.8, um usuário de teclado pode disparar uma dica de ferramenta de controle, concentrando-se no controle usando uma tecla Tab ou teclas de seta, com ou sem teclas modificadoras. Essa melhoria de acessibilidade específica exige uma outra [opção de AppContext](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md):

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
   <startup>
      <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.6.1"/>
   </startup>
   <runtime>
      <!-- AppContextSwitchOverrides values are in the form of 'key1=true|false;key2=true|false  -->
      <!-- Please note that disabling Switch.UseLegacyAccessibilityFeatures, Switch.UseLegacyAccessibilityFeatures.2 and Switch.UseLegacyAccessibilityFeatures.3 is required to disable Switch.System.Windows.Forms.UseLegacyToolTipDisplay -->
      <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false;Switch.System.Windows.Forms.UseLegacyToolTipDisplay=false"/>
   </runtime>
</configuration>
```

A figura a seguir mostrará a dica de ferramenta quando o usuário selecionar um botão com o teclado.

![Captura de tela da dica de ferramenta quando o usuário navega para o botão com o teclado.](./media/whats-new-in-accessibility/select-tooltip-with-keyboard.png)

<a name="wpf48"></a>

### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

A partir do .NET Framework 4.8, o WPF inclui vários aprimoramentos de acessibilidade.

**Narradores de tela não anunciam mais elementos com visibilidade recolhida ou oculta**

Elementos com visibilidade recolhida ou oculta não são mais anunciados pelo leitor de tela. As interfaces do usuário que contêm elementos com uma Visibilidade de <xref:System.Windows.Visibility.Collapsed?displayProperty=nameWithType> ou <xref:System.Windows.Visibility.Hidden?displayProperty=nameWithType> poderão ser adulteradas pelos leitores de tela se forem apresentadas ao usuário. A partir do .NET Framework 4.8, o WPF não inclui mais elementos recolhidos ou ocultos na Exibição de Controle da árvore do UIAutomation. Portanto, os leitores de tela não podem anunciar esses elementos.

**Propriedade SelectionTextBrush para uso com a seleção de texto não baseada em Adorno**

No .NET Framework 4.7.2, o WPF adicionou a capacidade de desenhar <xref:System.Windows.Controls.TextBox> e a seleção de texto <xref:System.Windows.Controls.PasswordBox> sem usar a camada de Adorno. A cor de primeiro plano do texto selecionado nesse cenário foi determinada pelo <xref:System.Windows.SystemColors.HighlightTextBrush?displayProperty=nameWithType>.

O .NET framework 4.8 adiciona uma nova propriedade, `SelectionTextBrush`, que permite que os desenvolvedores selecionem o pincel específico para o texto selecionado quando usar a seleção de texto não baseada em Adorno. Esta propriedade funciona apenas nos controles derivados de <xref:System.Windows.Controls.Primitives.TextBoxBase> e o controle <xref:System.Windows.Controls.PasswordBox> em aplicativos WPF com seleção de texto não baseada em Adorno habilitada. Ela não funciona no controle <xref:System.Windows.Controls.RichTextBox>. Se a seleção de texto não baseada em Adorno não estiver habilitada, essa propriedade será ignorada.

Para usar essa propriedade, basta adicioná-la ao seu código XAML e usar a associação ou o pincel adequado. A seleção de texto resultante tem esta aparência:

![Captura de tela do aplicativo em execução com as palavras Olá, Mundo selecionadas.](./media/whats-new-in-accessibility/selectiontextbrush-property.png)

Você pode combinar o uso das propriedades `SelectionBrush` e `SelectionTextBrush` para gerar qualquer combinação de plano de fundo e cor de primeiro plano que julgar adequada.

**Suporte para a propriedade ControllerFor do UIAutomation**

A propriedade `ControllerFor` do UIAutomation retorna uma matriz de elementos de automação manipulados pelo elemento de automação que dá suporte a essa propriedade. Essa propriedade normalmente é usada para acessibilidade de sugestão automática. `ControllerFor` é usado quando um elemento de automação afeta um ou mais segmentos de interface do usuário do aplicativo ou da área de trabalho. Caso contrário, é difícil associar o impacto da operação de controle com elementos de interface do usuário. Este recurso adiciona a capacidade de controles para fornecer um valor para a propriedade `ControllerFor`.

O .NET framework 4.8 adiciona um novo método virtual, o <xref:System.Windows.Automation.Peers.AutomationPeer.GetControlledPeersCore?displayProperty=nameWithType?displayProperty=nameWithType>. Para fornecer um valor para a propriedade `ControllerFor`, basta substituir este método e retornar um `List<AutomationPeer>` para os controles manipulados por este <xref:System.Windows.Automation.Peers.AutomationPeer>:

```csharp
public class AutoSuggestTextBox: TextBox
{
   protected override AutomationPeer OnCreateAutomationPeer()
   {
      return new AutoSuggestTextBoxAutomationPeer(this);
   }

   public ListBox SuggestionListBox;
}

internal class AutoSuggestTextBoxAutomationPeer : TextBoxAutomationPeer
{
   public AutoSuggestTextBoxAutomationPeer(AutoSuggestTextBox owner) : base(owner)
   {
   }

   protected override List<AutomationPeer> GetControlledPeersCore()
   {
      List<AutomationPeer> controlledPeers = new List<AutomationPeer>();
      AutoSuggestTextBox owner = Owner as AutoSuggestTextBox;
      controlledPeers.Add(UIElementAutomationPeer.CreatePeerForElement(owner.SuggestionListBox));
      return controlledPeers;
   }
}
```

**Dicas de ferramenta sobre acesso ao teclado**

No .NET Framework 4.7.2 e versões anteriores, as dicas de ferramentas são exibidas somente quando o usuário passar o cursor do mouse sobre um controle. No .NET Framework 4.8, as dicas de ferramentas também são exibidas no foco do teclado, bem como por meio de um atalho de teclado.

Para habilitar esse recurso, um aplicativo precisa direcionar o .NET Framework 4.8 ou aceitar ao usar as opções `Switch.UseLegacyAccessibilityFeatures.3` e `Switch.UseLegacyToolTipDisplay` [AppContext](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md). Veja a seguir um arquivo de exemplo de configuração de aplicativo:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
   <startup>
      <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
   </startup>
   <runtime>
      <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false;Switch.UseLegacyToolTipDisplay=false" />
   </runtime>
</configuration>
```

Depois de habilitados, todos os controles que contêm uma dica de ferramenta vão exibi-la depois que o controle receber o foco do teclado. A dica de ferramenta pode ser ignorada com o tempo ou quando o foco do teclado mudar. Os usuários também podem ignorar a dica de ferramenta manualmente usando um novo atalho de teclado, Ctrl + Shift + F10. Depois que a dica de ferramenta for ignorada, ela poderá ser exibida novamente usando o mesmo atalho de teclado.

> [!NOTE]
> As [dicas de ferramentas da faixa de opções](xref:System.Windows.Controls.Ribbon.RibbonToolTip) nos controles <xref:System.Windows.Controls.Ribbon.Ribbon> não serão exibidas no foco do teclado. São exibidas somente por meio do atalho de teclado.

**Adicionado suporte para propriedades SizeOfSet e PositionInSet do UIAutomation**

O Windows 10 introduziu duas novas propriedades do UIAutomation, `SizeOfSet` e `PositionInSet`, que são usadas por aplicativos para descrever a contagem de itens em um conjunto. Aplicativos de cliente do UIAutomation, como leitores de tela podem, então, consultar um aplicativo para essas propriedades e anunciar uma representação precisa da interface de usuário do aplicativo.

A partir do .NET Framework 4.8, o WPF expõe essas duas propriedades para o UIAutomation em aplicativos do WPF. Isso pode ser feito de duas maneiras:

- Usando propriedades de dependência.

  O WPF adiciona duas novas propriedades de dependência, <xref:System.Windows.Automation.AutomationProperties.SizeOfSet?displayProperty=nameWithType> e <xref:System.Windows.Automation.AutomationProperties.PositionInSet?displayProperty=nameWithType>. Um desenvolvedor pode usar o XAML para definir seus valores:

  ```xaml
  <Button AutomationProperties.SizeOfSet="3"
    AutomationProperties.PositionInSet="1">Button 1</Button>

  <Button AutomationProperties.SizeOfSet="3"
    AutomationProperties.PositionInSet="2">Button 2</Button>

  <Button AutomationProperties.SizeOfSet="3"
    AutomationProperties.PositionInSet="3">Button 3</Button>
  ```

- Ao substituir métodos virtuais do AutomationPeer.

  Os métodos virtuais <xref:System.Windows.Automation.Peers.AutomationPeer.GetSizeOfSetCore> e <xref:System.Windows.Automation.Peers.AutomationPeer.GetPositionInSetCore> foram adicionados à classe AutomationPeer. Um desenvolvedor pode fornecer valores para `SizeOfSet` e `PositionInSet` por meio da substituição desses métodos, conforme mostrado no exemplo a seguir:

  ```csharp
  public class MyButtonAutomationPeer : ButtonAutomationPeer
  {
    protected override int GetSizeOfSetCore()
    {
        // Call into your own logic to provide a value for SizeOfSet
        return CalculateSizeOfSet();
    }

    protected override int GetPositionInSetCore()
    {
        // Call into your own logic to provide a value for PositionInSet
        return CalculatePositionInSet();
    }
  }
  ```

Além disso, os itens nas instâncias <xref:System.Windows.Controls.ItemsControl> fornecem um valor para essas propriedades automaticamente sem ação adicional do desenvolvedor. Se um <xref:System.Windows.Controls.ItemsControl> for agrupado, a coleção de grupos será representada como um conjunto e cada grupo será contado como um conjunto separado, com cada item dentro desse grupo fornecendo sua posição dentro desse grupo, bem como o tamanho do grupo. Valores automáticos não são afetados pela virtualização. Mesmo que um item não seja percebido, ele ainda é contado para o tamanho total do conjunto e afeta a posição no conjunto de seus itens irmãos.

Valores automáticos serão fornecidos apenas se o aplicativo for destinado ao .NET Framework 4.8. Para aplicativos que se destinam a uma versão anterior do .NET Framework, você pode definir a `Switch.UseLegacyAccessibilityFeatures.3` [opção AppContext](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md), conforme mostrado no seguinte arquivo App.config:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
   <startup>
      <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
   </startup>
   <runtime>
      <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false" />
   </runtime>
</configuration>
```

<a name="wf48"></a>

### <a name="windows-workflow-foundation-wf-workflow-designer"></a>Designer de fluxo de trabalho do WF (Windows Workflow Foundation)

O designer de fluxo de trabalho inclui as seguintes alterações no .NET Framework 4.8:

- Os usuários que utilizam o Narrador verão melhorias em rótulos de caso do FlowSwitch.

- Os usuários que utilizam o Narrador verão melhorias em descrições de botão.

- Usuários que escolherem temas de Alto Contraste verão melhorias na visibilidade do Designer de Fluxo de Trabalho e em seus controles, como melhores taxas de contraste entre elementos e caixas de seleção mais perceptíveis usadas para elementos de foco.

Se o seu aplicativo for destinado ao .NET Framework 4.7.2 ou uma versão anterior, você poderá aceitar essas alterações, definindo a `Switch.UseLegacyAccessibilityFeatures.3` [opção de AppContext](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) para `false` no seu arquivo de configuração de aplicativo. Para obter mais informações, consulte a seção [Aproveitando os aprimoramentos de acessibilidade](#taking-advantage-of-accessibility-enhancements) neste artigo.

## <a name="whats-new-in-accessibility-in-net-framework-472"></a>Novidades na acessibilidade do .NET Framework 4.7.2

O .NET Framework 4.7.2 inclui novos recursos de acessibilidade nas seguintes áreas:

- [Windows Forms](#winforms472)

- [WPF (Windows Presentation Foundation)](#wpf472)

<a name="winforms472"></a>

### <a name="windows-forms"></a>Windows Forms

**Cores definidas pelo sistema operacional em temas de Alto Contraste**

Começando no .NET Framework 4.7.2, o Windows Forms usa cores definidas pelo sistema operacional em temas de Alto Contraste. Isso afeta os seguintes controles:

- A seta suspensa do controle <xref:System.Windows.Forms.ToolStripDropDownButton>.

- Os controles <xref:System.Windows.Forms.Button>, <xref:System.Windows.Forms.RadioButton> e <xref:System.Windows.Forms.CheckBox> com <xref:System.Windows.Forms.ButtonBase.FlatStyle> definido como <xref:System.Windows.Forms.FlatStyle.Flat?displayProperty=nameWithType> ou <xref:System.Windows.Forms.FlatStyle.Popup?displayProperty=nameWithType>. Anteriormente, as cores de texto e da tela de fundo selecionadas não eram contrastantes e eram difíceis de serem lidas.

- Os controles contidos em uma <xref:System.Windows.Forms.GroupBox> que tem sua propriedade <xref:System.Windows.Forms.Control.Enabled> definida como `false`.

- Os controles <xref:System.Windows.Forms.ToolStripButton>, <xref:System.Windows.Forms.ToolStripComboBox> e <xref:System.Windows.Forms.ToolStripDropDownButton>, que têm uma taxa maior de contraste de luminosidade no Modo de Alto Contraste.

- A propriedade <xref:System.Windows.Forms.DataGridViewLinkCell.LinkColor> do <xref:System.Windows.Forms.DataGridViewLinkCell>.

**Melhorias do Narrador**

A partir do .NET Framework 4.7.2, o suporte ao Narrador é aprimorado da seguinte maneira:

- Ele anuncia o valor da propriedade <xref:System.Windows.Forms.ToolStripMenuItem.ShortcutKeys?displayProperty=nameWithType> quando anuncia o texto de um <xref:System.Windows.Forms.ToolStripMenuItem>.

- Ele indica quando um <xref:System.Windows.Forms.ToolStripMenuItem> tem sua propriedade <xref:System.Windows.Forms.Control.Enabled> definida como `false`.

- Ele fornece comentários sobre o estado de uma caixa de seleção quando a propriedade <xref:System.Windows.Forms.ListView.CheckBoxes?displayProperty=nameWithType> está definida como `true`.

- A ordem de foco do Modo de Verificação do Narrador é consistente com a ordem visual dos controles na janela de diálogo de download do ClickOnce.

**Melhorias de DataGridView**

A partir do .NET Framework 4.7.2, o controle <xref:System.Windows.Forms.DataGridView> introduziu as seguintes melhorias de acessibilidade:

- As linhas podem ser classificadas com o teclado. Um usuário pode usar a tecla F3 para classificar pela coluna atual.

- Quando o <xref:System.Windows.Forms.DataGridView.SelectionMode?displayProperty=nameWithType> é definido como <xref:System.Windows.Forms.DataGridViewSelectionMode.FullRowSelect?displayProperty=nameWithType>, o cabeçalho da coluna muda de cor para indicar a coluna atual, à medida que o usuário pressiona Tab para percorrer as células na linha atual.

- A propriedade <xref:System.Windows.Forms.AccessibleObject.Parent?displayProperty=nameWithType> de um <xref:System.Windows.Forms.DataGridViewLinkCell.DataGridViewLinkCellAccessibleObject?displayProperty=nameWithType> retorna o controle pai correto.

**Indicações visuais aprimoradas**

- Os controles <xref:System.Windows.Forms.RadioButton> e <xref:System.Windows.Forms.CheckBox> com uma propriedade <xref:System.Windows.Forms.ButtonBase.Text> vazia exibem um indicador de foco quando recebem o foco.

**Melhoria no suporte à grade de propriedade**

- Os elementos filhos do controle <xref:System.Windows.Forms.PropertyGrid> agora retornam um `true` para a propriedade <xref:System.Windows.Automation.ValuePattern.IsReadOnlyProperty> somente quando um elemento PropertyGrid está habilitado.

- Os elementos filho do controle <xref:System.Windows.Forms.PropertyGrid> retornam um `false` para a propriedade <xref:System.Windows.Automation.AutomationElement.IsEnabledProperty> somente quando um elemento PropertyGrid pode ser alterado pelo usuário.

**Melhoria na navegação por teclado**

- O controle <xref:System.Windows.Forms.ToolStripButton> permite o foco quando contido em um <xref:System.Windows.Forms.ToolStripPanel> que tem a propriedade <xref:System.Windows.Forms.ToolStripPanel.TabStop> definida como `true`

<a name="wpf472"></a>

### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

**Alterações nos controles CheckBox e RadioButton**

No .NET Framework 4.7.1 e nas versões anteriores, os controles <xref:System.Windows.Controls.CheckBox?displayProperty=nameWIthType> e <xref:System.Windows.Controls.RadioButton?displayProperty=nameWIthType> do WPF têm visuais de foco inconsistentes e, nos temas Clássico e de Alto Contraste, têm visuais de foco incorretos.  Esses problemas ocorrem quando os controles não têm nenhum conjunto de conteúdo.  Isso pode dificultar a transição entre os temas e deixar o visual de foco confuso.

No .NET Framework 4.7.2, agora esses visuais são mais consistentes entre os temas e mais facilmente visíveis nos temas Clássico e de Alto Contraste.

**Controles do WinForms hospedados em um aplicativo WPF**

Para controles do WinForms hospedados em um aplicativo do WPF no .NET Framework 4.7.1 e versões anteriores, os usuários não poderão sair da guia da camada do WinForms se o primeiro ou o último controle da camada for o controle <xref:System.Windows.Forms.Integration.ElementHost> do WPF. No .NET Framework 4.7.2, os usuários agora podem sair da guia da camada do WinForms.

No entanto, os aplicativos automatizados que se baseiam em foco e que nunca são escapados da camada do WinForms podem deixar de funcionar conforme o esperado.

## <a name="whats-new-in-accessibility-in-net-framework-471"></a>Novidades na acessibilidade do .NET Framework 4.7.1

O .NET Framework 4.7.1 inclui novos recursos de acessibilidade nas seguintes áreas:

- [WPF (Windows Presentation Foundation)](#wpf471)

- [Windows Forms](#winforms471)

- [Controles da Web do ASP.NET](#aspnet471)

- [Ferramentas do SDK do .NET](#tools471)

- [Windows Workflow Foundation (WF) Designer de Fluxo de Trabalho](#wf471)

<a name="wpf471"></a>

### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

**Melhorias do leitor de tela**

Se as melhorias de acessibilidade estiverem habilitadas, o .NET Framework 4.7.1 incluirá as seguintes melhorias que afetam os leitores de tela:

- No .NET Framework 4.7 e em versões anteriores, os controles <xref:System.Windows.Controls.Expander> foram divulgados pelos leitores de tela como botões. A partir do .NET Framework 4.7.1, eles são corretamente divulgados como grupos expansíveis/recolhíveis.

- No .NET Framework 4.7 e em versões anteriores, os controles <xref:System.Windows.Controls.DataGridCell> foram divulgados pelos leitores de tela como “personalizados”. A partir do .NET Framework 4.7.1, agora eles são corretamente divulgados como célula de grade de dados (localizada).

- A partir do .NET Framework 4.7.1, os leitores de tela divulgam o nome de uma <xref:System.Windows.Controls.ComboBox> editável.

- No .NET Framework 4.7 e em versões anteriores, os controles <xref:System.Windows.Controls.PasswordBox> eram divulgados como “nenhum item em exibição” ou, de outro modo, apresentavam um comportamento incorreto. Esse problema é corrigido a partir do .NET Framework 4.7.1.

**Suporte a UIAutomation LiveRegion**

Leitores de tela, como o Narrador, ajudam as pessoas a ler o conteúdo da interface do usuário de um aplicativo, normalmente, pela saída de conversão de texto em fala do conteúdo da interface do usuário que tem o foco. No entanto, se um elemento da interface do usuário for alterado e não tiver o foco, o usuário poderá não ser notificado e perder informações importantes. As regiões dinâmicas pretendem resolver esse problema. Um desenvolvedor pode usá-las para informar o leitor de tela ou qualquer outro cliente de UIAutomation de que foi feita uma alteração importante em um elemento da interface do usuário. Em seguida, o leitor de tela pode decidir como e quando informar o usuário dessa alteração.

Para dar suporte a regiões dinâmicas, as seguintes APIs foram adicionadas ao WPF:

- Os campos <xref:System.Windows.Automation.AutomationElementIdentifiers.LiveSettingProperty?displayProperty=nameWithType> e <xref:System.Windows.Automation.AutomationElementIdentifiers.LiveRegionChangedEvent?displayProperty=nameWithType>, que identificam a propriedade **LiveSetting** e o evento **LiveRegionChanged**. Eles podem ser definidos com XAML.

- A propriedade anexada **AutomationProperties.LiveSetting**, que informa o leitor de tela da importância da alteração da interface do usuário para o usuário.

- A propriedade <xref:System.Windows.Automation.AutomationProperties.LiveSettingProperty?displayProperty=nameWithType>, que identifica a propriedade anexada **AutomationProperties.LiveSetting**.

- O método <xref:System.Windows.Automation.Peers.AutomationPeer.GetLiveSettingCore%2A?displayProperty=nameWithType>, que pode ser substituído para fornecer um valor de **LiveSetting**.

- Os métodos <xref:System.Windows.Automation.AutomationProperties.GetLiveSetting%2A?displayProperty=nameWithType> e <xref:System.Windows.Automation.AutomationProperties.SetLiveSetting%2A?displayProperty=nameWithType>, que obtêm e definem um valor de **LiveSetting**.

- A enumeração <xref:System.Windows.Automation.AutomationLiveSetting?displayProperty=nameWithType>, que define os seguintes possíveis valores de **LiveSetting**:

  - <xref:System.Windows.Automation.AutomationLiveSetting.Off?displayProperty=nameWithType>. O elemento não envia notificações se o conteúdo da região dinâmica foi alterado.
  - <xref:System.Windows.Automation.AutomationLiveSetting.Polite?displayProperty=nameWithType>. O elemento enviará notificações não interruptivas se o conteúdo da região dinâmica tiver sido alterado.

  - <xref:System.Windows.Automation.AutomationLiveSetting.Assertive?displayProperty=nameWithType>. O elemento enviará notificações interruptivas se o conteúdo da região dinâmica tiver sido alterado.

Crie uma LiveRegion configurar a propriedade **AutomationProperties.LiveSetting** no elemento de interesse, conforme mostrado no seguinte exemplo:

```xaml
<TextBlock Name="myTextBlock" AutomationProperties.LiveSetting="Assertive">announcement</TextBlock>
```

Quando os dados da região dinâmica forem alterados e você precisar informar um leitor de tela, acione um evento explicitamente, conforme mostrado na amostra a seguir.

```csharp
var peer = FrameworkElementAutomationPeer.FromElement(myTextBlock);

peer.RaiseAutomationEvent(AutomationEvents.LiveRegionChanged);
```

```vb
Dim peer = FrameworkElementAutomationPeer.FromElement(myTextBlock)
peer.RaiseAutomationEvent(AutomationEvents.LiveRegionChanged)

```

**Alto contraste**

A partir do .NET Framework 4.7.1, foram feitas melhorias de alto contraste em vários controles do WPF. Agora eles estão visíveis quando o tema <xref:System.Windows.SystemParameters.HighContrast%2A> é definido. Eles incluem:

- Controle <xref:System.Windows.Controls.Expander>

  O visual de foco do controle <xref:System.Windows.Controls.Expander> agora está visível. Os visuais de teclado dos controles <xref:System.Windows.Controls.ComboBox>, <xref:System.Windows.Controls.ListBox> e <xref:System.Windows.Controls.RadioButton> também estão visíveis. Por exemplo:

  Antes:

  ![Captura de tela do controle Expander com foco e nenhum visual de foco.](./media/whats-new-in-accessibility/expander-control-before.png)

  Após:

  ![Captura de tela do controle Expander com foco mostrando uma linha pontilhada ao lado do texto do controle.](./media/whats-new-in-accessibility/expander-control-after.png)

- Controles <xref:System.Windows.Controls.CheckBox> e <xref:System.Windows.Controls.RadioButton>

  O texto nos controles <xref:System.Windows.Controls.CheckBox> e <xref:System.Windows.Controls.RadioButton> agora é de visualização mais fácil quando selecionado em temas de alto contraste. Por exemplo:

  Antes:

  ![Captura de tela dos botões de rádio e de verificação com visibilidade de texto ruim em temas de alto contraste.](./media/whats-new-in-accessibility/high-contrast-radio-button-before.png)

  Após:

  ![Captura de tela dos botões de rádio e de verificação com melhor visibilidade de texto em temas de alto contraste.](./media/whats-new-in-accessibility/high-contrast-radio-button-after.png)

- Controle <xref:System.Windows.Controls.ComboBox>

  A partir do .NET Framework 4.7.1, a borda de um controle <xref:System.Windows.Controls.ComboBox> desabilitado é da mesma cor do texto desabilitado. Por exemplo:

  Antes:

  ![Captura de tela de uma ComboBox desabilitada com borda e texto de controle em cores diferentes.](./media/whats-new-in-accessibility/combo-disabled-before.png)

  Após:

  ![Captura de tela de uma ComboBox desabilitada com borda da mesma cor que o texto de controle.](./media/whats-new-in-accessibility/combo-disabled-after.png)

  Além disso, os botões desabilitados e com foco usam a cor de tema correta.

  Antes:

  ![Captura de tela de um botão preto com texto cinza dizendo me concentrar.](./media/whats-new-in-accessibility/button-theme-colors-before.png)

  Após:

  ![Captura de tela de um botão azul com texto preto dizendo me me concentrar.](./media/whats-new-in-accessibility/button-theme-colors-after.png)

  Por fim, no .NET Framework 4.7 e em versões anteriores, a configuração do estilo de um controle <xref:System.Windows.Controls.ComboBox> como `Toolbar.ComboBoxStyleKey` fazia com que a seta suspensa ficasse invisível. Esse problema é corrigido a partir do .NET Framework 4.7.1. Por exemplo:

  Antes:

  ![Captura de tela de um controle ComboBox com uma seta suspensa invisível.](./media/whats-new-in-accessibility/combo-box-style-key-before.png)

  Após:

  ![Captura de tela de um controle combinação exibindo a seta suspensa.](./media/whats-new-in-accessibility/combo-box-style-key-after.png)

- Controle <xref:System.Windows.Controls.DataGrid>

  A partir do .NET Framework 4.7.1, a seta do indicador de classificação nos controles <xref:System.Windows.Controls.DataGrid> agora usa as cores de tema corretas. Por exemplo:

  Antes:

  ![Captura de tela da seta do indicador de classificação antes dos aprimoramentos.](./media/whats-new-in-accessibility/sort-indicator-before.png)

  Após:

  ![Captura de tela da seta do indicador de classificação após os aprimoramentos.](./media/whats-new-in-accessibility/sort-indicator-after.png)

  Além disso, no .NET Framework 4.7 e em versões anteriores, o estilo de link padrão era alterado para uma cor incorreta ao passar o mouse nos modos de alto contraste. Isso é resolvido a partir do .NET Framework 4.7.1. Da mesma forma, as colunas da caixa de seleção <xref:System.Windows.Controls.DataGrid> usam as cores esperadas para os comentários do foco de teclado a partir do .NET Framework 4.7.1.

  Antes:

  ![Captura de tela de um link dizendo clique em mim! em vermelho.](./media/whats-new-in-accessibility/default-link-style-before.png)

  Após:

  ![Captura de tela de um link dizendo clique em mim! em amarelo.](./media/whats-new-in-accessibility/default-link-style-after.png)

Para obter mais informações sobre as melhorias de acessibilidade do WPF no .NET Framework 4.7.1, consulte [Melhorias de acessibilidade no WPF](../migration-guide/retargeting/4.7-4.7.1.md#accessibility-improvements-in-wpf).

<a name="winforms471"></a>

### <a name="windows-forms-accessibility-improvements"></a>Melhorias de acessibilidade do Windows Forms

No .NET Framework 4.7.1, o WinForms (Windows Forms) inclui alterações de acessibilidade nas áreas a seguir.

**Vídeo aprimorado no modo de Alto Contraste**

Do .NET Framework 4.7.1 em diante, vários controles do WinForms oferecem uma melhor renderização nos modos de HighContrast disponíveis no sistema operacional. O Windows 10 alterou os valores de algumas cores do sistema de alto contraste e o Windows Forms é baseado na estrutura Win32 do Windows 10. Para obter a melhor experiência, execute a última versão do Windows e aceite as últimas alterações do sistema operacional adicionando um arquivo app.manifest a um aplicativo de teste e remova a marca de comentário da linha de sistema operacional com suporte do Windows 10, de modo que ela fique parecida com a seguinte:

```xml
<!-- Windows 10 -->
<supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
```

Alguns exemplos de alterações de alto contraste incluem:

- Marcas de seleção em itens <xref:System.Windows.Forms.MenuStrip> são mais fáceis de serem exibidas.

- Quando selecionados, os itens <xref:System.Windows.Forms.MenuStrip> desabilitados são mais fáceis de serem exibidos.

- O texto em um controle <xref:System.Windows.Forms.Button> selecionado contrasta com a cor da seleção.

- O texto desabilitado é mais fácil de ser lido. Por exemplo:

  Antes:

  ![Captura de tela de um aplicativo que usa controles diferentes em execução no modo de alto contraste antes dos aprimoramentos de acessibilidade.](./media/whats-new-in-accessibility/high-contrast-mode-menu-items-before.png)

  Após:

  ![Captura de tela de um aplicativo que usa controles diferentes em execução no modo de alto contraste após melhorias de acessibilidade.](./media/whats-new-in-accessibility/high-contrast-mode-menu-items-after.png)

- Melhorias de alto contraste na caixa de diálogo Exceção de Thread.

**Melhor suporte ao Narrador**

O Windows Forms no .NET Framework 4.7.1 inclui as seguintes melhorias de acessibilidade para o Narrador:

- O controle <xref:System.Windows.Forms.MonthCalendar> pode ser acessado pelo Narrador, bem como por outras ferramentas de automação da interface do usuário.

- O controle <xref:System.Windows.Forms.CheckedListBox> notifica o Narrador quando o estado de seleção do item foi alterado, para que o usuário seja notificado de que ele alterou o valor de um item de lista.

- O controle <xref:System.Windows.Forms.DataGridViewCell> relata o status somente leitura correto para o Narrador.

- O Narrador agora pode ler o texto <xref:System.Windows.Forms.ToolStripMenuItem> desabilitado, enquanto, anteriormente, ele ignorava itens de menu desabilitados.

**Suporte aprimorado para padrões de acessibilidade de UIAutomation**

A partir do .NET Framework 4.7.1, os desenvolvedores de ferramentas de tecnologia de acessibilidade podem utilizar padrões comuns de acessibilidade de API e as propriedades de vários controles WinForms. Essas melhorias de acessibilidade incluem:

- O <xref:System.Windows.Forms.ComboBox> e <xref:System.Windows.Forms.ToolStripSplitButton> agora dão suporte ao [padrão expandir/recolher](../ui-automation/implementing-the-ui-automation-expandcollapse-control-pattern.md).

- O <xref:System.Windows.Forms.DataGridViewCheckBoxCell> agora dá suporte ao [padrão alternar](../ui-automation/implementing-the-ui-automation-toggle-control-pattern.md).

- O controle <xref:System.Windows.Forms.ToolStripItem> dá suporte à propriedade <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.Name> e ao [padrão expandir/recolher](../ui-automation/implementing-the-ui-automation-expandcollapse-control-pattern.md).

- Os controles <xref:System.Windows.Forms.NumericUpDown> e <xref:System.Windows.Forms.DomainUpDown> dão suporte à propriedade <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.Name>.

**Melhor experiência do navegador de propriedades**

A partir do .NET Framework 4.7.1, o Windows Forms inclui:

- Melhor navegação de teclado por meio das várias janelas de seleção suspensa.
- Uma redução de paradas de tabulação desnecessárias.
- Melhor relatório de tipos de controle.
- Melhor comportamento do Narrador.

<a name="aspnet471"></a>

### <a name="aspnet-web-controls"></a>Controles da Web do ASP.NET

A partir do .NET Framework 4.7.1 e do Visual Studio 2017 versão 15,3, o ASP.NET melhora a forma como os controles da Web do ASP.NET funcionam com a tecnologia de acessibilidade no Visual Studio. As alterações incluem as seguintes:

- Alterações para implementar padrões de acessibilidade de interface do usuário ausentes em controles, como a caixa de diálogo **Adicionar campo** no assistente de **exibição de detalhes** ou a caixa de diálogo **Configurar ListView** do assistente de **ListView** .

- Alterações para melhorar a exibição no modo de Alto Contraste, como o **Editor de campos de pager de dados**.

- Alterações para melhorar as experiências de navegação do teclado para controles, como a caixa de diálogo **campos** no assistente **editar campos de pager** do controle DataPager, a caixa de diálogo **Configurar ObjectContext** ou a caixa de diálogo **Configurar seleção de dados** do assistente para **Configurar fonte de dados** .

<a name="tools471"></a>

### <a name="net-sdk-tools"></a>Ferramentas do SDK do .NET

A [Ferramenta Configuration Editor (SvcConfigEditor.exe)](../wcf/configuration-editor-tool-svcconfigeditor-exe.md) e a [Ferramenta Service Trace Viewer (SvcTraceViewer.exe)](../wcf/service-trace-viewer-tool-svctraceviewer-exe.md) foram aprimoradas com a correção de problemas variados de acessibilidade. A maioria eram problemas pequenos, como um nome que não está definido ou certos padrões de automação de interface do usuário implementados incorretamente. Embora muitos usuários não estejam cientes desses valores incorretos, os clientes que usam tecnologias adaptativas como leitores de tela considerarão essas ferramentas de SDK mais acessíveis.

Essas melhorias alteram alguns comportamentos anteriores, como a ordem de foco do teclado.

<a name="wf471"></a>

### <a name="windows-workflow-foundation-wf-workflow-designer"></a>Designer de Fluxo de Trabalho do WF (Windows Workflow Foundation)

As alterações de acessibilidade no Designer de Fluxo de Trabalho incluem as seguintes:

- A ordem de tabulação é alterada para a esquerda para a direita e de cima para baixo em alguns controles:

  - A janela de inicialização de correlação para definir dados de correlação para a atividade <xref:System.ServiceModel.Activities.InitializeCorrelation>.

  - A janela de definição de conteúdo para as atividades <xref:System.ServiceModel.Activities.Receive>, <xref:System.ServiceModel.Activities.Send>, <xref:System.ServiceModel.Activities.SendReply> e <xref:System.ServiceModel.Activities.ReceiveReply>.

- Mais funções estão disponíveis por meio do teclado:

  - Ao editar as propriedades de uma atividade, grupos de propriedades podem ser recolhidos pelo teclado na primeira vez em que são colocados em foco.

  - Ícones de aviso podem ser acessados por teclado.

  - O botão **Mais Propriedades** na janela **Propriedades** pode ser acessado pelo teclado.

  - Usuários do teclado podem acessar os itens de cabeçalho nos painéis **Argumentos** e **Variáveis** do Designer de Fluxo de Trabalho.

- Melhor visibilidade de itens com foco, como ao:

  - Adicionar linhas às grades de dados usadas pelo Designer de Fluxo de Trabalho e por designers de atividades.

  - Percorrer campos nas atividades <xref:System.ServiceModel.Activities.ReceiveReply> e <xref:System.ServiceModel.Activities.SendReply>.

  - Definir valores padrão para variáveis ou argumentos

- Agora, leitores de tela podem reconhecer corretamente:

  - Pontos de interrupção definidos no Designer de Fluxo de Trabalho.

  - As atividades <xref:System.Activities.Statements.FlowSwitch%601>, <xref:System.Activities.Statements.FlowDecision> e <xref:System.ServiceModel.Activities.CorrelationScope>.
  - O conteúdo da atividade <xref:System.ServiceModel.Activities.Receive>.

  - O Tipo de Destino da atividade <xref:System.Activities.Statements.InvokeMethod>.

  - A caixa de combinação Exceção e a seção Finalmente na atividade <xref:System.Activities.Statements.TryCatch>.

  - A caixa de combinação Tipo de Mensagem, o divisor na janela Adicionar Inicializadores de Correlação, a janela Definição de Conteúdo e a janela de Definição de CorrelatesOn nas atividades de mensagens (<xref:System.ServiceModel.Activities.Receive>, <xref:System.ServiceModel.Activities.Send>, <xref:System.ServiceModel.Activities.SendReply> e <xref:System.ServiceModel.Activities.ReceiveReply>).

  - Transições de máquinas de estado e destinos de transição.

  - Anotações e conectores em atividades <xref:System.Activities.Statements.FlowDecision>.

  - Os menus de contexto (clique com o botão direito do mouse) para atividades.

  - Os editores de valor de propriedade, o botão Limpar Pesquisa, os botões de classificação Por Categoria e Ordem Alfabética e a caixa de diálogo Editor de Expressão na grade de propriedades.

  - A porcentagem de zoom no Designer de Fluxo de Trabalho.

  - O separador nas atividades <xref:System.Activities.Statements.Parallel> e <xref:System.Activities.Statements.Pick>.

  - A atividade <xref:System.Activities.Statements.InvokeDelegate>.

  - A janela Selecionar Tipos para atividades de dicionário (`Microsoft.Activities.AddToDictionary<TKey,TValue>`, `Microsoft.Activities.RemoveFromDictionary<TKey,TValue>` etc.).

  - A janela Procurar e Selecionar um Tipo .NET.

  - Trilhas de navegação no Designer de Fluxo de Trabalho.

- Usuários que escolherem temas de Alto Contraste verão muitas melhorias na visibilidade do Designer de Fluxo de Trabalho e em seus controles, como melhores taxas de contraste entre elementos e caixas de seleção mais perceptíveis usadas para elementos de foco.

## <a name="see-also"></a>Confira também

- [Novidades no .NET Framework](index.md)
