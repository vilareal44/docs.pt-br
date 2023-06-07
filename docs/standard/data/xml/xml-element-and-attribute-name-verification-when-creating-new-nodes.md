---
title: Verificação de nome de elemento XML e de atributo para criar novos nós
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: b489f647-a175-4659-ada4-170058bb41d0
ms.openlocfilehash: 1da893261b35c6b57c4f08eb53b7701ec1d81fae
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289844"
---
# <a name="xml-element-and-attribute-name-verification-when-creating-new-nodes"></a>Verificação de nome de elemento XML e de atributo para criar novos nós
O modelo de objeto de documento XML (DOM) verifica a validade de nomes ao criar novos nós ou nós de atributo do elemento. Se os nomes contenham caracteres inválidos, uma exceção é lançada. Para garantir que os nomes sejam válidos e codificados corretamente, você precisa usar a classe **XmlConvert** para codificar o nome e descodificá-lo de volta para o nível de aplicativo. **XmlWriter** tem métodos que realizam trabalho adicional para garantir que XML corretamente formado seja gerado.  
  
## <a name="see-also"></a>Veja também

- [XML Document Object Model (DOM)](xml-document-object-model-dom.md)
