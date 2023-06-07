---
title: Segurança de comportamento
ms.date: 03/30/2017
ms.assetid: 19710ae3-f197-4d28-ba9d-52e465006819
ms.openlocfilehash: 5d09fcc2068133b3bb302850a647a2539ab07ee3
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84575583"
---
# <a name="behavior-security"></a>Segurança de comportamento
Esta seção inclui exemplos que demonstram a configuração de segurança para comportamentos de serviço.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Comportamento de auditoria de serviço](service-auditing-behavior.md)  
 Este exemplo demonstra como usar o <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior> para habilitar a auditoria de eventos de segurança durante operações de serviço.  
  
 [Provedor de função e associação](membership-and-role-provider.md)  
 Este exemplo demonstra como um serviço pode usar a associação ASP.NET e os provedores de função para autenticar e autorizar clientes.  
  
 [Autorizando o acesso às operações de serviço](authorizing-access-to-service-operations.md)  
 Este exemplo demonstra como usar o [\<serviceAuthorization>](../../configure-apps/file-schema/wcf/serviceauthorization-element.md) para habilitar o uso do <xref:System.Security.Permissions.PrincipalPermissionAttribute> atributo para autorizar o acesso a operações de serviço.  
  
 [Representando o cliente](impersonating-the-client.md)  
 Este exemplo demonstra como representar o aplicativo chamador no serviço para que o serviço possa acessar recursos do sistema em nome do chamador.
