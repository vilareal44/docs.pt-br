---
title: Visão geral
description: Saiba como você pode usar Windows Forms para criar clientes inteligentes que atendam às necessidades das empresas e dos usuários finais atuais.
ms.date: 03/30/2017
helpviewer_keywords:
- smart clients
- Windows Forms, about Windows Forms
ms.assetid: 3a2b6284-c8d6-4e1c-8c69-0bed38f38cd4
ms.openlocfilehash: 820d5bae54ecb5a868314197d6a7e45e097b57de
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325987"
---
# <a name="windows-forms-overview"></a>Visão geral de Windows Forms

A visão geral a seguir descreve as vantagens dos aplicativos cliente inteligentes, os principais recursos de programação dos Windows Forms e como você pode usar o Windows Forms para criar clientes inteligentes que atendem às necessidades de empresas e usuários finais de hoje.

## <a name="windows-forms-and-smart-client-apps"></a>Aplicativos Windows Forms e Smart Client

 Com o Windows Forms, você desenvolve clientes inteligentes. *Clientes inteligentes* são aplicativos graficamente ricos que são fáceis de implantar e atualizar, podem funcionar quando eles estão conectados ou desconectados da Internet e podem acessar recursos no computador local de maneira mais segura do que os aplicativos tradicionais baseados no Windows.

### <a name="build-rich-interactive-user-interfaces"></a>Crie interfaces de usuário sofisticadas e interativas

 O Windows Forms é uma tecnologia de cliente inteligente para o .NET Framework, um conjunto de bibliotecas gerenciadas que simplificam as tarefas comuns de aplicativos como leitura e gravação no sistema de arquivos. Ao usar um ambiente de desenvolvimento como o Visual Studio, você pode criar aplicativos cliente inteligente do Windows Forms que exibem informações, solicitam entrada de usuários e se comunicam com computadores remotos em uma rede.

 nos Windows Forms, um *formulário* é uma superfície visual na qual são exibidas informações para o usuário. Normalmente, os aplicativos dos Windows Forms são compilados com o acréscimo de controles a formulários e de respostas de desenvolvimento a ações do usuário, como cliques do mouse ou pressionamentos de teclas. Um *controle* é um elemento discreto de interface do usuário que exibe dados ou aceita a entrada de dados.

 Quando um usuário executa alguma ação em seu formulário ou em um de seus controles, a ação gera um evento. O seu aplicativo reage a esses eventos usando código e os processa quando eles acontecem. Para obter mais informações, consulte [Criando manipuladores de eventos nos Windows Forms](creating-event-handlers-in-windows-forms.md).

 O Windows Forms contém uma variedade de controles que podem ser adicionados aos formulários: controles que exibem caixas de texto, botões, caixas suspensas, botões de opção e até mesmo páginas da Web. Para obter uma lista de todos os controles que podem ser usados em um formulário, consulte [Controles que podem ser usados nos Windows Forms](./controls/controls-to-use-on-windows-forms.md). Se um controle existente não atender às suas necessidades, Windows Forms também dará suporte à criação de seus próprios controles personalizados usando a <xref:System.Windows.Forms.UserControl> classe.

 O Windows Forms tem controles avançados de interface do usuário que emulam recursos em aplicativos de alta tecnologia, como o Microsoft Office. Quando você usa o <xref:System.Windows.Forms.ToolStrip> <xref:System.Windows.Forms.MenuStrip> controle e, pode criar barras de ferramentas e menus que contêm texto e imagens, exibir submenus e hospedar outros controles, como caixas de texto e caixas de combinação.

 Com o **Designer de formulários do Windows** do tipo "arrastar e soltar" no Visual Studio, você pode facilmente criar Windows Forms aplicativos. Basta selecionar os controles com o seu cursor e adicioná-los no local do formulário desejado. O designer oferece ferramentas como linhas de grade e linhas de alinhamento para facilitar o alinhamento dos controles. E se você usar o Visual Studio ou compilar na linha de comando, poderá usar os <xref:System.Windows.Forms.FlowLayoutPanel> <xref:System.Windows.Forms.TableLayoutPanel> controles e <xref:System.Windows.Forms.SplitContainer> para criar layouts de formulário avançados em menos tempo.

 Por fim, se você precisar criar seus próprios elementos personalizados de interface do usuário, o <xref:System.Drawing> namespace conterá uma grande seleção de classes para processar linhas, círculos e outras formas diretamente em um formulário.

