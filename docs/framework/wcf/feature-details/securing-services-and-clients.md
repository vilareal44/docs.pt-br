---
title: Protegendo serviços e clientes
ms.date: 03/30/2017
helpviewer_keywords:
- message security [WCF]
ms.assetid: e681f3bd-0c09-4a58-b0e4-0ecbdf1aa6c7
ms.openlocfilehash: db0a0dcfbe04a7b7dbfabfed59f9b8637d0a2797
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84586202"
---
# <a name="securing-services-and-clients"></a>Protegendo serviços e clientes
As informações contidas nesta seção concentram-se na segurança de programação no Windows Communication Foundation (WCF). Em geral, isso inclui a seleção de uma associação apropriada fornecida pelo sistema, a definição das propriedades do elemento de segurança e a configuração das propriedades dos comportamentos de serviço que regem o modo como as credenciais são recuperadas para uso pelo serviço ou pelo cliente. Essas técnicas abrangem os requisitos de segurança da maioria dos usuários para a maioria dos cenários, conforme mostrado em [cenários de segurança comuns](common-security-scenarios.md). Se seu cenário exigir mais recursos, primeiro consulte [recursos de segurança com associações personalizadas](security-capabilities-with-custom-bindings.md); se uma solução não for aparente, consulte [estendendo a segurança](../extending/extending-security.md). Se você estiver criando (ou Interoperando com) um sistema que usa declarações avançadas, consulte os tópicos em [autorização](authorization-in-wcf.md).  
  
## <a name="in-this-section"></a>Nesta seção  
 [Programação de segurança do WCF](programming-wcf-security.md)  
 Uma visão geral do modelo de programação usado para proteger mensagens.  
  
 [Visão geral de segurança de transporte](transport-security-overview.md)  
 Uma visão geral de como proteger mensagens por meio da camada de transporte.  
  
 [Segurança de mensagem](message-security-in-wcf.md)  
 Resume os motivos para usar a segurança em nível de mensagem no Windows Communication Foundation (WCF).  
  
 [Sessões seguras](secure-sessions.md)  
 Uma discussão sobre as considerações necessárias ao proteger uma sessão do WCF.  
  
 [Trabalhando com certificados](working-with-certificates.md)  
 Uma explicação de algumas das tarefas comuns necessárias ao usar certificados X. 509.  
  
## <a name="reference"></a>Referência  
 <xref:System.ServiceModel>  
  
 <xref:System.ServiceModel.Channels>  
  
 <xref:System.ServiceModel.Security>  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Conceitos de segurança](security-concepts.md)  
  
 [Estendendo a segurança](../extending/extending-security.md)  
  
 [Cenários comuns de segurança](common-security-scenarios.md)  
  
 [Associações e segurança](bindings-and-security.md)  
  
 [Recursos de segurança com associações personalizadas](security-capabilities-with-custom-bindings.md)  
  
 [Estendendo a segurança](../extending/extending-security.md)  
  
 [Autorização](authorization-in-wcf.md)  
  
## <a name="see-also"></a>Consulte também

- [Programação básica do WCF](../basic-wcf-programming.md)
- [Modelo de segurança para o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
