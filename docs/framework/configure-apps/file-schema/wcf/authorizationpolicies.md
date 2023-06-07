---
title: <authorizationPolicies>
ms.date: 03/30/2017
ms.assetid: 5b367489-54d7-408b-8f56-cb157dd68eaf
ms.openlocfilehash: 38d123a53b344ff1e4d781115093d0d1de5ae679
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "69926457"
---
# \<authorizationPolicies>
Esta seção de configuração contém uma coleção de tipos de política de autorização, que podem ser adicionados usando a `add` palavra-chave. Cada política de autorização contém um único `policyType` atributo necessário que é uma cadeia de caracteres. O atributo especifica uma política de autorização, que permite a transformação de um conjunto de declarações de entrada em outro conjunto de declarações. Controle de acesso pode ser concedido ou negado com base nisso. Para obter mais informações sobre como funciona uma diretiva de autorização, consulte <xref:System.IdentityModel.Policy.IAuthorizationPolicy> e [política de autorização](../../../wcf/samples/authorization-policy.md).  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.ServiceAuthorizationElement>
- <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.ExternalAuthorizationPolicies%2A>
- <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>
- <xref:System.ServiceModel.Configuration.AuthorizationPolicyTypeElement>
- <xref:System.ServiceModel.Configuration.ServiceAuthorizationElement.AuthorizationPolicies%2A>
- <xref:System.ServiceModel.Configuration.AuthorizationPolicyTypeElementCollection>
- <xref:System.IdentityModel.Policy.IAuthorizationPolicy>
- [Autorizando o acesso às operações de serviço](../../../wcf/samples/authorizing-access-to-service-operations.md)
- [Como criar gerenciador de autorização personalizado para um serviço](../../../wcf/extending/how-to-create-a-custom-authorization-manager-for-a-service.md)
- [\<add>](add-of-authorizationpolicies.md)
- [Política de autorização](../../../wcf/samples/authorization-policy.md)
