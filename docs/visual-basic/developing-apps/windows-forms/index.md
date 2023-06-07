---
title: Noções básicas de Aplicativo do Windows Forms
ms.date: 07/20/2015
helpviewer_keywords:
- Windows applications
- Windows Forms, Visual Basic
ms.assetid: 0b919d30-7fd6-42db-85c8-543d15312441
ms.openlocfilehash: 11216186a28509e1f10bafa1b24a440bcedaeeb6
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84398240"
---
# <a name="windows-forms-application-basics-visual-basic"></a>Noções básicas de Aplicativo do Windows Forms (Visual Basic)

Uma parte importante do Visual Basic é a capacidade de criar Windows Forms aplicativos que são executados localmente nos computadores dos usuários. Você pode usar o Visual Studio para criar o aplicativo e a interface do usuário usando Windows Forms. Um aplicativo Windows Forms é criado em classes do <xref:System.Windows.Forms> namespace.

## <a name="designing-windows-forms-applications"></a>Criando Windows Forms aplicativos

Você pode criar Windows Forms e aplicativos de serviço do Windows com o Visual Studio. Para obter mais informações, consulte estes tópicos:

- [Introdução com Windows Forms](../../../framework/winforms/getting-started-with-windows-forms.md). Fornece informações sobre como criar e programar Windows Forms.

- [Controles de Windows Forms](../../../framework/winforms/controls/index.md). Coleção de tópicos detalhando o uso de controles de Windows Forms.

- [Aplicativos de serviço do Windows](../../../framework/windows-services/index.md). Lista os tópicos que explicam como criar serviços do Windows.

## <a name="building-rich-interactive-user-interfaces"></a>Compilando interfaces do usuário sofisticadas e interativas

Windows Forms é o componente de cliente inteligente do .NET Framework, um conjunto de bibliotecas gerenciadas que habilitam tarefas comuns de aplicativos, como leitura e gravação no sistema de arquivos. Usando um ambiente de desenvolvimento como o Visual Studio, você pode criar Windows Forms aplicativos que exibem informações, solicitar entrada de usuários e se comunicar com computadores remotos por meio de uma rede.

nos Windows Forms, um formulário é uma superfície visual na qual são exibidas informações para o usuário. Normalmente, você cria Windows Forms aplicativos colocando controles em formulários e desenvolvendo respostas para ações do usuário, como cliques do mouse ou pressionamentos de tecla. Um *controle* é um elemento discreto de interface do usuário que exibe dados ou aceita a entrada de dados.

### <a name="events"></a>Eventos

Quando um usuário faz algo em seu formulário ou em um de seus controles, ele gera um evento. O seu aplicativo reage a esses eventos usando código e os processa quando eles acontecem. Para obter mais informações, consulte [Criando manipuladores de eventos nos Windows Forms](../../../framework/winforms/creating-event-handlers-in-windows-forms.md).

### <a name="controls"></a>Controles

O Windows Forms contém uma variedade de controles que você pode colocar em formulários: controles que exibem caixas de texto, botões, caixas suspensas, botões de opção e até mesmo páginas da Web. Para obter uma lista de todos os controles que podem ser usados em um formulário, consulte [Controles que podem ser usados nos Windows Forms](../../../framework/winforms/controls/controls-to-use-on-windows-forms.md). Se um controle existente não atender às suas necessidades, Windows Forms também dará suporte à criação de seus próprios controles personalizados usando a <xref:System.Windows.Forms.UserControl> classe.

O Windows Forms tem controles avançados de interface do usuário que emulam recursos em aplicativos de alta tecnologia, como o Microsoft Office. Usando o <xref:System.Windows.Forms.ToolStrip> <xref:System.Windows.Forms.MenuStrip> controle e, você pode criar barras de ferramentas e menus que contêm texto e imagens, exibir submenus e hospedar outros controles, como caixas de texto e caixas de combinação.

