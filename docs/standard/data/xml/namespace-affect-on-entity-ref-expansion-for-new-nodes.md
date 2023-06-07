---
title: Afetar o namespace da expansão de referência de entidade para novos elementos e atributos recipiente de nós
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 64359aee-aab0-4042-9a32-d19789af6ca7
ms.openlocfilehash: 05ec622f09106978281cd3e6f0a82f13703c2097
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288804"
---
# <a name="namespace-affect-on-entity-reference-expansion-for-new-nodes-containing-elements-and-attributes"></a>Afetar o namespace da expansão de referência de entidade para novos elementos e atributos recipiente de nós
Porque o conteúdo de uma declaração de entidade pode conter qualquer coisa, há uma chance de que o conteúdo pode conter um elemento como `<!ENTITY aname "<elem>test</elem>">`.  
  
 Quando XML é analisado, `&aname;` não é expandido com o conteúdo de substituição em tempo de análise. A expansão XML não é feita porque a resolução de namespace para o elemento não pode ocorrer até que o nó é colocado no documento. Até esse momento, eles não há nenhum conhecimento de namespace que está no escopo. Quando o nó é colocado no documento, então a resolução do namespace ocorre, e o conteúdo de entidade resultante é analisado em seus nós apropriadas.  
  
> [!NOTE]
> Uma vez que a expansão ocorre em um nó recém-criado de referência de entidade, nunca repete. Portanto, namespaces utilizados no texto de substituição para o elemento são associadas no momento em que o nó pai é definido. No entanto, o namespace pode ser alterado para nós existentes de referência de entidade quando você o remove e inserir em outro lugar, ou em nós de referência de entidade que são clonados com o método **CloneNode**.  
  
## <a name="see-also"></a>Veja também

- [XML Document Object Model (DOM)](xml-document-object-model-dom.md)
