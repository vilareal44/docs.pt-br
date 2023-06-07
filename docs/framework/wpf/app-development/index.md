---
title: Desenvolvimento de aplicativo
description: Saiba como criar uma variedade de aplicativos usando a estrutura do Windows Presentation Foundation (WPF).
ms.date: 01/26/2018
helpviewer_keywords:
- WPF [WPF], about application development
- application development [WPF], about
ms.assetid: 2996ce5e-81e9-49ae-881b-952db3dd1b7e
ms.openlocfilehash: ac4227785f2fc398217b3aa8984176844264bbaa
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85613729"
---
# <a name="application-development"></a>Desenvolvimento de aplicativo
<a name="introduction"></a>O Windows Presentation Foundation (WPF) é uma estrutura de apresentação que pode ser usada para desenvolver os seguintes tipos de aplicativos:  
  
- Aplicativos autônomos (aplicativos de estilo tradicional do Windows criados como assemblies executáveis que são instalados e executados no computador cliente).  
  
- Aplicativos de navegador XAML (XBAPs) (aplicativos compostos por páginas de navegação criadas como assemblies executáveis e hospedados por navegadores da Web, como o Microsoft Internet Explorer ou Mozilla Firefox).  
  
- Bibliotecas de controles personalizados (assemblies não executáveis contendo controles reutilizáveis).  
  
- Bibliotecas de classes (assemblies não executáveis que contêm classes reutilizáveis).  
  
> [!NOTE]
> Não é recomendável usar tipos do WPF em um serviço Windows. Se você tentar usar esses recursos em um serviço Windows, será possível que eles não funcionem conforme o esperado.  
  
 Para compilar esse conjunto de aplicativos, o WPF implementa um host de serviços. Este tópico fornece uma visão geral desses serviços e onde encontrar mais informações.  

<a name="Application_Management"></a>
## <a name="application-management"></a>Gerenciamento de aplicativos  
 Aplicativos executáveis do WPF normalmente exigem um conjunto principal de funcionalidades que inclui o seguinte:  
  
- Criar e gerenciar a infraestrutura de aplicativo comum (incluindo criar um método de ponto de entrada e um loop de mensagem do Windows para receber mensagens de entrada e do sistema).  
  
- Acompanhar o tempo de vida de um aplicativo e interagir com ele.  
  
- Recuperação e processamento de parâmetros de linha de comando.  
  
- Compartilhar recursos [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] e propriedades no escopo do aplicativo.  
  
- Detectar e processar exceções sem tratamento.  
  
- Retornar códigos de saída.  
  
- Gerenciamento de janelas de aplicativos autônomos.  
  
- Rastreamento de navegação em aplicativos de navegador XAML (XBAPs) e aplicativos autônomos com janelas de navegação e quadros.  
  
 Esses recursos são implementados pela classe <xref:System.Windows.Application>, que você adiciona a seus aplicativos usando uma *definição de aplicativo*.  
  
 Para obter mais informações, consulte [Visão geral do gerenciamento de aplicativos](application-management-overview.md).  
  
<a name="WPF_Application_Resource__Content__and_Data_Files"></a>
## <a name="wpf-application-resource-content-and-data-files"></a>Arquivos de recurso, conteúdo e dados do aplicativo WPF  
 O WPF estende o suporte principal no Microsoft .NET Framework para recursos incorporados com suporte para três tipos de arquivos de dados não executáveis: recurso, conteúdo e dados. Para mais informações, consulte [Arquivos de recurso, conteúdo e dados do aplicativo WPF](wpf-application-resource-content-and-data-files.md).  
  
 Um componente-chave do suporte a arquivos de dados não executáveis do WPF é a capacidade de identificá-los e carregá-los usando um URI exclusivo. Para obter mais informações, consulte [URIs "pack://" no WPF](pack-uris-in-wpf.md).  
  
<a name="Windows_and_Dialog_Boxes"></a>
## <a name="windows-and-dialog-boxes"></a>Janelas e caixas de diálogo  
 Os usuários interagem com aplicativos autônomos do WPF por meio do Windows. A finalidade de uma janela é hospedar conteúdo do aplicativo e expor a funcionalidade do aplicativo que normalmente permite aos usuários interagir com o conteúdo. No WPF, as janelas são encapsuladas pela <xref:System.Windows.Window> classe, que oferece suporte a:  
  
- Cria e exibir janelas.  
  
- Estabelecer relações de janela de proprietário/propriedade.  
  
- Configurar a aparência da janela (por exemplo, tamanho, localização, ícones, texto da barra de título, borda).  
  
- Acompanhar o tempo de vida de uma janela e interagir com ele.  
  
 Para obter mais informações, consulte [Visão geral do WPF do Windows](wpf-windows-overview.md).  
  
 <xref:System.Windows.Window> é compatível com a criação de um tipo especial de janela conhecido como caixa de diálogo. Tipos modais e sem janela restrita de caixas de diálogo podem ser criados.  
  
 Para sua conveniência e os benefícios da reutilização e uma experiência de usuário consistente entre aplicativos, o WPF expõe três das caixas de diálogo comuns do Windows: <xref:Microsoft.Win32.OpenFileDialog> , <xref:Microsoft.Win32.SaveFileDialog> e <xref:System.Windows.Controls.PrintDialog> .  
  
 Uma caixa de mensagem é um tipo especial de caixa de diálogo para mostrar informações textuais importantes para os usuários e para fazer perguntas simples como Sim/Não/OK/Cancelar. Use a classe <xref:System.Windows.MessageBox> para criar e exibir caixas de mensagem.  
  
 Para obter mais informações, consulte [Visão geral das caixas de diálogo](dialog-boxes-overview.md).  
  