Com o designer de formulários do tipo "arrastar e soltar" do Visual Studio, você pode facilmente criar Windows Forms aplicativos: basta selecionar os controles com o cursor e colocá-los onde você deseja no formulário. O designer fornece ferramentas como linhas de grade e "linhas de alinhamento" para tirar as complicações do alinhamento dos controles. E se você usar o Visual Studio ou compilar na linha de comando, poderá usar os <xref:System.Windows.Forms.FlowLayoutPanel> <xref:System.Windows.Forms.TableLayoutPanel> controles e <xref:System.Windows.Forms.SplitContainer> para criar layouts de formulário avançados com tempo mínimo e esforço.

### <a name="custom-ui-elements"></a>Elementos personalizados da interface do usuário

Por fim, se você precisar criar seus próprios elementos personalizados de interface do usuário, o <xref:System.Drawing> namespace conterá todas as classes necessárias para processar linhas, círculos e outras formas diretamente em um formulário.

Para obter informações passo a passo sobre como usar esses recursos, consulte os tópicos da ajuda a seguir.

|Para|Consulte|
|--------|---------|
|Criar um novo aplicativo Windows Forms com o Visual Studio|[Tutorial 1: Criar um visualizador de imagens](/visualstudio/ide/tutorial-1-create-a-picture-viewer)|
|Usar controles em formulários|[Como Adicionar Controles ao Windows Forms](../../../framework/winforms/controls/how-to-add-controls-to-windows-forms.md)|
|Criar elementos gráficos com<xref:System.Drawing>|[Introdução à Programação de Elementos Gráficos](../../../framework/winforms/advanced/getting-started-with-graphics-programming.md)|
|Criar controles personalizados|[Como herdar da classe UserControl](../../../framework/winforms/controls/how-to-inherit-from-the-usercontrol-class.md)|

## <a name="displaying-and-manipulating-data"></a>Exibindo e manipulando dados

Muitos aplicativos precisam exibir dados obtidos de um banco de dados, de um arquivo XML, de um serviço Web XML ou de outra fonte de dados. Windows Forms fornece um controle flexível chamado de <xref:System.Windows.Forms.DataGridView> controle para renderizar esses dados tabulares em um formato de linha e coluna tradicional, para que cada parte dos dados ocupe sua própria célula. Usando <xref:System.Windows.Forms.DataGridView> você pode personalizar a aparência de células individuais, bloquear linhas e colunas arbitrárias em vigor e exibir controles complexos dentro de células, entre outros recursos.

A conexão a fontes de dados pela rede é uma tarefa simples com clientes inteligentes dos Windows Forms. O <xref:System.Windows.Forms.BindingSource> componente, novo com Windows Forms no Visual Studio 2005 e o .NET Framework 2,0, representa uma conexão com uma fonte de dados e expõe métodos para associar dados a controles, navegando até os registros anteriores e seguintes, editando registros e salvando as alterações de volta na fonte original. O <xref:System.Windows.Forms.BindingNavigator> controle fornece uma interface simples sobre o <xref:System.Windows.Forms.BindingSource> componente para que os usuários naveguem entre os registros.

### <a name="data-bound-controls"></a>Controles vinculados a dados

Você pode criar controles vinculados a dados facilmente usando a janela fontes de dados, que exibe fontes de dados, como bancos de dados, serviços Web e objetos em seu projeto. Você pode criar controles de associação de dados ao arrastar itens dessa janela para os formulários do seu projeto. Você também pode associar controles existentes a dados ao arrastar objetos da janela Fontes de Dados para eles.

### <a name="settings"></a>Configurações

Outro tipo de vinculação de dados que você pode gerenciar nos Windows Forms é chamado de configurações. A maioria dos aplicativos de cliente inteligente deve reter algumas informações sobre seu estado de tempo de execução, como o último tamanho de formulários conhecido e manter os dados de preferência do usuário, como locais padrão para arquivos salvos. O recurso de configurações de aplicativo resolve esses requisitos fornecendo uma maneira fácil de armazenar os dois tipos de configurações no computador cliente. Uma vez definido usando o Visual Studio ou um editor de código, essas configurações são mantidas como XML e são automaticamente lidas de volta na memória em tempo de execução.

Para obter informações passo a passo sobre como usar esses recursos, consulte os tópicos da ajuda a seguir.

