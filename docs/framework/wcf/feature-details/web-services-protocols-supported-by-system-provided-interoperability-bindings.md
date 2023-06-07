---
title: Associações de interoperabilidade fornecidas pelo sistema oferece suporte para protocolos de serviços Web
ms.date: 03/30/2017
helpviewer_keywords:
- WS-protocols
- Web services protocols
- Windows Communication Foundation, Web service protocols
ms.assetid: 1f7fc4ff-30fe-4e46-adda-91caad3b06c6
ms.openlocfilehash: 0b901be2d90a70b4a44fdafb5005f9dc7fb9d556
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594901"
---
# <a name="web-services-protocols-supported-by-system-provided-interoperability-bindings"></a>Associações de interoperabilidade fornecidas pelo sistema oferece suporte para protocolos de serviços Web
O Windows Communication Foundation (WCF) foi criado para interoperar com serviços Web que dão suporte a um conjunto de especificações conhecidas como especificações de serviços da Web. Para simplificar a configuração de serviço para práticas recomendadas de interoperabilidade, o WCF apresenta três associações interoperáveis fornecidas pelo sistema: <xref:System.ServiceModel.BasicHttpBinding?displayProperty=nameWithType> , <xref:System.ServiceModel.WSHttpBinding?displayProperty=nameWithType> e <xref:System.ServiceModel.WSDualHttpBinding?displayProperty=nameWithType> . Para a interoperabilidade com a organização para o avanço de padrões de OASIS (padrões de informações estruturadas), o WCF inclui uma associação de sistema interoperável fornecida por sistemas: <xref:System.ServiceModel.WS2007HttpBinding?displayProperty=nameWithType> . Para a publicação de metadados, o WCF inclui duas associações interoperáveis fornecidas pelo sistema: [\<mexHttpBinding>](../../configure-apps/file-schema/wcf/mexhttpbinding.md) e [\<mexHttpsBinding>](../../configure-apps/file-schema/wcf/mexhttpsbinding.md) . Este tópico lista as especificações com suporte de associações interoperáveis fornecidas pelo sistema.  
  
## <a name="web-services-protocols-supported-by-basichttpbinding-wshttpbinding-ws2007httpbinding-and-wsdualhttpbinding-bindings"></a>Protocolos de serviços Web com suporte pelas associações basicHttpBinding, wsHttpBinding, ws2007HttpBinding e wsDualHttpBinding  
  
### <a name="all-bindings"></a>Todas as associações  
 As [\<basicHttpBinding>](../../configure-apps/file-schema/wcf/basichttpbinding.md) [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) associações, e [\<ws2007HttpBinding>](../../configure-apps/file-schema/wcf/ws2007httpbinding.md) dão suporte aos seguintes protocolos.  
  
> [!NOTE]
> Para obter informações sobre associações usadas para publicar metadados, consulte a seção "associações de metadados fornecidas pelo sistema" mais adiante neste tópico.  
  
