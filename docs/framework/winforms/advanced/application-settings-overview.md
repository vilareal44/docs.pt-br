---
title: Visão geral sobre configurações do aplicativo
description: Saiba mais sobre o recurso de configurações de aplicativo do Windows Forms, por exemplo, como criar e armazenar dados de configurações em nome do seu aplicativo e de seus usuários.
ms.date: 03/30/2017
f1_keywords:
- ApplicationsSettingsOverview
helpviewer_keywords:
- application settings [Windows Forms], about application settings
- dynamic properties
- user preferences [Windows Forms], tracking
ms.assetid: 0dd8bca5-a6bf-4ac4-8eec-5725d08b38dc
ms.openlocfilehash: 864cab72b26ff7989c570347fb88b4009e7d705a
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85324217"
---
# <a name="application-settings-overview"></a>Visão geral sobre configurações do aplicativo

Este artigo discute como criar e armazenar dados de configurações em nome do seu aplicativo e de seus usuários.

 O recurso Configurações de Aplicativo dos Windows Forms facilita a criação, o repositório e a manutenção de aplicativos personalizados e preferências do usuário no computador cliente. Com as configurações de aplicativo dos Windows Forms, você pode armazenar não apenas dados do aplicativo, como cadeias de conexão de banco de dados, mas também dados específicos do usuário, como as preferências de aplicativo do usuário. Usando o Visual Studio ou um código gerenciado personalizado, você pode criar novas configurações, lê-las do disco e gravá-las nele, associá-las a propriedades em seus formulários e validar os dados das configurações antes de carregar e salvar.

 As configurações do aplicativo permitem que os desenvolvedores Salvem o estado em seu aplicativo usando muito pouco código personalizado e é uma substituição das propriedades dinâmicas nas versões anteriores do .NET Framework. As configurações de aplicativo contêm muitas melhorias com relação às propriedades dinâmicas, que são somente leitura, têm associação tardia e exigem mais programação personalizada. As classes de propriedade dinâmica foram mantidas no .NET Framework 2,0, mas são apenas classes de shell que encapsulam de forma fina as classes de configurações do aplicativo.

## <a name="what-are-application-settings"></a>O que são as configurações de aplicativo?
 Seus aplicativos de Windows Forms geralmente exigirão dados críticos para a execução do aplicativo, mas que você não deseja incluir diretamente no código do aplicativo. Se seu aplicativo usar um serviço Web ou um servidor de banco de dados, talvez você queira armazenar essas informações em um arquivo separado, para que você possa alterá-las no futuro sem recompilar. Da mesma forma, seus aplicativos podem exigir o armazenamento de dados que são específicos ao usuário atual. A maioria dos aplicativos, por exemplo, tem preferências do usuário que personalizam a aparência e o comportamento do aplicativo.

 As configurações de aplicativo abordam essas duas necessidades fornecendo uma maneira fácil de armazenar configurações de escopo do aplicativo e de escopo do usuário no computador cliente. Usando o Visual Studio ou um editor de código, você define uma configuração para uma determinada propriedade especificando seu nome, tipo de dados e escopo (aplicativo ou usuário). Você pode até mesmo colocar configurações relacionadas em grupos nomeados para facilitar o uso e a legibilidade. Uma vez definidas, essas configurações são mantidas e lidas na memória automaticamente em tempo de execução. Uma arquitetura conectável permite que o mecanismo de persistência seja alterado, mas, por padrão, o sistema de arquivos local é usado.

 As configurações de aplicativo funcionam gravando dados como XML em arquivos de configuração (.config) diferentes, que indicam se a configuração tem escopo do aplicativo ou escopo do usuário. Na maioria dos casos, as configurações de escopo do aplicativo são somente leitura. Como elas são informações sobre o programa, normalmente você não precisará substituí-las. Por outro lado, as configurações de escopo do usuário poderão ser lidas e gravadas com segurança em tempo de execução, mesmo se seu aplicativo for executado com confiança parcial. Para obter mais informações sobre a confiança parcial, consulte [Visão geral da segurança dos Windows Forms](../security-in-windows-forms-overview.md).

 As configurações são armazenadas como fragmentos XML em arquivos de configuração. Configurações com escopo do aplicativo são representadas pelo elemento `<applicationSettings>` e geralmente são colocadas em *app*.exe.config, em que *app* é o nome do seu arquivo executável principal. As configurações com escopo do usuário são representadas pelo elemento `<userSettings>` e são colocadas em *usuário*.config, em que *usuário* é o nome de usuário da pessoa que está executando o aplicativo no momento. Você precisa implantar o arquivo *app*.exe.config com seu aplicativo. As configurações de arquitetura criarão os arquivos *usuário*.config sob demanda na primeira vez em que o aplicativo salvar configurações para o usuário. Você também pode definir um bloco `<userSettings>` em *app*.exe.config para fornecer valores padrão para configurações com escopo do usuário.

 Os controles personalizados também podem salvar suas próprias configurações implementando a <xref:System.Configuration.IPersistComponentSettings> interface, que expõe o <xref:System.Configuration.IPersistComponentSettings.SaveSettings%2A> método. O controle de Windows Forms <xref:System.Windows.Forms.ToolStrip> implementa essa interface para salvar a posição de barras de ferramentas e itens da barra de ferramentas entre sessões do aplicativo. Para obter mais informações sobre controles personalizados e configurações do aplicativo, consulte [Configurações do aplicativo para controles personalizados](application-settings-for-custom-controls.md).

