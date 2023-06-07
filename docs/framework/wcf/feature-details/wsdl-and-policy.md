---
title: WSDL e política
ms.date: 03/30/2017
ms.assetid: cea87440-3519-4640-8494-b8a2b0e88c84
ms.openlocfilehash: 201920a8ebf639c74acfb20b2e990c8bbc0c5b55
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600095"
---
# <a name="wsdl-and-policy"></a>WSDL e política
Este tópico aborda os detalhes de implementação Windows Communication Foundation (WCF) WSDL 1,1, WS-Policy e WS-PolicyAttachment, bem como declarações adicionais de WS-Policy e extensões WSDL 1,1 introduzidas pelo WCF.  
  
 O WCF implementa as especificações WS-Policy e WS-PolicyAttachment enviadas ao W3C com restrições e esclarecimentos descritos neste documento.  
  
 Este documento usa os prefixos e namespaces mostrados na tabela a seguir.  
  
|Prefixo|Namespace|  
|------------|---------------|  
|WSP (WS-Policy 1,2)|`http://schemas.xmlsoap.org/ws/2004/09/policy`|  
|WSP (WS-Policy 1,5)|`http://www.w3.org/ns/ws-policy`|  
|http|`http://schemas.microsoft.com/ws/06/2004/policy/http`|  
|1.0|`http://schemas.microsoft.com/ws/06/2004/mspolicy/msmq`|  
|MSF|`http://schemas.microsoft.com/ws/2006/05/framing/policy`|  
|mssp|`http://schemas.microsoft.com/ws/2005/07/securitypolicy`|  
|MSC|`http://schemas.microsoft.com/ws/2005/12/wsdl/contract`|  
|Protection|`http://schemas.microsoft.com/net/2006/06/duplex`|  
  
## <a name="wcf-wsdl11-extensions"></a>Extensões do WCF WSDL 1.1  
 O WCF usa as seguintes extensões WSDL 1.1 para descrever os requisitos de sessão de contrato.  
  
 wsdl:portType/wsdl:operation/@msc:isInitiating  
 xs: Boolean, indica que esta operação inicia uma sessão do WCF; o valor padrão é `false` .  
  
 wsdl:portType/wsdl:operation/@msc:isTerminating  
 xs: Boolean, indica que esta operação encerra uma sessão do WCF; o valor padrão é `false` .  
  
 wsdl:portType/wsdl:operation/@msc:usingSession  
 xs: Boolean, indica que este contrato requer que a sessão seja estabelecida.  
  
### <a name="soap-1x-http-binding-transport-uris"></a>URIs de transporte de associação HTTP SOAP 1. x  
 O WCF usa os seguintes URIs para indicar os transportes a serem usados para elementos de extensão de associação WSDL 1,1, SOAP 1,1 e SOAP 1,2.  
  
|Transport|URI|  
|---------------|---------|  
|HTTP|`http://schemas.xmlsoap.org/soap/http`|  
|TCP|`http://schemas.microsoft.com/soap/tcp`|  
|MSMQ|`http://schemas.microsoft.com/soap/msmq`|  
|Pipes nomeados|`http://schemas.microsoft.com/soap/named-pipe`|  
  
## <a name="policy-assertions-implemented-by-wcf"></a>Declarações de política implementadas pelo WCF  
 Além das declarações de política introduzidas nas especificações de serviços da Web (WS-*) e mencionadas em outras seções deste documento, o WCF implementa as seguintes declarações de política.  
  
|Declaração de política|Assunto da política|Descrição|  
|----------------------|--------------------|-----------------|  
|http: HttpBasicAuthentication|Ponto de Extremidade|O ponto de extremidade usa a autenticação básica HTTP.|  
|http: HttpDigestAuthentication|Ponto de Extremidade|O ponto de extremidade usa autenticação digest HTTP.|  
|http: HttpNegotiateAuthentication|Ponto de Extremidade|O ponto de extremidade usa autenticação de negociação HTTP.|  
|http: HttpNtlmAuthentication|Ponto de Extremidade|O ponto de extremidade usa a autenticação HTTP NTLM.|  
|MSF: em fluxo|Ponto de Extremidade|O ponto de extremidade usa enquadramento de mensagens em fluxo. Essa asserção é usada com o protocolo de enquadramento de mensagens fornecido para transportes como TCP e pipes nomeados.|  
|MSF: SslTransportSecurity|Ponto de Extremidade|O ponto de extremidade usa TLS (segurança de camada de transporte) com enquadramento de mensagem.|  
|MSF: WindowsTransportSecurity|Ponto de Extremidade|O ponto de extremidade usa negociação de provedor de segurança (SPNEGO) com enquadramento de mensagem.|  
|MSMQ: MsmqBestEffort|Ponto de Extremidade|MSMQ com garantias de melhor esforço.|  
|MSMQ: MsmqSession|Ponto de Extremidade|MSMQ com garantias de sessão.|  
|MSMQ: MsmqVolatile|Ponto de Extremidade|O MSMQ volátil.|  
|MSMQ: autenticado|Ponto de Extremidade|A autenticação é usada com o transporte MSMQ.|  
|MSMQ: WindowsDomain|Ponto de Extremidade|O MSMQ usa a autenticação de domínio do Windows.|  
|CDP: CompositeDuplex|Ponto de Extremidade|O ponto de extremidade usa duas conexões de transporte de inverso separadas para mensagens de entrada e saída.|  
|mssp:RsaToken|aninhado|Asserção de token de chave RSA. Esse requisito normalmente é atendido por uma chave RSA serializada diretamente como parte das informações de chave em uma assinatura de endosso.|  
|mssp:SslContextToken|aninhado|Requer que um SecurityContextToken obtido usando handshake TLS binário usando WS-Trust seja usado. As asserções aninhadas incluem: SP: RequireDerivedKeys, MSSP: MustNotSendCancel, MSSP: RequireClientCertificate.|  
|mssp:MustNotSendCancel|aninhado|Especifica um requisito de que as mensagens de solicitação de token de segurança (RST) de solicitação [WS-Trust] usando a associação de cancelamento [WS-Trust, WS-SC] não sejam enviadas ao emissor de um determinado SecurityContextToken. Se essa declaração estiver presente, essas mensagens de solicitação não deverão ser enviadas ao emissor. Se essa declaração não estiver presente, essas mensagens de solicitação poderão ser enviadas para o emissor.|  
|mssp:RequireClientCertificate|aninhado|Esse elemento opcional especifica um requisito para que um certificado de cliente seja fornecido como parte do protocolo TLSNEGO. Se essa declaração estiver presente, um certificado de cliente deverá ser fornecido. Se essa declaração não estiver presente, um certificado de cliente não deverá ser fornecido. Esta asserção não deve ser usada fora de MSSP: SslContextToken.|  
  
## <a name="see-also"></a>Consulte também

- [Publicação personalizada de WSDL](../samples/custom-wsdl-publication.md)
- [Como exportar o WSDL personalizado](../extending/how-to-export-custom-wsdl.md)
- [Como importar WSDL personalizado](../extending/how-to-import-custom-wsdl.md)
