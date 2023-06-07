---
title: Declarações e tokens
ms.date: 03/30/2017
helpviewer_keywords:
- claims [WCF], and tokens
ms.assetid: eff167f3-33f8-483d-a950-aa3e9f97a189
ms.openlocfilehash: cbc97f2224bce640757e1cef88fe325db477cfd7
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84587021"
---
# <a name="claims-and-tokens"></a>Declarações e tokens

Este tópico descreve os vários tipos de declaração que Windows Communication Foundation (WCF) cria com base nos tokens padrão aos quais ele dá suporte.

Você pode examinar as declarações de uma credencial do cliente usando <xref:System.IdentityModel.Claims.ClaimSet> as <xref:System.IdentityModel.Claims.Claim> classes e. O `ClaimSet` contém uma coleção de `Claim` objetos. Cada um `Claim` tem os seguintes membros importantes:

- A <xref:System.IdentityModel.Claims.Claim.ClaimType%2A> propriedade retorna um Uniform Resource Identifier (URI) que especifica o tipo de declaração que está sendo feita. Por exemplo, um tipo de declaração pode ser uma impressão digital de um certificado; nesse caso, o URI é `http://schemas.microsoft.com/ws/20005/05/identity/claims/thumprint` .

- A <xref:System.IdentityModel.Claims.Claim.Right%2A> propriedade retorna um URI que especifica a direita da declaração. Direitos predefinidos são encontrados na <xref:System.IdentityModel.Claims.Rights> classe ( <xref:System.IdentityModel.Claims.Rights.Identity%2A> , <xref:System.IdentityModel.Claims.Rights.PossessProperty%2A> ).

- A <xref:System.IdentityModel.Claims.Claim.Resource%2A> propriedade retorna o recurso associado à declaração.

Cada <xref:System.IdentityModel.Claims.ClaimSet> também tem uma <xref:System.IdentityModel.Claims.ClaimSet.Issuer%2A> propriedade, que representa o <xref:System.IdentityModel.Claims.ClaimSet> do `Issuer` .

## <a name="windows-accounts"></a>Contas do Windows

Quando uma credencial de cliente é mapeada para uma conta de usuário do Windows, o resultado <xref:System.IdentityModel.Claims.ClaimSet> tem os seguintes valores:

- O `Issuer` é o valor retornado pela propriedade estática do Windows da <xref:System.IdentityModel.Claims.ClaimSet> classe.

- As declarações na coleção são:

  - Um <xref:System.IdentityModel.Claims.Claim> com um <xref:System.IdentityModel.Claims.Claim.ClaimType%2A> valor de Sid (identificador de segurança), um <xref:System.IdentityModel.Claims.Claim.Right%2A> valor de propriedade de `Identity` e um <xref:System.IdentityModel.Claims.Claim.Resource%2A> que retorna o valor de Sid real. Um SID é um valor exclusivo que o controlador de domínio emite para cada usuário. O SID é usado para identificar o usuário em interações com a segurança do Windows.

  - Um <xref:System.IdentityModel.Claims.Claim> com um <xref:System.IdentityModel.Claims.Claim.ClaimType%2A> valor de Sid, um <xref:System.IdentityModel.Claims.Claim.Right%2A> de `PossessProperty` e um <xref:System.IdentityModel.Claims.Claim.Resource%2A> do valor de Sid.

  - Um <xref:System.IdentityModel.Claims.Claim> com um <xref:System.IdentityModel.Claims.Claim.ClaimType%2A> de <xref:System.IdentityModel.Claims.ClaimTypes.Name%2A> , um <xref:System.IdentityModel.Claims.Claim.Right%2A> de `PossessProperty` e um <xref:System.IdentityModel.Claims.Claim.Resource%2A> de cadeia de caracteres que contém o nome de usuário (por exemplo, "MYMACHINE\Bob").

  - Declarações SID adicionais com <xref:System.IdentityModel.Claims.Rights.PossessProperty%2A> para os vários grupos aos quais o usuário pertence.

## <a name="certificates"></a>Certificados

Onde a credencial do cliente é um certificado, o resultado <xref:System.IdentityModel.Claims.ClaimSet> tem os seguintes valores:

