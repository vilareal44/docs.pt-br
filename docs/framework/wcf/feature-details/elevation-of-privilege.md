---
title: Elevação de privilégio
ms.date: 03/30/2017
helpviewer_keywords:
- elevation of privilege [WCF]
- security [WCF], elevation of privilege
ms.assetid: 146e1c66-2a76-4ed3-98a5-fd77851a06d9
ms.openlocfilehash: 823b41f86080d4802f76fe69865279a7c3506238
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597404"
---
# <a name="elevation-of-privilege"></a>Elevação de privilégio
A *elevação de privilégio* resulta da concessão de permissões de autorização de invasor além daquelas inicialmente concedidas. Por exemplo, um invasor com um conjunto de privilégios de permissões "somente leitura", de alguma forma, eleva o conjunto para incluir "leitura e gravação".  
  
## <a name="trusted-sts-should-sign-saml-token-claims"></a>STS confiável deve assinar declarações de token SAML  
 Um token SAML (Security Asserts Markup Language) é um token XML genérico que é o tipo padrão para tokens emitidos. Um token SAML pode ser construído por um STS (serviço de token de segurança) que o serviço Web final confia em um intercâmbio típico. Os tokens SAML contêm declarações em instruções. Um invasor pode copiar as declarações de um token válido, criar um novo token SAML e conectá-lo a um emissor diferente. A intenção é determinar se o servidor está validando emissores e, caso contrário, utilizar o ponto fraco para construir tokens SAML que permitem privilégios além daqueles destinados por um STS confiável.  
  
 A <xref:System.IdentityModel.Tokens.SamlAssertion> classe verifica a assinatura digital contida em um token SAML, e o padrão <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator> requer que os tokens SAML sejam assinados por um certificado X. 509 que é válido quando o <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CertificateValidationMode%2A> da <xref:System.ServiceModel.Security.IssuedTokenServiceCredential> classe é definido como <xref:System.ServiceModel.Security.X509CertificateValidationMode.ChainTrust> . `ChainTrust`o modo sozinho é insuficiente para determinar se o emissor do token SAML é confiável. Os serviços que exigem um modelo de confiança mais granular podem usar políticas de autorização e imposição para verificar o emissor dos conjuntos de declarações produzidos pela autenticação de token emitida ou usar as configurações de validação X. 509 no <xref:System.ServiceModel.Security.IssuedTokenServiceCredential> para restringir o conjunto de certificados de assinatura permitidos. Para obter mais informações, consulte [Gerenciando declarações e autorização com o modelo de identidade e a](managing-claims-and-authorization-with-the-identity-model.md) [Federação e tokens emitidos](federation-and-issued-tokens.md).  
  
## <a name="switching-identity-without-a-security-context"></a>Alternando a identidade sem um contexto de segurança  
 O seguinte se aplica somente ao WinFX.  
  
 Quando uma conexão é estabelecida entre um cliente e um servidor, a identidade do cliente não é alterada, exceto em uma situação: depois que o cliente WCF for aberto, se todas as condições a seguir forem verdadeiras:  
  
