---
title: Protocolos de transação versão 1.0
ms.date: 03/30/2017
ms.assetid: 034679af-0002-402e-98a8-ef73dcd71bb6
ms.openlocfilehash: f725361b9a90c9336b763cc7f292ae043e445966
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598704"
---
# <a name="transaction-protocols-version-10"></a>Protocolos de transação versão 1.0
Windows Communication Foundation (WCF) versão 1 implementa a versão 1,0 dos protocolos WS-Atomic Transaction e WS-Coordination. Para obter mais informações sobre a versão 1,1, consulte [protocolos de transação](transaction-protocols.md).  
  
|Especificação/documento|Link|  
|-----------------------------|----------|  
|WS-Coordination|<https://specs.xmlsoap.org/ws/2004/10/wscoor/wscoor.pdf>|  
|WS-AtomicTransaction|<https://specs.xmlsoap.org/ws/2004/10/wsat/wsat.pdf>|  
  
 A interoperabilidade nessas especificações de protocolo é necessária em dois níveis: entre aplicativos e gerenciadores de transações (consulte a figura a seguir). As especificações descrevem detalhadamente os formatos de mensagem e a troca de mensagens para os níveis de interoperabilidade. Determinadas segurança, confiabilidade e codificações para troca de aplicativo para aplicativo se aplicam como fazem para o intercâmbio de aplicativos regular. No entanto, a interoperabilidade bem-sucedida entre gerenciadores de transações requer contrato na associação específica, pois normalmente não é configurada pelo usuário.  
  
 Este tópico descreve uma composição da especificação WS-Atomic (WS-AT) com segurança e descreve a associação segura usada para comunicação entre gerenciadores de transações. A abordagem descrita neste documento foi testada com êxito com outras implementações de WS-AT e WS-Coordination, incluindo IBM, IONA, Sun Microsystems e outros.  
  
 A figura a seguir descreve a interoperabilidade entre dois gerenciadores de transações, o Gerenciador de transações 1 e o Gerenciador de transações 2 e dois aplicativos, aplicativo 1 e aplicativo 2:  
  
 ![Captura de tela que mostra a interação entre gerenciadores de transações.](./media/transaction-protocols/transaction-managers-flow.gif)  
  
 Considere um cenário de transação WS-Coordination/WS-Atomic típico com um iniciador (I) e um participante (P). O iniciador e o participante têm gerenciadores de transações, (ITM e PTM, respectivamente). A confirmação de duas fases é conhecida como 2PC neste tópico.  
  
|||  
|-|-|  
|1. CreateCoordinationContext|12. resposta de mensagem do aplicativo|  
|2. CreateCoordinationContextResponse|13. Commit (conclusão)|  
|3. registrar (conclusão)|14. preparar (2PC)|  
|4. RegisterResponse|15. preparar (2PC)|  
|5. mensagem do aplicativo|16. preparado (2PC)|  
|6. CreateCoordinationContext com contexto|17. preparado (2PC)|  
|7. registrar (durável)|18. confirmado (conclusão)|  
|8. RegisterResponse|19. Commit (2PC)|  
|9. CreateCoordinationContextResponse|20. Commit (2PC)|  
|10. Register (durável)|21. confirmado (2PC)|  
|11. RegisterResponse|22. confirmado (2PC)|  
  
 Este documento descreve uma composição da especificação WS-AtomicTransaction com segurança e descreve a associação segura usada para comunicação entre gerenciadores de transações. A abordagem descrita neste documento foi testada com êxito com outras implementações de WS-AT e WS-Coordination.  
  
 A figura e a tabela ilustram quatro classes de mensagens do ponto de vista da segurança:  
  
- Mensagens de ativação (CreateCoordinationContext e CreateCoordinationContextResponse).  
  
- Mensagens de registro (Register and RegisterResponse)  
  
- Mensagens de protocolo (preparação, reversão, confirmação, anulada e assim por diante).  
  
