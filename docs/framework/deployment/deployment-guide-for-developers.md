---
title: Guia de implantação do .NET Framework para desenvolvedores
description: Leia o guia de implantação do .NET para desenvolvedores. Use essas informações se desejar instalar qualquer versão do .NET da versão 4,5 para a 4,8 com seus aplicativos.
ms.date: 01/17/2020
helpviewer_keywords:
- developer's guide, deploying .NET Framework
- deployment [.NET Framework], developer's guide
ms.assetid: 094d043e-33c4-40ba-a503-e0b20b55f4cf
ms.openlocfilehash: 47946121334fe45132a7469894f30081045e3a68
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558823"
---
# <a name="net-framework-deployment-guide-for-developers"></a>Guia de implantação do .NET Framework para desenvolvedores
Este tópico fornece informações para desenvolvedores que querem instalar qualquer versão do .NET Framework a partir do .NET Framework 4.5 até o [!INCLUDE[net_current](../../../includes/net-current-version.md)] com seus aplicativos.

Você pode baixar os pacotes redistribuíveis e os pacotes de idiomas para .NET Framework nas páginas de download:

- [.NET Framework 4,8](https://dotnet.microsoft.com/download/dotnet-framework/net48)
- [.NET Framework 4.7.2](https://dotnet.microsoft.com/download/dotnet-framework/net472)
- [.NET Framework 4.7.1](https://dotnet.microsoft.com/download/dotnet-framework/net471)
- [.NET Framework 4,7](https://dotnet.microsoft.com/download/dotnet-framework/net47)
- [.NET Framework 4.6.2](https://dotnet.microsoft.com/download/dotnet-framework/net462)
- [.NET Framework 4.6.1](https://dotnet.microsoft.com/download/dotnet-framework/net461)
- [.NET Framework 4.6](https://dotnet.microsoft.com/download/dotnet-framework/net46)
- [.NET Framework 4.5.2](https://dotnet.microsoft.com/download/dotnet-framework/net452)
- [.NET Framework 4.5.1](https://dotnet.microsoft.com/download/dotnet-framework/net451)
- [.NET Framework 4.5](https://dotnet.microsoft.com/download/dotnet-framework/net45)

 Observações importantes:

- As versões do .NET Framework de .NET Framework 4.5.1 a [!INCLUDE[net_current](../../../includes/net-current-version.md)] são atualizações in-loco para o .NET Framework 4.5, o que significa que eles usam a mesma versão de runtime, mas as versões de assembly foram atualizadas e incluem novos tipos e membros.

- .NET Framework 4,5 e versões posteriores são criadas incrementalmente no .NET Framework 4. Quando você instala o .NET Framework 4,5 ou versões posteriores em um sistema que tem o .NET Framework 4 instalado, os assemblies da versão 4 são substituídos por versões mais recentes.

- Se você estiver referenciando a um [pacote fora de banda](../get-started/the-net-framework-and-out-of-band-releases.md) da Microsoft em seu aplicativo, o assembly será incluído no pacote do aplicativo.

- Você deve ter privilégios de administrador para instalar o .NET Framework 4,5 ou versões posteriores.

- .NET Framework 4,5 está incluído no Windows 8 e no Windows Server 2012, para que você não precise implantá-lo com seu aplicativo nesses sistemas operacionais. Da mesma forma, o .NET Framework 4.5.1 está incluído no Windows 8.1 e no Windows Server 2012 R2. O .NET Framework 4.5.2 não está incluído em nenhum sistema operacional. O .NET Framework 4.6 está incluído no Windows 10, o .NET Framework 4.6.1 está incluído na Atualização de novembro para Windows 10 e o .NET Framework 4.6.2 está incluído na Atualização de Aniversário do Windows 10.  O .NET Framework 4.7 está incluído na Atualização do Windows 10 para Criadores, o .NET Framework 4.7.1 está incluído na Windows 10 Fall Creators Update e o .NET Framework 4.7.2 está incluído na Atualização de outubro de 2018 para Windows 10 e na Atualização de abril de 2018 para Windows 10. O .NET Framework 4.8 está incluído na Atualização de maio de 2019 para Windows 10. Para obter uma lista completa de requisitos de hardware e software, consulte [Requisitos do sistema](../get-started/system-requirements.md).

- A partir do .NET Framework 4.5, seus usuários podem exibir uma lista dos aplicativos .NET Framework em execução durante a instalação e encerrá-los com facilidade. Isso pode ajudar a evitar reinicializações do sistema causadas por instalações do .NET Framework. Consulte [Reduzindo reinicializações do sistema](reducing-system-restarts.md).

- A desinstalação do .NET Framework 4,5 ou versões posteriores também remove arquivos .NET Framework 4 já existentes. Se quiser voltar para o .NET Framework 4, você deverá reinstalá-lo e todas as suas atualizações. Consulte [instalando o .NET Framework 4](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/5a4x27ek(v=vs.100)).

- O .NET Framework 4.5 redistribuível foi atualizado em 9 de outubro de 2012 para corrigir um problema relacionado a um carimbo de data/hora incorreto em um certificado digital, que fazia com que a assinatura digital em arquivos produzidos e assinados pela Microsoft expirassem prematuramente. Se você já instalou o pacote redistribuível do .NET Framework 4,5 com data de 16 de agosto de 2012, recomendamos que atualize sua cópia com os pacotes redistribuíveis mais recentes da [página de download do .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework/net45). Para saber mais sobre esse problema, consulte [Comunicado de Segurança da Microsoft 2749655](https://docs.microsoft.com/security-updates/SecurityAdvisories/2012/2749655).

Para obter informações sobre como um administrador do sistema pode implantar o .NET Framework e suas dependências de sistema em uma rede, consulte [Guia de implantação para administradores](guide-for-administrators.md).

## <a name="deployment-options-for-your-app"></a>Opções de implantação para seu aplicativo

Quando estiver pronto para publicar seu aplicativo em um servidor Web ou outro local centralizado para que os usuários possam instalá-lo, é possível escolher dentre diversos métodos de implantação. Alguns deles são fornecidos com o Visual Studio. A tabela a seguir lista as opções de implantação para seu aplicativo e especifica o pacote redistribuível do .NET Framework que oferece suporte a cada opção. Além dessas opções, é possível gravar um programa de instalação personalizado para seu aplicativo; para obter mais informações, consulte a seção [Encadeando a instalação do .NET Framework com a instalação do seu aplicativo](#chaining).

|Estratégia de implantação para seu aplicativo|Métodos de implantação disponíveis|.NET Framework redistribuível para uso|
|--------------------------------------|----------------------------------|-------------------------------------------|
|Instalação da Web|- [InstallAware](#installaware-deployment)<br />- [InstallShield](#installshield-deployment)<br />- [Conjunto de ferramentas WiX](#wix)<br />- [Instalação manual](#installing_manually)|[Instalador da Web](#redistributable-packages)|
|Instalação de um disco|- [InstallAware](#installaware-deployment)<br />- [InstallShield](#installshield-deployment)<br />- [Conjunto de ferramentas WiX](#wix)<br />- [Instalação manual](#installing_manually)|[Instalador offline](#redistributable-packages)|
|Instalação de uma rede local (para aplicativos corporativos)|- [ClickOnce](#clickonce-deployment)|[Instalador da Web](#redistributable-packages) (consulte [ClickOnce](#clickonce-deployment) para encontrar as restrições) ou [instalador offline](#redistributable-packages)|

## <a name="redistributable-packages"></a>Pacotes redistribuíveis

O .NET Framework está disponível em dois pacotes redistribuíveis: o instalador da Web (bootstrapper) e o instalador offline (redistribuível independente). Todos os downloads de .NET Framework são hospedados na [página de .NET Framework de download](https://dotnet.microsoft.com/download/dotnet-framework/). A tabela a seguir compara os dois pacotes:

||Instalador da Web|Instalador offline|
|-|-------------------|-----------------------|
|É necessária conexão com a Internet?|Sim|Não|
|Tamanho do download|Menor (inclui somente o instalador para a plataforma de destino)*|Maior*|
|Pacotes de idiomas|Inclusos**|Devem ser [instalados separadamente](#chain_langpack), a menos que você use o pacote destinado a todos os sistemas operacionais|
|Método de implantação|Oferece suporte a todos os métodos:<br /><br />- [ClickOnce](#clickonce-deployment)<br />- [InstallAware](#installaware-deployment)<br />- [InstallShield](#installshield-deployment)<br />- [WiX (Windows Installer XML)](#wix)<br />- [Instalação manual](#installing_manually)<br />- [Instalação personalizada (encadeamento)](#chaining)|Oferece suporte a todos os métodos:<br /><br /> - [ClickOnce](#clickonce-deployment)<br />- [InstallAware](#installaware-deployment)<br />- [InstallShield](#installshield-deployment)<br />- [WiX (Windows Installer XML)](#wix)<br />- [Instalação manual](#installing_manually)<br />- [Instalação personalizada (encadeamento)](#chaining)|

\* O instalador offline é maior porque ele contém os componentes para todas as plataformas de destino. Ao terminar de executar a instalação, o sistema operacional Windows armazena em cache somente o instalador que foi utilizado. Se o instalador offline for excluído após a instalação, o espaço em disco usado será o mesmo que aquele usado pelo instalador da Web. Se a ferramenta usada (por exemplo, [InstallAware](#installaware-deployment) ou [InstallShield](#installshield-deployment)) para criar o programa de instalação do seu aplicativo oferecer uma pasta de arquivo de instalação que é removida após a instalação, o instalador offline poderá ser excluído automaticamente colocando-o na pasta de instalação.

\*\* Se estiver usando o instalador da Web com a instalação personalizada, será possível usar configurações de idioma padrão com base na configuração da MUI (Interface do Usuário Multilíngue) do usuário ou especificar outro pacote de idiomas usando a opção `/LCID` na linha de comando. Consulte a seção [Encadeamento usando a interface do usuário padrão do .NET Framework](#chaining_default) para obter exemplos.

## <a name="deployment-methods"></a>Métodos de implantação

 Estão disponíveis quatro métodos de implantação:

- É possível definir uma dependência no .NET Framework. Você pode especificar o .NET Framework como um pré-requisito na instalação do seu aplicativo usando um destes métodos:

  - Use a [implantação do ClickOnce](#clickonce-deployment) (disponível no Visual Studio)

  - Crie um [projeto do InstallAware](#installaware-deployment) (edição gratuita disponível para usuários do Visual Studio)

  - Crie um [projeto do InstallShield](#installshield-deployment) (disponível no Visual Studio)

  - Use o [conjunto de ferramentas do WiX (Windows Installer XML)](#wix)

- Você pode solicitar que seus usuários [instalem o .NET Framework manualmente](#installing_manually).

- É possível encadear (incluir) o processo de instalação do .NET Framework na instalação do seu aplicativo e decidir como deseja lidar com a experiência de instalação do .NET Framework:

  - [Use a interface do usuário padrão](#chaining_default). Deixe que o instalador do .NET Framework forneça a experiência de instalação.

  - [Personalize a interface do usuário](#chaining_custom) para apresentar uma experiência de instalação unificada e monitorar o progresso da instalação do .NET Framework.

Esses métodos de implantação são discutidos em detalhes nas seções a seguir.

## <a name="setting-a-dependency-on-the-net-framework"></a>Configurando uma dependência no .NET Framework

Se você usar o ClickOnce, o InstallAware, o InstallShield ou o WiX para implantar seu aplicativo, poderá adicionar uma dependência no .NET Framework para que ele possa ser instalado como parte do aplicativo.

### <a name="clickonce-deployment"></a>implantação ClickOnce

A implantação do ClickOnce está disponível para projetos criados com o Visual Basic e o Visual C#, mas não está disponível para o Visual C++.

No Visual Studio, para escolher a implantação do ClickOnce e adicionar uma dependência no .NET Framework:

1. Abra o projeto do aplicativo que deseja publicar.

2. No Gerenciador de Soluções, abra o menu de atalho para seu projeto e escolha **Propriedades**.

3. Escolha o painel **Publicar**.

4. Escolha o botão **Pré-requisitos**.

5. Na caixa de diálogo **Pré-requisitos**, verifique se a caixa de seleção **Criar programa de instalação para instalar os componentes dos pré-requisitos** está marcada.

6. Na lista de pré-requisitos, localize e selecione a versão do .NET Framework que você usou para criar seu projeto.

7. Escolha uma opção para especificar o local de origem para os pré-requisitos e escolha **OK**.

     Se você fornecer uma URL para o local de download .NET Framework, poderá especificar a página de download .NET Framework ou um site da sua preferência. Se estiver colocando o pacote distribuível em seu próprio servidor, o instalador offline deve ser usado, não o instalador da Web. Você só pode vincular ao instalador da Web na página de download do .NET Framework. A URL também pode especificar um disco no qual seu próprio aplicativo está sendo distribuído.

8. Na caixa de diálogo **Páginas de Propriedade**, escolha **OK**.

<a name="installaware"></a>

### <a name="installaware-deployment"></a>Implantação do InstallAware

O InstallAware compila pacotes de aplicativos do Windows (APPX), Windows Installer (MSI), Código Nativo (EXE) e App-V (Application Virtualization) de uma única fonte. [Inclua facilmente qualquer versão do .NET Framework](https://www.installaware.com/one-click-pre-requisite-installer.htm) em sua configuração, personalizando opcionalmente a instalação [editando os scripts padrão](https://www.installaware.com/msicode.htm). Por exemplo, o InstallAware instala previamente certificados no Windows 7, sem os quais a instalação do .NET Framework 4.7 falhará. Para saber mais sobre o InstallAware, confira o site do [InstallAware para Windows Installer](https://www.installaware.com/).

### <a name="installshield-deployment"></a>Implantação do InstallShield

O InstallShield cria pacotes de aplicativos do Windows (MSIX, APPX), pacotes de Windows Installer (MSI) e instaladores de código nativo (EXE). O InstallShield também fornece a integração do Visual Studio. Para obter mais informações, consulte o site do [InstallShield](https://www.flexerasoftware.com/install/products/installshield.html) .

<a name="wix"></a>

### <a name="windows-installer-xml-wix-deployment"></a>Implantação do WiX (Windows Installer XML)

O conjunto de ferramentas do Windows Installer XML (WiX) compila pacotes de instalação do Windows a partir do código-fonte XML. O WiX oferece suporte a um ambiente de linha de comando que pode ser integrado aos processos de compilação para compilar pacotes de instalação MSI e MSM. Usando o WiX, você pode [especificar o .NET Framework como um pré-requisito](https://wixtoolset.org/documentation/manual/v3/howtos/redistributables_and_install_checks/install_dotnet.html) ou [criar um encadeador](https://wixtoolset.org/documentation/manual/v3/xsd/wix/exepackage.html) para controlar totalmente a experiência de implantação do .NET Framework. Para obter mais informações sobre o WiX, consulte o site do [Conjunto de ferramentas WiX (Windows Installer XML)](https://wixtoolset.org/).

<a name="installing_manually"></a>

## <a name="installing-the-net-framework-manually"></a>Instalando o .NET Framework manualmente

Em algumas situações, talvez não seja prático instalar automaticamente o .NET Framework com seu aplicativo. Nesse caso, você pode fazer os usuários instalarem o .NET Framework por conta própria. O pacote redistribuível está disponível em [dois pacotes](#redistributable-packages). Em seu processo de instalação, forneça instruções de como os usuários devem localizar e instalar o .NET Framework.

<a name="chaining"></a>

## <a name="chaining-the-net-framework-installation-to-your-apps-setup"></a>Encadeando a instalação do .NET Framework com a instalação do seu aplicativo

Se estiver criando um programa de instalação personalizado, é possível encadear (incluir) o processo de instalação do .NET Framework no processo de instalação do seu aplicativo. O encadeamento oferece duas opções de interface do usuário para a instalação do .NET Framework:

- Use a interface do usuário padrão fornecida pelo instalador do .NET Framework.

- Crie uma interface do usuário personalizada para a instalação do .NET Framework para manter a consistência com o programa de instalação do seu aplicativo.

Os dois métodos permitem usar o instalador da Web ou o instalador offline. Cada pacote tem suas vantagens:

- Se usar o instalador da Web, o processo de instalação do .NET Framework decidirá qual pacote de instalação é necessário, fará download e instalará somente esse pacote da Web.

- Se você usar o instalador offline, poderá incluir o conjunto completo de pacotes de instalação do .NET Framework com sua mídia de redistribuição para que seus usuários não tenham que baixar qualquer arquivo adicional da Web durante a instalação.

<a name="chaining_default"></a>

### <a name="chaining-by-using-the-default-net-framework-ui"></a>Encadeamento usando a interface do usuário padrão do .NET Framework

Para encadear silenciosamente o processo de instalação do .NET Framework e fazer com que o instalador do .NET Framework forneça a interface do usuário, adicione o seguinte comando ao seu programa de instalação:

`<.NET Framework redistributable> /q /norestart /ChainingPackage <PackageName>`

Por exemplo, se o programa executável for Contoso.exe e você quiser instalar silenciosamente o pacote redistribuível offline do .NET Framework 4.5, use o comando:

`dotNetFx45_Full_x86_x64.exe /q /norestart /ChainingPackage Contoso`

Você pode usar opções adicionais de linha de comando para personalizar a instalação. Por exemplo:

- Para os usuários fecharem os aplicativos do .NET Framework em execução para minimizar reinicializações do sistema, defina o modo passivo e use a opção `/showrmui` da seguinte maneira:

    `dotNetFx45_Full_x86_x64.exe /norestart /passive /showrmui /ChainingPackage Contoso`

     Esse comando permite que o Gerenciador de Reinicialização exiba uma caixa de mensagem que dá aos usuários a oportunidade de fechar aplicativos do .NET Framework antes da instalação do .NET Framework.

- Se estiver usando o instalador da Web, é possível usar a opção `/LCID` para especificar um pacote de idiomas. Por exemplo, para encadear o instalador da Web do .NET Framework 4.5 com seu programa de instalação do Contoso e instalar o pacote de idioma japonês, adicione o seguinte comando ao processo de instalação do seu aplicativo:

    `dotNetFx45_Full_setup.exe /q /norestart /ChainingPackage Contoso /LCID 1041`

     Se a opção `/LCID` for omitida, será instalado o pacote de idiomas correspondente à configuração de MUI do usuário.

    > [!NOTE]
    > Pacotes de idiomas diferentes podem ter diferentes datas de lançamento. Se o pacote de idiomas especificado não estiver disponível no centro de download, o .NET Framework será instalado sem o pacote de idiomas. Se o .NET Framework já estiver instalado no computador do usuário, somente o pacote de idiomas será instalado.

Para obter uma lista completa de opções, consulte a seção [Opções da linha de comando](#command-line-options).

Para obter os códigos de retorno comuns, consulte a seção [Códigos de retorno](#return-codes).

<a name="chaining_custom"></a>

### <a name="chaining-by-using-a-custom-ui"></a>Encadeamento usando uma interface do usuário personalizada

Caso tenha um pacote de instalação personalizado, você pode lançar e monitorar silenciosamente a instalação do .NET Framework enquanto mostra sua própria visualização do progresso da instalação. Nesse caso, verifique se seu código cobre o seguinte:

- Verifique os [requisitos de hardware e software do .NET Framework](../get-started/system-requirements.md).

- [Detecte](#detect_net) se a versão correta do .NET Framework já está instalada no computador do usuário.

    > [!IMPORTANT]
    > Para determinar se a versão correta do .NET Framework já está instalada, você deve verificar se sua versão de destino *ou* uma versão posterior está instalada, não se a versão de destino está instalada. Em outras palavras, você deve avaliar se a chave de versão recuperada do Registro é maior ou igual à chave de versão da sua versão de destino, *não* se ela é igual à chave de versão da versão de destino.

- [Detecte](#detecting-the-language-packs) se os pacotes de idiomas já estão instalados no computador do usuário.

- Se quiser controlar a implantação, inicialize e monitore silenciosamente o processo de instalação do .NET Framework (consulte [Como acompanhar o progresso do instalador do .NET Framework 4.5](how-to-get-progress-from-the-dotnet-installer.md)).

- Se estiver implantando o instalador offline, [encadeie os pacotes de idiomas separadamente](#chain_langpack).

- Personalize a implantação usando as [opções da linha de comando](#command-line-options). Por exemplo, se estiver encadeando o instalador da Web .NET Framework, mas quiser substituir o pacote de idiomas padrão, use a opção `/LCID`, como descrito na seção anterior.

- [Solucionar problemas](#troubleshooting).

<a name="detect_net"></a>

### <a name="detecting-the-net-framework"></a>Detectando o .NET Framework

O instalador do .NET Framework grava chaves do Registro quando a instalação é bem-sucedida. Você pode testar se o .NET Framework 4.5 ou posterior está instalado verificando a pasta `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full` no registro quanto a um valor `DWORD` chamado `Release`. (Observe que "configuração do .NET Framework" não começa com um ponto.) A existência dessa chave indica que .NET Framework 4,5 ou uma versão posterior foi instalada nesse computador. O valor de `Release` indica qual versão do .NET Framework está instalada.

> [!IMPORTANT]
> Verifique se há um valor **maior que ou igual ao** valor de palavra-chave de versão ao tentar detectar se uma versão específica está presente.

[!INCLUDE[Release key values note](~/includes/version-keys-note.md)]

|Versão|Valor da liberação de DWORD|
|-------------|--------------------------------|
|.NET Framework 4.8 instalado na Atualização de maio de 2019 para Windows 10|528040|
|.NET Framework 4.8 instalado em todas as versões do sistema operacional diferentes da Atualização de maio de 2019 para Windows 10|528049|
|O .NET framework 4.7.2 instalado na Atualização de abril de 2018 para o Windows 10 e no Windows Server, versão 1803|461808|
|O .NET Framework 4.7.2 instalado em todas as versões de sistema operacional diferentes da Atualização de abril de 2018 para o Windows 10 e do Windows Server, versão 1803. Isso inclui a Atualização de outubro de 2018 do Windows 10. |461814|
|O .NET framework 4.7.1 instalado no Windows 10 Fall Creators Update e no Windows Server, versão 1709|461308|
|O .NET framework 4.7.1 instalado em todas as versões de sistema operacional diferentes do Windows 10 Fall Creators Update e do Windows Server, versão 1709|461310|
|.NET Framework 4.7 instalado no Windows 10 Creators Update|460798|
|.NET Framework 4.7 instalado em todas as versões do sistema operacional que não a Atualização do Windows 10 para Criadores|460805|
|.NET Framework 4.6.2 instalado na Edição de Aniversário do Windows 10 e no Windows Server 2016|394802|
|.NET Framework 4.6.2 instalado em todas as versões do sistema operacional diferentes da Edição de Aniversário do Windows 10 e do Windows Server 2016|394806|
|.NET Framework 4.6.1 instalado na Atualização de novembro para Windows 10|394254|
|.NET Framework 4.6.1 instalado em todas as versões do sistema operacional diferentes da Atualização de novembro para Windows 10|394271|
|.NET Framework 4.6 instalado no Windows 10|393295|
|.NET Framework 4.6 instalado em todas as versões do sistema operacional diferentes do Windows 10|393297|
|.NET Framework 4.5.2|379893|
|.NET Framework 4.5.1 instalado com Windows 8.1 ou Windows Server 2012 R2|378675|
|.NET Framework 4.5.1 instalado no Windows 8, Windows 7|378758|
|.NET Framework 4.5|378389|

### <a name="detecting-the-language-packs"></a>Detectando os pacotes de idiomas

Você pode testar se um pacote de idiomas específico está instalado verificando a pasta HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full\\*LCID* no Registro quanto a um valor DWORD chamado `Release`. (Observe que "configuração do .NET Framework" não começa com um ponto.) *LCID* especifica um identificador de localidade; consulte [idiomas com suporte](#supported-languages) para obter uma lista desses.

Por exemplo, para detectar se o pacote de idioma japonês completo (LCID = 1041) está instalado, recupere o seguinte valor nomeado do registro:

| | |
|-|-|
| Chave | HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full\1041 |
| Nome | Versão |
| Type | DWORD |

Para determinar se a versão de lançamento final de um pacote de idiomas está instalada para uma versão específica do .NET Framework do 4.5 ao 4.7.2, verifique o valor DWORD da chave RELEASE descrito na seção anterior, [Detectando o .NET Framework](#detect_net).

<a name="chain_langpack"></a>

### <a name="chaining-the-language-packs-to-your-app-setup"></a>Encadeando os pacotes de idiomas para a instalação do seu aplicativo

O .NET Framework oferece um conjunto de arquivos executáveis de pacotes de idiomas independentes que contém recursos localizados para culturas específicas. Os pacotes de idiomas estão disponíveis no download .NET Framework páginas:

- [.NET Framework 4,8](https://dotnet.microsoft.com/download/dotnet-framework/net48)
- [.NET Framework 4.7.2](https://dotnet.microsoft.com/download/dotnet-framework/net472)
- [.NET Framework 4.7.1](https://dotnet.microsoft.com/download/dotnet-framework/net471)
- [.NET Framework 4,7](https://dotnet.microsoft.com/download/dotnet-framework/net47)
- [.NET Framework 4.6.2](https://dotnet.microsoft.com/download/dotnet-framework/net462)
- [.NET Framework 4.6.1](https://dotnet.microsoft.com/download/dotnet-framework/net461)
- [.NET Framework 4.6](https://dotnet.microsoft.com/download/dotnet-framework/net46)
- [.NET Framework 4.5.2](https://dotnet.microsoft.com/download/dotnet-framework/net452)
- [.NET Framework 4.5.1](https://dotnet.microsoft.com/download/dotnet-framework/net451)
- [.NET Framework 4.5](https://dotnet.microsoft.com/download/dotnet-framework/net45)

> [!IMPORTANT]
> Os pacotes de idiomas não contêm os componentes do .NET Framework necessários para executar um aplicativo; você deve instalar o .NET Framework usando o instalador da Web ou offline antes de instalar um pacote de idiomas.

A partir do .NET Framework 4.5.1, os nomes de pacote assumem o formato NDP<`version`>-KB<`number`>-x86-x64-AllOS-<`culture`>.exe, em que `version` é o número de versão do .NET Framework, `number` é um número de artigo da Base de Dados de Conhecimento da Microsoft e `culture` especifica um [país/região](#supported-languages). Um exemplo desses pacotes é o `NDP452-KB2901907-x86-x64-AllOS-JPN.exe`. Nomes de pacote são listados na seção [Pacotes Redistribuíveis](#redistributable-packages) anteriormente nesse artigo.

Para instalar um pacote de idiomas com o instalador offline do .NET Framework, você deve encadeá-lo com a instalação do seu aplicativo. Por exemplo, para implantar o instalador offline do .NET Framework 4.5.1 com o pacote de idioma japonês, use o seguinte comando:

`NDP451-KB2858728-x86-x64-AllOS-JPN.exe /q /norestart /ChainingPackage <ProductName>`

Não é necessário encadear os pacotes de idiomas se você utilizar o instalador da Web; o pacote de idiomas que corresponde à configuração de MUI do usuário será instalado. Se quiser instalar um idioma diferente, use a opção `/LCID` para especificar um pacote de idiomas.

Para obter uma lista completa de opções da linha de comando, consulte a seção [Opções da linha de comando](#command-line-options).

### <a name="troubleshooting"></a>Solução de problemas

#### <a name="return-codes"></a>Códigos de retorno

A tabela a seguir lista os códigos de retorno mais comuns do instalador redistribuível do .NET Framework. Os códigos de retorno são os mesmos para todas as versões do instalador. Para obter links com informações detalhadas, consulte a próxima seção.

|Código de retorno|Descrição|
|-----------------|-----------------|
|0|A instalação foi concluída com êxito.|
|1602|O usuário cancelou a instalação.|
|1603|Um erro fatal ocorreu durante a instalação.|
|1641|É necessário reiniciar para concluir a instalação. Esta mensagem indica êxito.|
|3010|É necessário reiniciar para concluir a instalação. Esta mensagem indica êxito.|
|5100|O computador do usuário não atende aos requisitos do sistema.|

#### <a name="download-error-codes"></a>Códigos de erro de download

Veja o conteúdo a seguir:

- [Códigos de erro do BITS (Serviço de Transferência Inteligente em Segundo Plano)](https://go.microsoft.com/fwlink/?LinkId=180946)

- [Códigos de erro do moniker de URL](https://go.microsoft.com/fwlink/?LinkId=180947)

- [Códigos de erro WinHttp](https://go.microsoft.com/fwlink/?LinkId=180948)

#### <a name="other-error-codes"></a>Outros códigos de erro

Veja o conteúdo a seguir:

- [Códigos de erro do Windows Installer](https://go.microsoft.com/fwlink/?LinkId=180949)

- [Códigos de resultado do Windows Update Agent](https://go.microsoft.com/fwlink/?LinkId=180951)

## <a name="uninstalling-the-net-framework"></a>Desinstalando o .NET Framework

A partir do Windows 8, você pode desinstalar o .NET Framework 4,5 ou versões posteriores usando **ativar e desativar recursos do Windows** no painel de controle. Em versões mais antigas do Windows, você pode desinstalar o .NET Framework 4,5 ou versões posteriores usando **Adicionar ou remover programas** no painel de controle.

> [!IMPORTANT]
> Para sistemas operacionais Windows 7 e versões anteriores, desinstalar o .NET Framework 4.5.1, 4.5.2, 4.6, 4.6.1, 4.6.2, 4.7, 4.7.1, 4.7.2 ou 4.8 não restaura arquivos do .NET Framework 4.5, e desinstalar o .NET Framework 4.5 não restaura arquivos do .NET Framework 4. Se quiser retornar à versão mais antiga, você deverá reinstalá-lo e todas as suas atualizações.

## <a name="appendix"></a>Apêndice

### <a name="command-line-options"></a>Opções de linha de comando

A tabela a seguir lista opções que podem ser incluídas ao encadear o redistribuível do .NET Framework 4.5 para a instalação do seu aplicativo.

|Opção|Descrição|
|------------|-----------------|
|**/CEIPConsent**|Substitui o comportamento padrão e envia comentários anônimos à Microsoft para aprimorar experiências futuras de implantação. Essa opção só pode ser usada se o programa de instalação solicitar consentimento e se o usuário conceder permissão para enviar comentários anônimos à Microsoft.|
|**/chainingpackage** `packageName`|Especifica o nome do executável que está fazendo o encadeamento. Essas informações são enviadas à Microsoft como comentários anônimos para ajudar a aprimorar experiências futuras de implantação.<br /><br /> Se o nome do pacote contiver espaços, use aspas duplas como delimitadores: **/chainingpackage "Lucerne Publishing"**. Para obter um exemplo de um pacote de encadeamento, consulte [obtendo informações de progresso de um pacote de instalação](https://docs.microsoft.com/previous-versions/cc825975(v=vs.100)).|
|**/LCID**  `LCID`<br /><br /> em que `LCID` especifica um identificador de localidade (consulte os [idiomas com suporte](#supported-languages))|Instala o pacote de idiomas especificado por `LCID` e faz com que a interface do usuário exibida seja mostrada nesse idioma, a menos que o modo silencioso seja configurado.<br /><br /> No caso do instalador da Web, essa opção instala de maneira encadeada o pacote de idiomas da Web. **Observação:** use essa opção somente com o instalador da Web.|
|**/log** `file` &#124; `folder`|Especifica o local do arquivo de log. O padrão é a pasta temporária do processo, e o nome do arquivo padrão baseia-se no pacote. Se a extensão do arquivo for .txt, é produzido um log de texto. Se qualquer outra extensão ou nenhuma extensão for especificada, é criado um log HTML.|
|**/msioptions**|Especifica opções a serem transmitidas para itens .msi e .msp, por exemplo: `/msioptions "PROPERTY1='Value'"`.|
|**/norestart**|Impede que o programa de instalação reinicialize automaticamente. Se você usar essa opção, o aplicativo de encadeamento precisará capturar o código de retorno e manipular a reinicialização (consulte [obtendo informações de progresso de um pacote de instalação](https://docs.microsoft.com/previous-versions/cc825975(v=vs.100))).|
|**/Passive**|Define o modo passivo. Exibe a barra de progresso para indicar se a instalação está em progresso, mas não exibe nenhuma solicitação ou mensagem de erro ao usuário. Nesse modo, quando encadeado por um programa de instalação, o pacote de encadeamento deve lidar com [códigos de retorno](#return-codes).|
|**/pipe**|Cria um canal de comunicação para permitir que um pacote de encadeamento obtenha o progresso.|
|**/promptrestart**|Somente modo passivo; se o programa de instalação exigir reinicialização, o usuário será avisado. Essa opção exigirá a interação do usuário se uma reinicialização for necessária.|
|**/q**|Define o modo silencioso.|
|**/Repair**|Ativa a funcionalidade de reparo.|
|**/serialdownload**|Faz com que a instalação aconteça somente após ter sido feito download do pacote.|
|**/showfinalerror**|Define o modo passivo. Exibe erros somente se a instalação não for bem-sucedida. Essa opção exigirá a interação do usuário se a instalação não for bem-sucedida.|
|**/showrmui**|Usado somente com a opção **/passive**. Exibe uma caixa de mensagem que solicita que os usuários fechem aplicativos do .NET Framework que estão em execução no momento. Essa caixa de mensagem se comporta da mesma maneira no modo passivo e não passivo.|
|**/Uninstall**|Desinstala o redistribuível do .NET Framework.|

### <a name="supported-languages"></a>Idiomas com suporte

A tabela a seguir lista .NET Framework pacotes de idiomas que estão disponíveis para o .NET Framework 4,5 e versões posteriores.

|LCID|Idioma – país/região|Cultura|
|----------|--------------------------------|-------------|
|1025|Árabe - Arábia Saudita|ar|
|1028|Chinês – Tradicional|zh-Hans|
|1029|Tcheco|cs|
|1030|Dinamarquês|da|
|1031|Alemão – Alemanha|de|
|1032|Grego|el|
|1035|Finlandês|fi|
|1036|Francês – França|fr|
|1037|Hebraico|he|
|1038|Húngaro|hu|
|1040|Italiano – Itália|it|
|1041|Japonês|ja|
|1042|Coreano|ko|
|1043|Holandês – Holanda|nl|
|1044|Norueguês (Bokmål)|não|
|1045|Polonês|pl|
|1046|Português – Brasil|pt-BR|
|1049|Russo|ru|
|1053|Sueco|sv|
|1055|Turco|tr|
|2052|Chinês – Simplificado|zh-Hans|
|2070|Português – Portugal|pt-PT|
|3082|Espanhol - Espanha (Moderno)|es|

## <a name="see-also"></a>Confira também

- [Guia de implantação para administradores](guide-for-administrators.md)
- [Requisitos do sistema](../get-started/system-requirements.md)
- [Instalar o .NET Framework para desenvolvedores](../install/guide-for-developers.md)
- [Solução de problemas de instalações e desinstalações bloqueadas do .NET Framework](../install/troubleshoot-blocked-installations-and-uninstallations.md)
- [Reduzindo reinicializações do sistema durante instalações do .NET Framework 4.5](reducing-system-restarts.md)
- [Como: Acompanhar o progresso do Instalador do .NET Framework 4.5](how-to-get-progress-from-the-dotnet-installer.md)
