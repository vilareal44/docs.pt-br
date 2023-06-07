---
title: Como criar uma afirmação personalizada
description: Saiba como criar uma declaração personalizada no WCF. O WCF dá suporte a uma variedade de declarações internas e alguns aplicativos podem exigir declarações personalizadas.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d619976b-eda3-475e-ac23-c7988a2dceb0
ms.openlocfilehash: 89f2b1359b48b71720db6ff38f27883745cfe612
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247540"
---
# <a name="how-to-create-a-custom-claim"></a>Como criar uma afirmação personalizada
A infraestrutura do modelo de identidade no Windows Communication Foundation (WCF) fornece um conjunto de tipos de declaração internos e direitos com as funções auxiliares para criar <xref:System.IdentityModel.Claims.Claim> instâncias com esses tipos e direitos. Essas declarações internas são projetadas para modelar informações encontradas nos tipos de credencial do cliente que o WCF dá suporte por padrão. Em muitos casos, as declarações internas são suficientes; no entanto, alguns aplicativos podem exigir declarações personalizadas. Uma declaração consiste no tipo de declaração, o recurso ao qual a declaração se aplica e a direita que é declarada sobre esse recurso. Este tópico descreve como criar uma declaração personalizada.  
  
### <a name="to-create-a-custom-claim-that-is-based-on-a-primitive-data-type"></a>Para criar uma declaração personalizada baseada em um tipo de dados primitivo  
  
1. Crie uma declaração personalizada passando o tipo de declaração, o valor do recurso e o direito para o <xref:System.IdentityModel.Claims.Claim.%23ctor%28System.String%2CSystem.Object%2CSystem.String%29> Construtor.  
  
    1. Decida um valor exclusivo para o tipo de declaração.  
  
         O tipo de declaração é um identificador de cadeia de caracteres exclusivo. É responsabilidade do designer de declarações personalizado garantir que o identificador de cadeia de caracteres usado para o tipo de declaração seja exclusivo. Para obter uma lista de tipos de declaração que são definidos pelo WCF, consulte a <xref:System.IdentityModel.Claims.ClaimTypes> classe.  
  
    2. Escolha o tipo de dados primitivo e o valor para o recurso.  
  
         Um recurso é um objeto. O tipo CLR do recurso pode ser um primitivo, como ou ou <xref:System.String> <xref:System.Int32> qualquer tipo serializável. O tipo CLR do recurso deve ser serializável, pois as declarações são serializadas em vários pontos pelo WCF. Tipos primitivos são serializáveis.  
  
    3. Escolha um direito que seja definido pelo WCF ou um valor exclusivo para um direito personalizado.  
  
         A direita é um identificador de cadeia de caracteres exclusivo. Os direitos definidos pelo WCF são definidos na <xref:System.IdentityModel.Claims.Rights> classe.  
  
         É responsabilidade do designer de declarações personalizado garantir que o identificador de cadeia de caracteres usado para a direita seja exclusivo.  
  
         O exemplo de código a seguir cria uma declaração personalizada com um tipo de declaração de `http://example.org/claims/simplecustomclaim` , para um recurso chamado `Driver's License` e com o <xref:System.IdentityModel.Claims.Rights.PossessProperty%2A> direito.  
  
     [!code-csharp[c_CustomClaim#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customclaim/cs/c_customclaim.cs#4)]
     [!code-vb[c_CustomClaim#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customclaim/vb/c_customclaim.vb#4)]  
  
### <a name="to-create-a-custom-claim-that-is-based-on-a-non-primitive-data-type"></a>Para criar uma declaração personalizada baseada em um tipo de dados não primitivo  
  
1. Crie uma declaração personalizada passando o tipo de declaração, o valor do recurso e o direito para o <xref:System.IdentityModel.Claims.Claim.%23ctor%28System.String%2CSystem.Object%2CSystem.String%29> Construtor.  
  
    1. Decida um valor exclusivo para o tipo de declaração.  
  
         O tipo de declaração é um identificador de cadeia de caracteres exclusivo. É responsabilidade do designer de declarações personalizado garantir que o identificador de cadeia de caracteres usado para o tipo de declaração seja exclusivo. Para obter uma lista de tipos de declaração que são definidos pelo WCF, consulte a <xref:System.IdentityModel.Claims.ClaimTypes> classe.  
  
    2. Escolha ou defina um tipo não primitivo serializável para o recurso.  
  
         Um recurso é um objeto. O tipo CLR do recurso deve ser serializável, pois as declarações são serializadas em vários pontos pelo WCF. Tipos primitivos já são serializáveis.  
  
         Quando um novo tipo é definido, aplique o <xref:System.Runtime.Serialization.DataContractAttribute> à classe. Também aplique o <xref:System.Runtime.Serialization.DataMemberAttribute> atributo a todos os membros do novo tipo que precisa ser serializado como parte da declaração.  
  
         O exemplo de código a seguir define um tipo de recurso personalizado chamado `MyResourceType` .  
  
         [!code-csharp[c_CustomClaim#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customclaim/cs/c_customclaim.cs#2)]
         [!code-vb[c_CustomClaim#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customclaim/vb/c_customclaim.vb#2)]
  
    3. Escolha um direito que seja definido pelo WCF ou um valor exclusivo para um direito personalizado.  
  
         A direita é um identificador de cadeia de caracteres exclusivo. Os direitos definidos pelo WCF são definidos na <xref:System.IdentityModel.Claims.Rights> classe.  
  
         É responsabilidade do designer de declarações personalizado garantir que o identificador de cadeia de caracteres usado para a direita seja exclusivo.  
  
         O exemplo de código a seguir cria uma declaração personalizada com um tipo de declaração de `http://example.org/claims/complexcustomclaim` , um tipo de recurso personalizado de `MyResourceType` e com a <xref:System.IdentityModel.Claims.Rights.PossessProperty%2A> direita.  
  
         [!code-csharp[c_CustomClaim#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customclaim/cs/c_customclaim.cs#5)]
         [!code-vb[c_CustomClaim#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customclaim/vb/c_customclaim.vb#5)]
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir demonstra como criar uma declaração personalizada com um tipo de recurso primitivo e uma declaração personalizada com um tipo de recurso não primitivo.  
  
 [!code-csharp[c_CustomClaim#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customclaim/cs/c_customclaim.cs#0)]
 [!code-vb[c_CustomClaim#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customclaim/vb/c_customclaim.vb#0)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.IdentityModel.Claims.Claim>
- <xref:System.IdentityModel.Claims.Rights>
- <xref:System.IdentityModel.Claims.ClaimTypes>
- <xref:System.Runtime.Serialization.DataContractAttribute>
- <xref:System.Runtime.Serialization.DataMemberAttribute>
- [Gerenciamento de declarações e autorizações com o modelo de identidade](../feature-details/managing-claims-and-authorization-with-the-identity-model.md)
