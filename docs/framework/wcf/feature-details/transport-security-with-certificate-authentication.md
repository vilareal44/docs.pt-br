---
title: Segurança de transporte com autenticação de certificado
description: Saiba mais sobre como o WFC usa certificados para autenticação de servidor e cliente ao usar a segurança de transporte.
ms.date: 03/30/2017
dev_langs:
- csharp
ms.assetid: 3d726b71-4d8b-4581-a3bb-02b9af51d11b
ms.openlocfilehash: 3da1202a5ad3b953470b50dd5924b2ab45f301eb
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244772"
---
# <a name="transport-security-with-certificate-authentication"></a>Segurança de transporte com autenticação de certificado

Este artigo discute o uso de certificados X. 509 para autenticação de servidor e cliente ao usar a segurança de transporte. Para obter mais informações sobre os certificados X.509, confira [Certificados de chave pública X.509](/windows/desktop/SecCertEnroll/about-x-509-public-key-certificates). Os certificados devem ser emitidos por uma autoridade de certificação, que geralmente é um emissor de certificados de terceiros. Em um domínio do Windows Server, Active Directory serviços de certificados podem ser usados para emitir certificados para computadores cliente no domínio. Nesse cenário, o serviço é hospedado em Serviços de Informações da Internet (IIS) que é configurado com protocolo SSL (SSL). O serviço é configurado com um certificado SSL (X. 509) para permitir que os clientes verifiquem a identidade do servidor. O cliente também é configurado com um certificado X. 509 que permite que o serviço Verifique a identidade do cliente. O certificado do servidor deve ser confiável pelo cliente e o certificado do cliente deve ser confiável pelo servidor. A mecânica real de como o serviço e o cliente verifica a identidade de cada um está além do escopo deste artigo. Para obter mais informações, consulte [assinatura digital](https://en.wikipedia.org/wiki/Digital_signature) na Wikipédia.
  
 Esse cenário implementa um padrão de mensagem de solicitação/resposta, conforme ilustrado pelo diagrama a seguir.  
  
 ![Transferência segura usando certificados](media/8f7b8968-899f-4538-a9e8-0eaa872a291c.gif "8f7b8968-899f-4538-a9e8-0eaa872a291c")  
  
 Para obter mais informações sobre como usar um certificado com um serviço, consulte [trabalhando com certificados](working-with-certificates.md) e [como configurar uma porta com um certificado SSL](how-to-configure-a-port-with-an-ssl-certificate.md). A tabela a seguir descreve as várias características do cenário.  
  
|Característica|Descrição|  
|--------------------|-----------------|  
|Modo de segurança|Transporte|  
|Interoperabilidade|Com serviços e clientes de serviços Web existentes.|  
|Autenticação (servidor)<br /><br /> Autenticação (cliente)|Sim (usando um certificado SSL)<br /><br /> Sim (usando um certificado X. 509)|  
|Integridade dos Dados|Yes|  
|Confidencialidade dos dados|Yes|  
|Transporte|HTTPS|  
|Associação|<xref:System.ServiceModel.WSHttpBinding>|  
  
## <a name="configure-the-service"></a>Configurar o serviço  
 Como o serviço nesse cenário é hospedado no IIS, ele é configurado com um arquivo de web.config. O web.config a seguir mostra como configurar o <xref:System.ServiceModel.WSHttpBinding> para usar a segurança de transporte e as credenciais de cliente X. 509.  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <protocolMapping>  
      <add scheme="https" binding="wsHttpBinding" />  
    </protocolMapping>  
    <bindings>  
      <wsHttpBinding>  
        <!-- configure wsHttp binding with Transport security mode and clientCredentialType as Certificate -->  
        <binding>  
          <security mode="Transport">  
            <transport clientCredentialType="Certificate"/>
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->  
    <behaviors>  
      <serviceBehaviors>  
        <behavior>
           <serviceDebug includeExceptionDetailInFaults="True" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="configure-the-client"></a>Configurar o cliente  
 O cliente pode ser configurado no código ou em um arquivo de app.config. O exemplo a seguir mostra como configurar o cliente no código.  
  
```csharp
// Create the binding.  
var myBinding = new WSHttpBinding();  
myBinding.Security.Mode = SecurityMode.Transport;  
myBinding.Security.Transport.ClientCredentialType =  
   HttpClientCredentialType.Certificate;  
  
// Create the endpoint address. Note that the machine name
// must match the subject or DNS field of the X.509 certificate  
// used to authenticate the service.
var ea = new  
   EndpointAddress("https://localhost/CalculatorService/service.svc");  
  
// Create the client. The code for the calculator
// client is not shown here. See the sample applications  
// for examples of the calculator code.  
var cc =  
   new CalculatorClient(myBinding, ea);  
  
// The client must specify a certificate trusted by the server.  
cc.ClientCredentials.ClientCertificate.SetCertificate(  
    StoreLocation.CurrentUser,  
    StoreName.My,  
    X509FindType.FindBySubjectName,  
    "contoso.com");  
  
// Begin using the client.  
Console.WriteLine(cc.Add(100, 1111));  
//...  
cc.Close();  
```  
  
 Como alternativa, você pode configurar o cliente em um arquivo de App.config, conforme mostrado no exemplo a seguir:  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <client>  
      <!-- this endpoint has an https: address -->  
      <endpoint address=" https://localhost/CalculatorService/service.svc "
                behaviorConfiguration="endpointCredentialBehavior"  
                binding="wsHttpBinding"
                bindingConfiguration="Binding1"
                contract="Microsoft.Samples.TransportSecurity.ICalculator"/>  
    </client>  
    <behaviors>  
      <endpointBehaviors>  
        <behavior name="endpointCredentialBehavior">  
          <clientCredentials>  
            <clientCertificate findValue="contoso.com"  
                               storeLocation="CurrentUser"  
                               storeName="My"  
                               x509FindType="FindBySubjectName" />  
          </clientCredentials>  
        </behavior>  
      </endpointBehaviors>  
    </behaviors>  
    <bindings>  
      <wsHttpBinding>  
        <!-- configure wsHttpbinding with Transport security mode  
                   and clientCredentialType as Certificate -->  
        <binding name="Binding1">  
          <security mode="Transport">  
            <transport clientCredentialType="Certificate"/>  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
  </system.serviceModel>  
  
<startup><supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/></startup></configuration>  
```  
  
## <a name="see-also"></a>Veja também

- [Visão geral de segurança](security-overview.md)
- [Modelo de segurança para o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
