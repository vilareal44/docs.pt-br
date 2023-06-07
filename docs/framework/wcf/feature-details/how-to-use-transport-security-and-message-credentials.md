---
title: Como utilizar credenciais de mensagem e segurança de transporte
description: Saiba como implementar a segurança de transporte com credenciais de mensagem, que oferece o melhor dos modos de segurança de transporte e de mensagens no WCF.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- TransportWithMessageCredentials
ms.assetid: 6cc35346-c37a-4859-b82b-946c0ba6e68f
ms.openlocfilehash: f632a4389eafc155cedcae94707c9418b6696f2c
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246643"
---
# <a name="how-to-use-transport-security-and-message-credentials"></a>Como utilizar credenciais de mensagem e segurança de transporte
A proteção de um serviço com as credenciais de transporte e de mensagem usa o melhor dos modos de segurança de transporte e de mensagem no Windows Communication Foundation (WCF). Em Sum, a segurança da camada de transporte fornece integridade e confidencialidade, enquanto a segurança da camada de mensagens fornece uma variedade de credenciais que não são possíveis com mecanismos de segurança de transporte estritos. Este tópico mostra as etapas básicas para implementar o transporte com credenciais de mensagem usando as <xref:System.ServiceModel.WSHttpBinding> <xref:System.ServiceModel.NetTcpBinding> associações e. Para obter mais informações sobre como definir o modo de segurança, consulte [como: definir o modo de segurança](../how-to-set-the-security-mode.md).  
  
 Ao definir o modo de segurança como `TransportWithMessageCredential` , o transporte determina o mecanismo real que fornece a segurança em nível de transporte. Para HTTP, o mecanismo é protocolo SSL (SSL) via HTTP (HTTPS); para TCP, é SSL sobre TCP ou Windows.  
  
 Se o transporte for HTTP (usando o <xref:System.ServiceModel.WSHttpBinding> ), o SSL sobre http fornecerá a segurança em nível de transporte. Nesse caso, você deve configurar o computador que hospeda o serviço com um certificado SSL associado a uma porta, conforme mostrado posteriormente neste tópico.  
  
 Se o transporte for TCP (usando o <xref:System.ServiceModel.NetTcpBinding> ), por padrão, a segurança no nível de transporte fornecida é a segurança do Windows ou SSL sobre TCP. Ao usar SSL sobre TCP, você deve especificar o certificado usando o <xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential.SetCertificate%2A> método, conforme mostrado posteriormente neste tópico.  
  
### <a name="to-use-the-wshttpbinding-with-a-certificate-for-transport-security-in-code"></a>Para usar o WSHttpBinding com um certificado para segurança de transporte (no código)  
  
1. Use a ferramenta HttpCfg.exe para associar um certificado SSL a uma porta no computador. Para obter mais informações, consulte [como: configurar uma porta com um certificado SSL](how-to-configure-a-port-with-an-ssl-certificate.md).  
  
2. Crie uma instância da <xref:System.ServiceModel.WSHttpBinding> classe e defina a <xref:System.ServiceModel.WSHttpSecurity.Mode%2A> propriedade como <xref:System.ServiceModel.SecurityMode.TransportWithMessageCredential> .  
  
3. Defina a propriedade <xref:System.ServiceModel.HttpTransportSecurity.ClientCredentialType%2A> com um valor apropriado. (Para obter mais informações, consulte [selecionando um tipo de credencial](selecting-a-credential-type.md).) O código a seguir usa o <xref:System.ServiceModel.MessageCredentialType.Certificate> valor.  
  
4. Crie uma instância da <xref:System.Uri> classe com um endereço base apropriado. Observe que o endereço deve usar o esquema "HTTPS" e deve conter o nome real do computador e o número da porta ao qual o certificado SSL está associado. (Como alternativa, você pode definir o endereço base na configuração.)  
  
5. Adicione um ponto de extremidade de serviço usando o <xref:System.ServiceModel.ServiceHost.AddServiceEndpoint%2A> método.  
  