|Categoria|Protocolo|Especificação e uso|  
|--------------|--------------|-----------------------------|  
|Transport|HTTP 1.1|[HTTP 1.1](https://www.ietf.org/rfc/rfc2616.txt)<br /><br /> `BasicHttpBinding`, `WSHttpBinding` e `WS2007HttpBinding` usam os transportes http e HTTPS.|  
|Mensagens|MTOM|[MTOM](https://www.w3.org/TR/soap12-mtom/)<br /><br /> `basicHttpBinding`, `wsHttpBinding` e `ws2007HttpBinding` dão suporte ao mecanismo de otimização de transmissão de mensagens (MTOM). Não usado por padrão. Para usar o MTOM, defina o `messageEncoding` atributo como `"Mtom"` .<br /><br /> Exemplo:<br /><br /> `<wsHttpBinding> <binding messageEncoding="Mtom"/> </wsHttpBinding>`|  
|Metadados|WSDL 1,1|[WSDL 1,1](https://www.w3.org/TR/wsdl/)<br /><br /> O WCF usa WSDL (Web Services Description Language) para descrever os serviços.|  
|Metadados|WS-Policy|[WS-Policy](https://www.w3.org/Submission/WS-Policy/)<br /><br /> O WCF usa a especificação WS-Policy juntamente com declarações específicas de domínio para descrever os requisitos de serviço e os recursos.|  
|Metadados|WS-Policy 1,5|[WS-Policy 1,5](https://www.w3.org/TR/2007/CR-ws-policy-20070605/)<br /><br /> O WCF usa a especificação WS-Policy juntamente com declarações específicas de domínio para descrever os requisitos de serviço e os recursos.|  
|Metadados|WS-PolicyAttachment|[WS-PolicyAttachment](http://specs.xmlsoap.org/ws/2004/09/policy/ws-policyattachment.pdf)<br /><br /> O WCF implementa o WS-PolicyAttachment para anexar expressões de política em vários escopos no WSDL (Web Services Description Language).|  
|Metadados|WS-MetadataExchange|[WS-MetadataExchange](http://specs.xmlsoap.org/ws/2004/09/mex/WS-MetadataExchange.pdf)<br /><br /> O WCF implementa o WS-MetadataExchange para recuperar o esquema XML, o WSDL e o WS-Policy.|  
  
### <a name="basichttpbinding"></a>basicHttpBinding  
  
|Categoria|Protocolo|Especificação e uso|  
|--------------|--------------|-----------------------------|  
|Mensagens|SOAP 1,1|[SOAP 1,1](https://www.w3.org/TR/2000/NOTE-SOAP-20000508/)<br /><br /> De acordo com o perfil básico 1,1, o `basicHttpBinding` elemento implementa o protocolo de mensagem SOAP 1,1.|  
|Segurança|Segurança de mensagem SOAP do WSS 1,0|[Segurança de mensagem SOAP do WSS 1,0](http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-soap-message-security-1.0.pdf)<br /><br /> De acordo com o perfil de segurança básico, o `basicHttpBinding` elemento implementa a especificação de especificação Web Services Security (WSS) SOAP Message Security 1,0 para o nome de usuário/senha e a segurança baseada em X. 509.<br /><br /> `<basicHttpBinding> <binding name="Binding1"> <security mode="TransportWithMessageCredential &#124;                     "Message" .../> </binding> </basicHttpBinding>`|  
|Segurança|Perfil do UsernameToken de segurança de mensagem SOAP do WSS 1,0|[Perfil do UsernameToken de segurança de mensagem SOAP do WSS 1,0](http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-username-token-profile-1.0.pdf)<br /><br /> `<basicHttpBinding> <binding name="Binding1"> <security mode="TransportWithMessageCredential"> <transport clientCredentialType="Basic"/> </security> </basicHttpBinding>`|  
|Segurança|Segurança de mensagem SOAP do WSS X. 509 perfil de token de certificado 1,0|[Segurança de mensagem SOAP do WSS X. 509 perfil de token de certificado 1,0](http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-x509-token-profile-1.0.pdf)<br /><br /> `<basicHttpBinding>   <security mode="Message"> <message clientCredentialType="Certificate"/> </security> </basicHttpBinding>`|  
  
### <a name="wshttpbinding-ws2007httpbinding-and-wsdualhttpbinding"></a>wsHttpBinding, ws2007HttpBinding e wsDualHttpBinding  
  
|Categoria|Protocolo|Especificação e uso|  
|--------------|--------------|-----------------------------|  
|Mensagens|SOAP 1,2|[Instruções elementares](https://www.w3.org/TR/soap12-part0/)<br /><br /> [Estrutura de mensagens](https://www.w3.org/TR/2007/REC-soap12-part1-20070427/)<br /><br /> [Adjuncts (incluindo associação HTTP)](https://www.w3.org/TR/soap12-part2/)|  
|Mensagens|WS-Addressing 2005/08|[Endereçamento de serviços da Web 1,0-Core](https://www.w3.org/TR/ws-addr-core/)<br /><br /> [Endereçamento de serviços da Web 1,0-SOAP](https://www.w3.org/TR/ws-addr-soap/)<br /><br /> O `wsHttpBinding` , o `ws2007HttpBinding` e `wsDualHttpBinding` o implementam a recomendação de WS-addressing do World Wide Web Consortium (W3C) para habilitar mensagens assíncronas, correlação de mensagens e mecanismos de endereçamento de transporte neutro.<br /><br /> O WCF não dá suporte à criptografia de cabeçalhos de WS-Addressing, embora isso seja permitido pelas especificações WS-*.|  
|Mensagens|WS-Addressing 1,0-Metadata|[Metadados 1,0 do WS-Addressing](https://www.w3.org/2007/05/addressing/metadata/) O suporte para esse protocolo é habilitado pela definição da versão da política no comportamento de metadados-com PolicyVersion definido como 1,2 (o padrão), a descrição do WSDL é compatível com o WSDL do WS-Addressing, com PolicyVersion definido como 1,5, a descrição do WSDL é compatível com os metadados do WS-Addressing.<br /><br /> O WCF não dá suporte à criptografia de cabeçalhos de WS-Addressing, embora isso seja permitido pelas especificações WS-*.|  
|Segurança|Segurança de mensagem SOAP do WSS 1,0|[Segurança de mensagem SOAP do WSS 1,0](http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-soap-message-security-1.0.pdf)<br /><br /> Use quando o `securityMode` atributo for definido como "wsSecurityOverHttp" (padrão) e os parâmetros forem configurados usando um `wsSecurity` elemento filho.<br /><br /> `<wsHttpBinding>   <binding name="myBinding">      <security mode="Message" .../>   </binding> </wsHttpBinding>`|  
|Segurança|Perfil do UsernameToken de segurança de mensagem SOAP do WSS 1,1|[Perfil do UsernameToken de segurança de mensagem SOAP do WSS 1,0](https://www.oasis-open.org/committees/download.php/16782/wss-v1.1-spec-os-UsernameTokenProfile.pdf)<br /><br /> Use quando o `wsSecurity` atributo do elemento `authenticationMode` estiver definido como "username".<br /><br /> `<wsHttpBinding>   <binding name="MyBinding">     <security mode="Message>       <message           clientCredentialType="UserName        negotiateServiceCredential="false"        establishSecurityContext="false"/>     </security> </binding> </wsHttpBinding>`|  
|Segurança|Segurança de mensagem SOAP do WSS X. 509 perfil de token de certificado 1,1|[Segurança de mensagem SOAP do WSS X. 509 perfil de token de certificado 1,1](https://www.oasis-open.org/committees/download.php/16785/wss-v1.1-spec-os-x509TokenProfile.pdf)<br /><br /> Use para proteção de mensagem quando o `wsSecurity` atributo do elemento `authenticationMode` estiver definido como "username", "Certificate" ou "None". Além disso, use isso para autenticação de cliente quando o `wsSecurity` atributo do elemento `authenticationMode` estiver definido como "certificado".<br /><br /> `<wsHttpBinding>   <binding name="MyBinding">     <security mode="Message>       <message           clientCredentialType="Certificate"        negotiateServiceCredential="false"        establishSecurityContext="false"/>     </security>   </binding> </wsHttpBinding>`|  
|Segurança|Perfil de token Kerberos de segurança de mensagem SOAP do WSS 1,1|[Perfil de token Kerberos de segurança de mensagem SOAP do WSS 1,1](https://www.oasis-open.org/committees/download.php/16788/wss-v1.1-spec-os-KerberosTokenProfile.pdf)<br /><br /> Use para autenticação e proteção de mensagem quando o `wsSecurity` atributo do elemento `authenticationMode` estiver definido como "Windows".<br /><br /> `<wsHttpBinding>   <binding name="MyBinding">     <security mode="Message>       <message           clientCredentialType="Windows"        negotiateServiceCredential="false"        establishSecurityContext="false"/>     </security>   </binding> </wsHttpBinding>`|  
|Segurança|WS-SecureConversation|[WS-SecureConversation](http://specs.xmlsoap.org/ws/2005/02/sc/ws-secureconversation.pdf)<br /><br /> Use para fornecer uma sessão segura quando o `security/@mode` atributo for definido como "Message" e o `message/@establishSecurityContext` atributo for definido como "true" (padrão).|  
|Segurança|WS-Trust|[WS-Trust](http://specs.xmlsoap.org/ws/2005/02/trust/ws-trust.pdf)<br /><br /> Usado pelo WS-SecureConversation (veja acima).|  
|Mensagens confiáveis|WS-ReliableMessaging|[WS-ReliableMessaging](http://specs.xmlsoap.org/ws/2005/02/rm/ws-reliablemessaging.pdf)<br /><br /> Use quando a associação estiver configurada para usar `reliableSession` .<br /><br /> `<wsHttpBinding>  <binding name="myBinding">    <reliableSession/>   </binding> </wsHttpBinding>`|  
|Transactions|WS-AtomicTransaction|[WS-AtomicTransaction](http://specs.xmlsoap.org/ws/2004/10/wsat/wsat.pdf)<br /><br /> Use para comunicação entre gerenciadores de transações. Os clientes e serviços WCF sempre usam gerenciadores de transações locais.|  
|Transactions|WS-Coordination|[WS-Coordination](https://docs.microsoft.com/previous-versions/ms951231(v=msdn.10))<br /><br /> Use para fluir o contexto da transação quando o `flowTransactions` atributo estiver definido como "Allowed" ou "required".<br /><br /> `<wsHttpBinding>   <binding transactionFlow="true"/> </wsHttpBinding>`|  
  
## <a name="wsfederationhttpbinding-and-ws2007federationhttpbinding"></a>wsFederationHttpBinding e ws2007FederationHttpBinding  
 Os [\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md) [\<ws2007FederationHttpBinding>](../../configure-apps/file-schema/wcf/ws2007federationhttpbinding.md) elementos e são introduzidos para fornecer suporte a cenários federados, em que um terceiro emite um token usado para autenticar um cliente. Além dos protocolos usados pelo `wsHttpBinding` , `wsFederationHttpBinding` aproveita:  
  
- `WS-Trust`para emissão de tokens.  
  
- Perfil de token SAML (declaração de asserções de segurança) do WSS 1,0 e 1,1 para o formato de token emitido com mais frequência.  
  
 Exemplo:  
  
```xml  
<wsFederationHttpBinding>  
  <binding name="myBinding">  
     <security mode="Message">  
       <message issuedKeyType="Symmetric"
                issuedTokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1">  
         <issuerMetadata address =
         'http://localhost/FederationSample/HomeRealmSTS/STS.svc/mex'/>  
       </message>  
     </security>  
  </binding>  
</wsFederationHttpBinding>  
```  
  
 Para obter mais informações, consulte [Federação](federation.md) .  
  
## <a name="system-provided-metadata-bindings"></a>Associações de metadados fornecidas pelo sistema  
 As tabelas a seguir descrevem os protocolos compatíveis com as associações de metadados interoperáveis fornecidas pelo sistema expostas pela <xref:System.ServiceModel.Description.MetadataExchangeBindings?displayProperty=nameWithType> classe.  
  
### <a name="mexhttpbinding"></a>mexHttpBinding  
 A [\<mexHttpBinding>](../../configure-apps/file-schema/wcf/mexhttpbinding.md) Associação dá suporte aos seguintes protocolos. Para obter mais informações sobre como usar essa associação, consulte [publicando metadados](publishing-metadata.md).  
  
|Categoria|Protocolo|Especificação e uso|  
|--------------|--------------|-----------------------------|  
|Transport|HTTP 1.1|[HTTP 1.1](https://www.ietf.org/rfc/rfc2616.txt)|  
|Mensagens|SOAP 1,2|[Instruções elementares](https://www.w3.org/TR/soap12-part0/)<br /><br /> [Estrutura de mensagens](https://www.w3.org/TR/2007/REC-soap12-part1-20070427/)<br /><br /> [Adjuncts (incluindo associação HTTP)](https://www.w3.org/TR/soap12-part2/)|  
|Mensagens|WS-Addressing 2005/08|[Endereçamento de serviços da Web 1,0-Core](https://www.w3.org/TR/ws-addr-core/)<br /><br /> [Endereçamento de serviços da Web 1,0-SOAP](https://www.w3.org/TR/ws-addr-soap/)|  
|Metadados|WS-MetadataExchange|[WS-MetadataExchange](http://specs.xmlsoap.org/ws/2004/09/mex/WS-MetadataExchange.pdf)<br /><br /> O WCF implementa o WS-MetadataExchange para recuperar o esquema XML, o WSDL e o WS-Policy.|  
  
### <a name="mexhttpsbinding"></a>mexHttpsBinding  
 [\<mexHttpsBinding>](../../configure-apps/file-schema/wcf/mexhttpsbinding.md)o oferece suporte aos seguintes protocolos. Para obter mais informações sobre como usar essa associação, consulte [publicando metadados](publishing-metadata.md).  
  
|Categoria|Protocolo|Especificação e uso|  
|--------------|--------------|-----------------------------|  
|Transport|HTTP 1.1|[HTTP 1.1](https://www.ietf.org/rfc/rfc2616.txt)<br /><br /> A segurança de transporte está habilitada.|  
|Mensagens|SOAP 1,2|[Instruções elementares](https://www.w3.org/TR/soap12-part0/)<br /><br /> [Estrutura de mensagens](https://www.w3.org/TR/2007/REC-soap12-part1-20070427/)<br /><br /> [Adjuncts (incluindo associação HTTP)](https://www.w3.org/TR/soap12-part2/)|  
|Mensagens|WS-Addressing 2005/08|[Endereçamento de serviços da Web 1,0-Core](https://www.w3.org/TR/ws-addr-core/)<br /><br /> [Endereçamento de serviços da Web 1,0-SOAP](https://www.w3.org/TR/ws-addr-soap/)|  
|Metadados|WS-MetadataExchange|[WS-MetadataExchange](http://specs.xmlsoap.org/ws/2004/09/mex/WS-MetadataExchange.pdf)<br /><br /> O WCF implementa o WS-MetadataExchange para recuperar o esquema XML, o WSDL e o WS-Policy.|  
  
## <a name="see-also"></a>Consulte também

- [Associações fornecidas pelo sistema](../system-provided-bindings.md)
- [\<basicHttpBinding>](../../configure-apps/file-schema/wcf/basichttpbinding.md)
- [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md)
- [\<wsDualHttpBinding>](../../configure-apps/file-schema/wcf/wsdualhttpbinding.md)
- [\<mexHttpsBinding>](../../configure-apps/file-schema/wcf/mexhttpsbinding.md)
- [\<mexHttpBinding>](../../configure-apps/file-schema/wcf/mexhttpbinding.md)
