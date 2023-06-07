---
title: Procedimentos de propriedade
ms.date: 07/20/2015
helpviewer_keywords:
- Set statement [Visual Basic], Property procedures
- Visual Basic code, procedures
- return values [Visual Basic], Property procedures
- syntax [Visual Basic], Property procedures
- procedures [Visual Basic], property
- Visual Basic code, properties
- procedures [Visual Basic], calling
- properties [Visual Basic], custom
- property procedures
- Get statement [Visual Basic], property procedures
ms.assetid: 46a98379-e1a2-45dd-a48c-b51213f5ab07
ms.openlocfilehash: cb5b0e12512e476b7c96bbfb19f8e4f470f6b498
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84363727"
---
# <a name="property-procedures-visual-basic"></a>Procedimentos de propriedade (Visual Basic)

Um procedimento de propriedade é uma série de instruções Visual Basic que manipulam uma propriedade personalizada em um módulo, classe ou estrutura. Os procedimentos de propriedade também são conhecidos como *acessadores de propriedade*.

O Visual Basic fornece os seguintes procedimentos de propriedade:

- Um `Get` procedimento retorna o valor de uma propriedade. Ele é chamado quando você acessa a propriedade em uma expressão.
- Um `Set` procedimento define uma propriedade para um valor, incluindo uma referência de objeto. Ele é chamado quando você atribui um valor à propriedade.

Normalmente, você define procedimentos de propriedade em pares, usando as `Get` `Set` instruções e, mas você pode definir um dos dois procedimentos sozinho se a propriedade for somente leitura ([instrução Get](../../../language-reference/statements/get-statement.md)) ou somente gravação ([instrução SET](../../../language-reference/statements/set-statement.md)).

Você pode omitir `Get` o `Set` procedimento e ao usar uma propriedade implementada automaticamente. Para obter mais informações, consulte [Propriedades autoimplementadas](./auto-implemented-properties.md).

Você pode definir propriedades em classes, estruturas e módulos. As propriedades são `Public` por padrão, o que significa que você pode chamá-las de qualquer lugar em seu aplicativo que possa acessar o contêiner da propriedade.

Para obter uma comparação de propriedades e variáveis, consulte [diferenças entre propriedades e variáveis no Visual Basic](differences-between-properties-and-variables.md).

## <a name="declaration-syntax"></a>Sintaxe de declaração

Uma propriedade em si é definida por um bloco de código incluído na [instrução de propriedade](../../../language-reference/statements/property-statement.md) e na `End Property` instrução. Dentro desse bloco, cada procedimento de propriedade aparece como um bloco interno entre uma instrução de declaração ( `Get` ou `Set` ) e a `End` declaração de correspondência.

A sintaxe para declarar uma propriedade e seus procedimentos é a seguinte:

```vb
[Default] [Modifiers] Property PropertyName[(ParameterList)] [As DataType]
    [AccessLevel] Get
        ' Statements of the Get procedure.
        ' The following statement returns an expression as the property's value.
        Return Expression
    End Get
    [AccessLevel] Set[(ByVal NewValue As DataType)]
        ' Statements of the Set procedure.
        ' The following statement assigns newvalue as the property's value.
        LValue = NewValue
    End Set
End Property
' - or -
[Default] [Modifiers] Property PropertyName [(ParameterList)] [As DataType]
```

O `Modifiers` pode especificar o nível de acesso e as informações sobre sobrecarga, substituição, compartilhamento e sombreamento, bem como se a propriedade é somente leitura ou somente gravação. O `AccessLevel` no `Get` procedimento ou `Set` pode ser qualquer nível mais restritivo do que o nível de acesso especificado para a propriedade em si. Para obter mais informações, consulte [instrução de propriedade](../../../language-reference/statements/property-statement.md).

### <a name="data-type"></a>Tipo de Dados

O tipo de dados de uma propriedade e o nível de acesso de entidade de segurança são definidos na `Property` instrução, não nos procedimentos de propriedade. Uma propriedade pode ter apenas um tipo de dados. Por exemplo, você não pode definir uma propriedade para armazenar um `Decimal` valor, mas recuperar um `Double` valor.

