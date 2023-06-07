---
title: Como declarar uma constante
ms.date: 07/20/2015
f1_keywords:
- vb.constant
helpviewer_keywords:
- Integer data type [Visual Basic], declaring constants
- Single data type [Visual Basic], declaring constants
- Const statement [Visual Basic], declaring constants
- procedures [Visual Basic], declaration
- data types [Visual Basic], constants
- Long data type [Visual Basic], declaring constants
- Visual Basic code, declaring variables and constants
- Double data type [Visual Basic], declaring constants
- Boolean data type [Visual Basic], declaring constants
- modules, declaring constants
- Byte data type [Visual Basic], declaring constants
- constants [Visual Basic], declaring
- Date data type [Visual Basic], declaring constants
- String data type [Visual Basic], declaring constants
- declaring constants [Visual Basic], const keyword
- Currency data type [Visual Basic], declaring constants and variants
- module-level constants and variables
- Object data type [Visual Basic], declaring constants
ms.assetid: f901b4fa-481f-4621-822e-427060577ad1
ms.openlocfilehash: ffaa98f6af3d4b276f5c0b1153841acdea0809d7
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84414473"
---
# <a name="how-to-declare-a-constant-visual-basic"></a>Como declarar uma constante (Visual Basic)
Você usa a `Const` instrução para declarar uma constante e definir seu valor. Ao declarar uma constante, você atribui um nome significativo a um valor. Depois que uma constante é declarada, ela não pode ser modificada ou atribuída um novo valor.  
  
 Você declara uma constante em um procedimento ou na seção declarações de um módulo, classe ou estrutura. As constantes de nível de classe ou estrutura são `Private` por padrão, mas também podem ser declaradas como `Public` , `Friend` , `Protected` ou `Protected Friend` para o nível apropriado de acesso ao código.  
  
 A constante deve ter um nome simbólico válido (as regras são as mesmas que as para criar nomes de variáveis) e uma expressão composta por constantes numéricas ou de cadeia de caracteres e operadores (mas sem chamadas de função).  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
### <a name="to-declare-a-constant"></a>Para declarar uma constante  
  
- Escreva uma declaração que inclua um especificador de acesso, a `Const` palavra-chave e uma expressão, como nos exemplos a seguir:  
  
     [!code-vb[VbEnumsTask#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#8)]  
  
     Quando [Option Infer](../../../language-reference/statements/option-infer-statement.md) é `Off` e [Option Strict](../../../language-reference/statements/option-strict-statement.md) é `On` , você deve declarar uma constante explicitamente especificando um tipo de dados (,,,, `Boolean` `Byte` `Char` `DateTime` `Decimal` , `Double` , `Integer` , `Long` , `Short` , `Single` ou `String` ).  
  
     Quando `Option Infer` é `On` ou `Option Strict` é `Off` , você pode declarar uma constante sem especificar um tipo de dados com uma `As` cláusula. O compilador determina o tipo da constante do tipo da expressão. Para obter mais informações, consulte [constantes e tipos de dados literais](constant-and-literal-data-types.md).  
  
### <a name="to-declare-a-constant-that-has-an-explicitly-stated-data-type"></a>Para declarar uma constante que tem um tipo de dados explicitamente declarado  
  
- Escreva uma declaração que inclua a `As` palavra-chave e um tipo de dados explícito, como nos exemplos a seguir:  
  
     [!code-vb[VbEnumsTask#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#9)]  
  
     Você pode declarar várias constantes em uma única linha, embora seu código seja mais legível se você declarar apenas uma única constante por linha. Se você declarar várias constantes em uma única linha, todas elas deverão ter o mesmo nível de acesso ( `Public` , `Private` ,, `Friend` `Protected` ou `Protected Friend` ).  
  
### <a name="to-declare-multiple-constants-on-a-single-line"></a>Para declarar várias constantes em uma única linha  
  
- Separe as declarações com uma vírgula e um espaço, como no exemplo a seguir:  
  
    ```vb  
    Public Const Four As Integer = 4, Five As Integer = 5, Six As Integer = 44  
    ```  
  
## <a name="see-also"></a>Confira também

- [Instrução Const](../../../language-reference/statements/const-statement.md)
- [Tipos de dados constante e literal](constant-and-literal-data-types.md)
- [Visão geral de constantes](constants-overview.md)
- [Como declarar uma constante](how-to-declare-a-constant.md)
- [Constantes definidas pelo usuário](user-defined-constants.md)
- [Tipos de dados constante e literal](constant-and-literal-data-types.md)
- [Como agrupar valores constantes relacionados](how-to-group-related-constant-values-together.md)
- [Visão geral de enumerações](enumerations-overview.md)
- [Como Declarar Enumerações](how-to-declare-enumerations.md)
- [Como fazer referência a um membro de enumeração](how-to-refer-to-an-enumeration-member.md)
- [Enumerações e qualificação de nome](enumerations-and-name-qualification.md)
- [Como iterar em uma enumeração](how-to-iterate-through-an-enumeration.md)
- [Como determinar a cadeia de caracteres associada a um valor de enumeração](how-to-determine-the-string-associated-with-an-enumeration-value.md)
- [Quando usar uma enumeração](when-to-use-an-enumeration.md)

- [Visão geral de enumerações](enumerations-overview.md)
- [Visão geral de constantes](constants-overview.md)
- [Como declarar uma enumeração](how-to-declare-enumerations.md)
- [Enumerações e qualificação de nome](enumerations-and-name-qualification.md)
- [Instrução Option Strict](../../../language-reference/statements/option-strict-statement.md)
- [Constantes e enumerações](../../../language-reference/constants-and-enumerations.md)
