---
title: Particular
ms.date: 07/20/2015
f1_keywords:
- vb.Private
helpviewer_keywords:
- Private keyword [Visual Basic]
- Private keyword [Visual Basic], syntax
ms.assetid: aba74a2e-5824-4613-bf63-b9ec7787f4e6
ms.openlocfilehash: 524f03e77e075bef08a1b41b563985de41baacb6
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404804"
---
# <a name="private-visual-basic"></a>Particular (Visual Basic)
Especifica que um ou mais elementos de programação declarados são acessíveis somente de dentro de seu contexto de declaração, incluindo de dentro de qualquer tipo contido.  
  
## <a name="remarks"></a>Comentários  
 Se um elemento de programação representar a funcionalidade proprietária ou contiver dados confidenciais, você geralmente desejará limitar o acesso a ele o mais estritamente possível. Você Obtém a limitação máxima permitindo apenas o módulo, a classe ou a estrutura que a define para acessá-lo. Para limitar o acesso a um elemento dessa forma, você pode declará-lo com `Private` .  

> [!NOTE]
> Você também pode usar o modificador de acesso [protegido privado](private-protected.md) , que torna um membro acessível de dentro dessa classe e de classes derivadas localizadas em seu assembly recipiente.

## <a name="rules"></a>Regras  

- **Contexto de declaração.** Você pode usar `Private` somente no nível do módulo. Isso significa que o contexto de declaração para um `Private` elemento deve ser um módulo, uma classe ou uma estrutura e não pode ser um arquivo de origem, namespace, interface ou procedimento.  
  
## <a name="behavior"></a>Comportamento  
  
- **Nível de acesso.** Todo o código dentro de um contexto de declaração pode acessar seus `Private` elementos. Isso inclui o código dentro de um tipo contido, como uma classe aninhada ou uma expressão de atribuição em uma enumeração. Nenhum código fora do contexto da declaração pode acessar seus `Private` elementos.  
  
- **Modificadores de acesso.** As palavras-chave que especificam o nível de acesso são chamadas de *modificadores de acesso*. Para obter uma comparação dos modificadores de acesso, consulte [níveis de acesso em Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md).  
  
 O `Private` modificador pode ser usado nesses contextos:  
  
 [Instrução Class](../statements/class-statement.md)  
  
 [Instrução Const](../statements/const-statement.md)  
  
 [Instrução Declare](../statements/declare-statement.md)  
  
 [Instrução Delegate](../statements/delegate-statement.md)  
  
 [Instrução Dim](../statements/dim-statement.md)  
  
 [Instrução Enum](../statements/enum-statement.md)  
  
 [Instrução Event](../statements/event-statement.md)  
  
 [Instrução Function](../statements/function-statement.md)  
  
 [Instrução Interface](../statements/interface-statement.md)  
  
 [Instrução Property](../statements/property-statement.md)  
  
 [Instrução Structure](../statements/structure-statement.md)  
  
 [Instrução Sub](../statements/sub-statement.md)  
  
## <a name="see-also"></a>Confira também

- [Pública](public.md)
- [Protected](protected.md)
- [Público](friend.md)
- [Particular protegido](./private-protected.md)
- [Amigo Protegido](./protected-friend.md)
- [Níveis de acesso no Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md)
- [Procedimentos](../../programming-guide/language-features/procedures/index.md)
- [Estruturas](../../programming-guide/language-features/data-types/structures.md)
- [Objetos e classes](../../programming-guide/language-features/objects-and-classes/index.md)
