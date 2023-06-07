---
title: Implantar um aplicativo
description: Explore as tecnologias de implantação que o Windows e o .NET Framework usam para aplicativos do Windows Presentation Foundation (WPF).
ms.date: 03/30/2017
helpviewer_keywords:
- WPF applications [WPF], deployment
- deployment [WPF], applications
ms.assetid: 12cadca0-b32c-4064-9a56-e6a306dcc76d
ms.openlocfilehash: 62904ccd47800c8340d68c223e50688902170063
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619293"
---
# <a name="deploy-a-wpf-application"></a>Implantar um aplicativo WPF

Depois que os aplicativos do Windows Presentation Foundation (WPF) são criados, eles precisam ser implantados. O Windows e o .NET Framework incluem várias tecnologias de implantação. A tecnologia de implantação usada para implantar um aplicativo do WPF depende do tipo de aplicativo. Este tópico fornece uma breve visão geral de cada tecnologia de implantação e como elas são usadas em conjunto com os requisitos de implantação de cada tipo de aplicativo do WPF.

<a name="Deployment_Technologies"></a>
## <a name="deployment-technologies"></a>Tecnologias de implantação  
 O Windows e o .NET Framework incluem várias tecnologias de implantação, incluindo:  
  
- Implantação do XCopy.  
  
- Implantação de Windows Installer.  
  
- Implantação do ClickOnce.  
  
<a name="XCopy_Deployment"></a>
### <a name="xcopy-deployment"></a>Implantação do XCopy  
 Implantação do XCopy se refere ao uso do programa de linha de comando XCopy para copiar arquivos de um local para outro. A implantação do XCopy é adequada nas seguintes circunstâncias:  
  
- O aplicativo é independente. Ele não precisa atualizar o cliente para ser executado.  
  
- Os arquivos de aplicativo devem ser movidos de um local para outro, como de um local de compilação (disco local, compartilhamento de arquivos UNC e assim por diante) para um local de publicação (site da Web, compartilhamento de arquivos UNC e assim por diante).  
  
- O aplicativo não requer integração com o shell (atalho no menu Iniciar, ícone da área de trabalho e assim por diante).  
  
 Embora o XCopy seja adequado para cenários de implantação simples, ele é limitado quando recursos de implantação mais complexos são necessários. Em particular, o uso do XCopy frequentemente causa sobrecarga para criar, executar e manter scripts para gerenciar a implantação de forma robusta. Além disso, o XCopy não dá suporte a controle de versão, desinstalação ou reversão.  
  
