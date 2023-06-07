---
title: Procedimentos do operador
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- procedures [Visual Basic], operator
- Visual Basic code, operators
- syntax [Visual Basic], Operator procedures
- operators [Visual Basic], overloading
- overloaded operators [Visual Basic]
- operator overloading
- operator procedures
ms.assetid: 8c513d38-246b-4fb7-8b75-29e1364e555b
ms.openlocfilehash: a1dd183570c8aa50efff85bdaebef90bd3b0120f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84364312"
---
# <a name="operator-procedures-visual-basic"></a>Procedimentos do operador (Visual Basic)

Um procedimento de operador é uma série de instruções Visual Basic que definem o comportamento de um operador padrão (como `*` , `<>` ou `And` ) em uma classe ou estrutura que você definiu. Isso também é chamado de *sobrecarga de operador*.

## <a name="when-to-define-operator-procedures"></a>Quando definir procedimentos de operador

Quando você tiver definido uma classe ou estrutura, poderá declarar variáveis como sendo do tipo dessa classe ou estrutura. Às vezes, essa variável precisa participar de uma operação como parte de uma expressão. Para fazer isso, ele deve ser um operando de um operador.

Visual Basic define operadores somente em seus tipos de dados fundamentais. Você pode definir o comportamento de um operador quando um ou ambos os operandos forem do tipo de sua classe ou estrutura.

Para obter mais informações, consulte [instrução Operator](../../../language-reference/statements/operator-statement.md).

## <a name="types-of-operator-procedure"></a>Tipos de procedimento de operador

Um procedimento de operador pode ser um dos seguintes tipos:

- Uma definição de um operador unário em que o argumento é do tipo de sua classe ou estrutura.

- Uma definição de um operador binário em que pelo menos um dos argumentos é do tipo de sua classe ou estrutura.

- Uma definição de um operador de conversão em que o argumento é do tipo de sua classe ou estrutura.

- Uma definição de um operador de conversão que retorna o tipo de sua classe ou estrutura.

 Os operadores de conversão são sempre unários e você sempre usa `CType` como o operador que está definindo.

## <a name="declaration-syntax"></a>Sintaxe da Declaração

A sintaxe para declarar um procedimento de operador é a seguinte:

```vb
Public Shared [Widening | Narrowing] Operator operatorsymbol ( operand1 [,  operand2 ]) As datatype

' Statements of the operator procedure.

End Operator
```

Você usa a `Widening` `Narrowing` palavra-chave ou somente em um operador de conversão de tipo. O símbolo de operador é sempre a [função CType](../../../language-reference/functions/ctype-function.md) para um operador de conversão de tipo.

Você declara dois operandos para definir um operador binário e declara um operando para definir um operador unário, incluindo um operador de conversão de tipo. Todos os operandos devem ser declarados `ByVal` .

Você declara cada operando da mesma maneira que declara parâmetros para [procedimentos sub](./sub-procedures.md).

### <a name="data-type"></a>Tipo de Dados

Como você está definindo um operador em uma classe ou estrutura que você definiu, pelo menos um dos operandos deve ser do tipo de dados dessa classe ou estrutura. Para um operador de conversão de tipo, o operando ou o tipo de retorno deve ser do tipo de dados da classe ou estrutura.

Para obter mais detalhes, consulte a [instrução Operator](../../../language-reference/statements/operator-statement.md).

## <a name="calling-syntax"></a>Sintaxe de chamada

Você invoca um procedimento de operador implicitamente usando o símbolo do operador em uma expressão. Você fornece os operandos da mesma maneira que faz para operadores predefinidos.

A sintaxe de uma chamada implícita para um procedimento de operador é a seguinte:

`Dim testStruct As`  *structurename*

`Dim testNewStruct As`  *estruturaname* `= testStruct` *operatorsymbol*      `10`

### <a name="illustration-of-declaration-and-call"></a>Ilustração de declaração e chamada

A estrutura a seguir armazena um valor inteiro de 128 bits assinado como as partes de ordem superior e de ordem inferior do constituinte. Ele define o `+` operador para adicionar dois `veryLong` valores e gerar um `veryLong` valor resultante.

[!code-vb[VbVbcnProcedures#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#23)]

O exemplo a seguir mostra uma chamada típica para o `+` operador definido em `veryLong` .

[!code-vb[VbVbcnProcedures#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#24)]

## <a name="see-also"></a>Confira também

- [Procedimentos](./index.md)
- [Subprocedimentos](./sub-procedures.md)
- [Procedimentos de função](./function-procedures.md)
- [Procedimentos de propriedade](./property-procedures.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Instrução Operator](../../../language-reference/statements/operator-statement.md)
- [Como definir um operador](./how-to-define-an-operator.md)
- [Como definir um operador de conversão](./how-to-define-a-conversion-operator.md)
- [Como chamar um procedimento de operador](./how-to-call-an-operator-procedure.md)
- [Como usar uma classe que define operadores](./how-to-use-a-class-that-defines-operators.md)