6. Crie a instância do <xref:System.ServiceModel.ServiceHost> e chame o <xref:System.ServiceModel.ICommunicationObject.Open%2A> método, conforme mostrado no código a seguir.  
  
     [!code-csharp[c_SettingSecurityMode#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_settingsecuritymode/cs/source.cs#7)]
     [!code-vb[c_SettingSecurityMode#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_settingsecuritymode/vb/source.vb#7)]  
  
### <a name="to-use-the-nettcpbinding-with-a-certificate-for-transport-security-in-code"></a>Para usar o NetTcpbinding com um certificado para segurança de transporte (no código)  
  
1. Crie uma instância da <xref:System.ServiceModel.NetTcpBinding> classe e defina a <xref:System.ServiceModel.NetTcpSecurity.Mode%2A> propriedade como <xref:System.ServiceModel.SecurityMode.TransportWithMessageCredential> .  
  
2. Defina <xref:System.ServiceModel.MessageSecurityOverTcp.ClientCredentialType%2A> como um valor apropriado. O código a seguir usa o <xref:System.ServiceModel.MessageCredentialType.Certificate> valor.  
  
3. Crie uma instância da <xref:System.Uri> classe com um endereço base apropriado. Observe que o endereço deve usar o esquema "net. TCP". (Como alternativa, você pode definir o endereço base na configuração.)  
  
4. Crie a instância da <xref:System.ServiceModel.ServiceHost> classe.  
  
5. Use o <xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential.SetCertificate%2A> método da <xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential> classe para definir explicitamente o certificado X. 509 para o serviço.  
  
6. Adicione um ponto de extremidade de serviço usando o <xref:System.ServiceModel.ServiceHost.AddServiceEndpoint%2A> método.  
  
7. Chame o <xref:System.ServiceModel.ICommunicationObject.Open%2A> método, conforme mostrado no código a seguir.  
  
     [!code-csharp[c_SettingSecurityMode#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_settingsecuritymode/cs/source.cs#8)]
     [!code-vb[c_SettingSecurityMode#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_settingsecuritymode/vb/source.vb#8)]  
  
### <a name="to-use-the-nettcpbinding-with-windows-for-transport-security-in-code"></a>Para usar o NetTcpbinding com o Windows para segurança de transporte (no código)  
  
1. Crie uma instância da <xref:System.ServiceModel.NetTcpBinding> classe e defina a <xref:System.ServiceModel.NetTcpSecurity.Mode%2A> propriedade como <xref:System.ServiceModel.SecurityMode.TransportWithMessageCredential> .  
  
2. Defina a segurança de transporte para usar o Windows definindo o <xref:System.ServiceModel.TcpTransportSecurity.ClientCredentialType%2A> para <xref:System.ServiceModel.TcpClientCredentialType.Windows> . (Observe que esse é o padrão.)  
  
3. Defina <xref:System.ServiceModel.MessageSecurityOverTcp.ClientCredentialType%2A> como um valor apropriado. O código a seguir usa o <xref:System.ServiceModel.MessageCredentialType.Certificate> valor.  
  
4. Crie uma instância da <xref:System.Uri> classe com um endereço base apropriado. Observe que o endereço deve usar o esquema "net. TCP". (Como alternativa, você pode definir o endereço base na configuração.)  
  
5. Crie a instância da <xref:System.ServiceModel.ServiceHost> classe.  
  
6. Use o <xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential.SetCertificate%2A> método da <xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential> classe para definir explicitamente o certificado X. 509 para o serviço.  
  
7. Adicione um ponto de extremidade de serviço usando o <xref:System.ServiceModel.ServiceHost.AddServiceEndpoint%2A> método.  
  
8. Chame o <xref:System.ServiceModel.ICommunicationObject.Open%2A> método, conforme mostrado no código a seguir.  
  
     [!code-csharp[c_SettingSecurityMode#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_settingsecuritymode/cs/source.cs#9)]
     [!code-vb[c_SettingSecurityMode#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_settingsecuritymode/vb/source.vb#9)]  
  
## <a name="using-configuration"></a>Usando a configuração  
  
#### <a name="to-use-the-wshttpbinding"></a>Para usar o WSHttpBinding  
  
1. Configure o computador com um certificado SSL associado a uma porta. (Para obter mais informações, consulte [como: configurar uma porta com um certificado SSL](how-to-configure-a-port-with-an-ssl-certificate.md)). Você não precisa definir um <`transport`> valor do elemento com essa configuração.  
  
2. Especifique o tipo de credencial do cliente para a segurança em nível de mensagem. O exemplo a seguir define o `clientCredentialType` atributo do `message` elemento <> como `UserName` .  
  
    ```xml  
    <wsHttpBinding>  
    <binding name="WsHttpBinding_ICalculator">  
            <security mode="TransportWithMessageCredential" >  
               <message clientCredentialType="UserName" />  
            </security>  
    </binding>  
    </wsHttpBinding>  
    ```  
  
#### <a name="to-use-the-nettcpbinding-with-a-certificate-for-transport-security"></a>Para usar o NetTcpbinding com um certificado para segurança de transporte  
  
1. Para SSL sobre TCP, você deve especificar explicitamente o certificado no `<behaviors>` elemento. O exemplo a seguir especifica um certificado por seu emissor no local de armazenamento padrão (computador local e repositórios pessoais).  
  
    ```xml  
    <behaviors>  
     <serviceBehaviors>  
       <behavior name="mySvcBehavior">  
           <serviceCredentials>  
             <serviceCertificate findValue="contoso.com"  
                                 x509FindType="FindByIssuerName" />  
           </serviceCredentials>  
       </behavior>  
     </serviceBehaviors>  
    </behaviors>  
    ```  
  
2. Adicionar um [\<netTcpBinding>](../../configure-apps/file-schema/wcf/nettcpbinding.md) à seção de associações  
  
3. Adicione um elemento Binding e defina o `name` atributo como um valor apropriado.  
  
4. Adicione um <`security` elemento> e defina o `mode` atributo como `TransportWithMessageCredential` .  
  
5. Adicione um `message>` elemento <e defina o `clientCredentialType` atributo como um valor apropriado.  
  
    ```xml  
    <bindings>  
    <netTcpBinding>  
      <binding name="myTcpBinding">  
        <security mode="TransportWithMessageCredential" >  
           <message clientCredentialType="Windows" />  
        </security>  
      </binding>  
    </netTcpBinding>  
    </bindings>  
    ```  
  
#### <a name="to-use-the-nettcpbinding-with-windows-for-transport-security"></a>Para usar o NetTcpbinding com o Windows para segurança de transporte  
  
1. Adicionar um [\<netTcpBinding>](../../configure-apps/file-schema/wcf/nettcpbinding.md) à seção de associações,  
  
2. Adicione um `binding` elemento <> e defina o `name` atributo como um valor apropriado.  
  
3. Adicione um <`security` elemento> e defina o `mode` atributo como `TransportWithMessageCredential` .  
  
4. Adicione um `transport` elemento <> e defina o `clientCredentialType` atributo como `Windows` .  
  
5. Adicione um `message` elemento <> e defina o `clientCredentialType` atributo como um valor apropriado. O código a seguir define o valor para um certificado.  
  
    ```xml  
    <bindings>  
    <netTcpBinding>  
      <binding name="myTcpBinding">  
        <security mode="TransportWithMessageCredential" >  
           <transport clientCredentialType="Windows" />  
           <message clientCredentialType="Certificate" />  
        </security>  
      </binding>  
    </netTcpBinding>  
    </bindings>  
    ```  
  
## <a name="see-also"></a>Veja também

- [Como definir o modo de segurança](../how-to-set-the-security-mode.md)
- [Protegendo serviços](../securing-services.md)
- [Protegendo serviços e clientes](securing-services-and-clients.md)
