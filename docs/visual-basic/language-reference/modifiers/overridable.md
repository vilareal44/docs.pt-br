---
title: Substituível
ms.date: 07/20/2015
f1_keywords:
- Overridable
- vb.Overridable
helpviewer_keywords:
- elements [Visual Basic], concrete
- properties [Visual Basic], redefining
- overriding, Overridable keyword
- elements [Visual Basic], virtual
- virtual [elements VB]
- procedures [Visual Basic], overriding
- concrete [elements VB]
- procedures [Visual Basic], redefining
- Overridable keyword [Visual Basic]
- properties [Visual Basic], overriding
ms.assetid: 612581e7-8a4c-4a5d-beff-3402fffa6f35
ms.openlocfilehash: dcbabde8464dd8a0ce5fad24d7d72b1e780270d3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84392113"
---
# <a name="overridable-visual-basic"></a>Substituível (Visual Basic)
Especifica que uma propriedade ou procedimento pode ser substituído por uma propriedade nomeada de forma idêntica ou um procedimento em uma classe derivada.  
  
## <a name="remarks"></a>Comentários  
 O `Overridable` modificador permite que uma propriedade ou método em uma classe seja substituído em uma classe derivada. O modificador [NotOverridable](notoverridable.md) impede que uma propriedade ou um método seja substituído em uma classe derivada.  Para obter mais informações, consulte [Noções básicas de herança](../../programming-guide/language-features/objects-and-classes/inheritance-basics.md).  
  
 Se o `Overridable` `NotOverridable` modificador ou não for especificado, a configuração padrão dependerá se a propriedade ou o método substituirá uma propriedade ou método de classe base. Se a propriedade ou o método substituir uma propriedade ou método de classe base, a configuração padrão será `Overridable` ; caso contrário, será `NotOverridable` .  
  
 Você pode sombrear ou substituir para redefinir um elemento herdado, mas há diferenças significativas entre as duas abordagens. Para obter mais informações, consulte [sombreamento em Visual Basic](../../programming-guide/language-features/declared-elements/shadowing.md).  
  
 Um elemento que pode ser substituído às vezes é chamado de elemento *virtual* . Se ele puder ser substituído, mas não precisar ser, às vezes ele também é chamado de elemento *concreto* .  
  
 Você pode usar `Overridable` somente em uma instrução de declaração de propriedade ou de procedimento.  
  
## <a name="combined-modifiers"></a>Modificadores combinados  
 Você não pode especificar `Overridable` ou `NotOverridable` para um `Private` método.  
  
 Você não pode especificar `Overridable` juntos com `MustOverride` , `NotOverridable` ou `Shared` na mesma declaração.  
  
 Como um elemento de substituição é implicitamente substituível, você não pode combinar `Overridable` com `Overrides` .  
  
## <a name="usage"></a>Uso  
 O `Overridable` modificador pode ser usado nesses contextos:  
  
 [Instrução Function](../statements/function-statement.md)  
  
 [Instrução Property](../statements/property-statement.md)  
  
 [Instrução Sub](../statements/sub-statement.md)  
  
## <a name="see-also"></a>Confira também

- [Modificadores](index.md)
- [Noções básicas de herança](../../programming-guide/language-features/objects-and-classes/inheritance-basics.md)
- [MustOverride](mustoverride.md)
- [NotOverridable](notoverridable.md)
- [Substituições](overrides.md)
- [Palavras-chave](../keywords/index.md)
- [Sombreamento no Visual Basic](../../programming-guide/language-features/declared-elements/shadowing.md)
