---
title: Arquitetura de sindicalização
ms.date: 03/30/2017
ms.assetid: ed4ca86e-e3d8-4acb-87aa-1921fbc353be
ms.openlocfilehash: 718778993a953ae819a2bee5a4a050a81d3a4b84
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84587515"
---
# <a name="architecture-of-syndication"></a>Arquitetura de sindicalização
A API de distribuição foi projetada para fornecer um modelo de programação de formato neutro que permite que o conteúdo agregado seja gravado na conexão em uma variedade de formatos. O modelo de dados abstratos consiste nas seguintes classes:  
  
- <xref:System.ServiceModel.Syndication.SyndicationCategory>  
  
- <xref:System.ServiceModel.Syndication.SyndicationFeed>  
  
- <xref:System.ServiceModel.Syndication.SyndicationItem>  
  
- <xref:System.ServiceModel.Syndication.SyndicationLink>  
  
- <xref:System.ServiceModel.Syndication.SyndicationPerson>  
  
 Essas classes mapeiam fortemente para as construções definidas na especificação Atom 1,0, embora alguns dos nomes sejam diferentes.  
  
 No Windows Communication Foundation (WCF), os feeds de distribuição são modelados como outro tipo de operação de serviço, um em que o tipo de retorno é uma das classes derivadas do <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> . A recuperação de um feed é modelada como uma troca de mensagens de solicitação-resposta. Um cliente envia uma solicitação ao serviço e o serviço responde. A mensagem de solicitação é definida em um protocolo de infraestrutura (por exemplo, HTTP bruto) e a mensagem de resposta contém uma carga que consiste em um formato de distribuição comumente compreendido (RSS 2,0 ou Atom 1,0). Os serviços que implementam essas trocas de mensagens são chamados de serviços de distribuição.  
  
 O contrato para um serviço de distribuição consiste em um conjunto de operações que retorna uma instância da <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> classe. O exemplo a seguir demonstra uma declaração de interface para um serviço de distribuição.  
  
 [!code-csharp[S_UE_SyndicationBoth#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ue_syndicationboth/cs/service.cs#0)]  
  
 O suporte à distribuição é criado sobre o modelo de programação REST do WCF que define a <xref:System.ServiceModel.WebHttpBinding> associação, que é usada em conjunto com <xref:System.ServiceModel.Description.WebHttpBehavior> para disponibilizar os feeds como serviços. Para obter mais informações sobre o modelo de programação REST do WCF, consulte [visão geral do modelo de programação do WCF Web http](wcf-web-http-programming-model-overview.md).  
  
> [!NOTE]
> A especificação Atom 1,0 permite que os segundos fracionários sejam especificados em qualquer uma de suas construções de data. Ao serializar e desserializar a implementação do WCF, o ignora os segundos fracionários.  
  
## <a name="object-model"></a>Modelo de Objeto  
 O modelo de objeto para distribuição consiste nos grupos de classes nas tabelas a seguir.  
  
 Classes de formatação:  
  
|Classe|Descrição|  
|-----------|-----------------|  
|<xref:System.ServiceModel.Syndication.Atom10FeedFormatter>|Uma classe que serializa uma <xref:System.ServiceModel.Syndication.SyndicationFeed> instância para o formato Atom 1,0.|  
|<xref:System.ServiceModel.Syndication.Atom10FeedFormatter%601>|Uma classe que serializa <xref:System.ServiceModel.Syndication.SyndicationFeed> classes derivadas para o formato Atom 1,0.|  
|<xref:System.ServiceModel.Syndication.Atom10ItemFormatter>|Uma classe que serializa uma <xref:System.ServiceModel.Syndication.SyndicationItem> instância para o formato Atom 1,0.|  
|<xref:System.ServiceModel.Syndication.Atom10ItemFormatter%601>|Uma classe que serializa <xref:System.ServiceModel.Syndication.SyndicationItem> classes derivadas para o formato Atom 1,0.|  
|<xref:System.ServiceModel.Syndication.Rss20FeedFormatter>|Uma classe que serializa uma <xref:System.ServiceModel.Syndication.SyndicationFeed> instância para o formato RSS 2,0.|  
|<xref:System.ServiceModel.Syndication.Rss20FeedFormatter%601>|Uma classe que serializa <xref:System.ServiceModel.Syndication.SyndicationFeed> classes derivadas para o formato RSS 2,0.|  
|<xref:System.ServiceModel.Syndication.Rss20ItemFormatter>|Uma classe que serializa uma <xref:System.ServiceModel.Syndication.SyndicationItem> instância para o formato RSS 2,0.|  
|<xref:System.ServiceModel.Syndication.Rss20ItemFormatter%601>|Uma classe que serializa <xref:System.ServiceModel.Syndication.SyndicationItem> classes derivadas para o formato RSS 2,0.|  
  
 Classes de modelo de objeto:  
  
|Classe|Descrição|  
|-----------|-----------------|  
|<xref:System.ServiceModel.Syndication.SyndicationCategory>|Uma classe que representa a categoria de um feed de sindicalização.|  
|<xref:System.ServiceModel.Syndication.SyndicationContent>|Uma classe base que representa o conteúdo de sindicalização.|  
|<xref:System.ServiceModel.Syndication.SyndicationElementExtension>|Uma classe que representa uma extensão do elemento de sindicalização.|  
|<xref:System.ServiceModel.Syndication.SyndicationElementExtensionCollection>|Uma coleção de objetos <xref:System.ServiceModel.Syndication.SyndicationElementExtension>.|  
|<xref:System.ServiceModel.Syndication.SyndicationFeed>|Uma classe que representa um objeto de feed de nível superior.|  
|<xref:System.ServiceModel.Syndication.SyndicationItem>|Uma classe que representa um item de feed.|  
|<xref:System.ServiceModel.Syndication.SyndicationLink>|Uma classe que representa um link dentro de um feed ou item de distribuição.|  
|<xref:System.ServiceModel.Syndication.SyndicationPerson>|Uma classe que representa uma construção de pessoa Atom.|  
|<xref:System.ServiceModel.Syndication.SyndicationVersions>|Uma classe que representa as versões de protocolo de distribuição com suporte.|  
|<xref:System.ServiceModel.Syndication.TextSyndicationContent>|Uma classe que representa qualquer <xref:System.ServiceModel.Syndication.SyndicationItem> conteúdo a ser exibido para um usuário final.|  
|<xref:System.ServiceModel.Syndication.TextSyndicationContentKind>|Uma enumeração que representa os diferentes tipos de conteúdo de distribuição de texto com suporte.|  
|<xref:System.ServiceModel.Syndication.UrlSyndicationContent>|Uma classe que representa o conteúdo de distribuição que consiste em uma URL para outro recurso.|  
|<xref:System.ServiceModel.Syndication.XmlSyndicationContent>|Uma classe que representa o conteúdo de distribuição que não deve ser exibido em um navegador.|  
  
 As abstrações de dados principais no modelo de objeto são feed e item, que correspondem às <xref:System.ServiceModel.Syndication.SyndicationFeed> <xref:System.ServiceModel.Syndication.SyndicationItem> classes e. Um feed expõe alguns metadados no nível do feed (por exemplo, título, descrição e autor), um local para armazenar extensões desconhecidas e um conjunto de itens que compõem o restante do conteúdo das informações do feed. Um item disponibiliza alguns metadados em nível de item (por exemplo, título, resumo e PublicationDate), um local para armazenar extensões desconhecidas e um elemento de conteúdo que contém o restante do conteúdo de informações do item. As abstrações principais de feed e item são suportadas por classes adicionais que representam construções de dados comuns referenciadas nas especificações de Atom 1,0 e RSS.  
  
 As informações transportadas em uma instância de feed podem ser convertidas em uma variedade de formatos XML. O processo de conversão de e para XML é gerenciado pela <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> classe. Essa classe é abstrata; implementações concretas são fornecidas para Atom 1,0 e RSS 2,0 <xref:System.ServiceModel.Syndication.Atom10FeedFormatter> e <xref:System.ServiceModel.Syndication.Rss20FeedFormatter> . Para usar classes de feed derivadas, use <xref:System.ServiceModel.Syndication.Atom10FeedFormatter%601> ou <xref:System.ServiceModel.Syndication.Rss20FeedFormatter%601> como elas permitem que você especifique uma classe de feed derivada. Para usar classes de item derivado <xref:System.ServiceModel.Syndication.Atom10ItemFormatter%601> , use ou <xref:System.ServiceModel.Syndication.Rss20ItemFormatter%601> como elas permitem que você especifique uma classe de item derivada que terceiros podem derivar sua própria implementação de <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> para dar suporte a formatos de distribuição diferentes.  
  
## <a name="extensibility"></a>Extensibilidade  
  
- Um recurso importante dos protocolos de distribuição é A extensibilidade. O Atom 1,0 e o RSS 2,0 permitem que você adicione atributos e elementos a feeds de distribuição que não estão definidos nas especificações. O modelo de programação de agregação do WCF fornece duas maneiras de trabalhar com atributos e extensões personalizados: derivar uma nova classe e acesso com rigidez de tipos. Para obter mais informações, consulte [extensibilidade de distribuição](syndication-extensibility.md).  
  
## <a name="see-also"></a>Confira também

- [Visão geral de sindicalização do WCF](wcf-syndication-overview.md)
- [Como o modelo de objeto de sindicalização do WCF mapeia para Atom e RSS](how-the-wcf-syndication-object-model-maps-to-atom-and-rss.md)
- [Modelo de programação WCF Web HTTP](wcf-web-http-programming-model.md)
