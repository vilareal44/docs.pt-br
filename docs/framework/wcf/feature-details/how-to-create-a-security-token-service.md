---
title: Como criar um serviço de token de segurança
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, federation
- federation
ms.assetid: 98e82101-4cff-4bb8-a220-f7abed3556e5
ms.openlocfilehash: 1cfcca524e5dd2b0c1560eb7600795766e2db1d6
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598951"
---
# <a name="how-to-create-a-security-token-service"></a>Como criar um serviço de token de segurança
Um serviço de token de segurança implementa o protocolo definido na especificação WS-Trust. Esse protocolo define os formatos de mensagem e os padrões de troca de mensagens para emissão, renovação, cancelamento e validação de tokens de segurança. Um determinado serviço de token de segurança fornece um ou mais desses recursos. Este tópico analisa o cenário mais comum: implementando a emissão de tokens.  
  
## <a name="issuing-tokens"></a>Emitindo tokens  
 O WS-Trust define formatos de mensagem, com base no `RequestSecurityToken` elemento de esquema XSD (linguagem de definição de esquema XML) e no `RequestSecurityTokenResponse` elemento de esquema XSD para executar a emissão de tokens. Além disso, ele define os URIs (identificadores de recursos uniformes) da ação associada. O URI de ação associado à `RequestSecurityToken` mensagem é `http://schemas.xmlsoap.org/ws/2005/02/trust/RST/Issue` . O URI de ação associado à `RequestSecurityTokenResponse` mensagem é `http://schemas.xmlsoap.org/ws/2005/02/trust/RSTR/Issue` .  
  
### <a name="request-message-structure"></a>Estrutura da mensagem de solicitação  
 A estrutura da mensagem de solicitação de problema normalmente consiste nos seguintes itens:  
  
- Um URI de tipo de solicitação com um valor de `http://schemas.xmlsoap.org/ws/2005/02/trust/Issue` .
  
- Um URI de tipo de token. Para tokens 1,1 SAML (Security Asserts Markup Language), o valor desse URI é `http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1` .  
  
- Um valor de tamanho de chave que indica o número de bits na chave a ser associado ao token emitido.  
  
- Um URI de tipo de chave. Para chaves simétricas, o valor desse URI é `http://schemas.xmlsoap.org/ws/2005/02/trust/SymmetricKey` .  
  
 Além disso, alguns outros itens podem estar presentes:  
  
- Material da chave fornecido pelo cliente.  
  
- Informações de escopo que indicam o serviço de destino com o qual o token emitido será usado.  
  
 O serviço de token de segurança usa as informações na mensagem de solicitação de problema ao construir a mensagem de resposta do problema.  
  
## <a name="response-message-structure"></a>Estrutura da mensagem de resposta  
 A estrutura da mensagem de resposta do problema normalmente consiste nos seguintes itens;  
  
- O token de segurança emitido, por exemplo, uma Asserção SAML 1,1.  
  
- Um token de prova associado ao token de segurança. Para chaves simétricas, isso geralmente é uma forma criptografada do material da chave.  
  
- Referências ao token de segurança emitido. Normalmente, o serviço de token de segurança retorna uma referência que pode ser usada quando o token emitido aparece em uma mensagem subsequente enviada pelo cliente e outra que pode ser usada quando o token não está presente nas mensagens subsequentes.  
  
 Além disso, alguns outros itens podem estar presentes:  
  
- Material da chave fornecido pelo serviço de token de segurança.  
  
- O algoritmo necessário para computar a chave compartilhada.  
  
- Informações de tempo de vida para o token emitido.  
  
## <a name="processing-request-messages"></a>Processando mensagens de solicitação  
 O serviço de token de segurança processa a solicitação de problema examinando as várias partes da mensagem de solicitação e garantindo que ela possa emitir um token que satisfaça a solicitação. O serviço de token de segurança deve determinar o seguinte antes de construir o token a ser emitido:  
  
- A solicitação é realmente uma solicitação para que um token seja emitido.  
  
- O serviço de token de segurança dá suporte ao tipo de token solicitado.  
  
- O solicitante está autorizado a fazer a solicitação.  
  