- Mensagens de aplicativo.  
  
 As três primeiras classes de mensagem são consideradas mensagens do Gerenciador de transações e sua configuração de associação é descrita na "troca de mensagens do aplicativo" mais adiante neste tópico. A quarta classe de mensagem é aplicativo para mensagens de aplicativo e é descrita na seção "exemplos de mensagem" mais adiante neste tópico. Esta seção descreve as associações de protocolo usadas para cada uma dessas classes pelo WCF.  
  
 Os namespaces XML e os prefixos associados a seguir são usados em todo este documento.  
  
|Prefixo|URI de namespace|  
|------------|-------------------|  
|S11|`http://schemas.xmlsoap.org/soap/envelope`|  
|WSA|`http://www.w3.org/2004/08/addressing`|  
|wscoor|`http://schemas.xmlsoap.org/ws/2004/10/wscoor`|  
|WSAT|`http://schemas.xmlsoap.org/ws/2004/10/wsat`|  
|t|`http://schemas.xmlsoap.org/ws/2005/02/trust`|  
|o|`http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd`|  
|xsd|`http://www.w3.org/2001/XMLSchema`|  
  
## <a name="transaction-manager-bindings"></a>Associações do Gerenciador de transações  
 R1001: os gerenciadores de transações devem usar SOAP 1,1 e WS-Addressing 2004/08 para transações WS-atômica e trocas de mensagens WS-Coordination.  
  
 As mensagens de aplicativo não são restritas a essas associações e são descritas posteriormente.  
  
### <a name="transaction-manager-https-binding"></a>Associação HTTPS do Gerenciador de transações  
 A associação HTTPS do Gerenciador de transações depende exclusivamente da segurança de transporte para obter segurança e estabelecer confiança entre cada par remetente-destinatário na árvore de transações.  
  
#### <a name="https-transport-configuration"></a>Configuração de transporte HTTPS  
 Os certificados X. 509 são usados para estabelecer a identidade do Gerenciador de transações. A autenticação de cliente/servidor é necessária e a autorização de cliente/servidor é deixada como um detalhe de implementação:  
  
- R1111: os certificados X. 509 apresentados pela transmissão devem ter um nome de entidade que corresponda ao FQDN (nome de domínio totalmente qualificado) da máquina de origem.  
  
- B1112: o DNS deve ser funcional entre cada par remetente-destinatário no sistema para que as verificações de nome de entidade X. 509 tenham sucesso.  
  
#### <a name="activation-and-registration-binding-configuration"></a>Configuração de associação de registro e ativação  
 O WCF requer Associação duplex de solicitação/resposta com correlação por HTTPS. (Para obter mais informações sobre correlação e descrições dos padrões de troca de mensagens de solicitação/resposta, consulte WS-Atomic Transaction, seção 8.)  
  
#### <a name="2pc-protocol-binding-configuration"></a>Configuração de associação de protocolo 2PC  
 O WCF dá suporte a mensagens de uma via (datagrama) via HTTPS. A correlação entre as mensagens é deixada como um detalhe de implementação.  
  
 B2131: as implementações devem dar suporte `wsa:ReferenceParameters` conforme descrito em WS-Addressing para obter a correlação das mensagens 2pc do WCF.  
  
### <a name="transaction-manager-mixed-security-binding"></a>Associação de segurança mista do Gerenciador de transações  
 Essa é uma associação alternativa (modo misto) que usa a segurança de transporte combinada com o modelo de token emitido pelo WS-Coordination para fins de estabelecimento de identidade.  A ativação e o registro são os únicos elementos que diferem entre as duas associações.  
  
#### <a name="https-transport-configuration"></a>Configuração de transporte HTTPS  
 Os certificados X. 509 são usados para estabelecer a identidade do Gerenciador de transações. A autenticação de cliente/servidor é necessária e a autorização de cliente/servidor é deixada como um detalhe de implementação.  
  
