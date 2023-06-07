---
title: Como permitir solicitações de metadados durante a autorização
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- allowing metadata requests while authorizing [WCF]
ms.assetid: 90cec34f-b619-452b-a056-8b1c0de49d05
ms.openlocfilehash: 6d172f9b659804179d23fb382376f83f4898edc5
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601303"
---
# <a name="how-to-allow-metadata-requests-while-authorizing"></a>Como permitir solicitações de metadados durante a autorização
Durante a autorização personalizada, pode ser necessário permitir que uma solicitação de metadados seja processada. O tópico a seguir percorre as etapas para validar essa solicitação.  
  
 Para obter mais informações sobre a autorização do Windows Communication Foundation (WCF), consulte [Authorization](authorization-in-wcf.md).  
  
### <a name="to-allow-metadata-requests-during-authorization"></a>Para permitir solicitações de metadados durante a autorização  
  
1. Crie uma extensão da <xref:System.ServiceModel.ServiceAuthorizationManager> classe.  
  
2. Substituir o método <xref:System.ServiceModel.ServiceAuthorizationManager.CheckAccessCore%2A>. O método retorna `true` ou `false` dependendo se a autorização é permitida. As informações sobre o procedimento atual são encontradas no <xref:System.ServiceModel.OperationContext> passado como um parâmetro para o método.  
  
3. Na substituição, verifique o nome do contrato, o namespace e a ação, conforme mostrado no exemplo a seguir. Se as condições forem válidas, retorne`true.`  
  
4. Use o ponto de extensibilidade para empregar a classe. Para obter mais informações, consulte [como: criar um Gerenciador de autorização personalizado para um serviço](../extending/how-to-create-a-custom-authorization-manager-for-a-service.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra uma substituição do <xref:System.ServiceModel.ServiceAuthorizationManager.CheckAccessCore%2A> método.  
  
 [!code-csharp[C_HowtoCheckForMexRequestsInAuthorization#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howtocheckformexrequestsinauthorization/cs/source.cs#1)]
 [!code-vb[C_HowtoCheckForMexRequestsInAuthorization#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howtocheckformexrequestsinauthorization/vb/source.vb#1)]  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.ServiceAuthorizationManager>
- [Autorização](authorization-in-wcf.md)
- [Gerenciamento de declarações e autorizações com o modelo de identidade](managing-claims-and-authorization-with-the-identity-model.md)
