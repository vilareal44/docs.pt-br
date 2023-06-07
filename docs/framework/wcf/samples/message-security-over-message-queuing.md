---
title: Segurança de mensagem através do enfileiramento de mensagem
ms.date: 03/30/2017
ms.assetid: 329aea9c-fa80-45c0-b2b9-e37fd7b85b38
ms.openlocfilehash: 7d483ff8252469e95dfbddedf31d1506848e1b45
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84584916"
---
# <a name="message-security-over-message-queuing"></a>Segurança de mensagem através do enfileiramento de mensagem
Este exemplo demonstra como implementar um aplicativo que usa o WS-Security com autenticação de certificado X. 509v3 para o cliente e requer autenticação de servidor usando o certificado X. 509v3 do servidor no MSMQ. Às vezes, a segurança de mensagem é mais desejável garantir que as mensagens no armazenamento MSMQ permaneçam criptografadas e o aplicativo possa executar sua própria autenticação da mensagem.

 Este exemplo é baseado no exemplo de [associação MSMQ transacionado](transacted-msmq-binding.md) . As mensagens são criptografadas e assinadas.

### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Se o serviço for executado primeiro, ele verificará se a fila está presente. Se a fila não estiver presente, o serviço criará uma. Você pode executar o serviço primeiro para criar a fila ou pode criar um por meio do Gerenciador de filas MSMQ. Siga estas etapas para criar uma fila no Windows 2008.

    1. Abra Gerenciador do Servidor no Visual Studio 2012.

    2. Expanda a guia **recursos** .

    3. Clique com o botão direito do mouse em **filas de mensagens particulares**e selecione **nova** **fila privada**.

    4. Marque a caixa **transacional** .

    5. Insira `ServiceModelSamplesTransacted` como o nome da nova fila.

3. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

### <a name="to-run-the-sample-on-the-same-computer"></a>Para executar o exemplo no mesmo computador

1. Verifique se o caminho inclui a pasta que contém MakeCert. exe e FindPrivateKey. exe.

2. Execute setup. bat na pasta de instalação de exemplo. Isso instala todos os certificados necessários para executar o exemplo.

    > [!NOTE]
    > Certifique-se de remover os certificados executando Cleanup. bat quando tiver concluído o exemplo. Outros exemplos de segurança usam os mesmos certificados.  
  
3. Inicie o Service. exe em \service\bin.  
  
4. Inicie o Client. exe em \client\bin. A atividade do cliente é exibida no aplicativo de console do cliente.  
  
5. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
### <a name="to-run-the-sample-across-computers"></a>Para executar o exemplo entre computadores  
  
1. Copie os arquivos Setup. bat, Cleanup. bat e ImportClientCert. bat para o computador de serviço.  
  
2. Crie um diretório no computador cliente para os binários do cliente.  
  
3. Copie os arquivos de programa do cliente para o diretório cliente no computador cliente. Copie também os arquivos Setup. bat, Cleanup. bat e ImportServiceCert. bat para o cliente.  
  
4. No servidor, execute `setup.bat service` . `setup.bat`A execução com o `service` argumento cria um certificado de serviço com o nome de domínio totalmente qualificado do computador e exporta o certificado de serviço para um arquivo chamado Service. cer.  
  
5. Edite Service. exe. config do serviço para refletir o novo nome do certificado (no `findValue` atributo no [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) ), que é o mesmo que o nome de domínio totalmente qualificado do computador.  
  
6. Copie o arquivo Service. cer do diretório de serviço para o diretório cliente no computador cliente.  
  
7. No cliente, execute `setup.bat client` . `setup.bat`A execução com o `client` argumento cria um certificado de cliente chamado Client.com e exporta o certificado do cliente para um arquivo chamado Client. cer.  
  
8. No arquivo client. exe. config no computador cliente, altere o valor de endereço do ponto de extremidade para corresponder ao novo endereço do serviço. Faça isso substituindo localhost pelo nome de domínio totalmente qualificado do servidor.  Você também deve alterar o nome do certificado do serviço para ser o mesmo que o nome de domínio totalmente qualificado do computador de serviço (no `findValue` atributo no `defaultCertificate` elemento de `serviceCertificate` `clientCredentials` ).  
  
