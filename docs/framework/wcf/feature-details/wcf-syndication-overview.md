---
title: Visão geral de sindicalização do WCF
ms.date: 03/30/2017
ms.assetid: af6d4c39-e5e8-4099-aee6-5261feff9107
ms.openlocfilehash: 4f72a6d797f195108401a361cc73d74d309d5602
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600952"
---
# <a name="wcf-syndication-overview"></a>Visão geral de sindicalização do WCF
O Windows Communication Foundation (WCF) fornece suporte para expor feeds de distribuição de um serviço WCF. A distribuição é um mecanismo de integração de aplicativos no qual um servidor expõe alguns dados de aplicativo em um formato interoperável conhecido como feed. Um feed é uma coleção de dados de aplicativo que consiste em alguns metadados no nível do feed (título, autor, URL e outros metadados) e uma série de itens de feed. No feed, os itens de feed normalmente são ordenados por tempo na ordem cronológica inversa. Um item de feed consiste em um conjunto padrão de metadados em nível de item (título, URL, data de criação, categoria e outros metadados no nível de item) e uma quantidade arbitrária de dados específicos do aplicativo. Os dois tipos mais comuns de feeds de distribuição são RSS (Really Simple Syndication) 2,0 e Atom 1,0, ambos com suporte do WCF.  
  
## <a name="object-model"></a>Modelo de Objeto  
 O WCF define um conjunto de classes específicas de agregação que permitem que você trabalhe com feeds, itens de feed e os metadados relacionados de forma independente de formato: <xref:System.ServiceModel.Syndication.SyndicationFeed> ,, <xref:System.ServiceModel.Syndication.SyndicationItem> <xref:System.ServiceModel.Syndication.SyndicationPerson> , <xref:System.ServiceModel.Syndication.SyndicationLink> e outras classes específicas de distribuição. O WCF também define classes de infraestrutura que se baseiam no modelo de programação REST do WCF para fornecer suporte de distribuição, incluindo: <xref:System.ServiceModel.Syndication.Atom10FeedFormatter> e <xref:System.ServiceModel.Syndication.Rss20FeedFormatter> . As classes do formatador de feed dão suporte à serialização do modelo de objeto para e de RSS 2,0 e Atom 1,0.  
  
## <a name="scenarios"></a>Cenários  
 Um uso comum de agregação hoje é blog, onde o autor do blog publica periodicamente algum tipo de informação. Podem ser texto, imagens, áudio ou outros tipos de informações. Muitos jornais e revistas também publicam histórias de notícias ou artigos usando a distribuição. Ao assinar esse feed, um usuário pode manter-se atualizado com todas as novas informações provenientes de tais sites. Embora a agregação seja mais comumente associada a Blogs e publicadores, ela pode ser usada com qualquer aplicativo que expõe uma coleção de informações; por exemplo, um banco de dados de bugs que você deseja expor usando um feed de distribuição. Você pode criar um serviço WCF que expõe uma operação chamada `CodeDefects` . Esta operação pode usar um parâmetro que especifica o endereço de email da pessoa cujos bugs você deseja recuperar. Um cliente pode usar a seguinte URL para chamar a operação: `http://someserver/bugDatabase/CodeDefects?user=johndoe` .  
  
## <a name="syndication-formats"></a>Formatos de distribuição  
 A plataforma de distribuição do WCF dá suporte a RSS 2,0 e Atom 1,0.  
  
## <a name="see-also"></a>Consulte também

- [Modelo de programação WCF Web HTTP](wcf-web-http-programming-model.md)
