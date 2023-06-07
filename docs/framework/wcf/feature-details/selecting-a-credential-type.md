---
title: Selecionando um tipo de credencial
description: Saiba mais sobre as credenciais, como elas são usadas no WCF e como selecionar a credencial correta para seu aplicativo estabelecer uma identidade ou recursos reivindicados.
ms.date: 03/30/2017
ms.assetid: bf707063-3f30-4304-ab53-0e63413728a8
ms.openlocfilehash: 7a8a6880e5fc3982bb7f470c34a77c771c26effd
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244914"
---
# <a name="selecting-a-credential-type"></a>Selecionando um tipo de credencial
As *credenciais* são o uso de Windows Communication Foundation de dados (WCF) para estabelecer uma identidade ou recursos reivindicados. Por exemplo, um passaporte é uma credencial que um governo emite para provar a cidadania em um país ou região. No WCF, as credenciais podem ter muitas formas, como tokens de nome de usuário e certificados X. 509. Este tópico discute as credenciais, como elas são usadas no WCF e como selecionar a credencial correta para seu aplicativo.  
  
 Em muitos países e regiões, a licença de um driver é um exemplo de uma credencial. Uma licença contém dados que representam a identidade e os recursos de uma pessoa. Ele contém a prova de posse na forma da imagem do possessor. A licença é emitida por uma autoridade confiável, geralmente um departamento governamental de licenciamento. A licença é selada e pode conter um holograma, mostrando que ela não foi violada ou falsificada.  
  
 A apresentação de uma credencial envolve a apresentação dos dados e a prova de posse dos dados. O WCF dá suporte a uma variedade de tipos de credencial nos níveis de segurança de transporte e de mensagem. Por exemplo, considere dois tipos de credenciais com suporte no WCF: nome de usuário e credenciais de certificado (X. 509).  
  
 Para a credencial de nome de usuário, o nome de usuário representa a identidade solicitada e a senha fornece uma prova de posse. Nesse caso, a autoridade confiável é o sistema que valida o nome de usuário e a senha.  
  
 Com uma credencial de certificado X. 509, o nome da entidade, o nome alternativo da entidade ou campos específicos dentro do certificado podem ser usados como declarações de identidade, enquanto outros campos, como os `Valid From` `Valid To` campos e, especificam a validade do certificado.  
  
## <a name="transport-credential-types"></a>Tipos de credencial de transporte  
 A tabela a seguir mostra os possíveis tipos de credenciais de cliente que podem ser usadas por uma associação no modo de segurança de transporte. Ao criar um serviço, defina a `ClientCredentialType` propriedade como um desses valores para especificar o tipo de credencial que o cliente deve fornecer para se comunicar com o serviço. Você pode definir os tipos no código ou nos arquivos de configuração.  
  
