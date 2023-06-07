---
title: Compilando os exemplos do Windows Communication Foundation
ms.date: 03/30/2017
ms.assetid: 2899e7a5-9cb2-4e8d-b8d2-f31391549198
ms.openlocfilehash: 53599b3b1827651b48df9921bb59a679a36ee39c
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84592613"
---
# <a name="building-the-windows-communication-foundation-samples"></a>Compilando os exemplos do Windows Communication Foundation

Os exemplos de Windows Communication Foundation (WCF) podem ser criados usando o IDE do Visual Studio ou usando o comando **MSBuild** da linha de comando. Os dois procedimentos são descritos neste tópico.

> [!NOTE]
> Antes de criar ou executar qualquer um dos exemplos do WCF, verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

## <a name="to-build-the-sample-using-a-command-prompt"></a>Para criar o exemplo usando um prompt de comando

1. Abra Prompt de Comando do Desenvolvedor para o Visual Studio e navegue até o subdiretório específico do idioma sob o local do diretório onde você instalou o exemplo.

2. Digite `msbuild` na linha de comando. Os arquivos de programa do cliente são criados para o *client\bin* e os arquivos de programa do serviço são criados para o *create\bin*. Se o serviço for hospedado pelo Serviços de Informações da Internet (IIS), os arquivos de programa do serviço também serão copiados para o diretório *servicemodelsamples* e seu subdiretório *\Bin* .

> [!NOTE]
> Você deve definir as ACLs em *%systemdrive%\inetpub\wwwroot* para conceder permissões de modificação para a conta sob a qual você está executando. Caso contrário, ocorrerá falha em alguns eventos de Build. Como alternativa, você pode deixar as ACLs como estão e executar o prompt de comando do SDK como administrador.

## <a name="to-build-the-sample-using-visual-studio"></a>Para criar o exemplo usando Visual Studio

1. No menu **arquivo** do Visual Studio, selecione **abrir**  >  **projeto/solução**. Navegue até o subdiretório específico do idioma no diretório no qual você instalou o exemplo e clique duas vezes no ícone do arquivo. sln para abrir a solução no Visual Studio.

1. No menu **Compilar** , selecione **Recompilar solução**.

   Os arquivos de programas do cliente são criados no client\bin e os arquivos de programa do serviço são criados para o service\bin. Se o serviço estiver hospedado no IIS, os arquivos de programa do serviço também serão copiados para o diretório *servicemodelsamples* e seu subdiretório *\Bin* .

> [!NOTE]
> Você deve definir as ACLs em%systemdrive%\inetpub\wwwroot para conceder permissões de modificação para a conta sob a qual você está executando. Caso contrário, ocorrerá falha em alguns eventos de Build. Como alternativa, você pode deixar as ACLs como estão e executar o prompt de comando do SDK ou o Visual Studio como administrador. Algumas ações do Visual Studio (como anexar um depurador ao processo de trabalho do ASP.Net) também exigem privilégios administrativos.

## <a name="setup-batch-files-and-scripts"></a>Arquivos e scripts em lotes de instalação
 Os scripts e arquivos em lotes setup. exe e Cleanup. exe devem ser executados de Prompt de Comando do Desenvolvedor para o Visual Studio. Vários arquivos de configuração e limpeza executam tarefas que exigem privilégios administrativos e devem ser iniciados com privilégios de administrador.

## <a name="important-security-information-about-metadata-endpoints"></a>Informações de segurança importantes sobre os pontos de extremidade de metadados
 Para evitar a divulgação não intencional de metadados de serviço potencialmente confidenciais, a configuração padrão para Windows Communication Foundation (WCF) Services desabilita a publicação de metadados. Esse comportamento é seguro por padrão, mas também significa que você não pode usar uma ferramenta de importação de metadados (como SvcUtil. exe) para gerar o código do cliente necessário para chamar o serviço, a menos que o comportamento de publicação de metadados do serviço esteja explicitamente habilitado na configuração. Para facilitar a experiência com os exemplos, quase todos os exemplos expõem um ponto de extremidade de publicação de metadados não seguro. Esses pontos de extremidade estão potencialmente disponíveis para consumidores anônimos não autenticados e devem ser levados em vida antes da implantação desses pontos de extremidade para garantir que os metadados de um serviço sejam desmarcados publicamente. Para obter mais informações sobre como publicar metadados de serviço, consulte o exemplo de [comportamento de publicação de metadados](metadata-publishing-behavior.md) . Consulte o exemplo de [ponto de extremidade de metadados seguro personalizado](custom-secure-metadata-endpoint.md) para obter um exemplo de proteção de um ponto de extremidade de metadados.

## <a name="exception-handling"></a>Tratamento de Exceção
 Em geral, falar com esses exemplos não inclui tratamento de exceções para manter o código focado no assunto do exemplo. Para obter mais informações sobre manipulação de exceção, consulte o exemplo de [exceções esperadas](expected-exceptions.md) .

## <a name="regenerating-clients-and-configuration-with-svcutil"></a>Regenerando clientes e configuração com svcutil
 Você pode usar a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) para regenerar o código do cliente e a configuração para a maioria dos exemplos. Alguns exemplos exigem configuração editada manualmente. Por exemplo, se você usar svcutil. exe para regenerar a configuração de um exemplo que usa credenciais de certificado de cliente, será necessário especificar manualmente as credenciais configuradas anteriormente. Alguns exemplos usam opções svcutil. exe específicas para afetar o código gerado, essas opções são especificadas nos tópicos de exemplo específicos.

### <a name="to-regenerate-the-client-and-configuration-files"></a>Para regenerar os arquivos de configuração e de cliente

1. Abra um prompt de comando do SDK e navegue até o subdiretório específico do idioma no local do diretório onde você instalou o exemplo.

2. Se o serviço for um tipo hospedado na Web, use o comando a seguir.

    ```console
    svcutil.exe /n:"http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples" http://localhost/servicemodelsamples/service.svc/mex /out:generatedClient.cs
    ```

     Se o serviço for um tipo auto-hospedado, digite o comando a seguir.

    ```console
    svcutil.exe /n:"http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples" http://localhost:8000/servicemodelsamples/service.svc/mex /out:generatedClient.cs
    ```

     Substitua `http://localhost:8000/ServiceModelSamples/service.svc/mex` pelo endereço do ponto de extremidade MEX do serviço de hospedagem própria.

     Para gerar o cliente em um tipo de Visual Basic, use o comando a seguir.

    ```console
    svcutil.exe /n:"http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples" http://localhost/servicemodelsamples/service.svc/mex /l:vb /out:generatedClient.vb
    ```

     Se o serviço for um tipo auto-hospedado, use o comando a seguir.

    ```console
    svcutil.exe /n:"http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples" http://localhost:8000/servicemodelsamples/service.svc/mex /l:vb /out:generatedClient.vb
    ```

    > [!NOTE]
    > Para ignorar a geração de configuração de cliente, adicione a opção **/noconfig** .

## <a name="see-also"></a>Consulte também

- [Executando os exemplos do Windows Communication Foundation](running-the-samples.md)
- [Ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)
