---
ms.openlocfilehash: e528a41748d9353c96d443f68e15e7a98ee7f4ae
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617781"
---
### <a name="accessibility-improvements-in-windows-forms-controls-for-net-48"></a>Melhorias de acessibilidade nos controles do Windows Forms para o .NET 4.8

#### <a name="details"></a>Detalhes

O Windows Forms Framework está melhorando continuamente sua maneira de trabalhar com tecnologias de acessibilidade para melhorar o suporte aos clientes do Windows Forms. Isso inclui as seguintes alterações:

- Alterações para melhorar a exibição durante o modo de Alto Contraste.
- Alterações na interação com o Narrator.
- Alterações na hierarquia acessível (melhora da navegação por meio da árvore de automação de interface do usuário).

#### <a name="suggestion"></a>Sugestão

**Como aceitar ou recusar essas alterações** Para que o aplicativo se beneficie dessas alterações, ele deve ser executado no .NET Framework 4,8. O aplicativo pode aceitar essas alterações de qualquer uma das seguintes maneiras:

- Ser recompilado para direcionar o .NET Framework 4.8. Essas alterações de acessibilidade são habilitadas por padrão em aplicativos do Windows Forms que direcionam o .NET Framework 4.8.
- Ser direcionado ao .NET Framework 4.7.2 ou a uma versão anterior e recusar os comportamentos de acessibilidade herdados, adicionando o seguinte [comutador AppContext](https://docs.microsoft.com/dotnet/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element) à seção `<runtime>` do arquivo de configuração de aplicativo e definindo-o como `false`, como mostra o exemplo a seguir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7"/>
  </startup>
  <runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true/false;key2=true/false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false" />
  </runtime>
</configuration>
```

Observe que, para aceitar os recursos de acessibilidade adicionados no .NET Framework 4.8, também é necessário aceitar os recursos de acessibilidade do .NET Framework 4.7.1 e 4.7.2. Os aplicativos que direcionam o .NET Framework 4.8 e desejam preservar o comportamento de acessibilidade herdado podem aceitar o uso de recursos de acessibilidade herdados definindo explicitamente esse comutador AppContext como `true`.Habilitar o suporte à invocação de dica de ferramenta do teclado requer a adição da linha `Switch.System.Windows.Forms.UseLegacyToolTipDisplay=false` ao valor AppContextSwitchOverrides:

```xml
<AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false;Switch.System.Windows.Forms.UseLegacyToolTipDisplay=false" />
```

Observe que, para habilitar esse recurso, é necessário aceitar os recursos de acessibilidade mencionados anteriormente do .NET Framework 4.7.1 – 4.8. Além disso, se nenhum recurso de acessibilidade for aceito, mas o recurso de exibição da dica de ferramenta for aceito, o <xref:System.NotSupportedException> do runtime será gerado no primeiro acesso a esses recursos. A mensagem de exceção indica que as dicas de ferramenta de teclado exigem melhorias de acessibilidade do nível 3 para serem habilitadas.

**Uso de cores definidas pelo sistema operacional em temas de Alto Contraste**

- Melhoria de temas de alto contraste.

**Melhor suporte ao Narrador**

- Agora o narrador anuncia a direção de classificação do <xref:System.Windows.Forms.DataGridViewColumn> ao anunciar um nome acessível de um <xref:System.Windows.Forms.DataGridViewCell>.

**Melhoria no suporte à acessibilidade de CheckedListBox**

- Melhoria no suporte ao Narrador do controle <xref:System.Windows.Forms.CheckedListBox>. Ao navegar até o controle <xref:System.Windows.Forms.CheckedListBox> usando o teclado de controle, o Narrador concentra-se no item <xref:System.Windows.Forms.CheckedListBox> e o anuncia.
- Um controle CheckedListBox vazio agora tem um retângulo de foco desenhado para um primeiro item virtual quando o controle fica focalizado.

**Melhoria no suporte à acessibilidade de ComboBox**

- Habilitação do suporte à Automação de interface do usuário para o controle <xref:System.Windows.Forms.ComboBox>, com a capacidade de usar notificações de Automação de interface do usuário e outros recursos de automação de interface do usuário.
**Melhoria no suporte à acessibilidade da DataGridView**

- Habilitação do suporte à Automação de interface do usuário para o controle <xref:System.Windows.Forms.DataGridView>, com a capacidade de usar notificações de Automação de interface do usuário e outros recursos de automação de interface do usuário.
- O elemento de Automação de interface do usuário que corresponde ao <xref:System.Windows.Forms.DataGridViewComboBoxEditingControl> ou <xref:System.Windows.Forms.DataGridViewTextBoxEditingControl> agora é um filho da célula de edição correspondente.

**Aprimoramento do suporte à acessibilidade de LinkLabel**

- Acessibilidade de controle aprimorada <xref:System.Windows.Forms.LinkLabel> : o Narrator anuncia o estado desabilitado para o link se o <xref:System.Windows.Forms.LinkLabel> controle correspondente estiver desabilitado.

**Melhoria ao suporte à acessibilidade de ProgressBar**

- Habilitação do suporte à Automação de interface do usuário para o controle <xref:System.Windows.Forms.ProgressBar>, com a capacidade de usar notificações de Automação de interface do usuário e outros recursos de automação de interface do usuário. Agora, os desenvolvedores podem usar as notificações de automação de interface do usuário que o Narrator pode anunciar para indicar o progresso.
Para obter uma visão geral da visão geral dos eventos de automação da interface do usuário, incluindo eventos de notificação de automação da interface do usuário, consulte a [visão geral dos eventos](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-eventsoverview)

**Suporte aprimorado de acessibilidade de PropertyGrid**

- Habilitação do suporte à Automação de interface do usuário para o controle <xref:System.Windows.Forms.PropertyGrid>, com a capacidade de usar notificações de Automação de interface do usuário e outros recursos de automação de interface do usuário.
- O elemento de Automação de interface do usuário que corresponde à propriedade editada no momento agora é um filho do elemento de Automação da interface do usuário do item de propriedade correspondente.
- O elemento de item de propriedade de Automação de interface do usuário agora será um filho do elemento de categoria correspondente se o controle <xref:System.Windows.Forms.PropertyGrid> pai estiver definido como modo de exibição de categoria.

**Melhoria ao suporte ToolStrip**

- Habilitação do suporte à Automação de interface do usuário para o controle <xref:System.Windows.Forms.ToolStrip>, com a capacidade de usar notificações de Automação de interface do usuário e outros recursos de automação de interface do usuário.
- Melhoria da navegação por meio de itens <xref:System.Windows.Forms.ToolStrip>.
- No modo de itens, o foco do Narrador não desaparece e não vai para os itens ocultos.

**Melhoria das indicações visuais**

- Um controle <xref:System.Windows.Forms.CheckedListBox> vazio agora exibe um indicador de foco quando recebe o foco.
Observação: o suporte à automação da interface do usuário está habilitado para controles no tempo de execução, mas não é usado em tempo de design. Para obter uma visão geral de automação da interface do usuário, confira a [Visão geral de automação da interface do usuário](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-overview).

**Invocação de dicas de ferramenta de controles com um teclado**

- A dica de ferramenta do controle agora pode ser invocada focalizando o controle com o teclado. Esse recurso precisa ser habilitado explicitamente para o aplicativo (consulte a seção ** &quot; como aceitar ou recusar essas alterações &quot; **)

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Principal       |
| Versão | 4.8         |
| Type    | Redirecionando |