> [!NOTE]
> Os controles dos Windows Forms não são projetados para a realização de marshaling entre domínios de aplicativo. Por esse motivo, a Microsoft não dá suporte à passagem de um controle de Windows Forms em um <xref:System.AppDomain> limite, embora o <xref:System.Windows.Controls.Control> tipo base de <xref:System.MarshalByRefObject> pareça indicar que isso é possível. Os aplicativos dos Windows Forms que têm vários domínios de aplicativo têm suporte, desde que nenhum controle dos Windows Forms seja passado entre limites de domínio de aplicativo.

#### <a name="create-forms-and-controls"></a>Criar formulários e controles

Para obter informações passo a passo sobre como usar esses recursos, consulte os seguintes tópicos da Ajuda.

|Descrição|Tópico da ajuda|
|-----------------|----------------|
|Usando controles em formulários|[Como Adicionar Controles ao Windows Forms](./controls/how-to-add-controls-to-windows-forms.md)|
|Usando o <xref:System.Windows.Forms.ToolStrip> controle|[Como criar um ToolStrip básico com itens padrão usando o designer](./controls/create-a-basic-wf-toolstrip-with-standard-items-using-the-designer.md)|
|Criando elementos gráficos com<xref:System.Drawing>|[Introdução à Programação de Elementos Gráficos](./advanced/getting-started-with-graphics-programming.md)|
|Criando controles personalizados|[Como herdar da classe UserControl](./controls/how-to-inherit-from-the-usercontrol-class.md)|

### <a name="display-and-manipulate-data"></a>Exibir e manipular dados
 Muitos aplicativos precisam exibir dados obtidos de um banco de dados, de um arquivo XML, de um serviço Web XML ou de outra fonte de dados. Windows Forms fornece um controle flexível chamado de <xref:System.Windows.Forms.DataGridView> controle para exibir esses dados tabulares em um formato de linha e coluna tradicional, para que cada parte dos dados ocupe sua própria célula. Ao usar <xref:System.Windows.Forms.DataGridView> o, você pode personalizar a aparência de células individuais, bloquear linhas e colunas arbitrárias em vigor e exibir controles complexos dentro de células, entre outros recursos.

 A conexão a fontes de dados pela rede é uma tarefa simples com clientes inteligentes dos Windows Forms. O <xref:System.Windows.Forms.BindingSource> componente representa uma conexão com uma fonte de dados e expõe métodos para associação de dados a controles, navegando até os registros anteriores e seguintes, editando registros e salvando as alterações de volta na fonte original. O <xref:System.Windows.Forms.BindingNavigator> controle fornece uma interface simples sobre o <xref:System.Windows.Forms.BindingSource> componente para que os usuários naveguem entre os registros.

 É possível criar facilmente controles de associação de dados usando a janela Fontes de Dados. A janela exibe fontes de dados como bancos de dados, serviços Web e objetos em seu projeto. Você pode criar controles de associação de dados ao arrastar itens dessa janela para os formulários do seu projeto. Você também pode associar controles existentes a dados ao arrastar objetos da janela Fontes de Dados para eles.

 Outro tipo de vinculação de dados que você pode gerenciar nos Windows Forms é chamado de *configurações*. A maioria dos aplicativos de cliente inteligente deve manter algumas informações sobre seu estado de tempo de execução, como o último tamanho de formulário conhecido e reter dados de preferência do usuário, como os locais padrão para os arquivos salvos. O recurso de configuração de aplicativo lida com esses requisitos ao oferecer uma forma fácil de armazenar ambos os tipos de configuração no computador cliente. Depois de definir essas configurações usando o Visual Studio ou um editor de código, as configurações são mantidas como XML e são automaticamente lidas de volta na memória em tempo de execução.

#### <a name="display-and-manipulate-data"></a>Exibir e manipular dados

Para obter informações passo a passo sobre como usar esses recursos, consulte os seguintes tópicos da Ajuda.

|Descrição|Tópico da ajuda|
|-----------------|----------------|
|Usando o <xref:System.Windows.Forms.BindingSource> componente|[Como associar controles dos Windows Forms ao componente BindingSource usando o designer](./controls/bind-wf-controls-with-the-bindingsource.md)|
|Trabalhando com fontes de dados ADO.NET|[Como classificar e filtrar dados ADO.NET com o componente BindingSource do Windows Forms](./controls/sort-and-filter-ado-net-data-with-wf-bindingsource-component.md)|
|Usando a janela de Fontes de Dados|[Associar controles do Windows Forms a dados no Visual Studio](/visualstudio/data-tools/bind-windows-forms-controls-to-data-in-visual-studio)|
|Uso de configurações do aplicativo|[Como Criar Configurações de Aplicativo](./advanced/how-to-create-application-settings.md)|