9. Copie o arquivo client. cer do diretório cliente para o diretório de serviço no servidor.  
  
10. No cliente, execute `ImportServiceCert.bat` . Isso importa o certificado de serviço do arquivo Service. cer para o repositório CurrentUser-TrustedPeople.  
  
11. No servidor, execute `ImportClientCert.bat` , que importa o certificado do cliente do arquivo. cer do cliente para o repositório LocalMachine-TrustedPeople.  
  
12. No computador do serviço, inicie o Service. exe no prompt de comando.  
  
13. No computador cliente, inicie o Client. exe no prompt de comando. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo  
  
- Execute o Cleanup. bat na pasta Samples depois de concluir a execução do exemplo.  
  
    > [!NOTE]
    > Esse script não remove certificados de serviço em um cliente ao executar esse exemplo em computadores. Se você tiver executado Windows Communication Foundation (WCF) exemplos que usam certificados entre computadores, certifique-se de limpar os certificados de serviço que foram instalados no repositório CurrentUser-TrustedPeople. Para fazer isso, use o seguinte comando: `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>` por exemplo: `certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com` .

## <a name="requirements"></a>Requisitos
 Este exemplo requer que o MSMQ esteja instalado e em execução.

## <a name="demonstrates"></a>Demonstra
 O cliente criptografa a mensagem usando a chave pública do serviço e assina a mensagem usando seu próprio certificado. O serviço que lê a mensagem da fila autentica o certificado do cliente com o certificado em seu repositório de pessoas confiáveis. Em seguida, ele descriptografa a mensagem e envia a mensagem para a operação de serviço.

 Como a mensagem de Windows Communication Foundation (WCF) é transportada como uma carga no corpo da mensagem MSMQ, o corpo permanece criptografado no armazenamento MSMQ. Isso protege a mensagem contra a divulgação indesejada da mensagem. Observe que o MSMQ em si não reconhece se a mensagem que está carregando é criptografada.

 O exemplo demonstra como a autenticação mútua no nível da mensagem pode ser usada com o MSMQ. Os certificados são trocados fora de banda. Esse é sempre o caso com o aplicativo enfileirado porque o serviço e o cliente não precisam estar em funcionamento ao mesmo tempo.

## <a name="description"></a>Descrição
 O exemplo de código de cliente e serviço é o mesmo que o exemplo de [associação MSMQ transacionado](transacted-msmq-binding.md) com uma diferença. O contrato de operação é anotado com o nível de proteção, o que sugere que a mensagem deve ser assinada e criptografada.

```csharp
// Define a service contract.
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface IOrderProcessor
{
    [OperationContract(IsOneWay = true, ProtectionLevel=ProtectionLevel.EncryptAndSign)]
    void SubmitPurchaseOrder(PurchaseOrder po);
}
```

 Para garantir que a mensagem seja protegida usando o token necessário para identificar o serviço e o cliente, o app. config contém informações de credenciais.

 A configuração do cliente especifica o certificado de serviço para autenticar o serviço. Ele usa seu armazenamento LocalMachine como o repositório confiável para contar com a validade do serviço. Ele também especifica o certificado de cliente que é anexado com a mensagem para autenticação de serviço do cliente.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>

  <system.serviceModel>

    <client>
      <!-- Define NetMsmqEndpoint -->
      <endpoint address="net.msmq://localhost/private/ServiceModelSamplesMessageSecurity"
                binding="netMsmqBinding"
                bindingConfiguration="messageSecurityBinding"
                contract="Microsoft.ServiceModel.Samples.IOrderProcessor"
                behaviorConfiguration="ClientCertificateBehavior" />
    </client>

    <bindings>
        <netMsmqBinding>
            <binding name="messageSecurityBinding">
                <security mode="Message">
                    <message clientCredentialType="Certificate"/>
                </security>
            </binding>
        </netMsmqBinding>
    </bindings>

    <behaviors>
      <endpointBehaviors>
        <behavior name="ClientCertificateBehavior">
          <!--
        The clientCredentials behavior allows one to define a certificate to present to a service.
        A certificate is used by a client to authenticate itself to the service and provide message integrity.
        This configuration references the "client.com" certificate installed during the setup instructions.
        -->
          <clientCredentials>
            <clientCertificate findValue="client.com" storeLocation="CurrentUser" storeName="My" x509FindType="FindBySubjectName" />
            <serviceCertificate>
                <defaultCertificate findValue="localhost" storeLocation="CurrentUser" storeName="TrustedPeople" x509FindType="FindBySubjectName"/>
              <!--
            Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate
            is in the user's Trusted People store, then it is trusted without performing a
            validation of the certificate's issuer chain. This setting is used here for convenience so that the
            sample can be run without having to have certificates issued by a certification authority (CA).
            This setting is less secure than the default, ChainTrust. The security implications of this
            setting should be carefully considered before using PeerOrChainTrust in production code.
            -->
              <authentication certificateValidationMode="PeerOrChainTrust" />
            </serviceCertificate>
          </clientCredentials>
        </behavior>
      </endpointBehaviors>
    </behaviors>

  </system.serviceModel>