|Configuração|Descrição|  
|-------------|-----------------|  
|Nenhum|Especifica que o cliente não precisa apresentar nenhuma credencial. Isso se traduz em um cliente anônimo.|  
|Basic|Especifica a autenticação básica para o cliente. Para obter informações adicionais, consulte RFC2617 –[autenticação http: autenticação básica e resumida](ftp://ftp.rfc-editor.org/in-notes/rfc2617.txt).|  
|Digest|Especifica a autenticação Digest para o cliente. Para obter informações adicionais, consulte RFC2617 –[autenticação http: autenticação básica e resumida](ftp://ftp.rfc-editor.org/in-notes/rfc2617.txt).|  
|Ntlm|Especifica a autenticação NTLM (NT LAN Manager). Isso é usado quando você não pode usar a autenticação Kerberos por algum motivo. Você também pode desabilitar seu uso como um fallback definindo a <xref:System.ServiceModel.Security.WindowsClientCredential.AllowNtlm%2A> propriedade como `false` , o que faz com que o WCF faça um melhor esforço para gerar uma exceção se o NTLM for usado. Observe que a definição dessa propriedade como `false` pode não impedir que credenciais NTLM sejam enviadas pela conexão.|  
|Windows|Especifica autenticação do Windows. Para especificar apenas o protocolo Kerberos em um domínio do Windows, defina a <xref:System.ServiceModel.Security.WindowsClientCredential.AllowNtlm%2A> propriedade como `false` (o padrão é `true` ).|  
|Certificado|Executa a autenticação de cliente usando um certificado X. 509.|  
|Senha|O usuário deve fornecer um nome de usuário e senha. Valide o par de nome de usuário/senha usando a autenticação do Windows ou outra solução personalizada.|  
  
### <a name="message-client-credential-types"></a>Tipos de credencial do cliente da mensagem  
 A tabela a seguir mostra os possíveis tipos de credencial que você pode usar ao criar um aplicativo que usa a segurança de mensagem. Você pode usar esses valores no código ou nos arquivos de configuração.  
  
|Configuração|Descrição|  
|-------------|-----------------|  
|Nenhum|Especifica que o cliente não precisa apresentar uma credencial. Isso se traduz em um cliente anônimo.|  
|Windows|Permite que as trocas de mensagens SOAP ocorram sob o contexto de segurança estabelecido com uma credencial do Windows.|  
|Nome de Usuário|Permite que o serviço exija que o cliente seja autenticado com uma credencial de nome de usuário. Observe que o WCF não permite nenhuma operação criptográfica com nomes de usuário, como gerar uma assinatura ou criptografar dados. O WCF garante que o transporte seja protegido ao usar credenciais de nome de usuário.|  
|Certificado|Permite que o serviço exija que o cliente seja autenticado usando um certificado X. 509.|  
|Token emitido|Um tipo de token personalizado configurado de acordo com uma política de segurança. O tipo de token padrão é SAML (Security Asserties Markup Language). O token é emitido por um serviço de token seguro. Para obter mais informações, consulte [Federação e tokens emitidos](federation-and-issued-tokens.md).|  
  
### <a name="negotiation-model-of-service-credentials"></a>Modelo de negociação de credenciais de serviço  
 *Negociação* é o processo de estabelecer confiança entre um cliente e um serviço por meio da troca de credenciais. O processo é executado iterativamente entre o cliente e o serviço, portanto, para divulgar apenas as informações necessárias para a próxima etapa no processo de negociação. Na prática, o resultado final é a entrega da credencial de um serviço para o cliente a ser usada em operações subsequentes.  
  
 Com uma exceção, por padrão, as associações fornecidas pelo sistema no WCF negociam automaticamente a credencial de serviço ao usar a segurança em nível de mensagem. (A exceção é o <xref:System.ServiceModel.BasicHttpBinding> , que não habilita a segurança por padrão.) Para desabilitar esse comportamento, consulte as <xref:System.ServiceModel.MessageSecurityOverHttp.NegotiateServiceCredential%2A> <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.NegotiateServiceCredential%2A> Propriedades e.  
  
> [!NOTE]
> Quando a segurança SSL é usada com o .NET Framework 3,5 e posterior, um cliente WCF usa os certificados intermediários em seu repositório de certificados e os certificados intermediários recebidos durante a negociação SSL para executar a validação da cadeia de certificados no certificado do serviço. .NET Framework 3,0 usa apenas os certificados intermediários instalados no repositório de certificados local.  
  
#### <a name="out-of-band-negotiation"></a>Negociação fora de banda  
 Se a negociação automática estiver desabilitada, a credencial de serviço deverá ser provisionada no cliente antes do envio de qualquer mensagem para o serviço. Isso também é conhecido como provisionamento *fora de banda* . Por exemplo, se o tipo de credencial especificado for um certificado e a negociação automática estiver desabilitada, o cliente deverá entrar em contato com o proprietário do serviço para receber e instalar o certificado no computador que executa o aplicativo cliente. Isso pode ser feito, por exemplo, quando você deseja controlar estritamente quais clientes podem acessar um serviço em um cenário entre empresas. Essa negociação fora de banda pode ser feita por email, e o certificado X. 509 é armazenado no repositório de certificados do Windows, usando uma ferramenta como o snap-in de certificados do MMC (console de gerenciamento Microsoft).  
  
> [!NOTE]
> A <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> propriedade é usada para fornecer o serviço com um certificado que foi obtido por meio da negociação fora de banda. Isso é necessário ao usar a <xref:System.ServiceModel.BasicHttpBinding> classe porque a associação não permite a negociação automatizada. A propriedade também é usada em um cenário duplex não correlacionado. Esse é um cenário em que um servidor envia uma mensagem ao cliente sem exigir que o cliente envie uma solicitação ao servidor primeiro. Como o servidor não tem uma solicitação do cliente, ele deve usar o certificado do cliente para criptografar a mensagem para o cliente.  
  
## <a name="setting-credential-values"></a>Definindo valores de credenciais  
 Depois de selecionar um modo de segurança, você deve especificar as credenciais reais. Por exemplo, se o tipo de credencial for definido como "certificado", você deverá associar uma credencial específica (como um certificado X. 509 específico) ao serviço ou cliente.  
  
 Dependendo se você estiver programando um serviço ou um cliente, o método para definir o valor da credencial difere ligeiramente.  
  
### <a name="setting-service-credentials"></a>Definindo credenciais de serviço  
 Se você estiver usando o modo de transporte e estiver usando HTTP como transporte, deverá usar Serviços de Informações da Internet (IIS) ou configurar a porta com um certificado. Para obter mais informações, consulte [visão geral de segurança de transporte](transport-security-overview.md) e segurança de [transporte http](http-transport-security.md).  
  
 Para provisionar um serviço com credenciais no código, crie uma instância da <xref:System.ServiceModel.ServiceHost> classe e especifique a credencial apropriada usando a <xref:System.ServiceModel.Description.ServiceCredentials> classe, acessada por meio da <xref:System.ServiceModel.ServiceHostBase.Credentials%2A> propriedade.  
  
#### <a name="setting-a-certificate"></a>Configurando um certificado  
 Para provisionar um serviço com um certificado X. 509 a ser usado para autenticar o serviço para clientes, use o <xref:System.ServiceModel.Security.X509CertificateInitiatorServiceCredential.SetCertificate%2A> método da <xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential> classe.  
  
 Para provisionar um serviço com um certificado de cliente, use o <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A> método da <xref:System.ServiceModel.Security.X509CertificateInitiatorServiceCredential> classe.  
  
#### <a name="setting-windows-credentials"></a>Configurando credenciais do Windows  
 Se o cliente especificar um nome de usuário e senha válidos, essa credencial será usada para autenticar o cliente. Caso contrário, as credenciais do usuário conectado no momento serão usadas.  
  
### <a name="setting-client-credentials"></a>Definindo as credenciais do cliente  
 No WCF, os aplicativos cliente usam um cliente WCF para se conectar aos serviços. Cada cliente deriva da <xref:System.ServiceModel.ClientBase%601> classe e a <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> propriedade no cliente permite a especificação de vários valores de credenciais do cliente.  
  
#### <a name="setting-a-certificate"></a>Configurando um certificado  
 Para provisionar um serviço com um certificado X. 509 que é usado para autenticar o cliente para um serviço, use o <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A> método da <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential> classe.  
  
## <a name="how-client-credentials-are-used-to-authenticate-a-client-to-the-service"></a>Como as credenciais do cliente são usadas para autenticar um cliente para o serviço  
 As informações de credenciais de cliente necessárias para se comunicar com um serviço são fornecidas usando a <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> propriedade ou a <xref:System.ServiceModel.ChannelFactory.Credentials%2A> propriedade. O canal de segurança usa essas informações para autenticar o cliente para o serviço. A autenticação é realizada por meio de um dos dois modos:  
  
- As credenciais do cliente são usadas uma vez antes que a primeira mensagem seja enviada, usando a instância do cliente WCF para estabelecer um contexto de segurança. Todas as mensagens de aplicativo são então protegidas por meio do contexto de segurança.  
  
- As credenciais do cliente são usadas para autenticar cada mensagem de aplicativo enviada ao serviço. Nesse caso, nenhum contexto é estabelecido entre o cliente e o serviço.  
  
### <a name="established-identities-cannot-be-changed"></a>Não é possível alterar as identidades estabelecidas  
 Quando o primeiro método é usado, o contexto estabelecido é permanentemente associado à identidade do cliente. Ou seja, depois que o contexto de segurança tiver sido estabelecido, a identidade associada ao cliente não poderá ser alterada.  
  
> [!IMPORTANT]
> Há uma situação em que você deve estar atento quando a identidade não pode ser alternada (ou seja, quando estabelecer o contexto de segurança é on, o comportamento padrão). Se você criar um serviço que se comunique com um segundo serviço, a identidade usada para abrir o cliente WCF para o segundo serviço não poderá ser alterada. Isso se tornará um problema se vários clientes tiverem permissão para usar o primeiro serviço e o serviço representar os clientes ao acessar o segundo serviço. Se o serviço reutiliza o mesmo cliente para todos os chamadores, todas as chamadas para o segundo serviço são feitas sob a identidade do primeiro chamador que foi usado para abrir o cliente para o segundo serviço. Em outras palavras, o serviço usa a identidade do primeiro cliente para que todos os seus clientes se comuniquem com o segundo serviço. Isso pode levar à elevação de privilégio. Se esse não for o comportamento desejado do seu serviço, você deverá acompanhar cada chamador e criar um novo cliente para o segundo serviço para cada chamador distinto e garantir que o serviço use apenas o cliente correto para que o chamador correto se comunique com o segundo serviço.  
  
 Para obter mais informações sobre credenciais e sessões seguras, consulte [considerações de segurança para sessões seguras](security-considerations-for-secure-sessions.md).  
  
## <a name="see-also"></a>Veja também

- <xref:System.ServiceModel.ClientBase%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.ClientCredentials.ClientCertificate%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.BasicHttpMessageSecurity.ClientCredentialType%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.HttpTransportSecurity.ClientCredentialType%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageSecurityOverHttp.ClientCredentialType%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageSecurityOverMsmq.ClientCredentialType%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageSecurityOverTcp.ClientCredentialType%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.TcpTransportSecurity.ClientCredentialType%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.X509CertificateInitiatorServiceCredential.SetCertificate%2A?displayProperty=nameWithType>
- [Conceitos de segurança](security-concepts.md)
- [Protegendo serviços e clientes](securing-services-and-clients.md)
- [Programação de segurança do WCF](programming-wcf-security.md)
- [Segurança de transporte de HTTP](http-transport-security.md)
