---
title: Implantando o .NET Framework e aplicativos
description: Introdução à implantação do .NET com seu aplicativo. O .NET fornece aplicativos sem impacto, componentes privados por padrão, compartilhamento de código controlado e muito mais.
ms.date: 03/30/2017
helpviewer_keywords:
- deploying applications [.NET Framework], packaging
- deploying applications [.NET Framework]
- deploying applications [.NET Framework], features
- deploying applications [.NET Framework], distribution
- .NET Framework, deploying
- .NET Framework application deployment
ms.assetid: 238d8284-6042-4a38-a7f6-1ee8efd719da
ms.openlocfilehash: cce888c962c9ab83c13cce4040eb9ba50270972d
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803489"
---
# <a name="deploying-the-net-framework-and-applications"></a>Implantando o .NET Framework e aplicativos

Este artigo ajuda você a começar a implantar o .NET Framework com seu aplicativo. A maioria das informações destina-se a desenvolvedores, OEMs e administradores corporativos. Os usuários que desejam instalar o .NET Framework nos respectivos computadores devem ler o artigo [Instalando o .NET Framework](../install/index.md).

## <a name="key-deployment-resources"></a>Principais recursos de implantação

Use os links a seguir para outros tópicos do MSDN e veja as informações específicas sobre como implantar e prestar assistência ao .NET Framework.

**Configuração e implantação**