</configuration>
```

 Observe que o modo de segurança é definido como Message e o ClientCredentialtype é definido como Certificate.

 A configuração de serviço inclui um comportamento de serviço que especifica as credenciais do serviço que são usadas quando o cliente autentica o serviço. O nome da entidade do certificado do servidor é especificado no `findValue` atributo no [\<serviceCredentials>](../../configure-apps/file-schema/wcf/servicecredentials.md) .

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>

  <appSettings>
    <!-- Use appSetting to configure MSMQ queue name. -->
    <add key="queueName" value=".\private$\ServiceModelSamplesMessageSecurity" />
  </appSettings>

  <system.serviceModel>
    <services>
      <service
          name="Microsoft.ServiceModel.Samples.OrderProcessorService"
          behaviorConfiguration="PurchaseOrderServiceBehavior">
        <host>
          <baseAddresses>
            <add baseAddress="http://localhost:8000/ServiceModelSamples/service"/>
          </baseAddresses>
        </host>
        <!-- Define NetMsmqEndpoint -->
        <endpoint address="net.msmq://localhost/private/ServiceModelSamplesMessageSecurity"
                  binding="netMsmqBinding"
                  bindingConfiguration="messageSecurityBinding"
                  contract="Microsoft.ServiceModel.Samples.IOrderProcessor" />
        <!-- The mex endpoint is exposed at http://localhost:8000/ServiceModelSamples/service/mex. -->
        <endpoint address="mex"
                  binding="mexHttpBinding"
                  contract="IMetadataExchange" />
      </service>
    </services>

    <bindings>
        <netMsmqBinding>
            <binding name="messageSecurityBinding">
                <security mode="Message">
                    <message clientCredentialType="Certificate" />
                </security>
            </binding>
        </netMsmqBinding>
    </bindings>

    <behaviors>
      <serviceBehaviors>
        <behavior name="PurchaseOrderServiceBehavior">
          <serviceMetadata httpGetEnabled="True"/>
          <!--
               The serviceCredentials behavior allows one to define a service certificate.
               A service certificate is used by the service to authenticate itself to its clients and to provide message protection.
               This configuration references the "localhost" certificate installed during the setup instructions.
          -->
          <serviceCredentials>
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />
            <clientCertificate>
                <certificate findValue="client.com" storeLocation="LocalMachine" storeName="TrustedPeople" x509FindType="FindBySubjectName"/>
              <!--
            Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate
            is in the user's Trusted People store, then it is trusted without performing a
            validation of the certificate's issuer chain. This setting is used here for convenience so that the
            sample can be run without having to have certificates issued by a certification authority (CA).
            This setting is less secure than the default, ChainTrust. The security implications of this
            setting should be carefully considered before using PeerOrChainTrust in production code.
            -->
              <authentication certificateValidationMode="PeerOrChainTrust" />
            </clientCertificate>
          </serviceCredentials>
        </behavior>
      </serviceBehaviors>
    </behaviors>

  </system.serviceModel>

</configuration>
```

 O exemplo demonstra como controlar a autenticação usando a configuração e como obter a identidade do chamador do contexto de segurança, conforme mostrado no código de exemplo a seguir:

