---
title: Visão geral de segurança de transporte
description: Saiba mais sobre os principais mecanismos de segurança de transporte nas associações fornecidas pelo sistema do WCF. Esses mecanismos de segurança dependem da associação e do transporte usados.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 00959326-aa9d-44d0-af61-54933d4adc7f
ms.openlocfilehash: 6302a949e8d0a041446b75dd3769b8ba2d1fc2b5
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244823"
---
# <a name="transport-security-overview"></a>Visão geral de segurança de transporte
Os mecanismos de segurança de transporte no Windows Communication Foundation (WCF) dependem da associação e do transporte que estão sendo usados. Por exemplo, ao usar a <xref:System.ServiceModel.WSHttpBinding> classe, o transporte é http e o mecanismo primário para proteger o transporte é protocolo SSL (SSL) por http, normalmente chamado de HTTPS. Este tópico discute os principais mecanismos de segurança de transporte usados nas associações fornecidas pelo sistema do WCF.  
  
> [!NOTE]
> Quando a segurança SSL é usada com o .NET Framework 3,5 e posterior, um cliente WCF usa os certificados intermediários em seu repositório de certificados e os certificados intermediários recebidos durante a negociação SSL para executar a validação da cadeia de certificados no certificado do serviço. .NET Framework 3,0 usa apenas os certificados intermediários instalados no repositório de certificados local.  
  
> [!WARNING]
> Quando a segurança de transporte é usada, a <xref:System.Threading.Thread.CurrentPrincipal%2A?displayProperty=nameWithType> propriedade pode ser substituída. Para evitar que isso aconteça, defina <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.PrincipalPermissionMode%2A?displayProperty=nameWithType> como <xref:System.ServiceModel.Description.PrincipalPermissionMode.None?displayProperty=nameWithType> . <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>é um comportamento de serviço que pode ser definido na descrição do serviço.  
  
## <a name="basichttpbinding"></a>BasicHttpBinding  
 Por padrão, a <xref:System.ServiceModel.BasicHttpBinding> classe não fornece segurança. Essa associação é projetada para interoperabilidade com provedores de serviço Web que não implementam a segurança. No entanto, você pode alternar a segurança definindo a <xref:System.ServiceModel.BasicHttpSecurity.Mode%2A> propriedade como qualquer valor, exceto <xref:System.ServiceModel.BasicHttpSecurityMode.None> . Para habilitar a segurança de transporte, defina a propriedade como <xref:System.ServiceModel.BasicHttpSecurityMode.Transport> .  
  
