---
title: Validador de certificado X.509
ms.date: 03/30/2017
ms.assetid: 3b042379-02c4-4395-b927-e57c842fd3e0
ms.openlocfilehash: 32d99b93ef014967aa04bc70f73fbd2ebcfe2c60
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594823"
---
# <a name="x509-certificate-validator"></a>Validador de certificado X.509

Este exemplo demonstra como implementar um validador de certificado X. 509 personalizado. Isso é útil em casos em que nenhum dos modos de validação de certificado X. 509 internos é apropriado para os requisitos do aplicativo. Este exemplo mostra um serviço que tem um validador personalizado que aceita certificados emitidos automaticamente. O cliente usa esse certificado para autenticar o serviço.

Observação: como qualquer pessoa pode construir um certificado emitido por conta própria, o validador personalizado usado pelo serviço é menos seguro do que o comportamento padrão fornecido pelo ChainTrust X509CertificateValidationMode. As implicações de segurança disso devem ser cuidadosamente consideradas antes de usar essa lógica de validação no código de produção.

Em resumo, este exemplo demonstra como:

- O cliente pode ser autenticado usando um certificado X. 509.

- O servidor valida as credenciais do cliente em relação a um X509CertificateValidator personalizado.

- O servidor é autenticado usando o certificado X. 509 do servidor.

O serviço expõe um único ponto de extremidade para se comunicar com o serviço, definido usando o arquivo de configuração app. config. O ponto de extremidade consiste em um endereço, uma associação e um contrato. A associação é configurada com um padrão `wsHttpBinding` que usa o `WSSecurity` e a autenticação de certificado do cliente. O comportamento do serviço especifica o modo personalizado para validar certificados X. 509 do cliente junto com o tipo da classe do validador. O comportamento também especifica o certificado do servidor usando o elemento userCertificate. O certificado do servidor deve conter o mesmo valor para o as `SubjectName` `findValue` no [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) .

```xml
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceModel.Samples.CalculatorService"
               behaviorConfiguration="CalculatorServiceBehavior">
        <!-- use host/baseAddresses to configure base address -->
        <!-- provided by host -->
        <host>
          <baseAddresses>
            <add baseAddress =
                "http://localhost:8001/servicemodelsamples/service" />
          </baseAddresses>
        </host>
        <!-- use base address specified above, provide one endpoint -->
        <endpoint address="certificate"
               binding="wsHttpBinding"
               bindingConfiguration="Binding"
               contract="Microsoft.ServiceModel.Samples.ICalculator" />
      </service>
    </services>
    <bindings>
      <wsHttpBinding>
        <!-- X509 certificate binding -->
        <binding name="Binding">
          <security mode="Message">
            <message clientCredentialType="Certificate" />
          </security>
        </binding>
      </wsHttpBinding>
    </bindings>
    <behaviors>
      <serviceBehaviors>
        <behavior name="CalculatorServiceBehavior">
          <serviceDebug includeExceptionDetailInFaults ="true"/>
          <serviceCredentials>
            <!-- The serviceCredentials behavior allows one -->
            <!-- to specify authentication constraints on -->
            <!-- client certificates. -->
            <clientCertificate>
              <!-- Setting the certificateValidationMode to -->
              <!-- Custom means that if the custom -->
              <!-- X509CertificateValidator does NOT throw -->
              <!-- an exception, then the provided certificate -->
              <!-- will be trusted without performing any -->
              <!-- validation beyond that performed by the custom -->
              <!-- validator. The security implications of this -->
              <!-- setting should be carefully considered before -->
              <!-- using Custom in production code. -->
              <authentication
                 certificateValidationMode="Custom"
                 customCertificateValidatorType =
"Microsoft.ServiceModel.Samples.CustomX509CertificateValidator, service" />
            </clientCertificate>
            <!-- The serviceCredentials behavior allows one to -->
            <!-- define a service certificate. -->
            <!-- A service certificate is used by a client to -->
            <!-- authenticate the service and provide message -->
            <!-- protection. This configuration references the -->
            <!-- "localhost" certificate installed during the setup -->
            <!-- instructions. -->
            <serviceCertificate findValue="localhost"
                 storeLocation="LocalMachine"
                 storeName="My" x509FindType="FindBySubjectName" />
          </serviceCredentials>
        </behavior>
      </serviceBehaviors>
    </behaviors>
      </system.serviceModel>
```

