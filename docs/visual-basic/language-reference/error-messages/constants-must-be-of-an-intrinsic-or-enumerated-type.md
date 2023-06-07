---
title: As constantes devem ser do tipo intrínseco ou enumerado, e não classe, estrutura, parâmetro de tipo ou tipo de matriz
ms.date: 07/20/2015
f1_keywords:
- vbc30424
- bc30424
helpviewer_keywords:
- BC30424
ms.assetid: 2d402c2f-27ad-428b-b699-d45cd62f7196
ms.openlocfilehash: 9e36b84252c3d8762308e95323b8e284977df8c0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409758"
---
# <a name="constants-must-be-of-an-intrinsic-or-enumerated-type-not-a-class-structure-type-parameter-or-array-type"></a>As constantes devem ser do tipo intrínseco ou enumerado, e não classe, estrutura, parâmetro de tipo ou tipo de matriz
Você tentou declarar uma constante como uma classe, estrutura ou tipo de matriz, ou como um parâmetro de tipo definido por um tipo genérico recipiente.  
  
 Constantes devem ser de um tipo intrínseco (,,,,,,,, `Boolean` `Byte` `Date` `Decimal` `Double` `Integer` `Long` `Object` `SByte` , `Short` , `Single` ,,, `String` `UInteger` `ULong` , ou `UShort` ), ou um `Enum` tipo com base em um dos tipos integrais.  
  
 **ID do erro:** BC30424  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
1. Declare a constante como um tipo intrínseco ou `Enum` .  
  
2. Uma constante também pode ser um valor especial, como `True` , `False` ou `Nothing` . O compilador considera esses valores predefinidos como sendo do tipo intrínseco apropriado.  
  
## <a name="see-also"></a>Confira também

- [Constantes e enumerações](../constants-and-enumerations.md)
- [Tipos de dados](../../programming-guide/language-features/data-types/index.md)
- [Tipos de dados](../data-types/index.md)