|Para|Consulte|
|--------|---------|
|Usar o <xref:System.Windows.Forms.BindingSource> componente|[Como associar controles dos Windows Forms ao componente BindingSource usando o designer](../../../framework/winforms/controls/bind-wf-controls-with-the-bindingsource.md)|
|Trabalhar com fontes de dados ADO.NET|[Como classificar e filtrar dados ADO.NET com o componente BindingSource do Windows Forms](../../../framework/winforms/controls/sort-and-filter-ado-net-data-with-wf-bindingsource-component.md)|
|Usar a janela fontes de dados|[Passo a passo: exibindo dados em um Windows Form](/visualstudio/data-tools/accessing-data-in-visual-studio)|

## <a name="deploying-applications-to-client-computers"></a>Implantando aplicativos em computadores cliente

Depois de escrever seu aplicativo, você deve enviá-lo aos seus usuários para que eles possam instalá-lo e executá-lo em seus próprios computadores cliente. Usando a tecnologia ClickOnce, você pode implantar seus aplicativos de dentro do Visual Studio usando apenas alguns cliques e fornecer uma URL apontando para seu aplicativo na Web. O ClickOnce gerencia todos os elementos e dependências em seu aplicativo e garante que o aplicativo esteja instalado corretamente no computador cliente.

Os aplicativos ClickOnce podem ser configurados para serem executados somente quando o usuário estiver conectado à rede ou para serem executados online e offline. Quando você especifica que um aplicativo deve dar suporte à operação offline, o ClickOnce adiciona um link ao seu aplicativo no menu **Iniciar** do usuário, para que o usuário possa abri-lo sem usar a URL.

Quando você atualiza o seu aplicativo, publica um novo manifesto de implantação e uma nova cópia do seu aplicativo em seu servidor Web. O ClickOnce detecta que há uma atualização disponível e atualiza a instalação do usuário; nenhuma programação personalizada é necessária para atualizar assemblies antigos.

Para obter uma introdução completa ao ClickOnce, consulte [segurança e implantação do ClickOnce](/visualstudio/deployment/clickonce-security-and-deployment). Para obter informações passo a passo sobre como usar esses recursos, consulte os seguintes tópicos da ajuda:

|Para|Consulte|
|--------|---------|
|Implantar um aplicativo com o ClickOnce|[Como publicar um Aplicativo ClickOnce usando o Assistente de Publicação](/visualstudio/deployment/how-to-publish-a-clickonce-application-using-the-publish-wizard)<br /><br /> [Walkthrough: Implantando manualmente um aplicativo ClickOnce](/visualstudio/deployment/walkthrough-manually-deploying-a-clickonce-application)|
|Atualizar uma implantação do ClickOnce|[Como: gerenciar atualizações para um aplicativo ClickOnce](/visualstudio/deployment/how-to-manage-updates-for-a-clickonce-application)|
|Gerenciar a segurança com o ClickOnce|[Como habilitar as configurações de segurança do ClickOnce](/visualstudio/deployment/how-to-enable-clickonce-security-settings)|

## <a name="other-controls-and-features"></a>Outros controles e recursos

Existem muitos outros recursos dos Windows Forms que tornam as tarefas comuns de implementação mais fáceis e rápidas, como o suporte à criação de caixas de diálogo, impressão, adição da Ajuda e de documentação e localização do seu aplicativo para diversos idiomas. Além disso, Windows Forms se baseia no robusto sistema de segurança do .NET Framework, permitindo que você libere aplicativos mais seguros para seus clientes.

Para obter informações passo a passo sobre como usar esses recursos, consulte os seguintes tópicos da ajuda:

|Para|Consulte|
|--------|---------|
|Imprimir o conteúdo de um formulário|[Como imprimir elementos gráficos no Windows Forms](../../../framework/winforms/advanced/how-to-print-graphics-in-windows-forms.md)<br /><br /> [Como imprimir um arquivo de texto de várias páginas no Windows Forms](../../../framework/winforms/advanced/how-to-print-a-multi-page-text-file-in-windows-forms.md)|
|Saiba mais sobre a segurança dos Windows Forms|[Visão geral da Segurança do Windows Forms](../../../framework/winforms/security-in-windows-forms-overview.md)|

## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase>
- [Visão geral dos Windows Forms](../../../framework/winforms/windows-forms-overview.md)
- [Objeto My.Forms](../../language-reference/objects/my-forms-object.md)