A configuração de ponto de extremidade do cliente consiste em um nome de configuração, um endereço absoluto para o ponto de extremidade de serviço, a associação e o contrato. A associação de cliente é configurada com o modo e a mensagem apropriados `clientCredentialType` .

```xml
<system.serviceModel>
    <client>
      <!-- X509 certificate based endpoint -->
      <endpoint name="Certificate"
        address=
        "http://localhost:8001/servicemodelsamples/service/certificate"
                binding="wsHttpBinding"
                bindingConfiguration="Binding"
                behaviorConfiguration="ClientCertificateBehavior"
                contract="Microsoft.ServiceModel.Samples.ICalculator">
      </endpoint>
    </client>
    <bindings>
        <wsHttpBinding>
            <!-- X509 certificate binding -->
            <binding name="Binding">
                <security mode="Message">
                    <message clientCredentialType="Certificate" />
               </security>
            </binding>
       </wsHttpBinding>
    </bindings>
    <behaviors>
      <endpointBehaviors>
        <behavior name="ClientCertificateBehavior">
          <clientCredentials>
            <serviceCertificate>
              <!-- Setting the certificateValidationMode to -->
              <!-- PeerOrChainTrust means that if the certificate -->
              <!-- is in the user's Trusted People store, then it -->
              <!-- is trusted without performing a validation of -->
              <!-- the certificate's issuer chain. -->
              <!-- This setting is used here for convenience so -->
              <!-- that the sample can be run without having to -->
              <!-- have certificates issued by a certification -->
              <!-- authority (CA). This setting is less secure -->
              <!-- than the default, ChainTrust. The security -->
              <!-- implications of this setting should be -->
              <!-- carefully considered before using -->
              <!-- PeerOrChainTrust in production code.-->
              <authentication
                  certificateValidationMode="PeerOrChainTrust" />
            </serviceCertificate>
          </clientCredentials>
        </behavior>
      </endpointBehaviors>
    </behaviors>
  </system.serviceModel>
```

A implementação do cliente define o certificado do cliente a ser usado.

```csharp
// Create a client with Certificate endpoint configuration
CalculatorClient client = new CalculatorClient("Certificate");
try
{
    client.ClientCredentials.ClientCertificate.SetCertificate(StoreLocation.CurrentUser, StoreName.My, X509FindType.FindBySubjectName, "test1");

    // Call the Add service operation.
    double value1 = 100.00D;
    double value2 = 15.99D;
    double result = client.Add(value1, value2);
    Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);

    // Call the Subtract service operation.
    value1 = 145.00D;
    value2 = 76.54D;
    result = client.Subtract(value1, value2);
    Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);

    // Call the Multiply service operation.
    value1 = 9.00D;
    value2 = 81.25D;
    result = client.Multiply(value1, value2);
    Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);

    // Call the Divide service operation.
    value1 = 22.00D;
    value2 = 7.00D;
    result = client.Divide(value1, value2);
    Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);
    client.Close();
}
catch (TimeoutException e)
{
    Console.WriteLine("Call timed out : {0}", e.Message);
    client.Abort();
}
catch (CommunicationException e)
{
    Console.WriteLine("Call failed : {0}", e.Message);
    client.Abort();
}
catch (Exception e)
{
    Console.WriteLine("Call failed : {0}", e.Message);
    client.Abort();
}
```

Este exemplo usa um X509CertificateValidator personalizado para validar certificados. O exemplo implementa CustomX509CertificateValidator, derivado de <xref:System.IdentityModel.Selectors.X509CertificateValidator> . Consulte a documentação sobre <xref:System.IdentityModel.Selectors.X509CertificateValidator> para obter mais informações. Este exemplo de validador personalizado específico implementa o método Validate para aceitar qualquer certificado X. 509 que seja emitido por conta própria, conforme mostrado no código a seguir.

```csharp
public class CustomX509CertificateValidator : X509CertificateValidator
{
  public override void Validate ( X509Certificate2 certificate )
  {
   // Only accept self-issued certificates
   if (certificate.Subject != certificate.Issuer)
     throw new Exception("Certificate is not self-issued");
   }
}
```

 Depois que o validador for implementado no código de serviço, o host de serviço deverá ser informado sobre a instância do validador a ser usada. Isso é feito usando o código a seguir.