#### <a name="activation-message-binding-configuration"></a>Configuração de associação de mensagem de ativação  
 As mensagens de ativação geralmente não participam da interoperabilidade porque elas normalmente ocorrem entre um aplicativo e seu Gerenciador de transações local.  
  
 B1221: o WCF usa a associação HTTPS duplex (descrita em [protocolos de mensagens](messaging-protocols.md)) para mensagens de ativação. As mensagens de solicitação e de resposta são correlacionadas usando WS-Addressing 2004/08.  
  
 A especificação de transação WS-Atomic, seção 8, descreve mais detalhes sobre a correlação e os padrões de troca de mensagens.  
  
- R1222: após receber um `CreateCoordinationContext` , o coordenador deve emitir um `SecurityContextToken` com um segredo associado `STx` . Esse token é retornado dentro de um `t:IssuedTokens` cabeçalho após a especificação WS-Trust.  
  
- R1223: se a ativação ocorrer em um contexto de coordenação existente, o `t:IssuedTokens` cabeçalho com o `SecurityContextToken` contexto existente associado a deve fluir na `CreateCoordinationContext` mensagem.  
  
 Um novo `t:IssuedTokens` cabeçalho deve ser gerado para anexar à mensagem de saída `wscoor:CreateCoordinationContextResponse` .  
  
#### <a name="registration-message-binding-configuration"></a>Configuração de associação de mensagens de registro  
 B1231: o WCF usa a vinculação HTTPS duplex (descrita em [protocolos de mensagens](messaging-protocols.md)). As mensagens de solicitação e de resposta são correlacionadas usando WS-Addressing 2004/08.  
  
 O WS-AtomicTransaction, seção 8, descreve mais detalhes sobre correlação e descrições dos padrões de troca de mensagens.  
  
 R1232: `wscoor:Register` as mensagens de saída devem usar o `IssuedTokenOverTransport` modo de autenticação descrito em [protocolos de segurança](security-protocols.md).  
  
 O `wsse:Timestamp` elemento deve ser assinado usando o `SecurityContextToken STx` emitido. Essa assinatura é uma prova de posse do token associado à transação específica e é usada para autenticar um participante que está inscrito na transação. A mensagem RegistrationResponse é enviada de volta por HTTPS.  
  
#### <a name="2pc-protocol-binding-configuration"></a>Configuração de associação de protocolo 2PC  
 O WCF dá suporte a mensagens de uma via (datagrama) via HTTPS. A correlação entre as mensagens é deixada como um detalhe de implementação.  
  
 B2131: as implementações devem dar suporte `wsa:ReferenceParameters` conforme descrito em WS-Addressing para obter a correlação das mensagens 2pc do WCF.  
  
## <a name="application-message-exchange"></a>Troca de mensagens do aplicativo  
 Os aplicativos são livres para usar qualquer ligação específica para mensagens de aplicativo para aplicativo, desde que a ligação atenda aos seguintes requisitos de segurança:  
  
- R2001: as mensagens de aplicativo para aplicativo devem fluir o `t:IssuedTokens` cabeçalho junto com o `CoordinationContext` no cabeçalho da mensagem.  
  
- R2002: a integridade e a confidencialidade do `t:IssuedToken` devem ser fornecidas.  
  
 O `CoordinationContext` cabeçalho contém `wscoor:Identifier` . Embora a definição de `xsd:AnyURI` permita o uso de URIs absolutos e relativos, o WCF dá suporte apenas `wscoor:Identifiers` a URIs absolutos.  
  
 Se o `wscoor:Identifier` do `wscoor:CoordinationContext` for um URI relativo, as falhas serão retornadas dos serviços do WCF transacionais.  
  
## <a name="message-examples"></a>Exemplos de mensagem  
  
### <a name="createcoordinationcontext-requestresponse-messages"></a>Mensagens de solicitação/resposta do CreateCoordinationContext  
 As mensagens a seguir seguem um padrão de solicitação/resposta.  
  
#### <a name="createcoordinationcontext"></a>CreateCoordinationContext  
  
