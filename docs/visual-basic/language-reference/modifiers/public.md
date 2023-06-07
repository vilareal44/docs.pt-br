---
title: Público
ms.date: 07/20/2015
f1_keywords:
- vb.Public
helpviewer_keywords:
- Public keyword [Visual Basic]
- Public keyword [Visual Basic], syntax
- Public access modifier
ms.assetid: 284c9e1b-ed23-499b-9bc9-ad87c11485a5
ms.openlocfilehash: 35332e50227cdef6386362df17c10b5b2cdaa689
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415343"
---
# <a name="public-visual-basic"></a>Público (Visual Basic)
Especifica que um ou mais elementos de programação declarados não têm restrições de acesso.  
  
## <a name="remarks"></a>Comentários  
 Se você estiver publicando um componente ou conjunto de componentes, como uma biblioteca de classes, geralmente você desejará que os elementos de programação sejam acessíveis por qualquer código que interopere com seu assembly. Para confere tal acesso ilimitado em um elemento, você pode declará-lo com `Public` .  
  
 Acesso público é o nível normal de um elemento de programação quando você não precisa limitar o acesso a ele. Observe que o nível de acesso de um elemento declarado em uma interface, módulo, classe ou estrutura usa como padrão `Public` se você não declará-lo caso contrário.  
  
## <a name="rules"></a>Regras  
  
- **Contexto de declaração.** Você pode usar `Public` somente no nível do módulo, da interface ou do namespace. Isso significa que o contexto de declaração para um `Public` elemento deve ser um arquivo de origem, namespace, interface, módulo, classe ou estrutura, e não pode ser um procedimento.  
  
## <a name="behavior"></a>Comportamento  
  
- **Nível de acesso.** Todo o código que pode acessar um módulo, uma classe ou uma estrutura pode acessar seus `Public` elementos.  
  
- **Acesso padrão.** Variáveis locais dentro de um procedimento assumem o acesso público como padrão, e você não pode usar nenhum modificador de acesso neles.  
  
- **Modificadores de acesso.** As palavras-chave que especificam o nível de acesso são chamadas de *modificadores de acesso*. Para obter uma comparação dos modificadores de acesso, consulte [níveis de acesso em Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md).  
  
 O `Public` modificador pode ser usado nesses contextos:  
  
 [Instrução Class](../statements/class-statement.md)  
  
 [Instrução Const](../statements/const-statement.md)  
  
 [Instrução Declare](../statements/declare-statement.md)  
  
 [Instrução Delegate](../statements/delegate-statement.md)  
  
 [Instrução Dim](../statements/dim-statement.md)  
  
 [Instrução Enum](../statements/enum-statement.md)  
  
 [Instrução Event](../statements/event-statement.md)  
  
 [Instrução Function](../statements/function-statement.md)  
  
 [Instrução Interface](../statements/interface-statement.md)  
  
 [Instrução Module](../statements/module-statement.md)  
  
 [Instrução Operator](../statements/operator-statement.md)  
  
 [Instrução Property](../statements/property-statement.md)  
  
 [Instrução Structure](../statements/structure-statement.md)  
  
 [Instrução Sub](../statements/sub-statement.md)  
  
## <a name="see-also"></a>Confira também

- [Protected](protected.md)
- [Público](friend.md)
- [Privada](private.md)
- [Particular protegido](private-protected.md)
- [Amigo Protegido](protected-friend.md)
- [Níveis de acesso no Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md)
- [Procedimentos](../../programming-guide/language-features/procedures/index.md)
- [Estruturas](../../programming-guide/language-features/data-types/structures.md)
- [Objetos e classes](../../programming-guide/language-features/objects-and-classes/index.md)