### <a name="interoperation-with-iis"></a>Interoperação com o IIS  
 A <xref:System.ServiceModel.BasicHttpBinding> classe é usada principalmente para interoperar com os serviços Web existentes e muitos desses serviços são hospedados pelo serviços de informações da Internet (IIS). Consequentemente, a segurança de transporte para essa associação é projetada para interoperação direta com sites do IIS. Isso é feito definindo o modo de segurança como <xref:System.ServiceModel.BasicHttpSecurityMode.Transport> e, em seguida, definindo o tipo de credencial do cliente. Os valores de tipo de credencial correspondem aos mecanismos de segurança de diretório do IIS. O código a seguir mostra o modo que está sendo definido e o tipo de credencial definido como Windows. Você pode usar essa configuração quando o cliente e o servidor estiverem no mesmo domínio do Windows.  
  
 [!code-csharp[c_ProgrammingSecurity#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_programmingsecurity/cs/source.cs#10)]
 [!code-vb[c_ProgrammingSecurity#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_programmingsecurity/vb/source.vb#10)]  
  
 Ou, em configuração:  
  
```xml  
<bindings>  
  <basicHttpBinding>  
    <binding name="SecurityByTransport">  
      <security mode="Transport">  
        <transport clientCredentialType="Windows" />  
       </security>  
     </binding>  
  </basicHttpBinding>  
</bindings>  
```  
  
 As seções a seguir discutem outros tipos de credenciais de cliente.  
  
#### <a name="basic"></a>Basic  
 Isso corresponde ao método de autenticação básica no IIS. Ao usar esse modo, o servidor IIS deve ser configurado com as contas de usuário do Windows e as permissões apropriadas do sistema de arquivos NTFS. Para obter mais informações sobre o IIS 6,0, consulte [habilitando a autenticação básica e Configurando o nome do Realm](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc785293(v=ws.10)). Para obter mais informações sobre o IIS 7,0, consulte [Configurar a autenticação básica (IIS 7)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772009(v=ws.10)).  
  
#### <a name="certificate"></a>Certificado  
 O IIS tem uma opção para exigir que os clientes façam logon com um certificado. O recurso também permite que o IIS mapeie um certificado de cliente para uma conta do Windows. Para obter mais informações sobre o IIS 6,0, consulte [habilitando certificados de cliente no IIS 6,0](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc727994(v=ws.10)). Para obter mais informações sobre o IIS 7,0, consulte [Configurando certificados de servidor no IIS 7](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732230(v=ws.10)).  
  
#### <a name="digest"></a>Digest  
 A autenticação Digest é semelhante à autenticação básica, mas oferece a vantagem de enviar as credenciais como um hash, em vez de texto não criptografado. Para obter mais informações sobre o IIS 6,0, consulte [autenticação Digest no iis 6,0](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc782661(v=ws.10)). Para obter mais informações sobre o IIS 7,0, consulte [configurar autenticação Digest (IIS 7)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754104(v=ws.10)).  
  
#### <a name="windows"></a>Windows  
 Isso corresponde à autenticação integrada do Windows no IIS. Quando definido com esse valor, o servidor também deve existir em um domínio do Windows que usa o protocolo Kerberos como seu controlador de domínio. Se o servidor não estiver em um domínio com suporte para Kerberos ou se o sistema Kerberos falhar, você poderá usar o valor do NT LAN Manager (NTLM) descrito na próxima seção. Para obter mais informações sobre o IIS 6,0, consulte [autenticação integrada do Windows no iis 6,0](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc738016(v=ws.10)). Para obter mais informações sobre o IIS 7,0, consulte [Configurando certificados de servidor no IIS 7](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732230(v=ws.10)).
  
#### <a name="ntlm"></a>NTLM  
 Isso permite que o servidor use NTLM para autenticação se o protocolo Kerberos falhar. Para obter mais informações sobre como configurar o IIS no IIS 6,0, consulte [forçando a autenticação NTLM](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786486(v=ws.10)). Para o IIS 7,0, a autenticação do Windows inclui a autenticação NTLM. Para obter mais informações, consulte [Configurando certificados de servidor no IIS 7](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732230(v=ws.10)).
  
## <a name="wshttpbinding"></a>WsHttpBinding  
 A <xref:System.ServiceModel.WSHttpBinding> classe foi projetada para interoperação com serviços que implementam especificações WS-*. A segurança de transporte para essa associação é protocolo SSL (SSL) sobre HTTP ou HTTPS. Para criar um aplicativo WCF que usa SSL, use o IIS para hospedar o aplicativo. Como alternativa, se você estiver criando um aplicativo auto-hospedado, use a ferramenta HttpCfg.exe para associar um certificado X. 509 a uma porta específica em um computador. O número da porta é especificado como parte do aplicativo WCF como um endereço de ponto de extremidade. Ao usar o modo de transporte, o endereço do ponto de extremidade deve incluir o protocolo HTTPS ou uma exceção será lançada em tempo de execução. Para obter mais informações, consulte [segurança de transporte http](http-transport-security.md).  
  
 Para autenticação de cliente, defina a <xref:System.ServiceModel.HttpTransportSecurity.ClientCredentialType%2A> propriedade da <xref:System.ServiceModel.HttpTransportSecurity> classe como um dos <xref:System.ServiceModel.HttpClientCredentialType> valores de enumeração. Os valores de enumeração são idênticos aos tipos de credencial do cliente para <xref:System.ServiceModel.BasicHttpBinding> e são projetados para serem hospedados com os serviços do IIS.  
  
 O exemplo a seguir mostra a associação que está sendo usada com um tipo de credencial de cliente do Windows.  
  
 [!code-csharp[c_ProgrammingSecurity#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_programmingsecurity/cs/source.cs#11)]
 [!code-vb[c_ProgrammingSecurity#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_programmingsecurity/vb/source.vb#11)]  
  
## <a name="wsdualhttpbinding"></a>WSDualHttpBinding  
 Essa associação fornece apenas segurança em nível de mensagem, não segurança em nível de transporte.  
  
## <a name="nettcpbinding"></a>NetTcpBinding  
 A <xref:System.ServiceModel.NetTcpBinding> classe usa TCP para transporte de mensagens. A segurança para o modo de transporte é fornecida pela implementação do protocolo TLS sobre TCP. A implementação de TLS é fornecida pelo sistema operacional.  
  
 Você também pode especificar o tipo de credencial do cliente definindo a <xref:System.ServiceModel.TcpTransportSecurity.ClientCredentialType%2A> propriedade da <xref:System.ServiceModel.TcpTransportSecurity> classe como um dos <xref:System.ServiceModel.TcpClientCredentialType> valores, conforme mostrado no código a seguir.  
  
 [!code-csharp[c_ProgrammingSecurity#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_programmingsecurity/cs/source.cs#12)]
 [!code-vb[c_ProgrammingSecurity#12](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_programmingsecurity/vb/source.vb#12)]  
  
#### <a name="client"></a>Cliente  
 No cliente, você deve especificar um certificado usando o <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A> método da <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential> classe.  
  
> [!NOTE]
> Se você estiver usando a segurança do Windows, um certificado não será necessário.  
  
 O código a seguir usa a impressão digital do certificado, que o identifica exclusivamente. Para obter mais informações sobre certificados, consulte [Working with Certificates](working-with-certificates.md) (Trabalhando com certificados).  
  
 [!code-csharp[c_ProgrammingSecurity#13](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_programmingsecurity/cs/source.cs#13)]
 [!code-vb[c_ProgrammingSecurity#13](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_programmingsecurity/vb/source.vb#13)]  
  
 Como alternativa, especifique o certificado na configuração do cliente usando um [\<clientCredentials>](../../configure-apps/file-schema/wcf/clientcredentials.md) elemento na seção comportamentos.  
  
```xml  
<behaviors>  
  <behavior>  
   <clientCredentials>  
     <clientCertificate findValue= "101010101010101010101010101010000000000"
      storeLocation="LocalMachine" storeName="My"
      X509FindType="FindByThumbPrint">  
     </clientCertificate>  
   </clientCredentials>  
 </behavior>  
</behaviors>
```  
  
## <a name="netnamedpipebinding"></a>NetNamedPipeBinding  
 A <xref:System.ServiceModel.NetNamedPipeBinding> classe é projetada para comunicação eficiente entre computadores, ou seja, para processos em execução no mesmo computador, embora canais de pipe nomeado possam ser criados entre dois computadores na mesma rede. Essa associação fornece apenas segurança em nível de transporte. Ao criar aplicativos usando essa associação, os endereços do ponto de extremidade devem incluir "net. pipe" como o protocolo do endereço do ponto de extremidade.  
  
## <a name="wsfederationhttpbinding"></a>WSFederationHttpBinding  
 Ao usar a segurança de transporte, essa associação usa SSL sobre HTTP, conhecida como HTTPS com um token emitido ( <xref:System.ServiceModel.WSFederationHttpSecurityMode.TransportWithMessageCredential> ). Para obter mais informações sobre aplicativos de Federação, consulte [Federação e tokens emitidos](federation-and-issued-tokens.md).  
  
## <a name="netpeertcpbinding"></a>NetPeerTcpBinding  
 A <xref:System.ServiceModel.NetPeerTcpBinding> classe é um transporte seguro que é projetado para uma comunicação eficiente usando o recurso de rede ponto a ponto. Conforme indicado pelo nome da classe e associação, TCP é o protocolo. Quando o modo de segurança é definido como transporte, a associação implementa o TLS sobre TCP. Para obter mais informações sobre o recurso ponto a ponto, consulte [rede ponto a ponto](peer-to-peer-networking.md).  
  
## <a name="msmqintegrationbinding-and-netmsmqbinding"></a>MsmqIntegrationBinding e NetMsmqBinding  
 Para obter uma discussão completa sobre segurança de transporte com o serviço de enfileiramento de mensagens (anteriormente chamado de MSMQ), consulte [protegendo mensagens usando a segurança de transporte](securing-messages-using-transport-security.md).  
  
## <a name="see-also"></a>Veja também

- [Programação de segurança do WCF](programming-wcf-security.md)
