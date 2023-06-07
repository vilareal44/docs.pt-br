---
title: A estrutura '<structurename>' deve conter pelos menos uma variável membro da instância ou pelo menos uma declaração de evento da instância não marcada como 'Personalizada'
ms.date: 07/20/2015
f1_keywords:
- bc30941
- vbc30941
helpviewer_keywords:
- BC30941
ms.assetid: 7054cc1e-bac3-4c3d-82f3-35772bd8dd3b
ms.openlocfilehash: 7b5bda7b1a2ae37eb509c736deae1652dc5e6ab0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84374012"
---
# <a name="structure-structurename-must-contain-at-least-one-instance-member-variable-or-at-least-one-instance-event-declaration-not-marked-custom"></a>A estrutura '\<structurename>' deve conter pelos menos uma variável membro da instância ou pelo menos uma declaração de evento da instância não marcada como 'Personalizada'
Uma definição de estrutura não inclui nenhuma variável não compartilhada ou eventos não compartilhados não-personalizados.  
  
 Cada estrutura deve ter uma variável ou um evento que se aplica a cada instância específica (não compartilhada), em vez de a todas as instâncias coletivamente ([compartilhado](../modifiers/shared.md)). As constantes, as propriedades e os procedimentos não compartilhados não atendem a esse requisito. Além disso, se não houver nenhuma variável não compartilhada e apenas um evento não compartilhado, esse evento não poderá ser um `Custom` evento.  
  
 **ID do erro:** BC30941  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Defina pelo menos uma variável ou evento que não seja `Shared` . Se você definir apenas um evento, ele deverá ser não personalizado, bem como não compartilhado.  
  
## <a name="see-also"></a>Confira também

- [Estruturas](../../programming-guide/language-features/data-types/structures.md)
- [Como: Declarar uma estrutura](../../programming-guide/language-features/data-types/how-to-declare-a-structure.md)
- [Instrução Structure](../statements/structure-statement.md)