```csharp
serviceHost.Credentials.ClientCertificate.Authentication.CertificateValidationMode = X509CertificateValidationMode.Custom;
serviceHost.Credentials.ClientCertificate.Authentication.CustomCertificateValidator = new CustomX509CertificateValidator();
```

 Ou você pode fazer a mesma coisa na configuração da seguinte maneira.

```xml
<behaviors>
    <serviceBehaviors>
     <behavior name="CalculatorServiceBehavior">
       ...
   <serviceCredentials>
    <!--The serviceCredentials behavior allows one to specify -->
    <!--authentication constraints on client certificates.-->
    <clientCertificate>
    <!-- Setting the certificateValidationMode to Custom means -->
    <!--that if the custom X509CertificateValidator does NOT -->
    <!--throw an exception, then the provided certificate will -->
    <!--be trusted without performing any validation beyond that -->
    <!--performed by the custom validator. The security -->
    <!--implications of this setting should be carefully -->
    <!--considered before using Custom in production code. -->
    <authentication certificateValidationMode="Custom"
       customCertificateValidatorType =
"Microsoft.ServiceModel.Samples. CustomX509CertificateValidator, service" />
   </clientCertificate>
   </serviceCredentials>
   ...
  </behavior>
 </serviceBehaviors>
</behaviors>
```

Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. O cliente deve chamar com êxito todos os métodos. Pressione ENTER na janela do cliente para desligar o cliente.

## <a name="setup-batch-file"></a>Arquivo em lotes de instalação

O arquivo em lotes setup. bat incluído com este exemplo permite que você configure o servidor com certificados relevantes para executar um aplicativo auto-hospedado que requer segurança baseada em certificado do servidor. Esse arquivo em lotes deve ser modificado para funcionar em computadores ou para funcionar em um caso não hospedado.

Veja a seguir uma breve visão geral das diferentes seções dos arquivos em lotes para que eles possam ser modificados para serem executados na configuração apropriada:

- Criando o certificado do servidor:

     As linhas a seguir do arquivo em lotes setup. bat criam o certificado do servidor a ser usado. A variável% SERVER_NAME% especifica o nome do servidor. Altere essa variável para especificar seu próprio nome de servidor. O valor padrão é localhost.

    ```bash
    echo ************
    echo Server cert setup starting
    echo %SERVER_NAME%
    echo ************
    echo making server cert
    echo ************
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe
    ```

- Instalando o certificado do servidor no repositório de certificados confiáveis do cliente:

     As linhas a seguir no arquivo em lotes setup. bat copiam o certificado do servidor no repositório de pessoas confiáveis do cliente. Essa etapa é necessária, pois os certificados gerados pelo MakeCert. exe não são implicitamente confiáveis pelo sistema cliente. Se você já tiver um certificado com raiz em um certificado raiz confiável do cliente — por exemplo, um certificado emitido pela Microsoft — esta etapa de popular o repositório de certificados do cliente com o certificado do servidor não será necessária.

    ```bash
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople
    ```

- Criando o certificado do cliente:

     As linhas a seguir do arquivo em lotes setup. bat criam o certificado do cliente a ser usado. A variável% USER_NAME% especifica o nome do cliente. Esse valor é definido como "test1" porque esse é o nome que o código do cliente procura. Se você alterar o valor de% USER_NAME%, deverá alterar o valor correspondente no arquivo de origem Client.cs e recompilar o cliente.

     O certificado é armazenado no meu repositório (pessoal) no local de armazenamento CurrentUser.

    ```bash
    echo ************
    echo Client cert setup starting
    echo %USER_NAME%
    echo ************
    echo making client cert
    echo ************
    makecert.exe -sr CurrentUser -ss MY -a sha1 -n CN=%USER_NAME% -sky exchange -pe
    ```

- Instalando o certificado do cliente no repositório de certificados confiáveis do servidor:

     As linhas a seguir no arquivo em lotes setup. bat copiam o certificado do cliente para o repositório de pessoas confiáveis. Essa etapa é necessária porque os certificados gerados pelo MakeCert. exe não são implicitamente confiáveis pelo sistema de servidor. Se você já tiver um certificado com raiz em um certificado raiz confiável — por exemplo, um certificado emitido pela Microsoft — esta etapa de popular o repositório de certificados do servidor com o certificado do cliente não será necessária.

    ```bash
    certmgr.exe -add -r CurrentUser -s My -c -n %USER_NAME% -r LocalMachine -s TrustedPeople
    ```

