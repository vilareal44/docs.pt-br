---
title: Como desabilitar criptografia de assinaturas digitais
ms.date: 03/30/2017
ms.assetid: fd174313-ad81-4dca-898a-016ccaff8187
ms.openlocfilehash: 7ef305fdfd9a4ee61dfd89fdd46f2dc03a350977
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595395"
---
# <a name="how-to-disable-encryption-of-digital-signatures"></a>Como desabilitar criptografia de assinaturas digitais
Por padrão, uma mensagem é assinada e a assinatura é criptografada digitalmente. Isso é controlado por meio da criação de uma associação personalizada com uma instância do <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> ou do <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> e da configuração da `MessageProtectionOrder` propriedade de qualquer classe para um <xref:System.ServiceModel.Security.MessageProtectionOrder> valor de enumeração. O padrão é <xref:System.ServiceModel.Security.MessageProtectionOrder.SignBeforeEncryptAndEncryptSignature>. Esse processo consome até 30% mais tempo do que simplesmente assinar e criptografar com base no tamanho geral da mensagem (quanto menor a mensagem, maior o impacto no desempenho). No entanto, desabilitar a criptografia da assinatura pode permitir que um invasor Adivinhe o conteúdo da mensagem. Isso é possível porque o elemento Signature contém o código hash do texto sem formatação de todas as partes assinadas na mensagem. Por exemplo, embora o corpo da mensagem seja criptografado por padrão, a assinatura não criptografada contém o código hash do corpo da mensagem antes da criptografia. Se o conjunto de valores possíveis para a parte assinada e criptografada for pequeno, um invasor poderá deduzir o conteúdo examinando o valor de hash. A criptografia da assinatura reduz esse vetor de ataque.  
  
 Portanto, desabilite a criptografia da assinatura somente quando o valor do conteúdo estiver baixo ou o conjunto de possíveis valores de conteúdo for grande e não determinístico e o lucro de desempenho for mais importante do que reduzir o ataque descrito acima.  
  
> [!NOTE]
> Se não houver nada na mensagem criptografada, o elemento Signature não será criptografado, mesmo quando a <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement.MessageProtectionOrder%2A?displayProperty=nameWithType> propriedade ou <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement.MessageProtectionOrder%2A?displayProperty=nameWithType> for definida como <xref:System.ServiceModel.Security.MessageProtectionOrder.SignBeforeEncryptAndEncryptSignature> . Esse comportamento ocorre mesmo com associações fornecidas pelo sistema; todas as associações fornecidas pelo sistema têm a ordem de proteção de mensagem definida como `SignBeforeEncryptAndEncryptSignature` . No entanto, as gerações do WCF WSDL (Web Services Description Language) ainda conterá a `<sp:EncryptSignature>` asserção.  
  
### <a name="to-disable-digital-signing"></a>Para desabilitar a assinatura digital  
  
1. Criará um <xref:System.ServiceModel.Channels.CustomBinding>. Para obter mais informações, consulte [como: criar uma associação personalizada usando o SecurityBindingElement](how-to-create-a-custom-binding-using-the-securitybindingelement.md).  
  
2. Adicione um <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> ou um <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> à coleção de associação.  
  
3. Defina a <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement.MessageProtectionOrder%2A?displayProperty=nameWithType> propriedade como <xref:System.ServiceModel.Security.MessageProtectionOrder.SignBeforeEncrypt> ou defina a <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement.MessageProtectionOrder%2A?displayProperty=nameWithType> propriedade como <xref:System.ServiceModel.Security.MessageProtectionOrder.SignBeforeEncrypt> .  
  
## <a name="see-also"></a>Consulte também

- [Recursos de segurança com associações personalizadas](security-capabilities-with-custom-bindings.md)
