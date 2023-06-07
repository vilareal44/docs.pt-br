---
title: O tipo <typename> não é compatível com CLS
ms.date: 07/20/2015
f1_keywords:
- bc40041
- vbc40041
helpviewer_keywords:
- BC40041
ms.assetid: 634132c2-5646-44aa-98c6-f773e2e63882
ms.openlocfilehash: eacf5036ebc6fc31dfa0e8de39c4fb574c9072b3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84386952"
---
# <a name="type-typename-is-not-cls-compliant"></a>O tipo \<typename> não é compatível com CLS
Uma variável, propriedade ou retorno de função é declarada com um tipo de dados que não tem conformidade com CLS.  
  
 Para que um aplicativo seja compatível com a [independência de linguagem e com os componentes independentes de linguagem](../../../standard/language-independence-and-language-independent-components.md) (CLS), ele deve usar somente tipos em conformidade com CLS.  
  
 Os tipos de dados a seguir Visual Basic não são compatíveis com CLS:  
  
- [Tipo de Dados SByte](../data-types/sbyte-data-type.md)  
  
- [Tipo de Dados UInteger](../data-types/uinteger-data-type.md)  
  
- [Tipo de Dados ULong](../data-types/ulong-data-type.md)  
  
- [Tipo de Dados UShort](../data-types/ushort-data-type.md)  
  
 **ID do erro:** BC40041  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Se seu aplicativo precisar ser compatível com CLS, altere o tipo de dados desse elemento para o tipo mais próximo em conformidade com CLS. Por exemplo, no lugar de `UInteger` você pode ser capaz de usar `Integer` se não precisar do intervalo de valores acima de 2.147.483.647. Se você precisar do intervalo estendido, poderá substituir `UInteger` por `Long` .  
  
- Se seu aplicativo não precisar ser compatível com CLS, você não precisará alterar nada. No entanto, você deve estar ciente de sua não conformidade.
