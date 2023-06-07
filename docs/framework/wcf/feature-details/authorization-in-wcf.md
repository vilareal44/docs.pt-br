---
title: Autorização no WCF
ms.date: 03/30/2017
helpviewer_keywords:
- authorization [WCF]
- security [WCF], authorization
ms.assetid: 8ea0b552-af65-45b0-a157-c6c111b8ce5e
ms.openlocfilehash: d67d64dcf0003de28775ac947f8b5f72d7c2ba2a
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597625"
---
# <a name="authorization-in-wcf"></a>Autorização no WCF
A autorização é o processo de controle de acesso e direitos a recursos, como serviços ou arquivos. Os tópicos nesta seção mostram como executar essa tarefa básica no Windows Communication Foundation (WCF) de várias maneiras.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Mecanismos de controle de acesso](access-control-mechanisms.md)  
 Fornece uma breve descrição dos mecanismos de autorização no WCF e usos sugeridos.  
  
 [Como restringir o acesso com a classe PrincipalPermissionAttribute](../how-to-restrict-access-with-the-principalpermissionattribute-class.md)  
 Mostra o processo de restringir o acesso a um serviço com o <xref:System.Security.Permissions.PrincipalPermissionAttribute> .  
  
 [Como utilizar o provedor de função do ASP.NET com um serviço](how-to-use-the-aspnet-role-provider-with-a-service.md)  
 Percorre a configuração de um serviço para habilitá-lo a usar o recurso de provedor de função do ASP.NET.  
  
 [Como usar o provedor de função do gerenciador de autorização do ASP.NET com um serviço](how-to-use-the-aspnet-authorization-manager-role-provider-with-a-service.md)  
 O ASP.NET pode usar o Gerenciador de autorização para gerenciar a autorização de um site da Web. Da mesma forma, o WCF pode aproveitar a combinação do Gerenciador de ASP.NET/Authorization para autorização de clientes.  
  
 [Gerenciamento de declarações e autorizações com o modelo de identidade](managing-claims-and-authorization-with-the-identity-model.md)  
 Explica as noções básicas do uso da infraestrutura do modelo de identidade para autorização baseada em declarações.  
  
 [Delegação e representação](delegation-and-impersonation-with-wcf.md)  
 Explica a diferença entre delegação e representação.  
  
## <a name="reference"></a>Referência  
 <xref:System.ServiceModel.Security>  
  
 <xref:System.ServiceModel.Description.PrincipalPermissionMode>  
  
 <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>  
  
 <xref:System.Security.Permissions.PrincipalPermissionAttribute>  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Autenticação](authentication-in-wcf.md)  
  
## <a name="see-also"></a>Consulte também

- [Visão geral de segurança](security-overview.md)
- [Modelo de segurança para o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
