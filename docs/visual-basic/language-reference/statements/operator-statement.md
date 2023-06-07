---
title: Instrução Operator
ms.date: 07/20/2015
f1_keywords:
- vb.operator
helpviewer_keywords:
- operators [Visual Basic]
- procedures [Visual Basic], operator
- Narrowing keyword [Visual Basic], conversion operators
- Visual Basic code, operators
- Widening keyword [Visual Basic], conversion operators
- syntax [Visual Basic], Operator procedures
- operators [Visual Basic], overloading
- overloaded operators [Visual Basic]
- operator overloading
- operator procedures
- Operator statement [Visual Basic]
- CType function [Visual Basic], Operator statement
ms.assetid: b12ec4af-1ad7-4a17-865b-c5ee96320ae5
ms.openlocfilehash: f9e6ffe5a49715592399321ab471d73826e05d8e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404389"
---
# <a name="operator-statement"></a>Instrução Operator

Declara o símbolo do operador, os operandos e o código que definem um procedimento de operador em uma classe ou estrutura.

## <a name="syntax"></a>Sintaxe

```vb
[ <attrlist> ] Public [ Overloads ] Shared [ Shadows ] [ Widening | Narrowing ]
Operator operatorsymbol ( operand1 [, operand2 ]) [ As [ <attrlist> ] type ]
    [ statements ]
    [ statements ]
    Return returnvalue
    [ statements ]
End Operator
```

## <a name="parts"></a>Partes

`attrlist`  
Opcional. Consulte a [lista de atributos](attribute-list.md).

`Public`  
Obrigatórios. Indica que este procedimento de operador tem acesso [público](../modifiers/public.md) .

`Overloads`  
Opcional. Veja [sobrecargas](../modifiers/overloads.md).

`Shared`  
Obrigatórios. Indica que esse procedimento de operador é um procedimento [compartilhado](../modifiers/shared.md) .

`Shadows`  
Opcional. Consulte [Shadows](../modifiers/shadows.md).

`Widening`  
Necessário para um operador de conversão, a menos que você especifique `Narrowing` . Indica que este procedimento de operador define uma conversão de [ampliação](../modifiers/widening.md) . Consulte "conversões de alargamento e estreitamento" nesta página de ajuda.

`Narrowing`  
Necessário para um operador de conversão, a menos que você especifique `Widening` . Indica que este procedimento de operador define uma conversão de [restrição](../modifiers/narrowing.md) . Consulte "conversões de alargamento e estreitamento" nesta página de ajuda.

`operatorsymbol`  
Obrigatórios. O símbolo ou identificador do operador que este procedimento de operador define.

`operand1`  
Obrigatórios. O nome e o tipo do operando único de um operador unário (incluindo um operador de conversão) ou o operando esquerdo de um operador binário.

`operand2`  
Necessário para operadores binários. O nome e o tipo do operando direito de um operador binário.

`operand1`e `operand2` têm a seguinte sintaxe e partes:

`[ ByVal ] operandname [ As operandtype ]`

|Parte|Descrição|
|----------|-----------------|
|`ByVal`|Opcional, mas o mecanismo de passagem deve ser [ByVal](../modifiers/byval.md).|
|`operandname`|Obrigatórios. Nome da variável que representa esse operando. Consulte [nomes de elementos declarados](../../programming-guide/language-features/declared-elements/declared-element-names.md).|
|`operandtype`|Opcional, a menos que `Option Strict` seja `On` . Tipo de dados deste operando.|

`type`  
Opcional, a menos que `Option Strict` seja `On` . Tipo de dados do valor que o procedimento do operador retorna.

`statements`  
Opcional. Bloco de instruções que o procedimento de operador executa.

`returnvalue`  
Obrigatórios. O valor que o procedimento do operador retorna para o código de chamada.

`End` `Operator`  
Obrigatórios. Encerra a definição deste procedimento de operador.

## <a name="remarks"></a>Comentários

Você pode usar `Operator` somente em uma classe ou estrutura. Isso significa que o *contexto de declaração* para um operador não pode ser um arquivo de origem, namespace, módulo, interface, procedimento ou bloco. Para obter mais informações, consulte [Contextos de declaração e níveis de acesso padrão](declaration-contexts-and-default-access-levels.md).

Todos os operadores devem ser `Public Shared` . Você não pode especificar `ByRef` , `Optional` ou `ParamArray` para qualquer operando.

Você não pode usar o identificador ou símbolo do operador para manter um valor de retorno. Você deve usar a `Return` instrução e deve especificar um valor. Qualquer número de `Return` instruções pode aparecer em qualquer lugar no procedimento.

A definição de um operador dessa maneira é chamada de *sobrecarga de operador*, independentemente de você usar ou não a `Overloads` palavra-chave. A tabela a seguir lista os operadores que você pode definir.

