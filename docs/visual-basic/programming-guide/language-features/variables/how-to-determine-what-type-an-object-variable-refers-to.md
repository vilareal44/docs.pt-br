---
title: Como determinar a que tipo uma variável de objeto se refere
ms.date: 07/20/2015
helpviewer_keywords:
- TypeOf operator [Visual Basic], determining object variable type
- variables [Visual Basic], object
- object variables [Visual Basic], determining type
ms.assetid: 6f6a138d-58a4-40d1-9f4e-0a3c598eaf81
ms.openlocfilehash: d3224000f5958a8619e38c4d2f6dc5dbb275ad45
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410484"
---
# <a name="how-to-determine-what-type-an-object-variable-refers-to-visual-basic"></a>Como determinar a que tipo uma variável de objeto se refere (Visual Basic)

Uma variável de objeto contém um ponteiro para dados que são armazenados em outro lugar. O tipo desses dados pode ser alterado durante o tempo de execução. A qualquer momento, você pode usar o <xref:System.Type.GetTypeCode%2A> método para determinar o tipo de tempo de execução atual ou o [operador typeof](../../../language-reference/operators/typeof-operator.md) para descobrir se o tipo de tempo de execução atual é compatível com um tipo especificado.

### <a name="to-determine-the-exact-type-an-object-variable-currently-refers-to"></a>Para determinar o tipo exato que uma variável de objeto atualmente faz referência

1. Na variável de objeto, chame o <xref:System.Object.GetType%2A> método para recuperar um <xref:System.Type?displayProperty=nameWithType> objeto.

    ```vb
    Dim myObject As Object
    myObject.GetType()
    ```

2. Na <xref:System.Type?displayProperty=nameWithType> classe, chame o método compartilhado <xref:System.Type.GetTypeCode%2A> para recuperar o <xref:System.TypeCode> valor de enumeração para o tipo do objeto.

    ```vb
    Dim myObject As Object
    Dim datTyp As Integer = Type.GetTypeCode(myObject.GetType())
    MsgBox("myObject currently has type code " & CStr(datTyp))
    ```

    Você pode testar o <xref:System.TypeCode> valor de enumeração em relação a qualquer membro de enumeração que seja de interesse, como `Double` .

### <a name="to-determine-whether-an-object-variables-type-is-compatible-with-a-specified-type"></a>Para determinar se o tipo de uma variável de objeto é compatível com um tipo especificado

- Use o `TypeOf` operador em combinação com o [operador is](../../../language-reference/operators/is-operator.md) para testar o objeto com uma `TypeOf` expressão.... `Is`

    ```vb
    If TypeOf objA Is System.Windows.Forms.Control Then
        MsgBox("objA is compatible with the Control class")
    End If
    ```

    A `TypeOf` expressão... `Is` retorna `True` se o tipo de tempo de execução do objeto for compatível com o tipo especificado.

    O critério de compatibilidade depende se o tipo especificado é uma classe, estrutura ou interface. Em geral, os tipos são compatíveis se o objeto for do mesmo tipo que, herda de ou implementa o tipo especificado. Para obter mais informações, consulte [operador typeof](../../../language-reference/operators/typeof-operator.md).

## <a name="compile-the-code"></a>Compilar o código

Observe que o tipo especificado não pode ser uma variável ou expressão. Ele deve ser o nome de um tipo definido, como uma classe, estrutura ou interface. Isso inclui tipos intrínsecos, como `Integer` e `String` .

## <a name="see-also"></a>Confira também

- <xref:System.Object.GetType%2A>
- <xref:System.Type?displayProperty=nameWithType>
- <xref:System.Type.GetTypeCode%2A>
- <xref:System.TypeCode>
- [Variáveis de Objeto](object-variables.md)
- [Valores de Variável de Objeto](object-variable-values.md)
- [Tipo de dados Object](../../../language-reference/data-types/object-data-type.md)