### <a name="access-level"></a>Nível de acesso

No entanto, você pode definir um nível de acesso de entidade para uma propriedade e restringir ainda mais o nível de acesso em um de seus procedimentos de propriedade. Por exemplo, você pode definir uma `Public` propriedade e, em seguida, definir um `Private Set` procedimento. O `Get` procedimento permanece `Public` . Você pode alterar o nível de acesso em apenas um dos procedimentos de uma propriedade e só pode torná-lo mais restritivo do que o nível de acesso de entidade de segurança. Para obter mais informações, consulte [como: declarar uma propriedade com níveis de acesso mistos](how-to-declare-a-property-with-mixed-access-levels.md).

## <a name="parameter-declaration"></a>Declaração de parâmetro

Você declara cada parâmetro da mesma maneira que faz para [procedimentos sub](sub-procedures.md), exceto que o mecanismo de passagem deve ser `ByVal` .

A sintaxe para cada parâmetro na lista de parâmetros é a seguinte:

```vb
[Optional] ByVal [ParamArray] parametername As datatype
```

Se o parâmetro for opcional, você também deverá fornecer um valor padrão como parte de sua declaração. A sintaxe para especificar um valor padrão é a seguinte:

```vb
Optional ByVal parametername As datatype = defaultvalue
```

## <a name="property-value"></a>Valor da propriedade

Em um `Get` procedimento, o valor de retorno é fornecido para a expressão de chamada como o valor da propriedade.

Em um `Set` procedimento, o novo valor da propriedade é passado para o parâmetro da `Set` instrução. Se você declarar explicitamente um parâmetro, deverá declará-lo com o mesmo tipo de dados que a propriedade. Se você não declarar um parâmetro, o compilador usará o parâmetro implícito `Value` para representar o novo valor a ser atribuído à propriedade.

## <a name="calling-syntax"></a>Sintaxe de chamada

Você invoca um procedimento de propriedade implicitamente fazendo referência à propriedade. Você usa o nome da propriedade da mesma maneira que usaria o nome de uma variável, exceto que você deve fornecer valores para todos os argumentos que não são opcionais, e você deve colocar a lista de argumentos entre parênteses. Se nenhum argumento for fornecido, você pode opcionalmente omitir os parênteses.

A sintaxe de uma chamada implícita para um `Set` procedimento é a seguinte:

```vb
propertyname[(argumentlist)] = expression
```

A sintaxe de uma chamada implícita para um `Get` procedimento é a seguinte:

```vb
lvalue = propertyname[(argumentlist)]
Do While (propertyname[(argumentlist)] > expression)
```

### <a name="illustration-of-declaration-and-call"></a>Ilustração de declaração e chamada

A propriedade a seguir armazena um nome completo como dois nomes de constituintes, o primeiro nome e o sobrenome. Quando o código de chamada lê `fullName` , o `Get` procedimento combina os dois nomes constituintes e retorna o nome completo. Quando o código de chamada atribui um novo nome completo, o `Set` procedimento tenta dividi-lo em dois nomes constituintes. Se não encontrar um espaço, ele o armazenará como o primeiro nome.

[!code-vb[VbVbcnProcedures#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#8)]

O exemplo a seguir mostra as chamadas típicas para os procedimentos de propriedade de `fullName` :

[!code-vb[VbVbcnProcedures#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#9)]

## <a name="see-also"></a>Confira também

- [Procedimentos](index.md)
- [Procedimentos de função](function-procedures.md)
- [Procedimentos do operador](operator-procedures.md)
- [Parâmetros e Argumentos de Procedimento](procedure-parameters-and-arguments.md)
- [Diferenças entre propriedades e variáveis no Visual Basic](differences-between-properties-and-variables.md)
- [Como criar uma propriedade](how-to-create-a-property.md)
- [Como chamar um procedimento de propriedade](how-to-call-a-property-procedure.md)
- [Como declarar e chamar uma propriedade padrão no Visual Basic](how-to-declare-and-call-a-default-property.md)
- [Como inserir um valor em uma propriedade](how-to-put-a-value-in-a-property.md)
- [Como obter um valor a partir de uma propriedade](how-to-get-a-value-from-a-property.md)
