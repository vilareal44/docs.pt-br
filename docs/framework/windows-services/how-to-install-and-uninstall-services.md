---
title: Como instalar e desinstalar os serviços do Windows
description: Consulte Como instalar e desinstalar os serviços do Windows. Se você estiver desenvolvendo um serviço do Windows com o .NET, poderá usar o InstallUtil.exe ou o PowerShell.
ms.date: 02/05/2019
helpviewer_keywords:
- Windows Service applications, deploying
- services, uninstalling
- services, installing
- installing Windows Services
- uninstalling applications, apps, Windows services
- installation, Windows services
- uninstalling Windows services
- installutil.exe tool
ms.assetid: c89c5169-f567-4305-9d62-db31a1de5481
author: ghogen
ms.openlocfilehash: 883b587a7ef60bc686d6f453c775f6651f0ccb7f
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063816"
---
# <a name="how-to-install-and-uninstall-windows-services"></a>Como instalar e desinstalar os serviços do Windows

Se você estiver desenvolvendo um serviço do Windows com o .NET Framework, poderá instalar rapidamente seu aplicativo de serviço usando o utilitário de linha de comando [*InstallUtil.exe*](../tools/installutil-exe-installer-tool.md) ou o [PowerShell](/powershell/scripting/overview). Os desenvolvedores que desejam lançar um serviço do Windows que os usuários podem instalar e desinstalar podem usar o conjunto de ferramentas do [WiX](https://wixtoolset.org/) , ou os recursos comerciais, como o [Advanced Installer](https://www.advancedinstaller.com/), o [InstallShield](https://www.revenera.com/install/products/installshield.html)ou outros. Para obter mais informações, consulte [criar um pacote de instalador (Windows Desktop)](/visualstudio/deployment/deploying-applications-services-and-components#create-an-installer-package-windows-desktop).

> [!WARNING]
> Se você deseja desinstalar um serviço do seu computador, não execute as etapas neste artigo. Nesse caso, descubra qual pacote de software ou programa instalou o serviço e, em seguida, escolha **Aplicativos** em Configurações para desinstalar o programa. Observe que muitos serviços são partes integrais do Windows, se você removê-los, poderá causar instabilidade do sistema.

Para seguir as etapas neste artigo, primeiro você precisa adicionar um instalador de serviço no serviço Windows. Para obter mais informações, consulte [Walkthrough: Criando um aplicativo de serviço do Windows](walkthrough-creating-a-windows-service-application-in-the-component-designer.md).

Não é possível executar projetos de serviço Windows diretamente no ambiente de desenvolvimento do Visual Studio pressionando F5. Antes de executar o projeto, você precisa instalar o serviço no projeto.

> [!TIP]
> Você pode usar o **Gerenciador de Servidores** para verificar se instalou ou desinstalou o serviço. Para obter mais informações, confira [Como usar o Gerenciador de Servidores no Visual Studio](https://support.microsoft.com/help/316649/how-to-use-the-server-explorer-in-visual-studio-net-and-visual-studio).

### <a name="install-your-service-manually-using-installutilexe-utility"></a>Instalar o serviço manualmente usando o utilitário InstallUtil.exe

1. No menu **Iniciar** , selecione o diretório do **Visual \<*version*> Studio** e, em seguida, selecione ** \<*version*> prompt de comando do desenvolvedor para vs **.

     O Prompt de Comando do Desenvolvedor para Visual Studio é exibido.

2. Acesse o diretório onde o arquivo executável compilado do seu projeto está localizado.

3. Execute *InstallUtil.exe* do prompt de comando com o executável do projeto como um parâmetro:

    ```console
    installutil <yourproject>.exe
    ```

     Se você estiver usando o Prompt de Comando do Desenvolvedor para Visual Studio, *InstallUtil.exe* deverá estar no caminho do sistema. Caso contrário, você poderá adicioná-lo ao caminho ou usar o caminho totalmente qualificado para invocá-lo. Essa ferramenta é instalada com o .NET Framework no *%windir%\Microsoft.NET\Framework [64] \\<framework_version \> *.

     Por exemplo:
     - para a versão de 32 bits do .NET Framework 4 ou 4.5 e posterior, se o diretório de instalação do Windows for *C:\Windows*, o caminho padrão será *C:\Windows\Microsoft.NET\Framework\v4.0.30319\InstallUtil.exe*.
     - para a versão de 64 bits do .NET Framework 4 ou 4.5 e posterior, o caminho padrão é *C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe*.

### <a name="uninstall-your-service-manually-using-installutilexe-utility"></a>Desinstalar o serviço manualmente usando o utilitário InstallUtil.exe

1. No menu **Iniciar** , selecione o diretório do **Visual \<*version*> Studio** e, em seguida, selecione ** \<*version*> prompt de comando do desenvolvedor para vs **.

     O Prompt de Comando do Desenvolvedor para Visual Studio é exibido.

2. Execute *InstallUtil.exe* no prompt de comando com a saída do projeto como um parâmetro:

    ```console
    installutil /u <yourproject>.exe
    ```

3. Depois que o executável de um serviço for excluído, o serviço ainda poderá estar presente no Registro. Nesse caso, use o comando [sc delete](/windows-server/administration/windows-commands/sc-delete) para remover a entrada do serviço do Registro.

### <a name="install-your-service-manually-using-powershell"></a>Instalar o serviço manualmente usando o PowerShell

1. No menu **Iniciar** , selecione o diretório do **Windows PowerShell** e, em seguida, selecione **Windows PowerShell**.

2. Acesse o diretório onde o arquivo executável compilado do seu projeto está localizado.

3. Execute o cmdlet [**New-Service**](/powershell/module/microsoft.powershell.management/new-service) com o com a saída do seu projeto e um nome de serviço como parâmetros:

    ```powershell
    New-Service -Name "YourServiceName" -BinaryPathName <yourproject>.exe
    ```

### <a name="uninstall-your-service-manually-using-powershell"></a>Desinstalar o serviço manualmente usando o PowerShell

1. No menu **Iniciar** , selecione o diretório do **Windows PowerShell** e, em seguida, selecione **Windows PowerShell**.

2. Execute o cmdlet [**Remove-Service**](/powershell/module/microsoft.powershell.management/remove-service) com o nome do seu serviço como parâmetro:

    ```powershell
    Remove-Service -Name "YourServiceName"
    ```

3. Depois que o executável de um serviço for excluído, o serviço ainda poderá estar presente no Registro. Nesse caso, use o comando [sc delete](/windows-server/administration/windows-commands/sc-delete) para remover a entrada do serviço do Registro.

    ```powershell
    sc.exe delete "YourServiceName"
    ```

## <a name="see-also"></a>Consulte também

- [Introdução aos aplicativos de serviço do Windows](introduction-to-windows-service-applications.md)
- [Como: criar serviços do Windows](how-to-create-windows-services.md)
- [Como: Adicionar instaladores ao seu aplicativo de serviço](how-to-add-installers-to-your-service-application.md)
- [Installutil.exe (Ferramenta de Instalação)](../tools/installutil-exe-installer-tool.md)
