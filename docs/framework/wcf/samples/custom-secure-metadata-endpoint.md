---
title: Ponto de extremidade de metadados seguros personalizados
ms.date: 03/30/2017
ms.assetid: 9e369e99-ea4a-49ff-aed2-9fdf61091a48
ms.openlocfilehash: 6e392f396b62ad2a3d3cda6e7d6ff31f186f0964
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84592431"
---
# <a name="custom-secure-metadata-endpoint"></a>Ponto de extremidade de metadados seguros personalizados
Este exemplo demonstra como implementar um serviço com um ponto de extremidade de metadados seguro que usa uma das associações de troca de não-metadados e como configurar a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) ou clientes para buscar os metadados desse ponto de extremidade de metadados. Há duas associações fornecidas pelo sistema disponíveis para expor pontos de extremidade de metadados: mexHttpBinding e mexHttpsBinding. mexHttpBinding é usado para expor um ponto de extremidade de metadados por HTTP de maneira não segura. mexHttpsBinding é usado para expor um ponto de extremidade de metadados por HTTPS de maneira segura. Este exemplo ilustra como expor um ponto de extremidade de metadados seguro usando o <xref:System.ServiceModel.WSHttpBinding> . Você desejaria fazer isso quando quiser alterar as configurações de segurança na associação, mas não quiser usar HTTPS. Se você usar o mexHttpsBinding, seu ponto de extremidade de metadados será seguro, mas não há como modificar as configurações de associação.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
## <a name="service"></a>Serviço  
 O serviço neste exemplo tem dois pontos de extremidade. O ponto de extremidade do aplicativo serve o `ICalculator` contrato em um `WSHttpBinding` com `ReliableSession` habilitado e `Message` segurança usando certificados. O ponto de extremidade de metadados também usa `WSHttpBinding` , com as mesmas configurações de segurança, mas sem `ReliableSession` . Aqui está a configuração relevante:  
  
```xml  
<services>
    <service name="Microsoft.ServiceModel.Samples.CalculatorService"  
             behaviorConfiguration="CalculatorServiceBehavior">  
     <!-- use base address provided by host -->  
     <endpoint address=""  
       binding="wsHttpBinding"  
       bindingConfiguration="Binding2"  
       contract="Microsoft.ServiceModel.Samples.ICalculator" />  
     <endpoint address="mex"  
       binding="wsHttpBinding"  
       bindingConfiguration="Binding1"  
       contract="IMetadataExchange" />  
     </service>  
 </services>  
 <bindings>  
   <wsHttpBinding>  
     <binding name="Binding1">  
       <security mode="Message">  
         <message clientCredentialType="Certificate" />  
       </security>  
     </binding>  
     <binding name="Binding2">  
       <reliableSession inactivityTimeout="00:01:00" enabled="true" />  
       <security mode="Message">  
         <message clientCredentialType="Certificate" />  
       </security>  
     </binding>  
   </wsHttpBinding>  
 </bindings>  
```  
  
 Em muitos dos outros exemplos, o ponto de extremidade de metadados usa o padrão `mexHttpBinding` , que não é seguro. Aqui, os metadados são protegidos usando `WSHttpBinding` with `Message` Security. Para que os clientes de metadados recuperem esses metadados, eles devem ser configurados com uma associação correspondente. Este exemplo demonstra dois clientes desse tipo.  
  
 O primeiro cliente usa svcutil. exe para buscar os metadados e gerar o código do cliente e a configuração em tempo de design. Como o serviço usa uma associação não padrão para os metadados, a ferramenta svcutil. exe deve ser configurada especificamente para que possa obter os metadados do serviço usando essa associação.  
  
 O segundo cliente usa o `MetadataResolver` para buscar dinamicamente os metadados de um contrato conhecido e, em seguida, invocar operações no cliente gerado dinamicamente.  
  
## <a name="svcutil-client"></a>Cliente svcutil  
 Ao usar a associação padrão para hospedar seu `IMetadataExchange` ponto de extremidade, você pode executar svcutil. exe com o endereço desse ponto de extremidade:  
  