<a name="Navigation"></a>
## <a name="navigation"></a>Navegação  
 O WPF dá suporte à navegação de estilo da Web usando páginas ( <xref:System.Windows.Controls.Page> ) e hiperlinks ( <xref:System.Windows.Documents.Hyperlink> ). A navegação pode ser implementada de uma variedade de formas, que incluem o seguinte:  
  
- Páginas autônomas que são hospedadas em um navegador da Web.  
  
- Páginas compiladas em um XBAP que é hospedado em um navegador da Web.  
  
- Páginas compiladas em um aplicativo autônomo e hospedadas por uma janela de navegação (<xref:System.Windows.Navigation.NavigationWindow>).  
  
- Páginas que são hospedadas por um frame ( <xref:System.Windows.Controls.Frame> ), que podem ser hospedadas em uma página autônoma ou uma página compilada em um XBAP ou em um aplicativo autônomo.  
  
 Para facilitar a navegação, o WPF implementa o seguinte:  
  
- <xref:System.Windows.Navigation.NavigationService>, o mecanismo de navegação compartilhado para processar solicitações de navegação que é usado pelo <xref:System.Windows.Controls.Frame> , <xref:System.Windows.Navigation.NavigationWindow> e XBAPs para dar suporte à navegação dentro do aplicativo.  
  
- Métodos de navegação para iniciar a navegação.  
  
- Eventos de navegação para monitorar e interagir com o tempo de vida de navegação.  
  
- Lembrar-se da navegação progressiva e regressiva usando um diário, que também pode ser inspecionado e manipulado.  
  
 Para obter informações, consulte [Visão geral de navegação](navigation-overview.md).  
  
 O WPF também dá suporte a um tipo especial de navegação conhecido como navegação estruturada. A navegação estruturada pode ser usada para chamar uma ou mais páginas que retornam dados de forma estruturada e previsível, consistente com a chamada de funções. Essa funcionalidade depende da classe <xref:System.Windows.Navigation.PageFunction%601>, que é descrita posteriormente em [Visão geral da navegação estruturada](structured-navigation-overview.md). O <xref:System.Windows.Navigation.PageFunction%601> também serve para simplificar a criação de topologias complexas de navegação, que são descritas em [Visão geral de topologias de navegação](navigation-topologies-overview.md).  
  
<a name="Hosting"></a>
## <a name="hosting"></a>Hosting  
 XBAPs podem ser hospedados no Microsoft Internet Explorer ou no Firefox. Cada modelo de hospedagem tem seu próprio conjunto de considerações e restrições que são abordados em [Hospedagem](hosting-wpf-applications.md).  
  
<a name="Build_and_Deploy"></a>
## <a name="build-and-deploy"></a>Compilar e implantar  
 Embora aplicativos simples do WPF possam ser criados a partir de um prompt de comando usando compiladores de linha de comando, o WPF integra-se com o Visual Studio para fornecer suporte adicional que simplificava o processo de desenvolvimento e compilação. Para obter mais informações, consulte [Compilando um aplicativo WPF](building-a-wpf-application-wpf.md).  
  
 Dependendo do tipo de aplicativo que você compilar, há uma ou mais opções de implantação para escolher. Para obter mais informações, consulte [Implantando um aplicativo WPF](deploying-a-wpf-application-wpf.md).  
  
<a name="related_topics"></a>
## <a name="related-topics"></a>Tópicos Relacionados  
  
|Title|Descrição|  
|-----------|-----------------|  
|[Visão geral de gerenciamento do aplicativo](application-management-overview.md)|Fornece uma visão geral da classe <xref:System.Windows.Application>, incluindo o gerenciamento do tempo de vida do aplicativo, janelas, recursos de aplicativos e navegação.|  
|[Janelas no WPF](windows-in-wpf-applications.md)|Fornece detalhes do gerenciamento de janelas em seu aplicativo, incluindo como usar a classe <xref:System.Windows.Window> e caixas de diálogo.|  
|[Visão geral da navegação](navigation-overview.md)|Fornece uma visão geral do gerenciamento de navegação entre páginas do seu aplicativo.|  
|[Hosting](hosting-wpf-applications.md)|Fornece uma visão geral dos aplicativos de navegador XAML (XBAPs).|  
|[Compilar e implantar](building-and-deploying-wpf-applications.md)|Descreve como compilar e implantar seu aplicativo WPF.|  
|[Introdução ao WPF no Visual Studio](../getting-started/introduction-to-wpf-in-vs.md)|Descreve os principais recursos do WPF.|  
|[Passo a passo: Meu primeiro aplicativo da área de trabalho do WPF](../getting-started/walkthrough-my-first-wpf-desktop-application.md)|Um passo a passo que mostra como criar um aplicativo WPF usando a navegação de página, layout, controles, imagens, estilos e associação.|
