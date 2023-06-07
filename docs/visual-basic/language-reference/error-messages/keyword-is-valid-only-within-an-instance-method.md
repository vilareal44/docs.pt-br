---
title: "'<keyword>' só é valido dentro de um método de instância"
ms.date: 07/20/2015
f1_keywords:
- bc30043
- vbc30043
helpviewer_keywords:
- BC30043
ms.assetid: 7973aa82-a681-440c-9bca-242627d7ba86
ms.openlocfilehash: 537689405ea30bdd7c075320eba58a8723a93cdb
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397396"
---
# <a name="keyword-is-valid-only-within-an-instance-method"></a>'\<keyword>' só é valido dentro de um método de instância
As `Me` `MyClass` `MyBase` palavras-chave,, e se referem a instâncias de classe específicas. Você não pode usá-los dentro de um `Function` procedimento compartilhado ou `Sub` .  
  
 **ID do erro:** BC30043  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Remova a palavra-chave do procedimento ou remova a `Shared` palavra-chave da declaração do procedimento.  
  
## <a name="see-also"></a>Confira também

- [Atribuição de variável do objeto](../../programming-guide/language-features/variables/object-variable-assignment.md)
- [Me, My, MyBase e MyClass](../../programming-guide/program-structure/me-my-mybase-and-myclass.md)
- [Noções básicas de herança](../../programming-guide/language-features/objects-and-classes/inheritance-basics.md)