<a name="Windows_Installer"></a>
### <a name="windows-installer"></a>Windows Installer  
 Windows Installer permite que os aplicativos sejam empacotados como executáveis independentes que podem ser facilmente distribuídos aos clientes e executados. Além disso, o Windows Installer é instalado com o Windows e permite a integração com a área de trabalho, o menu iniciar e o painel de controle programas.  
  
 O Windows Installer simplifica a instalação e a desinstalação de aplicativos, mas não fornece recursos para garantir que os aplicativos instalados sejam mantidos atualizados a partir de um ponto de vista de controle de versão.  
  
 Para obter mais informações sobre Windows Installer, consulte [Windows Installer implantação](/visualstudio/deployment/deploying-applications-services-and-components#create-an-installer-package-windows-desktop).
  
<a name="ClickOnce_Deployment"></a>
### <a name="clickonce-deployment"></a>Implantação do ClickOnce  
 O ClickOnce permite a implantação de aplicativos de estilo da Web para aplicativos que não são da Web. Os aplicativos são publicados e implantados de servidores Web ou de arquivos. Embora o ClickOnce não ofereça suporte a toda a gama de recursos de cliente que Windows Installer aplicativos instalados, ele oferece suporte a um subconjunto que inclui o seguinte:  
  
- Integração com o menu Iniciar e o painel de controle Programas.  
  
- Controle de versão, reversão e desinstalação.  
  
- Modo de instalação online, que sempre inicializa um aplicativo do local de implantação.  
  
- Atualização automática quando novas versões são lançadas.  
  
- Registro de extensões de arquivo.  
  
 Para obter mais informações sobre o ClickOnce, consulte [segurança e implantação do ClickOnce](/visualstudio/deployment/clickonce-security-and-deployment).  
  
<a name="Deploying_WPF_Applications"></a>
## <a name="deploying-wpf-applications"></a>Implantando aplicativos do WPF  
 As opções de implantação para um aplicativo WPF dependem do tipo de aplicativo. De uma perspectiva de implantação, o WPF tem três tipos de aplicativos significativos:  
  
- Aplicativos autônomos.  
  
- Aplicativos [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] somente de marcação.  
  
- Aplicativos de navegador XAML (XBAPs).  
  
<a name="Deploying_Standalone_Applications"></a>
### <a name="deploying-standalone-applications"></a>Implantando aplicativos autônomos  
 Os aplicativos autônomos são implantados usando o ClickOnce ou o Windows Installer. De qualquer maneira, aplicativos autônomos exigem confiança total para serem executados. A confiança total é concedida automaticamente a aplicativos autônomos que são implantados usando Windows Installer. Os aplicativos autônomos que são implantados usando o ClickOnce não recebem automaticamente confiança total. Em vez disso, o ClickOnce exibe uma caixa de diálogo de aviso de segurança que os usuários devem aceitar antes que um aplicativo autônomo seja instalado. Se for aceita, o aplicativo autônomo será instalado e terá confiança total. Caso contrário, o aplicativo autônomo não será instalado.  
  
<a name="Deploying_Markup_Only_XAML_Applications"></a>
### <a name="deploying-markup-only-xaml-applications"></a>Implantando aplicativos XAML somente marcação  
 As páginas somente de marcação [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] geralmente são publicadas em servidores Web, como páginas HTML, e podem ser exibidas usando o Internet Explorer. Páginas de [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] somente marcação são executadas em uma área restrita de segurança com confiança parcial, com restrições que são definidas pelo conjunto de permissões da zona da Internet. Isso fornece uma área restrita de segurança equivalente a aplicativos Web baseados em HTML.  
  
 Para obter mais informações sobre segurança para aplicativos do WPF, consulte [segurança](../security-wpf.md).  
  
 As páginas somente de marcação [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] podem ser instaladas no sistema de arquivos local usando o Xcopy ou o Windows Installer. Essas páginas podem ser exibidas usando o Internet Explorer ou o Windows Explorer.  
  
 Para obter mais informações sobre XAML, consulte [Visão geral de XAML (WPF)](../../../desktop-wpf/fundamentals/xaml.md).  
  
<a name="Deploying_XAML_Browser_Applications"></a>
### <a name="deploying-xaml-browser-applications"></a>Implantando aplicativos de navegador XAML  
 XBAPs são aplicativos compilados que exigem que os três arquivos a seguir sejam implantados:  
  
- *ApplicationName*.exe: o arquivo de aplicativo do assembly executável.  
  
- *ApplicationName*.xbap: o manifesto de implantação.  
  
- *ApplicationName*.exe.manifest: o manifesto do aplicativo.  
  
> [!NOTE]
> Para obter mais informações sobre manifestos de aplicativo e de implantação, consulte [Compilando um aplicativo WPF](building-a-wpf-application-wpf.md).  
  
 Esses arquivos são produzidos quando um XBAP é criado. Para obter mais informações, consulte [Como criar um novo projeto de aplicativo de navegador do WPF](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/bb628663(v=vs.100)). Como páginas somente de marcação [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] , XBAPs normalmente são publicados em um servidor Web e exibidos usando o Internet Explorer.  
  
 XBAPs podem ser implantados em clientes usando qualquer uma das técnicas de implantação. No entanto, o ClickOnce é recomendado, pois fornece os seguintes recursos:  
  
1. Atualizações automáticas quando uma nova versão é publicada.  
  
2. Privilégios de elevação para o XBAP em execução com confiança total.  
  
 Por padrão, o ClickOnce publica os arquivos do aplicativo com a extensão .deploy. Isso pode ser problemático, mas pode ser desabilitado. Para obter mais informações, consulte [Problemas de configuração de servidor e cliente em implantações do ClickOnce](/visualstudio/deployment/server-and-client-configuration-issues-in-clickonce-deployments).  
  
 Para obter mais informações sobre como implantar aplicativos de navegador XAML (XBAPs), consulte [visão geral de aplicativos de navegador XAML WPF](wpf-xaml-browser-applications-overview.md).  
  
<a name="Installing__NET_Framework_3_0"></a>
## <a name="installing-the-net-framework"></a>Instalando o .NET Framework  
 Para executar um aplicativo do WPF, o Microsoft .NET Framework deve ser instalado no cliente. O Internet Explorer detecta automaticamente se os clientes estão instalados com .NET Framework quando os aplicativos hospedados no navegador WPF são exibidos. Se o .NET Framework não estiver instalado, o Internet Explorer solicitará que os usuários o instalem.  
  
 Para detectar se o .NET Framework está instalado, o Internet Explorer inclui um aplicativo bootstrapper que é registrado como o manipulador MIME (Multipurpose Internet Mail Extensions) de fallback para arquivos de conteúdo com as seguintes extensões:. XAML,. XPS,. XBAP e. Application. Se você navegar para esses tipos de arquivo e o .NET Framework não estiver instalado no cliente, o aplicativo bootstrapper solicitará permissão para instalá-lo. Se a permissão não for fornecida, nem a .NET Framework nem o aplicativo será instalado.  
  
 Se a permissão for concedida, o Internet Explorer baixará e instalará o .NET Framework usando o Microsoft Serviço de Transferência Inteligente em Segundo Plano (BITS). Após a instalação bem-sucedida do .NET Framework, o arquivo originalmente solicitado é aberto em uma nova janela do navegador.  
  
 Para obter mais informações, consulte [Implantando o .NET Framework e aplicativos](../../deployment/index.md).  
  
## <a name="see-also"></a>Consulte também

- [Compilando um aplicativo WPF](building-a-wpf-application-wpf.md)
- [Segurança](../security-wpf.md)
