---
title: Transporte de WS com credencial de mensagem
ms.date: 03/30/2017
ms.assetid: 0d092f3a-b309-439b-920b-66d8f46a0e3c
ms.openlocfilehash: 0082a9df5c112b66315236aad91bc891b80d27c7
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596378"
---
# <a name="ws-transport-with-message-credential"></a>Transporte de WS com credencial de mensagem
Este exemplo demonstra o uso da segurança de transporte SSL em combinação com a credencial do cliente que está sendo executada na mensagem. Este exemplo usa a `wsHttpBinding` associação.  
  
 Por padrão, a `wsHttpBinding` associação fornece comunicação http. Quando configurado para segurança de transporte, a associação dá suporte à comunicação HTTPS. O HTTPS fornece proteção de confidencialidade e integridade para as mensagens que são transmitidas pela rede. No entanto, o conjunto de mecanismos de autenticação que pode ser usado para autenticar o cliente para o serviço é limitado ao que o transporte HTTPS dá suporte. O Windows Communication Foundation (WCF) oferece um `TransportWithMessageCredential` modo de segurança que foi projetado para superar essa limitação. Quando esse modo de segurança é configurado, a segurança de transporte é usada para fornecer confidencialidade e integridade para as mensagens transmitidas e para executar a autenticação do serviço. No entanto, a autenticação do cliente é executada colocando a credencial do cliente diretamente na mensagem. Isso permite que você use qualquer tipo de credencial com suporte no modo de segurança da mensagem para a autenticação do cliente, mantendo o benefício de desempenho do modo de segurança de transporte.  
  
 Neste exemplo, um `UserName` tipo de credencial é usado para autenticar o cliente para o serviço.  
  
 Este exemplo é baseado no [introdução](getting-started-sample.md) que implementa um serviço de calculadora. A `wsHttpBinding` associação é especificada e configurada nos arquivos de configuração do aplicativo para o cliente e o serviço.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 O código do programa no exemplo é quase idêntico ao do serviço de [introdução](getting-started-sample.md) . Há uma operação adicional fornecida pelo contrato de serviço- `GetCallerIdentity` . Essa operação retorna o nome da identidade do chamador para o chamador.  

```csharp
public string GetCallerIdentity()  
{  
    // Use ServiceSecurityContext.WindowsIdentity to get the name of the caller.  
    return ServiceSecurityContext.Current.WindowsIdentity.Name;  
}  
```

 Você deve criar um certificado e atribuí-lo usando o assistente de certificado do servidor Web antes de compilar e executar o exemplo. A definição do ponto de extremidade e a definição de associação nas configurações do arquivo de configuração habilitam o `TransportWithMessageCredential` modo de segurança, conforme mostrado no exemplo de configuração a seguir para o cliente.  
  
```xml  
<system.serviceModel>  
  <client>  
    <endpoint name=""  
              address="https://localhost/servicemodelsamples/service.svc"
              binding="wsHttpBinding"
              bindingConfiguration="Binding1"
              contract="Microsoft.ServiceModel.Samples.ICalculator" />  
  </client>  
  
  <bindings>  
    <wsHttpBinding>  
      <!--   
        This configuration defines the security mode as TransportWithMessageCredential.  
        and the clientCredentialType as UserName.  
        -->  
      <binding name="Binding1">  
        <security mode ="TransportWithMessageCredential">  
          <message clientCredentialType="UserName" />  
        </security>  
      </binding>  
    </wsHttpBinding>  
  </bindings>  
</system.serviceModel>  
```  
  
 O endereço especificado usa o `https://` esquema. A configuração de associação define o modo de segurança como `TransportWithMessageCredential` . O mesmo modo de segurança deve ser especificado no arquivo Web. config do serviço.  
  
 Como o certificado usado neste exemplo é um certificado de teste criado com MakeCert. exe, um alerta de segurança é exibido quando você tenta acessar um https: address, como `https://localhost/servicemodelsamples/service.svc` , no seu navegador. Para permitir que o cliente WCF trabalhe com um certificado de teste em vigor, algum código adicional foi adicionado ao cliente para suprimir o alerta de segurança. Esse código e a classe que o acompanham não são necessários ao usar certificados de produção.  

```csharp
// WARNING: This code is only needed for test certificates such as those created by makecert. It is
// not recommended for production code.  
PermissiveCertificatePolicy.Enact("CN=ServiceModelSamples-HTTPS-Server");  
```
  
 Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.  
  
```console  
Username authentication required.  
Provide a valid machine or domain account. [domain\\user]  
   Enter username:
YourDomainName\YourAccountName  
   Enter password:
********  
YourDomainName\YourAccountName  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
  
Press <ENTER> to terminate client.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Verifique se você executou as [instruções de instalação do certificado do servidor serviços de informações da Internet (IIS)](iis-server-certificate-installation-instructions.md).  
  
3. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
4. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
