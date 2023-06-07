---
title: Cláusula Implements
ms.date: 07/20/2015
f1_keywords:
- vb.ImplementsClause
helpviewer_keywords:
- interface implementation [Visual Basic], reimplementation
- interface members [Visual Basic], reimplementation
- interface members [Visual Basic], Implements keyword
- interface members
- members [Visual Basic], reimplementation
- interface implementation [Visual Basic], Implements keyword
- interface members [Visual Basic], implementing
- Implements keyword [Visual Basic]
- Implements statement [Visual Basic], about Implements
- members [Visual Basic], implementing
- members [Visual Basic], Implements keyword
- reimplementation
ms.assetid: 5252cdf9-964d-4fc6-af0f-0449b7126b5a
ms.openlocfilehash: 46ab1a1148e8d73d91293aedfc407e5efdc7cfb4
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404557"
---
# <a name="implements-clause-visual-basic"></a>Cláusula Implements (Visual Basic)
Indica que um membro de classe ou estrutura está fornecendo a implementação para um membro definido em uma interface.  
  
## <a name="remarks"></a>Comentários  
A `Implements` palavra-chave não é a mesma que a [instrução Implements](implements-statement.md). Você usa a `Implements` instrução para especificar que uma classe ou estrutura implementa uma ou mais interfaces e, em seguida, para cada membro, você usa a `Implements` palavra-chave para especificar qual interface e qual membro ele implementa.

Se uma classe ou estrutura implementa uma interface, ela deve incluir a `Implements` instrução imediatamente após a [instrução de classe](class-statement.md) ou a [instrução de estrutura](structure-statement.md)e deve implementar todos os membros definidos pela interface.

## <a name="reimplementation"></a>Reimplementação  
Em uma classe derivada, você pode reimplementar um membro de interface que a classe base já tenha implementado. Isso é diferente de substituir o membro da classe base nos seguintes aspectos:

- O membro da classe base não precisa ser [substituível](../modifiers/overridable.md) para ser reimplementado.
- Você pode reimplementar o membro com um nome diferente.

A `Implements` palavra-chave pode ser usada nos seguintes contextos:

- [Instrução Event](event-statement.md)
- [Instrução Function](function-statement.md)
- [Instrução Property](property-statement.md)
- [Instrução Sub](sub-statement.md)  
  
## <a name="see-also"></a>Confira também

- [Instrução Implements](implements-statement.md)
- [Instrução Interface](interface-statement.md)
- [Instrução Class](class-statement.md)
- [Instrução Structure](structure-statement.md)
