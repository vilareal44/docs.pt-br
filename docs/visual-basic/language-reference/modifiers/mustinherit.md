---
title: MustInherit
ms.date: 07/20/2015
f1_keywords:
- MustInherit
- vb.MustInherit
helpviewer_keywords:
- classes [Visual Basic], abstract
- MustInherit classes [Visual Basic], MustInherit keyword
- abstract classes [Visual Basic], MustInherit class
- MustInherit keyword [Visual Basic]
ms.assetid: b8f05185-90e3-4dd7-adc2-90d852fab5b4
ms.openlocfilehash: 84df7a5cfdad3b5bc6764675725a5d0cb402b0b7
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396201"
---
# <a name="mustinherit-visual-basic"></a>MustInherit (Visual Basic)
Especifica que uma classe pode ser usada somente como uma classe base e que você não pode criar um objeto diretamente dela.  
  
## <a name="remarks"></a>Comentários  
 A finalidade de uma *classe base* (também conhecida como *classe abstrata*) é definir a funcionalidade que é comum a todas as classes derivadas dela. Isso salva as classes derivadas de ter que redefinir os elementos comuns. Em alguns casos, essa funcionalidade comum não é completa o suficiente para tornar um objeto utilizável, e cada classe derivada define a funcionalidade ausente. Nesse caso, você deseja que o código de consumo Crie objetos somente de classes derivadas. Você usa `MustInherit` na classe base para impor isso.  
  
 Outro uso de uma `MustInherit` classe é restringir uma variável a um conjunto de classes relacionadas. Você pode definir uma classe base e derivar todas essas classes relacionadas. A classe base não precisa fornecer nenhuma funcionalidade comum a todas as classes derivadas, mas pode servir como um filtro para atribuir valores a variáveis. Se o código de consumo declarar uma variável como a classe base, Visual Basic permite atribuir apenas um objeto de uma das classes derivadas a essa variável.  
  
 A .NET Framework define várias `MustInherit` classes, entre elas <xref:System.Array> , <xref:System.Enum> e <xref:System.ValueType> . <xref:System.ValueType>é um exemplo de uma classe base que restringe uma variável. Todos os tipos de valor derivam de <xref:System.ValueType> . Se você declarar uma variável como <xref:System.ValueType> , poderá atribuir apenas tipos de valor a essa variável.  
  
## <a name="rules"></a>Regras  
  
- **Contexto de declaração.** Você pode usar `MustInherit` somente em uma `Class` instrução.  
  
- **Modificadores combinados.** Você não pode especificar `MustInherit` juntos com `NotInheritable` na mesma declaração.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir ilustra a herança forçada e a substituição forçada. A classe base `shape` define uma variável `acrossLine` . As classes `circle` e `square` derivam de `shape` . Eles herdam a definição de `acrossLine` , mas devem definir a função `area` porque esse cálculo é diferente para cada tipo de forma.  
  
 [!code-vb[VbVbalrKeywords#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class1.vb#2)]  
  
 Você pode declarar `shape1` e `shape2` ser do tipo `shape` . No entanto, não é possível criar um objeto a partir do `shape` porque ele não tem a funcionalidade da função `area` e está marcado `MustInherit` .  
  
 Como elas são declaradas como `shape` , as variáveis `shape1` e `shape2` são restritas a objetos das classes derivadas `circle` e `square` . Visual Basic não permite que você atribua nenhum outro objeto a essas variáveis, o que oferece um alto nível de segurança de tipo.  
  
## <a name="usage"></a>Uso  
 O `MustInherit` modificador pode ser usado neste contexto:  
  
 [Instrução Class](../statements/class-statement.md)  
  
## <a name="see-also"></a>Confira também

- [Instrução Inherits](../statements/inherits-statement.md)
- [NotInheritable](notinheritable.md)
- [Palavras-chave](../keywords/index.md)
- [Noções básicas de herança](../../programming-guide/language-features/objects-and-classes/inheritance-basics.md)