#### <a name="to-set-up-and-build-the-sample"></a>Para configurar e compilar o exemplo

1. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

2. Para executar o exemplo em uma configuração de computador único ou entre computadores, use as instruções a seguir.

#### <a name="to-run-the-sample-on-the-same-computer"></a>Para executar o exemplo no mesmo computador

1. Execute setup. bat da pasta de instalação de exemplo dentro de um prompt de comando do Visual Studio 2012 aberto com privilégios de administrador. Isso instala todos os certificados necessários para executar o exemplo.

    > [!IMPORTANT]
    > O arquivo em lotes setup. bat foi projetado para ser executado em um prompt de comando do Visual Studio 2012. A variável de ambiente PATH definida no prompt de comando do Visual Studio 2012 aponta para o diretório que contém os executáveis exigidos pelo script setup. bat.

2. Inicie o Service. exe em service\bin.

3. Inicie o Client. exe em \client\bin. A atividade do cliente é exibida no aplicativo de console do cliente.

4. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).

#### <a name="to-run-the-sample-across-computers"></a>Para executar o exemplo entre computadores

1. Crie um diretório no computador de serviço.

2. Copie os arquivos de programa do serviço do \service\bin para o diretório virtual no computador do serviço. Copie também os arquivos Setup. bat, Cleanup. bat, GetComputerName. vbs e ImportClientCert. bat para o computador de serviço.

3. Crie um diretório no computador cliente para os binários do cliente.

4. Copie os arquivos de programa do cliente para o diretório cliente no computador cliente. Copie também os arquivos Setup. bat, Cleanup. bat e ImportServiceCert. bat para o cliente.

5. No servidor, execute `setup.bat service` em um prompt de comando do desenvolvedor para Visual Studio aberto com privilégios de administrador. `setup.bat`A execução com o `service` argumento cria um certificado de serviço com o nome de domínio totalmente qualificado do computador e exporta o certificado de serviço para um arquivo chamado Service. cer.

6. Edite Service. exe. config para refletir o novo nome de certificado (no `findValue` atributo em [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) ), que é o mesmo que o nome de domínio totalmente qualificado do computador. Além disso, altere o nome do computador no \<service> / \<baseAddresses> elemento do localhost para o nome totalmente qualificado do seu computador de serviço.

7. Copie o arquivo Service. cer do diretório de serviço para o diretório cliente no computador cliente.

8. No cliente, execute `setup.bat client` em um prompt de comando do desenvolvedor para Visual Studio aberto com privilégios de administrador. `setup.bat`A execução com o `client` argumento cria um certificado de cliente chamado Client.com e exporta o certificado do cliente para um arquivo chamado Client. cer.

9. No arquivo client. exe. config no computador cliente, altere o valor de endereço do ponto de extremidade para corresponder ao novo endereço do serviço. Faça isso substituindo localhost pelo nome de domínio totalmente qualificado do servidor.

10. Copie o arquivo client. cer do diretório cliente para o diretório de serviço no servidor.

11. No cliente, execute ImportServiceCert. bat em um Prompt de Comando do Desenvolvedor para Visual Studio aberto com privilégios de administrador. Isso importa o certificado de serviço do arquivo Service. cer para o repositório CurrentUser-TrustedPeople.

12. No servidor, execute ImportClientCert. bat em um Prompt de Comando do Desenvolvedor para Visual Studio aberto com privilégios de administrador. Isso importa o certificado do cliente do arquivo client. cer para o repositório LocalMachine-TrustedPeople.

13. No computador servidor, inicie o Service. exe na janela do prompt de comando.

14. No computador cliente, inicie o Client. exe em uma janela de prompt de comando. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).

#### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo

1. Execute o Cleanup. bat na pasta Samples depois de concluir a execução do exemplo. Isso remove os certificados de cliente e servidor do repositório de certificados.

> [!NOTE]
> Esse script não remove certificados de serviço em um cliente ao executar esse exemplo em computadores. Se você tiver executado Windows Communication Foundation (WCF) exemplos que usam certificados entre computadores, certifique-se de limpar os certificados de serviço que foram instalados no repositório CurrentUser-TrustedPeople. Para fazer isso, use o seguinte comando: `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>` por exemplo: `certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com` .
