---
title: Criptografia de assinaturas digitais
ms.date: 03/30/2017
helpviewer_keywords:
- encryption of digital signatures [WCF]
- digital signatures [WCF], encryption
- digital signatures [WCF]
ms.assetid: 0868866d-40b4-4341-8e42-eee3b7f15b69
ms.openlocfilehash: 41094b2eee50e97cf60887cfa29f8110f2895bfa
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597391"
---
# <a name="encryption-of-digital-signatures"></a>Criptografia de assinaturas digitais
Por padrão, uma mensagem é assinada e criptografada e a assinatura é criptografada digitalmente. Você pode controlar isso criando uma associação personalizada com uma instância do <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> ou o <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> e, em seguida, definindo a `MessageProtectionOrder` propriedade de qualquer uma das classes para um <xref:System.ServiceModel.Security.MessageProtectionOrder> valor de enumeração. O padrão é <xref:System.ServiceModel.Security.MessageProtectionOrder.SignBeforeEncryptAndEncryptSignature>. Esse processo leva de 10 a 40 por cento mais do que simplesmente assinar e criptografar. No entanto, desabilitar a criptografia da assinatura pode permitir que um invasor Adivinhe o conteúdo da mensagem. Isso é possível porque o elemento Signature contém o código hash do texto sem formatação de todas as partes assinadas na mensagem. Por exemplo, embora o corpo da mensagem seja criptografado por padrão, a assinatura não criptografada contém o código hash do corpo da mensagem. Se a mensagem for pequena, um invasor poderá deduzir o conteúdo. Criptografar a assinatura reduz ou elimina essa possibilidade.  
  
 Portanto, desabilite a criptografia da assinatura somente quando o valor do conteúdo for baixo, e o lucro de desempenho será significativo, por exemplo, ao enviar arquivos binários grandes que não têm nenhuma implicação de segurança.  
  
### <a name="to-disable-digital-signing"></a>Para desabilitar a assinatura digital  
  
1. Criará um <xref:System.ServiceModel.Channels.CustomBinding>. Para obter mais informações, consulte [como: criar uma associação personalizada usando o SecurityBindingElement](how-to-create-a-custom-binding-using-the-securitybindingelement.md).  
  
2. Adicione um <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> ou um <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> à coleção de associação.  
  
3. Defina a <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement.MessageProtectionOrder%2A?displayProperty=nameWithType> propriedade como <xref:System.ServiceModel.Security.MessageProtectionOrder.SignBeforeEncrypt> ou defina a <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement.MessageProtectionOrder%2A?displayProperty=nameWithType> propriedade como <xref:System.ServiceModel.Security.MessageProtectionOrder.SignBeforeEncrypt> .  
  
 Para obter mais informações sobre como criar associações personalizadas, consulte [Criando associações definidas pelo usuário](../extending/creating-user-defined-bindings.md). Para obter mais informações sobre como criar uma associação personalizada para um modo de autenticação específico, consulte [como: criar um SecurityBindingElement para um modo de autenticação especificado](how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md).  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.Security.MessageProtectionOrder>
- <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement>
- <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement>
- [Como criar uma associação personalizada utilizando o SecurityBindingElement](how-to-create-a-custom-binding-using-the-securitybindingelement.md)
- [Criando associações definidas pelo usuário](../extending/creating-user-defined-bindings.md)
- [Como criar um SecurityBindingElement para um modo de autenticação especificado](how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)