- Informações gerais sobre o instalador e a implantação:

  - Opções do instalador:

    - [Instalador da Web](../install/guide-for-developers.md#to-install-or-download-the-net-framework-redistributable)

    - [Instalador offline](../install/guide-for-developers.md#to-install-or-download-the-net-framework-redistributable)

  - Modos de instalação:

    - [Instalação silenciosa](deployment-guide-for-developers.md#chaining_custom)

    - [Exibição de uma interface do usuário](deployment-guide-for-developers.md#chaining_default)

  - [Redução de reinicializações do sistema durante instalações do .NET Framework 4.5](reducing-system-restarts.md)

  - [Solução de problemas de instalações e desinstalações bloqueadas do .NET Framework](../install/troubleshoot-blocked-installations-and-uninstallations.md)

- Implantação do .NET Framework com um aplicativo cliente (para desenvolvedores):

  - [Uso do InstallShield](deployment-guide-for-developers.md#installshield-deployment) em um projeto de configuração e implantação

  - [Uso de um aplicativo ClickOnce do Visual Studio](deployment-guide-for-developers.md#clickonce-deployment)

  - [Criação de um pacote de instalação WiX](deployment-guide-for-developers.md#wix)

  - [Uso de um instalador personalizado](deployment-guide-for-developers.md#chaining)

  - [Informações adicionais](deployment-guide-for-developers.md) para desenvolvedores

- Implantação do .NET Framework (para OEMs e administradores):

  - [Windows ADK (Kit de Avaliação e Implantação do Windows)](https://go.microsoft.com/fwlink/p/?LinkId=254976)

  - [Guia do administrador](guide-for-administrators.md)

**Manutenção**

- Para obter informações gerais, consulte o [blog .NET Framework](https://devblogs.microsoft.com/dotnet/).

- [Como detectar versões](../migration-guide/how-to-determine-which-versions-are-installed.md)

- [Como detectar service packs e atualizações](../migration-guide/how-to-determine-which-net-framework-updates-are-installed.md)

## <a name="features-that-simplify-deployment"></a>Recursos que simplificam a implantação

O .NET Framework fornece vários recursos básicos que facilitam a implantação de aplicativos:

- Aplicativos sem impacto.

     Esse recurso fornece isolamento de aplicativo e elimina conflitos de DLL. Por padrão, os componentes não afetam outros aplicativos.

- Componentes privados por padrão.

     Por padrão, os componentes são implantados no diretório de aplicativo e ficam visíveis apenas para o aplicativo que os contém.

- Compartilhamento de código controlado.

     O compartilhamento de código exige que você disponibilize explicitamente o código para compartilhamento em vez de ser o comportamento padrão.

- Controle de versão lado a lado.

     É possível a coexistência de várias versões de um componente ou aplicativo. Você pode escolher quais versões usar e o Common Language Runtime impõe a política de controle de versão.

- Implantação e replicação de XCOPY.

     Os aplicativos e componentes autodescritos e autossuficientes podem ser implantados sem entradas nem dependências de Registro.

- Atualizações dinâmicas.

     Os administradores podem usar hosts, como o ASP.NET, para atualizar DLLs de programa, mesmo em computadores remotos.

- Integração ao Windows Installer.

     O anúncio, a publicação, o reparo e a instalação sob demanda estão disponíveis na implantação do aplicativo.

- Implantação corporativa.

     Esse recurso permite a fácil distribuição de software, incluindo o uso do Active Directory.

- Download e armazenamento em cache.

     Os downloads incrementais reduzem o tamanho dos downloads, e os componentes podem ser isolados apenas para uso do aplicativo para implantação de baixo impacto.

- Código parcialmente confiável.

     A identidade se baseia no código, e não no usuário, e nenhuma caixa de diálogo de certificado é exibida.

## <a name="packaging-and-distributing-net-framework-applications"></a>Empacotando e distribuindo aplicativos .NET Framework

Algumas das informações de empacotamento e implantação para o .NET Framework são descritas em outras seções da documentação. Essas seções fornecem informações sobre a autodescrição das unidades chamadas [assemblies](../../standard/assembly/index.md), que não exigem entradas de Registro, os [assemblies com nome forte](../../standard/assembly/strong-named.md), que garantem a exclusividade do nome e impedem sua falsificação, e o [controle de versão do assembly](../../standard/assembly/versioning.md), que soluciona muitos dos problemas associados ao conflitos de DLL. As seções a seguir fornecem informações sobre empacotamento e distribuição de aplicativos .NET Framework.

### <a name="packaging"></a>Empacotamento

O .NET Framework fornece as seguintes opções para empacotamento de aplicativos:

- Como um único assembly ou uma coleção de assemblies.

     Com essa opção, você simplesmente usa os arquivos .dll ou .exe como eles foram criados.

- Como arquivos de gabinete (CAB).

     Com essa opção, você pode compactar arquivos em arquivos .cab para que a distribuição ou o download leve menos tempo.

- Como um pacote do Windows Installer ou em outros formatos de instalador.

     Com essa opção, você cria arquivos .msi para uso com o Windows Installer ou empacota o aplicativo para uso com algum outro instalador.

### <a name="distribution"></a>Distribuição

O .NET Framework fornece as seguintes opções para distribuição de aplicativos:

- Usar XCOPY ou FTP.

     Como os aplicativos Common Language Runtime são autodescritivos e não exigem entradas de Registro, você pode usar o XCOPY ou o FTP para simplesmente copiar o aplicativo para um diretório apropriado. Assim, o aplicativo pode ser executado nesse diretório.

- Usar o download de código.

     Se você estiver distribuindo o aplicativo pela Internet ou por meio de uma intranet corporativa, basta baixar o código em um computador e executar o aplicativo nele.

- Usar um programa de instalação, como o Windows Installer 2.0.

     O Windows Installer 2.0 pode instalar, reparar ou remover assemblies do .NET Framework no cache de assembly global e em diretórios privados.

### <a name="installation-location"></a>Local de instalação

Para determinar onde implantar assemblies do aplicativo para que eles possam ser encontrados pelo runtime, confira [Como o runtime localiza assemblies](how-the-runtime-locates-assemblies.md).

As considerações de segurança também podem afetar como você implanta o aplicativo. As permissões de segurança são concedidas ao código gerenciado de acordo com o local do código. Implantar um aplicativo ou componente em um local em que ele recebe pouca confiança, como a Internet, limita o que o aplicativo ou componente pode fazer. Para saber mais sobre considerações de implantação e segurança, confira [Noções básicas sobre segurança de acesso do código](../misc/code-access-security-basics.md).

## <a name="related-topics"></a>Tópicos Relacionados

|Title|Descrição|
|-----------|-----------------|
|[Como o runtime localiza assemblies](how-the-runtime-locates-assemblies.md)|Descreve como o Common Language Runtime determina qual assembly usar para atender a uma solicitação de associação.|
|[Práticas recomendadas para carregamento de assemblies](best-practices-for-assembly-loading.md)|Descreve maneiras de evitar problemas de identidade de tipo que podem levar a <xref:System.InvalidCastException>, <xref:System.MissingMethodException> e outros erros.|
|[Reduzindo reinicializações do sistema durante instalações do .NET Framework 4,5](reducing-system-restarts.md)|Descreve o Gerenciador de Reinicialização, que impede reinicializações sempre que possível, além de explicar como os aplicativos que instalam o .NET Framework podem aproveitá-lo.|
|[Guia de implantação para administradores](guide-for-administrators.md)|Explica como um administrador do sistema pode implantar o .NET Framework e suas dependências do sistema em uma rede usando o Microsoft Endpoint Configuration Manager.|
|[Guia de implantação para desenvolvedores](deployment-guide-for-developers.md)|Explica como os desenvolvedores podem instalar o .NET Framework nos computadores dos usuários com seus aplicativos.|
|[Implantando aplicativos, serviços e componentes](/visualstudio/deployment/deploying-applications-services-and-components)|Aborda as opções de implantação no Visual Studio, incluindo instruções para publicar um aplicativo usando as tecnologias ClickOnce e Windows Installer.|
|[Publicando aplicativos ClickOnce](/visualstudio/deployment/publishing-clickonce-applications)|Descreve como empacotar um aplicativo do Windows Forms e implantá-lo com o ClickOnce em computadores cliente em uma rede.|
|[Empacotando e implantando recursos](../resources/packaging-and-deploying-resources-in-desktop-apps.md)|Descreve o modelo de hub e spoke usado pelo .NET Framework para empacotar e implantar recursos; aborda convenções de nomenclatura de recurso, processo de fallback e alternativas de empacotamento.|
|[Implantando um aplicativo de interoperabilidade](../interop/deploying-an-interop-application.md)|Explica como enviar e instalar aplicativos de interoperabilidade, que geralmente incluem um assembly de cliente do .NET Framework, um ou mais assemblies de interoperabilidade que representam diferentes bibliotecas de tipo COM e um ou mais componentes COM registrados.|
|[Como acompanhar o progresso do instalador do .NET Framework 4.5](how-to-get-progress-from-the-dotnet-installer.md)|Descreve como inicializar e rastrear silenciosamente o processo de instalação do .NET Framework ao mesmo tempo que mostra sua própria exibição do progresso de instalação.|

## <a name="see-also"></a>Consulte também

- [Guia de desenvolvimento](../development-guide.md)
