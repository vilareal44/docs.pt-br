---
title: Atualizações dinâmicas a NodeLists e a NamedNodeMaps
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 76c511fd-6704-4ca4-8078-860a68d898ad
ms.openlocfilehash: 0199f24e9ef8dd28f91976edd50f78399dc893ef
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84292079"
---
# <a name="dynamic-updates-to-nodelists-and-namednodemaps"></a>Atualizações dinâmicas a NodeLists e a NamedNodeMaps
Como **XmlNodeList** e **XmlNamedNodeMap** contêm um conjunto de nós, mas o documento XML é carregado na memória e está sendo alterado, o W3C (World Wide Web Consortium) indica que os objetos que contêm conjuntos de nós precisam ser dinâmicos. Isto é, se o documento subjacente é modificada, então os dados nesses dois objetos também deve alterar. Portanto, se você tiver um **XmlNodeList** que contenha todos os elementos filho de um elemento específico (por exemplo, o elemento X), inclua um elemento adicional, o elemento Q, no documento sob o elemento X. **XmlNodeList** também deve ter esse novo elemento Q adicionado à coleção. O mesmo vale em ordem inversa. Se um nó é adicionado a **XmlNodeList**, o documento subjacente é atualizado também.  
  
## <a name="see-also"></a>Veja também

- [XML Document Object Model (DOM)](xml-document-object-model-dom.md)