```console  
svcutil http://localhost/servicemodelsamples/service.svc/mex  
```  
  
 e funciona. Mas, neste exemplo, o servidor usa um ponto de extremidade não padrão para hospedar os metadados. Portanto, svcutil. exe deve ser instruído para usar a ligação correta. Isso pode ser feito usando um arquivo svcutil. exe. config.  
  
 O arquivo svcutil. exe. config é semelhante a um arquivo de configuração de cliente normal. Os únicos aspectos incomuns são o nome e o contrato do ponto de extremidade do cliente:  
  
```xml  
<endpoint name="http"  
          binding="wsHttpBinding"  
          bindingConfiguration="Binding1"  
          behaviorConfiguration="ClientCertificateBehavior"  
          contract="IMetadataExchange" />  
```  
  
 O nome do ponto de extremidade deve ser o nome do esquema do endereço onde os metadados estão hospedados e o contrato do ponto de extremidade deve ser `IMetadataExchange` . Assim, quando svcutil. exe é executado com uma linha de comando, como a seguinte:  
  
```console  
svcutil http://localhost/servicemodelsamples/service.svc/mex  
```  
  
 Ele procura o ponto de extremidade chamado "http" e o contrato `IMetadataExchange` para configurar a associação e o comportamento da troca de comunicação com o ponto de extremidade de metadados. O restante do arquivo svcutil. exe. config no exemplo especifica a configuração de associação e as credenciais de comportamento para corresponder à configuração do servidor do ponto de extremidade de metadados.  
  
 Para que svcutil. exe Pegue a configuração em svcutil. exe. config, svcutil. exe deve estar no mesmo diretório que o arquivo de configuração. Como resultado, você deve copiar svcutil. exe de seu local de instalação para o diretório que contém o arquivo svcutil. exe. config. Em seguida, a partir desse diretório, execute o seguinte comando:  
  
```console  
.\svcutil.exe http://localhost/servicemodelsamples/service.svc/mex  
```  
  
 O ". \\ " líder garante que a cópia de svcutil. exe nesse diretório (aquela que tem um svcutil. exe. config) seja executada.  
  
## <a name="metadataresolver-client"></a>Cliente MetadataResolver  
 Se o cliente souber o contrato e como se comunicar com os metadados em tempo de design, o cliente poderá descobrir dinamicamente a ligação e o endereço dos pontos de extremidade do aplicativo usando o `MetadataResolver` . Este cliente de exemplo demonstra isso, mostrando como configurar a associação e as credenciais usadas pelo `MetadataResolver` criando e configurando um `MetadataExchangeClient` .  
  
 As mesmas informações de associação e certificado que apareciam em svcutil. exe. config podem ser especificadas de forma imperativa no `MetadataExchangeClient` :  
  
```csharp  
// Specify the Metadata Exchange binding and its security mode  
WSHttpBinding mexBinding = new WSHttpBinding(SecurityMode.Message);  
mexBinding.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;  
  
// Create a MetadataExchangeClient for retrieving metadata, and set the // certificate details  
MetadataExchangeClient mexClient = new MetadataExchangeClient(mexBinding);  
mexClient.SoapCredentials.ClientCertificate.SetCertificate(    StoreLocation.CurrentUser, StoreName.My,  
    X509FindType.FindBySubjectName, "client.com");  
mexClient.SoapCredentials.ServiceCertificate.Authentication.CertificateValidationMode = X509CertificateValidationMode.PeerOrChainTrust;  
mexClient.SoapCredentials.ServiceCertificate.SetDefaultCertificate(    StoreLocation.CurrentUser, StoreName.TrustedPeople,  
    X509FindType.FindBySubjectName, "localhost");  
```  
  
 Com o `mexClient` configurado, podemos enumerar os contratos nos quais estamos interessados e usar `MetadataResolver` para buscar uma lista de pontos de extremidade com esses contratos:  
  
```csharp  
// The contract we want to fetch metadata for  
Collection<ContractDescription> contracts = new Collection<ContractDescription>();  
ContractDescription contract = ContractDescription.GetContract(typeof(ICalculator));  
contracts.Add(contract);  
// Find endpoints for that contract  
EndpointAddress mexAddress = new EndpointAddress(ConfigurationManager.AppSettings["mexAddress"]);  
ServiceEndpointCollection endpoints = MetadataResolver.Resolve(contracts, mexAddress, mexClient);  
```  
  
 Por fim, podemos usar as informações desses pontos de extremidade para inicializar a ligação e o endereço de um `ChannelFactory` usado para criar canais para se comunicar com os pontos de extremidade do aplicativo.  
  