- Os procedimentos para estabelecer um contexto de segurança (usando uma sessão de segurança de transporte ou uma sessão de segurança de mensagem) são desligados ( <xref:System.ServiceModel.NonDualMessageSecurityOverHttp.EstablishSecurityContext%2A> a propriedade é definida como `false` no caso de segurança de mensagem ou o transporte não capaz de estabelecer sessões de segurança é usado no caso de segurança de transporte. HTTPS é um exemplo desse transporte.  
  
- Você está usando a autenticação do Windows.  
  
- Você não define explicitamente a credencial.  
  
- Você está chamando o serviço sob o contexto de segurança representado.  
  
 Se essas condições forem verdadeiras, a identidade usada para autenticar o cliente para o serviço poderá ser alterada (talvez não seja a identidade representada, mas a identidade do processo) depois que o cliente WCF for aberto. Isso ocorre porque a credencial do Windows usada para autenticar o cliente para o serviço é transmitida com cada mensagem, e a credencial usada para autenticação é obtida da identidade do Windows do thread atual. Se a identidade do Windows do thread atual mudar (por exemplo, representando um chamador diferente), a credencial anexada à mensagem e usada para autenticar o cliente para o serviço também pode ser alterada.  
  
 Se você quiser ter um comportamento determinístico ao usar a autenticação do Windows junto com a representação, precisará definir explicitamente a credencial do Windows ou precisará estabelecer um contexto de segurança com o serviço. Para fazer isso, use uma sessão de segurança de mensagem ou uma sessão de segurança de transporte. Por exemplo, o transporte net. TCP pode fornecer uma sessão de segurança de transporte. Além disso, você deve usar apenas uma versão síncrona de operações de cliente ao chamar o serviço. Se você estabelecer um contexto de segurança de mensagem, não deverá manter a conexão com o serviço aberta por mais tempo do que o período de renovação da sessão configurada, pois a identidade também pode ser alterada durante o processo de renovação da sessão.  
  
### <a name="credentials-capture"></a>Captura de credenciais  
 O seguinte se aplica a .NET Framework 3,5 e a versões subsequentes.  
  
 As credenciais usadas pelo cliente ou pelo serviço são baseadas no thread de contexto atual. As credenciais são obtidas quando o `Open` método (ou `BeginOpen` , para chamadas assíncronas) do cliente ou serviço é chamado. Para as <xref:System.ServiceModel.ServiceHost> classes e <xref:System.ServiceModel.ClientBase%601> , os `Open` métodos e são `BeginOpen` herdados dos <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> <xref:System.ServiceModel.Channels.CommunicationObject.BeginOpen%2A> métodos e da <xref:System.ServiceModel.Channels.CommunicationObject> classe.  
  
> [!NOTE]
> Ao usar o `BeginOpen` método, as credenciais capturadas não podem ser garantidas como credenciais do processo que chama o método.  
  
## <a name="token-caches-allow-replay-using-obsolete-data"></a>Caches de token permitem a repetição usando dados obsoletos  
 O WCF usa a função de autoridade de segurança local (LSA) `LogonUser` para autenticar usuários por nome de usuário e senha. Como a função de logon é uma operação dispendiosa, o WCF permite que você armazene em cache tokens que representam usuários autenticados para aumentar o desempenho. O mecanismo de cache salva os resultados de `LogonUser` para usos subsequentes. Esse mecanismo está desabilitado por padrão; para habilitá-lo, defina a <xref:System.ServiceModel.Security.UserNamePasswordServiceCredential.CacheLogonTokens%2A> propriedade como `true` ou use o `cacheLogonTokens` atributo do [\<userNameAuthentication>](../../configure-apps/file-schema/wcf/usernameauthentication.md) .  
  
 Você pode definir um TTL (vida útil) para os tokens armazenados em cache definindo a <xref:System.ServiceModel.Security.UserNamePasswordServiceCredential.CachedLogonTokenLifetime%2A> propriedade como a <xref:System.TimeSpan> ou usar o `cachedLogonTokenLifetime` atributo do `userNameAuthentication` elemento; o padrão é 15 minutos. Observe que, enquanto um token é armazenado em cache, qualquer cliente que apresente o mesmo nome de usuário e senha pode usar o token, mesmo se a conta de usuário for excluída do Windows ou se sua senha tiver sido alterada. Até que o TTL expire e o token seja removido do cache, o WCF permite que o usuário (possivelmente mal-intencionado) autentique.  
  
 Para atenuar isso: diminua a janela de ataque definindo o `cachedLogonTokenLifetime` valor para o menor tempo que os usuários precisam.  
  
## <a name="issued-token-authorization-expiration-reset-to-large-value"></a>Autorização de token emitida: redefinição de expiração para valor grande  
 Em determinadas condições, a <xref:System.IdentityModel.Policy.AuthorizationContext.ExpirationTime%2A> propriedade de <xref:System.IdentityModel.Policy.AuthorizationContext> pode ser definida como um valor maior inesperadamente (o <xref:System.DateTime.MaxValue> valor do campo menos um dia ou 20 de dezembro de 9999).  
  
 Isso ocorre ao usar o <xref:System.ServiceModel.WSFederationHttpBinding> e qualquer uma das associações fornecidas pelo sistema que têm um token emitido como o tipo de credencial do cliente.  
  
 Isso também ocorre quando você cria associações personalizadas usando um dos seguintes métodos:  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenBindingElement%2A>  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenForCertificateBindingElement%2A>  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenForSslBindingElement%2A>  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenOverTransportBindingElement%2A>  
  
 Para atenuar isso, a política de autorização deve verificar a ação e o tempo de expiração de cada política de autorização.  
  
## <a name="the-service-uses-a-different-certificate-than-the-client-intended"></a>O serviço usa um certificado diferente do que o cliente pretendido  
 Em determinadas condições, um cliente pode assinar digitalmente uma mensagem com um certificado X. 509 e fazer com que o serviço recupere um certificado diferente daquele pretendido.  
  
 Isso pode ocorrer nas seguintes circunstâncias:  
  
- O cliente assina digitalmente uma mensagem usando um certificado X. 509 e não anexa o certificado X. 509 à mensagem, mas, em vez disso, faz referência ao certificado usando seu identificador de chave de entidade.  
  
- O computador do serviço contém dois ou mais certificados com a mesma chave pública, mas eles contêm informações diferentes.  
  
- O serviço recupera um certificado que corresponde ao identificador de chave da entidade, mas não é aquele que o cliente pretende usar. Quando o WCF recebe a mensagem e verifica a assinatura, o WCF mapeia as informações no certificado X. 509 indesejado para um conjunto de declarações que são diferentes e possivelmente elevadas do que o cliente esperava.  
  
 Para atenuar isso, referencie o certificado X. 509 de outra maneira, como usar <xref:System.ServiceModel.Security.Tokens.X509KeyIdentifierClauseType.IssuerSerial> .  
  
## <a name="see-also"></a>Consulte também

- [Considerações sobre segurança](security-considerations-in-wcf.md)
- [Divulgação de informações](information-disclosure.md)
- [Negação de serviço](denial-of-service.md)
- [Ataques por repetição](replay-attacks.md)
- [Adulteração](tampering.md)
- [Cenários sem suporte](unsupported-scenarios.md)
