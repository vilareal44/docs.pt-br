---
title: Exemplo de identidade de serviço
ms.date: 03/30/2017
ms.assetid: 79fa8c1c-85bb-4b67-bc67-bfaf721303f8
ms.openlocfilehash: e4b5e739db04fbb3270c9870468433aec7787061
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599900"
---
# <a name="service-identity-sample"></a>Exemplo de identidade de serviço
Este exemplo de identidade de serviço demonstra como definir a identidade de um serviço. Em tempo de design, um cliente pode recuperar a identidade usando os metadados do serviço e, em seguida, em tempo de execução, o cliente pode autenticar a identidade do serviço. O conceito de identidade de serviço é permitir que um cliente autentique um serviço antes de chamar qualquer uma de suas operações, protegendo assim o cliente contra chamadas não autenticadas. Em uma conexão segura, o serviço também autentica as credenciais de um cliente antes de permitir o acesso, mas esse não é o foco deste exemplo. Consulte os exemplos no [cliente](client.md) que mostram a autenticação do servidor.

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

 Este exemplo ilustra os seguintes recursos:

- Como definir os diferentes tipos de identidade em pontos de extremidade diferentes para um serviço. Cada tipo de identidade tem recursos diferentes. O tipo de identidade a ser usado depende do tipo de credenciais de segurança usadas na associação do ponto de extremidade.

- A identidade pode ser definida declarativamente na configuração ou imperativa no código. Normalmente, tanto para o cliente quanto para o serviço, você deve usar a configuração para definir a identidade.

- Como definir uma identidade personalizada no cliente. Uma identidade personalizada normalmente é uma personalização de um tipo de identidade existente que permite ao cliente examinar outras informações de declaração fornecidas nas credenciais do serviço para tomar decisões de autorização antes de chamar o serviço.

    > [!NOTE]
    > Este exemplo verifica a identidade de um certificado específico chamado identity.com e a chave RSA contida nesse certificado. Ao usar o certificado e os tipos de identidade RSA na configuração no cliente, uma maneira fácil de obter esses valores é inspecionar o WSDL para o serviço em que esses valores são serializados.

 O código de exemplo a seguir mostra como configurar a identidade de um ponto de extremidade de serviço com o DNS (servidor de nomes de domínio) de um certificado usando uma WSHttpBinding.

```csharp
//Create a service endpoint and set its identity to the certificate's DNS
WSHttpBinding wsAnonbinding = new WSHttpBinding (SecurityMode.Message);
// Client are Anonymous to the service
wsAnonbinding.Security.Message.ClientCredentialType = MessageCredentialType.None;
WServiceEndpoint ep = serviceHost.AddServiceEndpoint(typeof(ICalculator),wsAnonbinding, String.Empty);
EndpointAddress epa = new EndpointAddress(dnsrelativeAddress,EndpointIdentity.CreateDnsIdentity("identity.com"));
ep.Address = epa;
```

 A identidade também pode ser especificada na configuração no arquivo app. config. O exemplo a seguir mostra como definir a identidade UPN (nome principal do usuário) para um ponto de extremidade de serviço.

```xml
<endpoint address="upnidentity"
        behaviorConfiguration=""
        binding="wsHttpBinding"
        bindingConfiguration="WSHttpBinding_Windows"
        name="WSHttpBinding_ICalculator_Windows"
        contract="Microsoft.ServiceModel.Samples.ICalculator">
  <!-- Set the UPN identity for this endpoint -->
  <identity>
      <userPrincipalName value="host\myservice.com" />
  </identity >
</endpoint>
```

 Uma identidade personalizada pode ser definida no cliente derivando das <xref:System.ServiceModel.EndpointIdentity> <xref:System.ServiceModel.Security.IdentityVerifier> classes e. Conceitualmente, a <xref:System.ServiceModel.Security.IdentityVerifier> classe pode ser considerada como o equivalente do cliente da classe do serviço `AuthorizationManager` . O exemplo de código a seguir mostra uma implementação de `OrgEndpointIdentity` , que armazena um nome de organização para corresponder ao nome da entidade do certificado do servidor. A verificação de autorização para o nome da organização ocorre no `CheckAccess` método na `CustomIdentityVerifier` classe.

```csharp
// This custom EndpointIdentity stores an organization name
public class OrgEndpointIdentity : EndpointIdentity
{
    private string orgClaim;
    public OrgEndpointIdentity(string orgName)
    {
        orgClaim = orgName;
    }
    public string OrganizationClaim
    {
        get { return orgClaim; }
        set { orgClaim = value; }
    }
}

//This custom IdentityVerifier uses the supplied OrgEndpointIdentity to
//check that X.509 certificate's distinguished name claim contains
//the organization name e.g. the string value "O=Contoso"
class CustomIdentityVerifier : IdentityVerifier
{
    public override bool CheckAccess(EndpointIdentity identity, AuthorizationContext authContext)
    {
        bool returnvalue = false;
        foreach (ClaimSet claimset in authContext.ClaimSets)
        {
            foreach (Claim claim in claimset)
            {
                if (claim.ClaimType == "http://schemas.microsoft.com/ws/2005/05/identity/claims/x500distinguishedname")
                {
                    X500DistinguishedName name = (X500DistinguishedName)claim.Resource;
                    if (name.Name.Contains(((OrgEndpointIdentity)identity).OrganizationClaim))
                    {
                        Console.WriteLine("Claim Type: {0}",claim.ClaimType);
                        Console.WriteLine("Right: {0}", claim.Right);
                        Console.WriteLine("Resource: {0}",claim.Resource);
                        Console.WriteLine();
                        returnvalue = true;
                    }
                }
            }
        }
        return returnvalue;
    }
}
```

 Este exemplo usa um certificado chamado identity.com que está na pasta de solução de identidade específica do idioma.

### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

3. Para executar o exemplo em uma configuração de computador único ou entre computadores, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

### <a name="to-run-the-sample-on-the-same-computer"></a>Para executar o exemplo no mesmo computador

1. No Windows XP ou no Windows Vista, importe o arquivo de certificado Identity. pfx na pasta de solução de identidade para o repositório de certificados LocalMachine/My (pessoal) usando a ferramenta snap-in do MMC. Esse arquivo é protegido por senha. Durante a importação, você será solicitado a fornecer uma senha. Digite `xyz` na caixa senha. Para obter mais informações, consulte o tópico [How to: View Certificates with the MMC Snap-in](../feature-details/how-to-view-certificates-with-the-mmc-snap-in.md) . Quando isso for feito, execute Setup. bat em um Prompt de Comando do Desenvolvedor para o Visual Studio com privilégios de administrador, que copia esse certificado para o repositório de pessoas CurrentUser/confiáveis para uso no cliente.

2. No Windows Server 2003, execute Setup. bat na pasta de instalação de exemplo dentro de um prompt de comando do Visual Studio 2012 com privilégios de administrador. Isso instala todos os certificados necessários para executar o exemplo.

    > [!NOTE]
    > O arquivo em lotes setup. bat foi projetado para ser executado em um prompt de comando do Visual Studio 2012. A variável de ambiente PATH definida no prompt de comando do Visual Studio 2012 aponta para o diretório que contém os executáveis exigidos pelo script setup. bat. Certifique-se de remover os certificados executando Cleanup. bat quando tiver concluído o exemplo. Outros exemplos de segurança usam os mesmos certificados.  
  
3. Inicie o Service. exe no diretório \service\bin Verifique se o serviço indica que ele está pronto e exibe um prompt para pressionar \<Enter> para encerrar o serviço.  
  
4. Inicie o Client. exe no diretório \client\bin ou pressione F5 no Visual Studio para compilar e executar. A atividade do cliente é exibida no aplicativo de console do cliente.  
  
5. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
### <a name="to-run-the-sample-across-computers"></a>Para executar o exemplo entre computadores  
  
1. Antes de criar a parte cliente do exemplo, certifique-se de alterar o valor do endereço do ponto de extremidade do serviço no arquivo Client.cs no `CallServiceCustomClientIdentity` método. Em seguida, compile o exemplo.  
  
2. Crie um diretório no computador de serviço.  
  
3. Copie os arquivos de programa do serviço de Service \Bin para o diretório no computador do serviço. Copie também os arquivos Setup. bat e Cleanup. bat para o computador de serviço.  
  
4. Crie um diretório no computador cliente para os binários do cliente.  
  
5. Copie os arquivos de programa do cliente para o diretório cliente no computador cliente. Copie também os arquivos Setup. bat, Cleanup. bat e ImportServiceCert. bat para o cliente.  
  
6. No serviço, execute `setup.bat service` em um prompt de comando do desenvolvedor para Visual Studio aberto com privilégios de administrador. `setup.bat`A execução com o `service` argumento cria um certificado de serviço com o nome de domínio totalmente qualificado do computador e exporta o certificado de serviço para um arquivo chamado Service. cer.  
  
7. Copie o arquivo Service. cer do diretório de serviço para o diretório cliente no computador cliente.  
  
8. No arquivo client. exe. config no computador cliente, altere o valor de endereço do ponto de extremidade para corresponder ao novo endereço do serviço. Há várias instâncias que devem ser alteradas.  
  
9. No cliente, execute ImportServiceCert. bat em um Prompt de Comando do Desenvolvedor para Visual Studio aberto com privilégios de administrador. Isso importa o certificado de serviço do arquivo Service. cer para o repositório CurrentUser-TrustedPeople.  
  
10. No computador do serviço, inicie o Service. exe no prompt de comando.  
  
11. No computador cliente, inicie o Client. exe em um prompt de comando. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo  
  
- Execute o Cleanup. bat na pasta Samples depois de concluir a execução do exemplo.  
  
    > [!NOTE]
    > Esse script não remove certificados de serviço em um cliente ao executar esse exemplo em computadores. Se você tiver executado Windows Communication Foundation (WCF) exemplos que usam certificados entre computadores, certifique-se de limpar os certificados de serviço que foram instalados no repositório CurrentUser-TrustedPeople. Para fazer isso, use o seguinte comando: `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>` por exemplo: `certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com` .
