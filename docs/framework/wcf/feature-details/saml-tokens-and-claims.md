---
title: Declarações e tokens de SAML
description: Saiba como o WFC usa tokens SAML para transportar instruções que são conjuntos de declarações feitas por uma entidade sobre outra entidade.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, federation
- federation
- issued tokens
- SAML token
ms.assetid: 930b6e34-9eab-4e95-826c-4e06659bb977
ms.openlocfilehash: c054e594af69def96879852a5145675b3123614a
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244940"
---
# <a name="saml-tokens-and-claims"></a>Declarações e tokens de SAML
*Tokens* SAML (Security Asserts Markup Language) são representações XML de declarações. Por padrão, os tokens SAML Windows Communication Foundation (WCF) usam em cenários de segurança federada são *tokens emitidos*.  
  
 Tokens SAML carregam instruções que são conjuntos de declarações feitas por uma entidade sobre outra entidade. Por exemplo, em cenários de segurança federada, as instruções são feitas por um serviço de token de segurança sobre um usuário no sistema. O serviço de token de segurança assina o token SAML para indicar o veracidade das instruções contidas no token. Além disso, o token SAML é associado ao material de chave de criptografia que o usuário do token SAML comprova conhecimento. Essa prova atende à terceira parte confiável que o token SAML foi, de fato, emitido para esse usuário. Por exemplo, em um cenário típico:  
  
1. Um cliente solicita um token SAML de um serviço de token de segurança, Autenticando nesse serviço de token de segurança usando as credenciais do Windows.  
  
2. O serviço de token de segurança emite um token SAML para o cliente. O token SAML é assinado com um certificado associado ao serviço de token de segurança e contém uma chave de prova criptografada para o serviço de destino.  
  
3. O cliente também recebe uma cópia da *chave de prova*. Em seguida, o cliente apresenta o token SAML para o serviço de aplicativo (a terceira parte *confiável*) e assina a mensagem com essa chave de prova.  
  
4. A assinatura sobre o token SAML informa à terceira parte confiável que o serviço de token de segurança emitiu o token. A assinatura de mensagem criada com a chave de prova informa a terceira parte confiável que o token foi emitido para o cliente.  
  
## <a name="from-claims-to-samlattributes"></a>De declarações para Samlattributes  
 No WCF, as instruções em tokens SAML são modeladas como <xref:System.IdentityModel.Tokens.SamlAttribute> objetos, que podem ser preenchidas diretamente de <xref:System.IdentityModel.Claims.Claim> objetos, desde que o <xref:System.IdentityModel.Claims.Claim> objeto tenha uma <xref:System.IdentityModel.Claims.Claim.Right%2A> propriedade de <xref:System.IdentityModel.Claims.Rights.PossessProperty%2A> e a <xref:System.IdentityModel.Claims.Claim.Resource%2A> propriedade seja do tipo <xref:System.String> . Por exemplo:  
  
 [!code-csharp[c_CreateSTS#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#8)]
 [!code-vb[c_CreateSTS#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#8)]  
  
> [!NOTE]
> Quando os tokens SAML são serializados em mensagens, quando são emitidos por um serviço de token de segurança ou quando são apresentados por clientes para serviços como parte da autenticação, a cota de tamanho máximo de mensagem deve ser suficientemente grande para acomodar o token SAML e as outras partes da mensagem. Em casos normais, as cotas padrão de tamanho da mensagem são suficientes. No entanto, nos casos em que um token SAML é grande porque contém centenas de declarações, talvez seja necessário aumentar as cotas para acomodar o token serializado. Para obter mais informações, consulte [considerações de segurança para dados](security-considerations-for-data.md).  
  
## <a name="from-samlattributes-to-claims"></a>De Samlattributes a declarações  
 Quando os tokens SAML são recebidos em mensagens, as várias instruções no token SAML são <xref:System.IdentityModel.Policy.IAuthorizationPolicy> transformadas em objetos que são colocados no <xref:System.IdentityModel.Policy.AuthorizationContext> . As declarações de cada instrução SAML são retornadas pela <xref:System.IdentityModel.Policy.AuthorizationContext.ClaimSets%2A> propriedade de <xref:System.IdentityModel.Policy.AuthorizationContext> e podem ser examinadas para determinar se deseja autenticar e autorizar o usuário.  
  
## <a name="see-also"></a>Veja também

- <xref:System.IdentityModel.Policy.AuthorizationContext>
- <xref:System.IdentityModel.Policy.IAuthorizationPolicy>
- <xref:System.IdentityModel.Claims.ClaimSet>
- [Federação](federation.md)
- [Como criar um cliente federado](how-to-create-a-federated-client.md)
- [Como configurar credenciais em um serviço de federação](how-to-configure-credentials-on-a-federation-service.md)
- [Gerenciamento de declarações e autorizações com o modelo de identidade](managing-claims-and-authorization-with-the-identity-model.md)
- [Declarações e tokens](claims-and-tokens.md)
- [Valores de recursos e criação de declarações](claim-creation-and-resource-values.md)
- [Como criar uma declaração personalizada](../extending/how-to-create-a-custom-claim.md)
