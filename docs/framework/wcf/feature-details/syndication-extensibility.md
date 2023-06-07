---
title: Extensibilidade de sindicalização
ms.date: 03/30/2017
ms.assetid: 4d941175-74a2-4b15-81b3-086e8a95d25f
ms.openlocfilehash: 5e8f47b45897f46e15847c793c986e953523e66b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600718"
---
# <a name="syndication-extensibility"></a>Extensibilidade de sindicalização
A API de distribuição foi projetada para fornecer um modelo de programação de formato neutro que permite que o conteúdo agregado seja gravado na conexão em uma variedade de formatos. O modelo de dados abstratos consiste nas seguintes classes:  
  
- <xref:System.ServiceModel.Syndication.SyndicationCategory>  
  
- <xref:System.ServiceModel.Syndication.SyndicationFeed>  
  
- <xref:System.ServiceModel.Syndication.SyndicationItem>  
  
- <xref:System.ServiceModel.Syndication.SyndicationLink>  
  
- <xref:System.ServiceModel.Syndication.SyndicationPerson>  
  
 Essas classes mapeiam fortemente para as construções definidas na especificação Atom 1,0, embora alguns dos nomes sejam diferentes.  
  
 Um recurso importante dos protocolos de distribuição é A extensibilidade. O Atom 1,0 e o RSS 2,0, adicionam atributos e elementos aos feeds de distribuição que não estão definidos nas especificações. O modelo de programação de distribuição do Windows Communication Foundation (WCF) fornece as seguintes maneiras de trabalhar com atributos e extensões personalizados, acesso com rigidez de tipos e derivar uma nova classe.  
  
## <a name="loosely-typed-access"></a>Acesso com rigidez de tipos  
 Adicionar extensões derivando uma nova classe requer a gravação de código adicional. Outra opção é acessar extensões de forma menos tipada. Todos os tipos definidos no modelo de dados abstract de distribuição contêm propriedades nomeadas `AttributeExtensions` e `ElementExtensions` (com uma exceção, <xref:System.ServiceModel.Syndication.SyndicationContent> tem uma `AttributeExtensions` propriedade, mas nenhuma `ElementExtensions` Propriedade). Essas propriedades são coleções de extensões não processadas pelos `TryParseAttribute` métodos e, `TryParseElement` respectivamente. Você pode acessar essas extensões não processadas chamando <xref:System.ServiceModel.Syndication.SyndicationElementExtensionCollection.ReadElementExtensions%2A?displayProperty=nameWithType> a `ElementExtensions` propriedade de <xref:System.ServiceModel.Syndication.SyndicationFeed> ,,, <xref:System.ServiceModel.Syndication.SyndicationItem> <xref:System.ServiceModel.Syndication.SyndicationLink> <xref:System.ServiceModel.Syndication.SyndicationPerson> e <xref:System.ServiceModel.Syndication.SyndicationCategory> . Esse conjunto de métodos localiza todas as extensões com o nome e o namespace especificados, os desserializa individualmente em instâncias do `TExtension` e as retorna como uma coleção de `TExtension` objetos.  
  
## <a name="deriving-a-new-class"></a>Derivando uma nova classe  
 Você pode derivar uma nova classe de qualquer uma das classes de modelo de dados abstratos existentes. Faça isso ao implementar um aplicativo no qual a maioria dos feeds com os quais você está trabalhando tem uma extensão específica. Neste tópico, a maioria dos feeds com os quais o programa funciona contém uma `MyExtension` extensão. Para fornecer uma experiência de programação aprimorada, execute as seguintes etapas:  
  
- Crie uma classe para manter os dados de extensão. Nesse caso, crie uma classe chamada MyExtension.  
  
- Derive uma classe chamada MyExtensionItem de <xref:System.ServiceModel.Syndication.SyndicationItem> para expor uma propriedade do tipo MyExtension para fins de programação.  
  
- Substitua <xref:System.ServiceModel.Syndication.SyndicationItem.TryParseElement%28System.Xml.XmlReader%2CSystem.String%29> na classe MyExtensionItem para instanciar uma nova instância MyExtension quando uma MyExtension for lida em.  
  
- Substitua <xref:System.ServiceModel.Syndication.SyndicationItem.WriteElementExtensions%28System.Xml.XmlWriter%2CSystem.String%29> na classe MyExtensionItem para gravar o conteúdo da propriedade MyExtension em um gravador de XML.  
  
- Derive uma classe chamada MyExtensionFeed de <xref:System.ServiceModel.Syndication.SyndicationFeed> .  
  
- Substitua <xref:System.ServiceModel.Syndication.SyndicationFeed.CreateItem> na classe MyExtensionFeed para instanciar um MyExtensionItem em vez do padrão <xref:System.ServiceModel.Syndication.SyndicationItem> . Uma série de métodos é definida em <xref:System.ServiceModel.Syndication.SyndicationFeed> e <xref:System.ServiceModel.Syndication.SyndicationItem> que pode criar <xref:System.ServiceModel.Syndication.SyndicationLink> <xref:System.ServiceModel.Syndication.SyndicationCategory> objetos, e <xref:System.ServiceModel.Syndication.SyndicationPerson> (por exemplo,, <xref:System.ServiceModel.Syndication.SyndicationFeed.CreateLink> <xref:System.ServiceModel.Syndication.SyndicationFeed.CreateCategory> e <xref:System.ServiceModel.Syndication.SyndicationFeed.CreatePerson> ). Todos os quais podem ser substituídos para criar uma classe derivada personalizada.  
  
## <a name="see-also"></a>Consulte também

- [Visão geral de sindicalização do WCF](wcf-syndication-overview.md)
- [Arquitetura de sindicalização](architecture-of-syndication.md)