|Tipo|Operadores|
|----------|---------------|
|Unário|`+`, `-`, `IsFalse`, `IsTrue`, `Not`|
|Binário|`+`, `-`, `*`, `/`, `\`, `&`, `^`, `>>`, `<<`, `=`, `<>`, `>`, `>=`, `<`, `<=`, `And`, `Like`, `Mod`, `Or`, `Xor`|
|Conversão (unário)|`CType`|

Observe que o operador `=` na lista binária é o operador de comparação, não o operador de atribuição.

Ao definir `CType` o, você deve especificar `Widening` ou `Narrowing` .

## <a name="matched-pairs"></a>Pares correspondentes

Você deve definir certos operadores como pares correspondentes. Se você definir qualquer um dos dois operadores, também deverá definir o outro. Os pares correspondentes são os seguintes:

- `=` e `<>`

- `>` e `<`

- `>=` e `<=`

- `IsTrue` e `IsFalse`

## <a name="data-type-restrictions"></a>Restrições de tipo de dados

Cada operador que você define deve envolver a classe ou estrutura na qual você o define. Isso significa que a classe ou estrutura deve aparecer como o tipo de dados do seguinte:

- O operando de um operador unário.

- Pelo menos um dos operandos de um operador binário.

- O operando ou o tipo de retorno de um operador de conversão.

 Determinados operadores têm restrições de tipo de dados adicionais, da seguinte maneira:

- Se você definir os `IsTrue` `IsFalse` operadores e, eles deverão retornar o `Boolean` tipo.

- Se você definir os `<<` `>>` operadores e, eles deverão especificar o `Integer` tipo para o `operandtype` de `operand2` .

O tipo de retorno não precisa corresponder ao tipo de um dos operandos. Por exemplo, um operador de comparação como `=` ou `<>` pode retornar `Boolean` mesmo se nenhum operando for `Boolean` .

## <a name="logical-and-bitwise-operators"></a>Operadores lógicos e bit a bit

Os `And` `Or` operadores,, `Not` , e `Xor` podem executar operações lógicas ou bit-a-em Visual Basic. No entanto, se você definir um desses operadores em uma classe ou estrutura, você poderá definir apenas sua operação bit-up.

Você não pode definir o `AndAlso` operador diretamente com uma `Operator` instrução. No entanto, você pode usar `AndAlso` se tiver atendido às seguintes condições:

- Você definiu `And` os mesmos tipos de operando para os quais deseja usar `AndAlso` .

- Sua definição de `And` retorna o mesmo tipo da classe ou estrutura na qual você o definiu.

- Você definiu o `IsFalse` operador na classe ou estrutura na qual você definiu `And` .

Da mesma forma, você pode usar `OrElse` se tiver definido `Or` nos mesmos operandos, com o tipo de retorno da classe ou estrutura, e você tiver definido `IsTrue` na classe ou estrutura.

## <a name="widening-and-narrowing-conversions"></a>Conversões de Widening e Narrowing

Uma *conversão de ampliação* sempre é bem-sucedida em tempo de execução, enquanto uma *conversão de restrição* pode falhar em tempo de execução. Para obter mais informações, consulte [Ampliando e restringindo conversões](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md).

Se você declarar um procedimento de conversão para ser `Widening` , o código do procedimento não deverá gerar nenhuma falha. Isso significa o seguinte:

- Ele sempre deve retornar um valor válido do tipo `type` .

- Ele deve lidar com todas as possíveis exceções e outras condições de erro.

- Ele deve lidar com qualquer erro retornado de qualquer procedimento chamado por ele.

Se houver alguma possibilidade de que um procedimento de conversão não tenha sucesso ou que possa causar uma exceção sem tratamento, você deverá declará-lo como sendo `Narrowing` .

## <a name="example"></a>Exemplo

O exemplo de código a seguir usa a `Operator` instrução para definir o contorno de uma estrutura que inclui procedimentos de operador para os `And` `Or` operadores,, `IsFalse` e `IsTrue` . `And`e `Or` cada um tem dois operandos do tipo `abc` e do tipo de retorno `abc` . `IsFalse`e `IsTrue` cada um tem um único operando do tipo `abc` e do retorno `Boolean` . Essas definições permitem que o código de chamada use `And` ,, `AndAlso` `Or` e `OrElse` com operandos do tipo `abc` .

[!code-vb[VbVbalrStatements#44](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#44)]

## <a name="see-also"></a>Confira também

- [Operador IsFalse](../operators/isfalse-operator.md)
- [Operador IsTrue](../operators/istrue-operator.md)
- [Widening](../modifiers/widening.md)
- [Narrowing](../modifiers/narrowing.md)
- [Conversões de Widening e Narrowing](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [Procedimentos do operador](../../programming-guide/language-features/procedures/operator-procedures.md)
- [Como definir um operador](../../programming-guide/language-features/procedures/how-to-define-an-operator.md)
- [Como definir um operador de conversão](../../programming-guide/language-features/procedures/how-to-define-a-conversion-operator.md)
- [Como chamar um procedimento de operador](../../programming-guide/language-features/procedures/how-to-call-an-operator-procedure.md)
- [Como usar uma classe que define operadores](../../programming-guide/language-features/procedures/how-to-use-a-class-that-defines-operators.md)