- Para certificados emitidos automaticamente, o `Issuer` é o <xref:System.IdentityModel.Claims.ClaimSet> próprio. O <xref:System.IdentityModel.Claims.ClaimSet> retorna um <xref:System.IdentityModel.Claims.Claim.ClaimType%2A> de <xref:System.IdentityModel.Claims.ClaimTypes.Thumbprint%2A> , um <xref:System.IdentityModel.Claims.Claim.Right%2A> de `Identity` , e um <xref:System.IdentityModel.Claims.Claim.Resource%2A> valor que é uma <xref:System.Byte> matriz que contém a impressão digital do certificado.

- Para um certificado emitido por uma autoridade de certificação, o emissor é a `ClaimSet` representação do certificado da autoridade de certificação.

- O `Claims` na coleção inclui:

  - A `Claim` com uma `ClaimType` impressão digital, a `Right` de possuiproperty, e a `Resource` que é uma matriz de bytes que contém a impressão digital do certificado

  - Declarações adicionais de posse de vários tipos, incluindo X500DistinguishedName, DNS, Name, UPN e RSA, representam várias propriedades do certificado. O recurso para a declaração RSA é a chave pública associada ao certificado. **Observação** Onde o tipo de credencial do cliente é um certificado que o serviço mapeia para uma conta do Windows, dois `ClaimSet` objetos são gerados. A primeira contém todas as declarações relacionadas à conta do Windows e a segunda contém todas as declarações relacionadas ao certificado.

## <a name="user-namepassword"></a>Nome de usuário/senha

Onde a credencial do cliente é um nome de usuário/senha (ou equivalente) que não é mapeado para uma conta do Windows, o resultado `ClaimSet` é emitido pela <xref:System.IdentityModel.Claims.ClaimSet.System%2A> propriedade estática da `ClaimSet` classe. O `ClaimSet` contém uma `Identity` declaração do tipo <xref:System.IdentityModel.Claims.ClaimTypes.Name%2A> cujo recurso é o nome de usuário que o cliente fornece. Uma declaração correspondente tem um `Right` de `PossessProperty` .

## <a name="rsa-keys"></a>Chaves RSA

Quando uma chave RSA não associada a um certificado é usada, o resultado `ClaimSet` é emitido automaticamente e contém uma `Identity` declaração do tipo <xref:System.IdentityModel.Claims.ClaimTypes.Rsa%2A> cujo recurso é a chave RSA. Uma declaração correspondente tem um `Right` de `PossessProperty` .

## <a name="saml"></a>SAML

Onde o cliente é autenticado com um token SAML (Security Asserties Markup Language), o resultado `ClaimSet` é emitido pela entidade que assinou o token SAML, geralmente o certificado do STS (serviço de token de segurança) que emitiu o token SAML. O `ClaimSet` contém várias declarações como encontradas no token SAML. Se o token SAML contiver um `SamlSubject` com um nome diferente de `null` , uma `Identity` declaração com um tipo de <xref:System.IdentityModel.Claims.ClaimTypes.NameIdentifier%2A> e um tipo de recurso de <xref:System.IdentityModel.Tokens.SamlNameIdentifierClaimResource> será criada.

## <a name="identity-claims-and-servicesecuritycontextisanonymous"></a>Declarações de identidade e ServiceSecurityContext. IsAnonymous

Se nenhum dos `ClaimSet` objetos resultantes das credenciais do cliente contiver uma declaração com um `Right` de `Identity,` , a <xref:System.ServiceModel.ServiceSecurityContext.IsAnonymous%2A> propriedade retornará `true` . Se uma ou mais dessas declarações estiverem presentes, a `IsAnonymous` propriedade retornará `false` .

## <a name="see-also"></a>Consulte também

- <xref:System.IdentityModel.Claims.ClaimSet>
- <xref:System.IdentityModel.Claims.Claim>
- <xref:System.IdentityModel.Claims.Rights>
- <xref:System.IdentityModel.Claims.ClaimTypes>
- [Gerenciamento de declarações e autorizações com o modelo de identidade](managing-claims-and-authorization-with-the-identity-model.md)