- O serviço de token de segurança pode atender às expectativas do solicitante em relação ao material da chave.  
  
 Duas partes vitais da construção de um token estão determinando a chave com a qual assinar o token e a chave com a qual criptografar a chave compartilhada. O token precisa ser assinado para que, quando o cliente apresentar o token para o serviço de destino, esse serviço possa determinar que o token foi emitido por um serviço de token de segurança que ele confia. O material da chave precisa ser criptografado de forma que o serviço de destino possa descriptografar esse material da chave.  
  
 Assinar uma Asserção SAML envolve a criação de uma <xref:System.IdentityModel.Tokens.SigningCredentials> instância. O construtor para essa classe usa o seguinte:  
  
- Um <xref:System.IdentityModel.Tokens.SecurityKey> para a chave a ser usada para assinar a ASSERÇÃO SAML.  
  
- Uma cadeia de caracteres que identifica o algoritmo de assinatura a ser usado.  
  
- Uma cadeia de caracteres que identifica o algoritmo de resumo a ser usado.  
  
- Opcionalmente, um <xref:System.IdentityModel.Tokens.SecurityKeyIdentifier> que identifica a chave a ser usada para assinar a asserção.  
  
 [!code-csharp[c_CreateSTS#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#1)]
 [!code-vb[c_CreateSTS#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#1)]  
  
 Criptografar a chave compartilhada envolve pegar o material da chave e criptografá-lo com uma chave que o serviço de destino pode usar para descriptografar a chave compartilhada. Normalmente, a chave pública do serviço de destino é usada.  
  
 [!code-csharp[c_CreateSTS#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#2)]
 [!code-vb[c_CreateSTS#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#2)]  
  
 Além disso, <xref:System.IdentityModel.Tokens.SecurityKeyIdentifier> é necessário um para a chave criptografada.  
  
 [!code-csharp[c_CreateSTS#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#3)]
 [!code-vb[c_CreateSTS#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#3)]  
  
 Isso <xref:System.IdentityModel.Tokens.SecurityKeyIdentifier> é usado para criar um `SamlSubject` como parte do `SamlToken` .  
  
 [!code-csharp[c_CreateSTS#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#4)]
 [!code-vb[c_CreateSTS#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#4)]  
  
 Para obter mais informações, consulte [exemplo de Federação](../samples/federation-sample.md).  
  
## <a name="creating-response-messages"></a>Criando mensagens de resposta  
 Depois que o serviço de token de segurança processa a solicitação de problema e constrói o token a ser emitido junto com a chave de prova, a mensagem de resposta precisa ser construída, incluindo pelo menos o token solicitado, o token de prova e as referências de token emitidas. O token emitido normalmente é <xref:System.IdentityModel.Tokens.SamlSecurityToken> criado a partir do <xref:System.IdentityModel.Tokens.SamlAssertion> , conforme mostrado no exemplo a seguir.  
  
 [!code-csharp[c_CreateSTS#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#5)]
 [!code-vb[c_CreateSTS#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#5)]  
  
 No caso em que o serviço de token de segurança fornece o material de chave compartilhada, o token de prova é construído pela criação de um <xref:System.ServiceModel.Security.Tokens.BinarySecretSecurityToken> .  
  
 [!code-csharp[c_CreateSTS#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#6)]
 [!code-vb[c_CreateSTS#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#6)]  
  
 Para obter mais informações sobre como construir o token de prova quando o cliente e o serviço de token de segurança fornecem o material de chave para a chave compartilhada, consulte [exemplo de Federação](../samples/federation-sample.md).  
  
 As referências de token emitidas são criadas pela criação de instâncias da <xref:System.IdentityModel.Tokens.SecurityKeyIdentifierClause> classe.  
  
 [!code-csharp[c_CreateSTS#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#7)]
 [!code-vb[c_CreateSTS#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#7)]  
  
 Esses vários valores são serializados na mensagem de resposta retornada ao cliente.  
  
## <a name="example"></a>Exemplo  
 Para obter o código completo de um serviço de token de segurança, consulte [exemplo de Federação](../samples/federation-sample.md).  
  
## <a name="see-also"></a>Consulte também

- <xref:System.IdentityModel.Tokens.SigningCredentials>
- <xref:System.IdentityModel.Tokens.SecurityKey>
- <xref:System.IdentityModel.Tokens.SecurityKeyIdentifier>
- <xref:System.IdentityModel.Tokens.SamlSecurityToken>
- <xref:System.IdentityModel.Tokens.SamlAssertion>
- <xref:System.ServiceModel.Security.Tokens.BinarySecretSecurityToken>
- <xref:System.IdentityModel.Tokens.SecurityKeyIdentifierClause>
- [Exemplo de federação](../samples/federation-sample.md)