```csharp
// Service class which implements the service contract.
// Added code to write output to the console window.
public class OrderProcessorService : IOrderProcessor
{
    private string GetCallerIdentity()
    {
        // The client certificate is not mapped to a Windows identity by default.
        // ServiceSecurityContext.PrimaryIdentity is populated based on the information
        // in the certificate that the client used to authenticate itself to the service.
        return ServiceSecurityContext.Current.PrimaryIdentity.Name;
    }

    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
    public void SubmitPurchaseOrder(PurchaseOrder po)
    {
        Console.WriteLine("Client's Identity {0} ", GetCallerIdentity());
        Orders.Add(po);
        Console.WriteLine("Processing {0} ", po);
    }
  //…
}
```

 Quando executado, o código de serviço exibe a identificação do cliente. Veja a seguir um exemplo de saída do código de serviço:

```console
The service is ready.
Press <ENTER> to terminate service.

Client's Identity CN=client.com; ECA6629A3C695D01832D77EEE836E04891DE9D3C
Processing Purchase Order: 6536e097-da96-4773-9da3-77bab4345b5d
        Customer: somecustomer.com
        OrderDetails
                Order LineItem: 54 of Blue Widget @unit price: $29.99
                Order LineItem: 890 of Red Widget @unit price: $45.89
        Total cost of this order: $42461.56
        Order status: Pending
```

## <a name="comments"></a>Comentários

- Criando o certificado do cliente.

     A linha a seguir no arquivo em lotes cria o certificado do cliente. O nome do cliente especificado é usado no nome da entidade do certificado criado. O certificado é armazenado no `My` repositório no `CurrentUser` local da loja.

    ```bat
    echo ************
    echo making client cert
    echo ************
    makecert.exe -sr CurrentUser -ss MY -a sha1 -n CN=%CLIENT_NAME% -sky exchange -pe
    ```

- Instalando o certificado do cliente no repositório de certificados confiáveis do servidor.

     A linha a seguir no arquivo em lotes copia o certificado do cliente no repositório TrustedPeople do servidor para que o servidor possa fazer as decisões relevantes de confiança ou sem confiança. Para que um certificado instalado na TrustedPeople Store seja confiável para um serviço Windows Communication Foundation (WCF), o modo de validação de certificado do cliente deve ser definido como `PeerOrChainTrust` ou `PeerTrust` valor. Consulte o exemplo de configuração de serviço anterior para saber como isso pode ser feito usando um arquivo de configuração.

    ```bat
    echo ************
    echo copying client cert to server's LocalMachine store
    echo ************
    certmgr.exe -add -r CurrentUser -s My -c -n %CLIENT_NAME% -r LocalMachine -s TrustedPeople
    ```

- Criando o certificado do servidor.

     As linhas a seguir do arquivo em lotes setup. bat criam o certificado do servidor a ser usado:

    ```bat
    echo ************
    echo Server cert setup starting
    echo %SERVER_NAME%
    echo ************
    echo making server cert
    echo ************
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe
    ```

     A variável% SERVER_NAME% especifica o nome do servidor. O certificado é armazenado no armazenamento LocalMachine. Se o arquivo em lotes de instalação for executado com um argumento de serviço (como, `setup.bat service` ) o% server_name% conterá o nome de domínio totalmente qualificado do computador. Caso contrário, o padrão é localhost

- Instalando o certificado do servidor no repositório de certificados confiáveis do cliente.

     A linha a seguir copia o certificado do servidor no repositório de pessoas confiáveis do cliente. Essa etapa é necessária porque os certificados gerados pelo MakeCert. exe não são implicitamente confiáveis pelo sistema cliente. Se você já tiver um certificado com raiz em um certificado raiz confiável do cliente — por exemplo, um certificado emitido pela Microsoft — essa etapa de popular o repositório de certificados do cliente com o certificado do servidor não será necessária.

    ```console
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople
    ```

    > [!NOTE]
    > Se você estiver usando uma edição que não seja a U. S. inglês do Microsoft Windows, deverá editar o arquivo setup. bat e substituir o nome da conta "NT AUTHORITY\NETWORK SERVICE" pelo equivalente regional.

> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Net\MSMQ\MessageSecurity`  
