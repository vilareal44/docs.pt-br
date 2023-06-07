---
title: Sessões seguras
ms.date: 03/30/2017
ms.assetid: 7b50602f-d7b5-42e9-8e92-1f0413df0d8b
ms.openlocfilehash: cb02adc7514e27175088e7664b12e45bc8ca5cd9
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84590079"
---
# <a name="secure-sessions"></a>Sessões seguras
Um recurso do Windows Communication Foundation (WCF) é uma sessão confiável que garante que as mensagens sejam recebidas na ordem em que foram enviadas. Os tópicos nesta seção discutem as implicações de segurança a serem consideradas ao criar uma sessão confiável. Para obter mais informações sobre sessões confiáveis, consulte [usando sessões](../using-sessions.md).  
  
> [!NOTE]
> Quando a representação é necessária no Windows XP, use uma sessão segura sem um símbolo de contexto de segurança com estado (SCT). Quando SCTs com estado são usados com representação, um <xref:System.InvalidOperationException> é gerado. Para obter mais informações, consulte [cenários sem suporte](unsupported-scenarios.md).  
  
## <a name="in-this-section"></a>Nesta seção  
  
|||  
|-|-|  
|[Sessões seguras e conversas seguras](secure-conversations-and-secure-sessions.md)|Conversas seguras e sessões seguras são sinônimos. Este tópico explica como funciona uma conversa segura e quando e por que usar o padrão.|  
|[Como criar uma sessão segura](how-to-create-a-secure-session.md)|Percorre as noções básicas da criação de uma sessão segura.|  
|[Como criar um token de contexto de segurança para uma sessão segura](how-to-create-a-security-context-token-for-a-secure-session.md)|Percorre as etapas de criação de um Web farm que manterá o estado e as sessões com clientes.|  
|[Considerações de segurança para sessões seguras](security-considerations-for-secure-sessions.md)|Descreve considerações especiais para sessões seguras.|  
  
## <a name="reference"></a>Referência  
 <xref:System.ServiceModel>  
  
 <xref:System.ServiceModel.Channels>  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Sessões, instanciação e simultaneidade](sessions-instancing-and-concurrency.md)  
  
 [Serviços de design e implantação](../designing-and-implementing-services.md)  
  
## <a name="see-also"></a>Consulte também

- [Como habilitar a detecção de repetição de mensagem](how-to-enable-message-replay-detection.md)
- [Ataques por repetição](replay-attacks.md)
- [Como criar um serviço que requer sessões](how-to-create-a-service-that-requires-sessions.md)
