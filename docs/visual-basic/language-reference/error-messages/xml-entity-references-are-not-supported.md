---
title: As referências de entidade XML não são suportadas
ms.date: 07/20/2015
f1_keywords:
- vbc31180
- bc31180
helpviewer_keywords:
- BC31180
ms.assetid: 2a393327-d8e2-4187-85b1-642b4f53b4ae
ms.openlocfilehash: ae997d853a93999a3b29215ea1257da7a1d48c84
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406450"
---
# <a name="xml-entity-references-are-not-supported"></a>As referências de entidade XML não são suportadas
Uma referência de entidade (por exemplo, `©` ) que não está definida na especificação XML 1,0 é incluída como um valor para um literal XML. Somente `&` as `"` `<` referências de entidade XML,,, `>` e `'` são suportadas em literais XML.  
  
 **ID do erro:** BC31180  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Remova a referência de entidade sem suporte.  
  
## <a name="see-also"></a>Confira também

- [Especificação dos literais XML e do XML 1.0](../../programming-guide/language-features/xml/xml-literals-and-the-xml-1-0-specification.md)
- [Literais XML](../xml-literals/index.md)
- [XML](../../programming-guide/language-features/xml/index.md)
