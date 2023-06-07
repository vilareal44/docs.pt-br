---
title: Validação de cliente
ms.date: 03/30/2017
ms.assetid: f0c1f805-1a81-4d0d-a112-bf5e2e87a631
ms.openlocfilehash: dce11ec2e3ef552c0c53e1faf89a12bc13b66ae0
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84585318"
---
# <a name="client-validation"></a>Validação de cliente
Os serviços geralmente publicam metadados para habilitar a geração automática e a configuração de tipos de proxy do cliente. Quando o serviço não é confiável, os aplicativos cliente devem validar que os metadados estão em conformidade com a política do aplicativo cliente referente à segurança, às transações, ao tipo de contrato de serviço e assim por diante. O exemplo a seguir demonstra como gravar um comportamento de ponto de extremidade de cliente que valida o ponto de extremidade de serviço para garantir que o ponto de extremidade de serviço seja seguro para uso.  
  
 O serviço expõe quatro pontos de extremidade de serviço. O primeiro ponto de extremidade usa o WSDualHttpBinding, o segundo ponto de extremidade usa a autenticação NTLM, o terceiro ponto de extremidade habilita o fluxo de transações e o quarto ponto de extremidade usa a autenticação baseada em certificado.  
  
 O cliente usa a <xref:System.ServiceModel.Description.MetadataResolver> classe para recuperar os metadados para o serviço. O cliente impõe uma política de proibir associações duplex, autenticação NTLM e fluxo de transações usando um comportamento de validação. Para cada <xref:System.ServiceModel.Description.ServiceEndpoint> instância importada dos metadados do serviço, o aplicativo cliente adiciona uma instância do `InternetClientValidatorBehavior` comportamento do ponto de extremidade ao <xref:System.ServiceModel.Description.ServiceEndpoint> antes de tentar usar um cliente Windows Communication Foundation (WCF) para se conectar ao ponto de extremidade. O método do comportamento `Validate` é executado antes de qualquer operação no serviço ser chamada e impõe a política do cliente por meio do lançamento `InvalidOperationExceptions` .  
  
### <a name="to-build-the-sample"></a>Para criar o exemplo  
  
1. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
### <a name="to-run-the-sample-on-the-same-computer"></a>Para executar o exemplo no mesmo computador  
  
1. Abra um Prompt de Comando do Desenvolvedor para o Visual Studio com privilégios de administrador e execute Setup. bat na pasta de instalação de exemplo. Isso instala todos os certificados necessários para executar o exemplo.  
  
2. Executar o aplicativo de serviço do \service\bin\Debug.  
  
3. Executar o aplicativo cliente do \client\bin\Debug. A atividade do cliente é exibida no aplicativo de console do cliente.  
  
4. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
5. Remova os certificados executando Cleanup. bat quando tiver concluído o exemplo. Outros exemplos de segurança usam os mesmos certificados.  
  
### <a name="to-run-the-sample-across-computers"></a>Para executar o exemplo entre computadores  
  
1. No servidor, em um Prompt de Comando do Desenvolvedor para o Visual Studio executar com privilégios de administrador, digite `setup.bat service` . `setup.bat`A execução com o `service` argumento cria um certificado de serviço com o nome de domínio totalmente qualificado do computador e exporta o certificado de serviço para um arquivo chamado Service. cer.  
  
2. No servidor, edite app. config para refletir o novo nome do certificado. Ou seja, altere o `findValue` atributo no [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-clientcredentials-element.md) elemento para o nome de domínio totalmente qualificado do computador.  
  
3. Copie o arquivo Service. cer do diretório de serviço para o diretório cliente no computador cliente.  
  
4. No cliente, abra um Prompt de Comando do Desenvolvedor para o Visual Studio com privilégios de administrador e digite `setup.bat client` . `setup.bat`A execução com o `client` argumento cria um certificado de cliente chamado Client.com e exporta o certificado do cliente para um arquivo chamado Client. cer.  
  
5. No arquivo client.cs, altere o valor de endereço do ponto de extremidade MEX e o `findValue` para definir o certificado de servidor padrão para corresponder ao novo endereço do serviço. Você faz isso substituindo localhost pelo nome de domínio totalmente qualificado do servidor. Constitui.  
  
6. Copie o arquivo client. cer do diretório cliente para o diretório de serviço no servidor.  
  
7. No cliente, execute ImportServiceCert. bat em um Prompt de Comando do Desenvolvedor para Visual Studio aberto com privilégios de administrador. Isso importa o certificado de serviço do arquivo Service. cer para o repositório CurrentUser-TrustedPeople.  
  
8. No servidor, execute ImportClientCert. bat em um Prompt de Comando do Desenvolvedor para Visual Studio aberto com privilégios de administrador. Isso importa o certificado do cliente do arquivo client. cer para o repositório LocalMachine-TrustedPeople.  
  
9. No computador do serviço, crie o projeto de serviço no Visual Studio e execute o Service. exe.  
  
10. No computador cliente, execute Client. exe.  
  
    1. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo  
  
- Execute o Cleanup. bat na pasta Samples depois de concluir a execução do exemplo.  
  
    > [!NOTE]
    > Esse script não remove certificados de serviço em um cliente ao executar esse exemplo em computadores. Se você tiver executado os exemplos do WCF que usam certificados entre computadores, certifique-se de limpar os certificados de serviço que foram instalados no repositório CurrentUser-TrustedPeople. Para fazer isso, use o seguinte comando: `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>. For example: certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com` .  
  
## <a name="see-also"></a>Consulte também

- [Utilizando metadados](../feature-details/using-metadata.md)