```csharp  
ChannelFactory<ICalculator> cf = new ChannelFactory<ICalculator>(endpoint.Binding, endpoint.Address);  
```  
  
 O ponto principal deste cliente de exemplo é demonstrar que, se você estiver usando o `MetadataResolver` , e precisar especificar associações ou comportamentos personalizados para a comunicação de troca de metadados, poderá usar um `MetadataExchangeClient` para especificar essas configurações personalizadas.  
  
#### <a name="to-set-up-and-build-the-sample"></a>Para configurar e compilar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
#### <a name="to-run-the-sample-on-the-same-machine"></a>Para executar o exemplo no mesmo computador  
  
1. Execute setup. bat na pasta de instalação de exemplo. Isso instala todos os certificados necessários para executar o exemplo. Observe que setup. bat usa a ferramenta FindPrivateKey. exe, que é instalada executando o setupCertTool. bat do [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Executar o aplicativo cliente do \MetadataResolverClient\bin ou \SvcutilClient\bin. A atividade do cliente é exibida no aplicativo de console do cliente.  
  
3. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
4. Remova os certificados executando Cleanup. bat quando tiver concluído o exemplo. Outros exemplos de segurança usam os mesmos certificados.  
  
#### <a name="to-run-the-sample-across-machines"></a>Para executar o exemplo entre computadores  
  
1. No servidor, execute `setup.bat service` . `setup.bat`A execução com o `service` argumento cria um certificado de serviço com o nome de domínio totalmente qualificado do computador e exporta o certificado de serviço para um arquivo chamado Service. cer.  
  
2. No servidor, edite Web. config para refletir o novo nome do certificado. Ou seja, altere o `findValue` atributo no [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-clientcredentials-element.md) elemento para o nome de domínio totalmente qualificado do computador.  
  
3. Copie o arquivo Service. cer do diretório de serviço para o diretório do cliente no computador cliente.  
  
4. No cliente, execute `setup.bat client` . `setup.bat`A execução com o `client` argumento cria um certificado de cliente chamado Client.com e exporta o certificado do cliente para um arquivo chamado Client. cer.  
  
5. No arquivo app. config do `MetadataResolverClient` no computador cliente, altere o valor de endereço do ponto de extremidade MEX para corresponder ao novo endereço do serviço. Você faz isso substituindo localhost pelo nome de domínio totalmente qualificado do servidor. Além disso, altere a ocorrência de "localhost" no arquivo metadataResolverClient.cs para o novo nome do certificado de serviço (o nome de domínio totalmente qualificado do servidor). Faça a mesma coisa para o app. config do projeto SvcutilClient.  
  
6. Copie o arquivo client. cer do diretório cliente para o diretório de serviço no servidor.  
  
7. No cliente, execute `ImportServiceCert.bat` . Isso importa o certificado de serviço do arquivo Service. cer para o repositório CurrentUser-TrustedPeople.  
  
8. No servidor, execute `ImportClientCert.bat` , que importa o certificado do cliente do arquivo. cer do cliente para o repositório LocalMachine-TrustedPeople.  
  
9. Na máquina de serviço, crie o projeto de serviço no Visual Studio e selecione a página de ajuda em um navegador da Web para verificar se ele está em execução.  
  
10. No computador cliente, execute o MetadataResolverClient ou o SvcutilClient do VS.  
  
    1. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
#### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo  
  
- Execute o Cleanup. bat na pasta Samples depois de concluir a execução do exemplo.  
  
    > [!NOTE]
    > Esse script não remove certificados de serviço em um cliente ao executar esse exemplo em computadores. Se você tiver executado Windows Communication Foundation (WCF) exemplos que usam certificados entre computadores, certifique-se de limpar os certificados de serviço que foram instalados no repositório CurrentUser-TrustedPeople. Para fazer isso, use o seguinte comando: `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>` . Por exemplo: `certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com`.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Metadata\CustomMexEndpoint`  
