---
title: "'<expression>' não pode ser usado como restrição de tipo"
ms.date: 07/20/2015
f1_keywords:
- bc32061
- vbc32061
helpviewer_keywords:
- BC32061
ms.assetid: b17821b7-fa14-4397-a211-6e2a14079f09
ms.openlocfilehash: e2ba411a5f0db21539a9cf99c7645b8c9309caab
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409550"
---
# <a name="expression-cannot-be-used-as-a-type-constraint"></a>'\<expression>' não pode ser usado como restrição de tipo
Uma lista de restrições inclui uma expressão que não representa uma restrição válida em um parâmetro de tipo.  
  
 Uma lista de restrições impõe requisitos no argumento de tipo passado para o parâmetro de tipo. Você pode especificar os seguintes requisitos em qualquer combinação:  
  
- O argumento de tipo deve implementar uma ou mais interfaces  
  
- O argumento de tipo deve herdar de no máximo uma classe  
  
- O argumento de tipo deve expor um construtor sem parâmetros que o código de criação possa acessar (incluir a `New` restrição)  
  
 Se você não incluir nenhuma classe ou interface específica na lista de restrições, poderá impor um requisito mais geral especificando um dos seguintes:  
  
- O argumento de tipo deve ser um tipo de valor (incluir a `Structure` restrição)  
  
- O argumento de tipo deve ser um tipo de referência (incluir a `Class` restrição)  
  
 Não é possível especificar ambos `Structure` e `Class` para o mesmo parâmetro de tipo, e você não pode especificar um mais de uma vez.  
  
 **ID do erro:** BC32061  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Verifique se a expressão e seus elementos estão escritos corretamente.  
  
- Se a expressão não for qualificada para a lista anterior de requisitos, remova-a da lista de restrições.  
  
- Se a expressão se referir a uma interface ou classe, verifique se o compilador tem acesso a essa interface ou classe. Talvez seja necessário qualificar seu nome, e talvez seja necessário adicionar uma referência ao seu projeto. Para obter mais informações, consulte "referências a projetos" em [referências a elementos declarados](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md).  
  
## <a name="see-also"></a>Confira também

- [Tipos genéricos no Visual Basic](../../programming-guide/language-features/data-types/generic-types.md)
- [Tipos de valor e referência](../../programming-guide/language-features/data-types/value-types-and-reference-types.md)
- [Referências a elementos declarados](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)
