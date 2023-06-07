---
title: Como instalar e configurar componentes de ativação do WCF
description: Saiba como configurar o WAS (serviço de ativação de processos do Windows) no Windows Vista para hospedar serviços WCF que não se comunicam via HTTP.
ms.date: 03/30/2017
helpviewer_keywords:
- HTTP activation [WCF]
ms.assetid: 33a7054a-73ec-464d-83e5-b203aeded658
ms.openlocfilehash: 84a0dcc4fed28ebd7a536bdabfcdc389be6072d8
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246877"
---
# <a name="how-to-install-and-configure-wcf-activation-components"></a>Como instalar e configurar componentes de ativação do WCF

Este tópico descreve as etapas necessárias para configurar o serviço de ativação de processos do Windows (também conhecido como WAS) no Windows Vista para hospedar serviços de Windows Communication Foundation (WCF) que não se comunicam por protocolos de rede HTTP. As seções a seguir descrevem as etapas para essa configuração:

- Instale (ou confirme a instalação do) dos componentes de ativação do WCF.

- Configurar o WAS para dar suporte a um protocolo não HTTP. O procedimento a seguir configura o Windows Vista para ativação TCP.

Depois de instalar e configurar o WAS, consulte [como hospedar um serviço WCF no was](how-to-host-a-wcf-service-in-was.md) para os procedimentos para criar um serviço WCF que expõe um ponto de extremidade não http que emprega o was.

## <a name="to-install-the-wcf-non-http-activation-components"></a>Para instalar os componentes de ativação não HTTP do WCF

1. Clique no botão **Iniciar** e clique em **Painel de Controle**.

2. Clique em **Programas** e clique em **Programas e Recursos**.

3. No menu **tarefas** , clique em **Ativar ou desativar recursos do Windows**.

4. Localize o nó WinFX, selecione e expanda-o.

5. Selecione a caixa **componentes de ativação não http do WCF** e salve a configuração.

## <a name="to-configure-the-was-to-support-tcp-activation"></a>Para configurar o WAS para dar suporte à ativação de TCP

1. Para dar suporte à ativação net. TCP, o site padrão deve primeiro ser associado a uma porta Net. TCP. Você pode fazer isso usando Appcmd.exe, que é instalado com o conjunto de ferramentas de gerenciamento do IIS 7,0. Em uma janela de prompt de comando de nível de administrador, execute o comando a seguir.

    ```console
    %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site" -+bindings.[protocol='net.tcp',bindingInformation='808:*']
    ```

    > [!NOTE]
    > Esse comando é uma única linha de texto. Esse comando adiciona uma associação de site net. TCP ao site padrão escutando na porta TCP 808 com qualquer nome de host.

2. Embora todos os aplicativos em um site compartilhem uma associação net. TCP comum, cada aplicativo pode habilitar o suporte a net. TCP individualmente. Para habilitar net. TCP para o aplicativo, execute o comando a seguir em um prompt de comando de nível de administrador.

    ```console
    %windir%\system32\inetsrv\appcmd.exe set app
    "Default Web Site/<WCF Application>" /enabledProtocols:http,net.tcp
    ```

    > [!NOTE]
    > Esse comando é uma única linha de texto. Esse comando permite que o/ \<*WCF Application*> aplicativo seja acessado usando o `http://localhost/<WCF Application>` e o `net.tcp://localhost/<WCF Application>` .

     Remova a associação de site net. TCP que você adicionou para este exemplo.

     Como uma conveniência, as duas etapas a seguir são implementadas em um arquivo em lotes chamado RemoveNetTcpSiteBinding. cmd localizado no diretório de exemplo.

    1. Remova net. TCP da lista de protocolos habilitados executando o comando a seguir em uma janela de prompt de comando de nível de administrador.

        ```console
        %windir%\system32\inetsrv\appcmd.exe set app
        "Default Web Site/servicemodelsamples<WCF Application>" " /enabledProtocols:http
        ```

        > [!NOTE]
        > Esse comando é uma única linha de texto.

    2. Remova a associação de site net. TCP executando o seguinte comando em uma janela de prompt de comandos com privilégios elevados:

        ```console
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site"
        --bindings.[protocol='net.tcp',bindingInformation='808:*']
        ```

        > [!NOTE]
        > Esse comando é uma única linha de texto.

## <a name="to-remove-nettcp-from-the-list-of-enabled-protocols"></a>Para remover net. TCP da lista de protocolos habilitados

1. Para remover net. TCP da lista de protocolos habilitados, execute o comando a seguir em uma janela de prompt de comando de nível de administrador.

    ```console
    %windir%\system32\inetsrv\appcmd.exe set app "Default Web Site/servicemodelsamples<WCF Application>" " /enabledProtocols:http
    ```

    > [!NOTE]
    > Esse comando é uma única linha de texto.

## <a name="to-remove-the-nettcp-site-binding"></a>Para remover a associação do site net. TCP

1. Para remover a associação de site net. TCP, execute o comando a seguir em uma janela de prompt de comando de nível de administrador.

    ```console
    %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site"
    -bindings.[protocol='net.tcp',bindingInformation='808:*']
    ```

    > [!NOTE]
    > Esse comando é uma única linha de texto.

## <a name="see-also"></a>Veja também

- [Ativação TCP](../samples/tcp-activation.md)
- [Ativação de MSMQ](../samples/msmq-activation.md)
- [NamedPipe Activation](../samples/namedpipe-activation.md)
- [Recursos de hospedagem do Windows Server AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
