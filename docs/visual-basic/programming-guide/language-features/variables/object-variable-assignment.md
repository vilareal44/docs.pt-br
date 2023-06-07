---
title: Atribuição de variável do objeto
ms.date: 07/20/2015
helpviewer_keywords:
- Nothing keyword [Visual Basic], object variable assignment
- object variables [Visual Basic], initializing
- variables [Visual Basic], initializing
- objects [Visual Basic], current instance
- object variables [Visual Basic], assigning
- variables [Visual Basic], object variables
- current instance [Visual Basic], defined
- variables [Visual Basic], assigning
- assignment statements [Visual Basic], object variable assignment
- Me keyword [Visual Basic], as object variable
ms.assetid: 3706811d-fd40-44fe-8727-d692e8e55d6d
ms.openlocfilehash: 9ae1a307e8c886166d516140b7f100a411cedcfa
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410368"
---
# <a name="object-variable-assignment-visual-basic"></a>Atribuição de variável do objeto (Visual Basic)

Você usa uma instrução de atribuição normal para atribuir um objeto a uma variável de objeto. Você pode atribuir uma expressão de objeto ou a palavra-chave [Nothing](../../../language-reference/nothing.md) , como ilustra o exemplo a seguir.

```vb
Dim thisObject As Object
' The following statement assigns an object reference.
thisObject = Form1
' The following statement discontinues association with any object.
thisObject = Nothing
```

`Nothing`significa que não há nenhum objeto atribuído atualmente à variável.

## <a name="initialization"></a>Inicialização

Quando o código começar a ser executado, suas variáveis de objeto serão inicializadas para `Nothing` . Aquelas cujas declarações incluem inicialização são reinicializadas para os valores que você especifica quando as instruções de declaração são executadas.

Você pode incluir a inicialização em sua declaração usando a [nova](../../../language-reference/operators/new-operator.md) palavra-chave. As instruções de declaração a seguir declaram variáveis `testUri` de objeto e `ver` atribuem objetos específicos a elas. Cada um deles usa um dos construtores sobrecarregados da classe apropriada para inicializar o objeto.

```vb
Dim testUri As New System.Uri("https://www.microsoft.com")
Dim ver As New System.Version(6, 1, 0)
```

## <a name="disassociation"></a>Dissociação

Definir uma variável de objeto para `Nothing` interromper a associação da variável com qualquer objeto específico. Isso impede que você altere acidentalmente o objeto alterando a variável. Ele também permite que você teste se a variável de objeto aponta para um objeto válido, como mostra o exemplo a seguir.

```vb
If otherObject IsNot Nothing Then
    ' otherObject refers to a valid object, so your code can use it.
End If
```

Se o objeto ao qual sua variável se refere estiver em outro aplicativo, esse teste não poderá determinar se o aplicativo foi encerrado ou simplesmente invalidará o objeto.

Uma variável de objeto com um valor de `Nothing` também é chamada de *referência nula*.

## <a name="current-instance"></a>Instância atual

A *instância atual* de um objeto é aquela em que o código está sendo executado no momento. Como todo o código é executado dentro de um procedimento, a instância atual é aquela em que o procedimento foi invocado.

A `Me` palavra-chave age como uma variável de objeto que faz referência à instância atual. Se um procedimento não for [compartilhado](../../../language-reference/modifiers/shared.md), ele poderá usar a `Me` palavra-chave para obter um ponteiro para a instância atual. Procedimentos compartilhados não podem ser associados a uma instância específica de uma classe.

O uso do `Me` é particularmente útil para passar a instância atual para um procedimento em outro módulo. Por exemplo, suponha que você tenha um número de documentos XML e queira adicionar texto padrão a todos eles. O exemplo a seguir define um procedimento para fazer isso.

```vb
Sub addStandardText(XmlDoc As System.Xml.XmlDocument)
    XmlDoc.CreateTextNode("This text goes into every XML document.")
End Sub
```

Cada objeto de documento XML poderia então chamar o procedimento e passar sua instância atual como um argumento. O exemplo a seguir demonstra isso.

```vb
addStandardText(Me)
```

## <a name="see-also"></a>Confira também

- [Variáveis de Objeto](object-variables.md)
- [Declaração de Variável do Objeto](object-variable-declaration.md)
- [Valores de Variável de Objeto](object-variable-values.md)
- [Como declarar uma variável de objeto e atribuir um objeto a ela no Visual Basic](how-to-declare-an-object-variable-and-assign-an-object-to-it.md)
- [Como fazer uma variável de objeto não se referir a nenhuma instância](how-to-make-an-object-variable-not-refer-to-any-instance.md)
- [Me, My, MyBase e MyClass](../../program-structure/me-my-mybase-and-myclass.md)