## <a name="limitations-of-application-settings"></a>Limitações das configurações de aplicativo
 Não é possível usar as configurações do aplicativo em um aplicativo não gerenciado que hospede o .NET Framework. As configurações não funcionam em ambientes como suplementos do Visual Studio, C++ para Microsoft Office, hospedagem de controles no Internet Explorer ou projetos e suplementos do Microsoft Outlook.

 Atualmente, não é possível associar a algumas propriedades nos Windows Forms. O exemplo mais notável é a <xref:System.Windows.Forms.Form.ClientSize%2A> propriedade, pois a associação a essa propriedade causaria um comportamento imprevisível no tempo de execução. Geralmente, você pode contornar esses problemas salvando e carregando essas configurações programaticamente.

 As configurações de aplicativo não têm nenhum recurso interno para criptografar informações automaticamente. Você nunca deve armazenar informações relacionadas à segurança, como senhas de banco de dados, em texto não criptografado. Se quiser armazenar esse tipo de informações confidenciais, você, como desenvolvedor de aplicativos, será responsável por garantir sua segurança. Se quiser armazenar cadeias de conexão, recomendamos que você use a Segurança Integrada do Windows e não recorra ao hard-coding de senhas na URL. Para obter mais informações, consulte [Segurança de acesso do código e ADO.NET](../../data/adonet/code-access-security.md).

## <a name="getting-started-with-application-settings"></a>Introdução às Configurações de Aplicativo
 Se usar o Visual Studio, você poderá definir configurações dentro do Designer de Formulários do Windows usando a propriedade **(ApplicationSettings)** na janela **Propriedades**. Quando você define as configurações dessa forma, o Visual Studio cria automaticamente uma classe wrapper gerenciada personalizada que associa cada configuração a uma propriedade de classe. O Visual Studio também cuida da associação da configuração a uma propriedade em um formulário ou controle, de modo que as configurações do controle sejam restauradas automaticamente quando seu formulário é exibido e sejam salvas automaticamente quando o formulário é fechado.

 Se quiser controlar de forma mais detalhada as configurações, você poderá definir sua própria classe wrapper de configurações de aplicativo personalizada. Isso é feito pela derivação de uma classe <xref:System.Configuration.ApplicationSettingsBase> , adicionando uma propriedade que corresponde a cada configuração e aplicando atributos especiais a essas propriedades. Para obter detalhes sobre como criar classes wrapper, consulte [Arquitetura das configurações do aplicativo](application-settings-architecture.md).

 Você também pode usar a <xref:System.Windows.Forms.Binding> classe para associar configurações programaticamente a propriedades em formulários e controles.

## <a name="see-also"></a>Veja também

- <xref:System.Configuration.ApplicationSettingsBase>
- <xref:System.Configuration.SettingsProvider>
- <xref:System.Configuration.LocalFileSettingsProvider>
- <xref:System.Configuration.IPersistComponentSettings>
- [Como validar configurações do aplicativo](how-to-validate-application-settings.md)
- [Gerenciando configurações de aplicativo (.NET)](/visualstudio/ide/managing-application-settings-dotnet)
- [Como ler configurações em tempo de execução com C#](how-to-read-settings-at-run-time-with-csharp.md)
- [Usando configurações do aplicativo e configurações do usuário](using-application-settings-and-user-settings.md)
- [Arquitetura das configurações do aplicativo](application-settings-architecture.md)
- [Configurações do Aplicativo para Controles Personalizados](application-settings-for-custom-controls.md)