```xml  
<s:Envelope>  
  <s:Header>  
    <a:Action>http://.../ws/2004/10/wscoor/CreateCoordinationContext</Action>  
    <a:MessageID>urn:uuid:069f5104-fd88-4264-9f99-60032a82854e</MessageID>  
    <a:ReplyTo>  
      <Address>https://...</a:Address>  
    </a:ReplyTo>  
    <a:To>https://...</a:To>  
    <wsse:Security>  
      <u:Timestamp>  
        <wsu:Created>2005-12-15T23:36:09.921Z</wsu:Created>  
        <wsu:Expires>2005-12-15T23:41:09.921Z</wsu:Expires>  
      </u:Timestamp>  
    </wsse:Security>  
  </s:Header>  
  <s:Body xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">  
    <wscoor:CreateCoordinationContext>  
      <wscoor:CoordinationType>...</wscoor:CoordinationType>  
    </wscoor:CreateCoordinationContext>  
  </s:Body>  
</s11:Envelope>  
```  
  
#### <a name="createcoordinationcontextresponse"></a>CreateCoordinationContextResponse  
  
```xml  
<s:Envelope>  
  <!-- Data below is shown in the clear for  
       illustration purposes only. -->  
  <s:Header>  
    <a:Action>./ws/2004/10/wscoor/CreateCoordinationContextResponse </a:Action>  
    <a:RelatesTo>urn:uuid:069f5104-fd88-4264-9f99-60032a82854e</a:RelatesTo>  
    <a:To s:mustUnderstand="1">https://... </a:To>  
    <t:IssuedTokens>  
 <wst:RequestSecurityTokenResponse
    xmlns:wsse="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"  
    xmlns:wssu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd"
    xmlns:wst="http://schemas.xmlsoap.org/ws/2005/02/trust"  
    xmlns:wsc="http://schemas.xmlsoap.org/ws/2005/02/sc"  
    xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">  
    <wst:TokenType>http://schemas.xmlsoap.org/ws/2005/02/sc/sct</wst:TokenType>  
    <wst:RequestedSecurityToken>  
      <wsc:SecurityContextToken>  
        <wssu:Identifier>  
          http://fabrikam123.com/SCTi  
        </wssu:Identifier>  
      </wsc:SecurityContextToken>
    </wst:RequestedSecurityToken>  
    <wsp:AppliesTo>  
        http://fabrikam123.com/CCi  
    </wsp:AppliesTo>
    <wst:RequestedAttachedReference>  
      <wsse:SecurityTokenReference >  
        <wsse:Reference
           ValueType="http://schemas.xmlsoap.org/ws/2005/02/sc/sct"  
           URI="http://fabrikam123.com/SCTi"/>  
      </wsse:SecurityTokenReference>  
    </wst:RequestedAttachedReference>  
    <wst:RequestedUnattachedReference>  
      <wsse:SecurityTokenReference>  
        <wsse:Reference
          ValueType="http://schemas.xmlsoap.org/ws/2005/02/sc/sct"  
          URI="http://fabrikam123.com/SCTi"/>  
      </wsse:SecurityTokenReference>  
    </wst:RequestedUnattachedReference>  
    <wst:RequestedProofToken>  
      <wst:BinarySecret
        Type="http://schemas.xmlsoap.org/ws/2005/02/trust/SymmetricKey">  
        <!-- base64 encoded value -->  
      </wst:BinarySecret>  
    </wst:RequestedProofToken>  
    <wst:Lifetime>  
      <wssu:Created>2005-10-24T20:19:26.526Z</wssu:Created>  
      <wssu:Expires>2005-10-25T06:24:26.526Z</wssu:Expires>  
    </wst:Lifetime>  
    <wst:KeySize>256</wst:KeySize>  
</wst:RequestSecurityTokenResponse>  
    </t:IssuedTokens>  
    <o:Security xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd">  
      <u:Timestamp u:Id="_0">  
        <u:Created>2005-12-15T23:36:12.015Z</u:Created>  
        <u:Expires>2005-12-15T23:41:12.015Z</u:Expires>  
      </u:Timestamp>  
    </o:Security>  
  </s:Header>  
  <s:Body>  
    <wscoor:CreateCoordinationContextResponse>  
      <wscoor:CoordinationContext>  
        <wscoor:Identifier>  
     http://fabrikam123.com/CCi  
      </wscoor:Identifier>  
        <wscoor:Expires>...</wscoor:Expires>  
        <wscoor:CoordinationType>...</wscoor:CoordinationType>  
        <wscoor:RegistrationService>  
          <a:Address>https://...</a:Address>  
          <a:ReferenceParameters>  
             ...  
          </a:ReferenceParameters>  
        </wscoor:RegistrationService>  
      </wscoor:CoordinationContext>  
    </wscoor:CreateCoordinationContextResponse>  
  </s:Body>  
</s:Envelope>  
```  
  