### <a name="deploy-apps-to-client-computers"></a>Implantar aplicativos em computadores cliente

Depois de escrever o seu aplicativo, você precisa enviá-lo para seus usuários para que eles possam instalá-lo e executá-lo em seus próprios computadores cliente. Ao usar a tecnologia ClickOnce, você pode implantar seus aplicativos de dentro do Visual Studio usando apenas alguns cliques e fornecer aos usuários uma URL apontando para seu aplicativo na Web. O ClickOnce gerencia todos os elementos e dependências em seu aplicativo e garante que o aplicativo esteja instalado corretamente no computador cliente.

Os aplicativos ClickOnce podem ser configurados para serem executados somente quando o usuário estiver conectado à rede ou para serem executados online e offline. Quando você especifica que um aplicativo deve dar suporte à operação offline, o ClickOnce adiciona um link ao seu aplicativo no menu **Iniciar** do usuário. O usuário pode, em seguida, abrir o aplicativo sem usar a URL.

Quando você atualiza o seu aplicativo, publica um novo manifesto de implantação e uma nova cópia do seu aplicativo em seu servidor Web. O ClickOnce detectará que há uma atualização disponível e atualizará a instalação do usuário; nenhuma programação personalizada é necessária para atualizar assemblies antigos.

#### <a name="deploy-clickonce-apps"></a>Implantar aplicativos ClickOnce

Para obter uma introdução completa ao ClickOnce, consulte [segurança e implantação do ClickOnce](/visualstudio/deployment/clickonce-security-and-deployment). Para informações passo a passo sobre como usar esses recursos, consulte os seguintes tópicos da Ajuda,

|Descrição|Tópico da ajuda|
|-----------------|----------------|
|Implantando um aplicativo usando o ClickOnce|[Como publicar um Aplicativo ClickOnce usando o Assistente de Publicação](/visualstudio/deployment/how-to-publish-a-clickonce-application-using-the-publish-wizard)<br /><br /> [Walkthrough: Implantando manualmente um aplicativo ClickOnce](/visualstudio/deployment/walkthrough-manually-deploying-a-clickonce-application)|
|Atualizando uma implantação do ClickOnce|[Como: gerenciar atualizações para um aplicativo ClickOnce](/visualstudio/deployment/how-to-manage-updates-for-a-clickonce-application)|
|Gerenciando a segurança com o ClickOnce|[Como habilitar as configurações de segurança do ClickOnce](/visualstudio/deployment/how-to-enable-clickonce-security-settings)|

### <a name="other-controls-and-features"></a>Outros controles e recursos

Existem muitos outros recursos dos Windows Forms que tornam as tarefas comuns de implementação mais fáceis e rápidas, como o suporte à criação de caixas de diálogo, impressão, adição da Ajuda e de documentação e localização do seu aplicativo para diversos idiomas. Além disso, Windows Forms se baseia no robusto sistema de segurança do .NET Framework. Com esse sistema, você pode liberar aplicativos mais seguros a seus clientes.

#### <a name="implement-other-controls-and-features"></a>Implementar outros controles e recursos

Para obter informações passo a passo sobre como usar esses recursos, consulte os seguintes tópicos da Ajuda.

|Descrição|Tópico da ajuda|
|-----------------|----------------|
|Imprimindo o conteúdo de um formulário|[Como imprimir elementos gráficos no Windows Forms](./advanced/how-to-print-graphics-in-windows-forms.md)<br /><br /> [Como imprimir um arquivo de texto de várias páginas no Windows Forms](./advanced/how-to-print-a-multi-page-text-file-in-windows-forms.md)|
|Saiba mais sobre a segurança dos Windows Forms|[Visão geral da Segurança do Windows Forms](security-in-windows-forms-overview.md)|

## <a name="see-also"></a>Veja também

- [Introdução com Windows Forms](getting-started-with-windows-forms.md)
- [Criando um novo formulário do Windows Forms](creating-a-new-windows-form.md)
- [Visão geral do controle ToolStrip](./controls/toolstrip-control-overview-windows-forms.md)
- [Visão geral do controle DataGridView](./controls/datagridview-control-overview-windows-forms.md)
- [Visão geral do componente BindingSource](./controls/bindingsource-component-overview.md)
- [Visão geral das configurações do aplicativo](./advanced/application-settings-overview.md)
- [Segurança e implantação do ClickOnce](/visualstudio/deployment/clickonce-security-and-deployment)