### <a name="registration-messages"></a>Mensagens de registro  
 As mensagens a seguir são mensagens de registro.  
  
#### <a name="register"></a>Registre-se  
  
```xml  
<s:Envelope>  
  <s:Header>  
    <a:Action>http://schemas.xmlsoap.org/ws/2004/10/wscoor/Register</a:Action>  
    <a:MessageID>urn:uuid:ed418b86-a75e-4aea-9d4e-a5d0cb5c088e</a:MessageID>  
    <a:ReplyTo>  
      <a:Address>https://...</a:Address>
    </a:ReplyTo>  
    <a:To>https://...</a:To>  
    <wsse:Security
      s:mustUnderstand="1"
      xmlns:wsse="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"  
      xmlns:wssu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">  
      <wssu:Timestamp wssu:Id="_0" >  
        <wssu:Created>2005-12-15T23:36:13.827Z</wssu:Created>  
        <wssu:Expires>2005-12-15T23:41:13.827Z</wssu:Expires>  
      </wssu:Timestamp>  
      <wsc:SecurityContextToken>  
      <wssu:Identifier>  
          http://fabrikam123.com/SCTi  
      </wssu:Identifier>  
      </wsc:SecurityContextToken>  
      <!-- supporting signature over the timestamp -->  
      <wsse:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">  
        <ds:SignedInfo>  
          <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>  
          <ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#hmac-sha1"/>  
          <ds:Reference URI="#_0">  
            <ds:Transforms>  
              <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>  
            </ds:Transforms>  
            <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>  
            <ds:DigestValue>  
              alRzyhjLgoUOYoh8cx4n75eTcUk=  
            </ds:DigestValue>  
          </ds:Reference>  
        </ds:SignedInfo>  
        <ds:SignatureValue>YZYjnVvSOVasAQqQxaaviTSWtqI=</ds:SignatureValue>  
        <ds:KeyInfo>  
          <wsse:SecurityTokenReference  
            xmlns:wsse="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd">  
            <wsse:Reference
              URI="http://fabrikam123.com/SCTi"/>  
          </wsse:SecurityTokenReference>  
        </ds:KeyInfo>  
      </wsse:Signature>  
    </wsse:Security>  
  </s:Header>  
  <s:Body xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">  
    <wscoor:Register>  
      <wscoor:ProtocolIdentifier>...</wscoor:ProtocolIdentifier>  
      <wscoor:ParticipantProtocolService>  
        <a:Address>https://... </a:Address>  
      </wscoor:ParticipantProtocolService>  
    </wscoor:Register>  
  </s:Body>  
</s:Envelope>  
```  
  
#### <a name="register-response"></a>Registrar resposta  
  
```xml  
<s:Envelope>  
  <s:Header>  
    <a:Action>  
      http://schemas.xmlsoap.org/ws/2004/10/wscoor/RegisterResponse  
    </a:Action>  
    <a:MessageID>urn:uuid:ed418b86-a75e-4aea-9d4e-a5d0cb5c088d</a:MessageID>  
    <a:RelatesTo>  
      urn:uuid:ed418b86-a75e-4aea-9d4e-a5d0cb5c088e
    </a:RelatesTo>  
    <a:To>https://...</a:To>  
    <wsse:Security
      s:mustUnderstand="1"
      xmlns:wsse="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"  
      xmlns:wssu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">  
      <wssu:Timestamp>  
        <wssu:Created>2005-12-15T23:36:13.827Z</wssu:Created>  
        <wssu:Expires>2005-12-15T23:41:13.827Z</wssu:Expires>  
      </wssu:Timestamp>  
    </wsse:Security>  
  </s:Header>  
  <s:Body xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">  
    <wscoor:RegisterResponse>  
      <wscoor:CoordinatorProtocolService>  
        <a:Address>https://...</a:Address>  
        <a:ReferenceParameters>  
          ...  
        </a:ReferenceParameters>  
      </wscoor:CoordinatorProtocolService>  
    </wscoor:RegisterResponse>  
  </s:Body>  
</s:Envelope>  
```  
  
### <a name="two-phase-commit-protocol-messages"></a>Mensagens de protocolo de confirmação de duas fases  
 A mensagem a seguir está relacionada ao protocolo de confirmação de duas fases (2PC).  
  
#### <a name="commit"></a>Commit  
  
```xml  
<s:Envelope>  
  <s:Header>  
    <a:Action>http://.../ws/2004/10/wsat/Commit</a:Action>  
    <a:To>https://...</a:To>  
    <wsse:Security
      s:mustUnderstand="1"
      xmlns:wsse="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"  
      xmlns:wssu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">  
      <wssu:Timestamp wssu:Id="_0" >  
        <wssu:Created>2005-12-15T23:36:13.827Z</wssu:Created>  
        <wssu:Expires>2005-12-15T23:41:13.827Z</wssu:Expires>  
      </wssu:Timestamp>  
   </wsse:Security>  
  </s:Header>  
  <s:Body xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">  
    <wsat:Commit />  
  </s:Body>  
</s:Envelope>  
```  
  
### <a name="application-messages"></a>Mensagens de aplicativo  
 As mensagens a seguir são mensagens de aplicativo.  
  
#### <a name="application-message-request"></a>Solicitação de mensagem de aplicativo  
  
```xml  
<s:Envelope>  
  <s:Header>  
<!-- Addressing headers, all signed-->  
    <wsse:Security s:mustUnderstand="1">  
      <wssu:Timestamp wssu:Id="timestamp">
        <wssu:Created>2005-10-25T06:29:18.703Z</wssu:Created>  
        <wssu:Expires>2005-10-25T06:34:18.703Z</wssu:Expires>  
      </wssu:Timestamp>  
      <wsse:BinarySecurityToken
          wssu:Id="IA_Certificate"
          ValueType="...#X509v3"
          EncodingType="...#Base64Binary">  
        <!-- IA certificate -->  
      </wsse:BinarySecurityToken>  
      <e:EncryptedKey Id="encrypted_key">  
            <!-- ephemeral key encrypted for PA certificate -->
        <e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#">  
          <e:DataReference URI="#encrypted_body"/>  
          <e:DataReference URI="#encrypted_CCi"/>  
          <e:DataReference URI="#encrypted_issuedtokens"/>  
        </e:ReferenceList>  
      </e:EncryptedKey>  
      <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">  
        <!-- signature over Addressing headers, Timestamp, and Body -->  
      </Signature>  
    </wsse:Security>  
    <wsse11:EncryptedHeader>  
     <!-- encrypted wscoor:CoordinationContext header containing CCi -->  
    </wsse11:EncryptedHeader>  
    <wsse11:EncryptedHeader>
      <!-- encrypted wst:IssuedTokens header containing SCTi -->  
      <!-- wst:IssuedTokens header is taken verbatim from message #2 above, omitted for brevity -->  
    </wsse11:EncryptedHeader>  
  </s:Header>  
  <s:Body wssu:Id="body">  
    <!-- encrypted content of the Body element of the application message -->
    <e:EncryptedData Id="encrypted_body"
           Type="http://www.w3.org/2001/04/xmlenc#Content"
           xmlns:e="http://www.w3.org/2001/04/xmlenc#">  
...  
    </e:EncryptedData>  
  </s:Body>  
</s:Envelope>  
```
