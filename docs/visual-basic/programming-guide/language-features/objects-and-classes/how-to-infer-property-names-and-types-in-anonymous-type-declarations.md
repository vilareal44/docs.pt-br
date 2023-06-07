---
title: Como inferir nomes e tipos de propriedade na declaração de tipo anônimo
ms.date: 07/20/2015
helpviewer_keywords:
- inferring property names [Visual Basic]
- anonymous types [Visual Basic], inferring property names and types
- inferring property types [Visual Basic]
ms.assetid: 7c748b22-913f-4d9d-b747-6b7bf296a0bc
ms.openlocfilehash: 9ebbbe9d2e6d36f5ab2bc7f7c916d18c9240a06d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404882"
---
# <a name="how-to-infer-property-names-and-types-in-anonymous-type-declarations-visual-basic"></a>Como inferir nomes e tipos de propriedade em declarações de tipo anônimo (Visual Basic)

Tipos anônimos não fornecem nenhum mecanismo para especificar diretamente os tipos de dados de propriedades. Tipos de todas as propriedades são inferidos. No exemplo a seguir, os tipos de `Name` e `Price` são inferidos diretamente dos valores que são usados para inicializá-los.

[!code-vb[VbVbalrAnonymousTypes#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#1)]

Tipos anônimos também podem inferir nomes de propriedade e tipos de outras fontes. As seções a seguir fornecem uma lista das circunstâncias em que a inferência é possível e exemplos de situações em que não é.

## <a name="successful-inference"></a>Inferência bem-sucedida

#### <a name="anonymous-types-can-infer-property-names-and-types-from-the-following-sources"></a>Tipos anônimos podem inferir nomes e tipos de propriedade das seguintes fontes:

- De nomes de variáveis. O tipo anônimo `anonProduct` terá duas propriedades, `productName` e `productPrice` . Seus tipos de dados serão aqueles das variáveis originais `String` e `Double` , respectivamente.

  [!code-vb[VbVbalrAnonymousTypes#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#11)]

- Dos nomes de propriedade ou campo de outros objetos. Por exemplo, considere um `car` objeto de um `CarClass` tipo que inclui `Name` `ID` Propriedades e. Para criar uma nova instância de tipo anônimo, `car1` com `Name` `ID` as propriedades e que são inicializadas com os valores do `car` objeto, você pode escrever o seguinte:

  [!code-vb[VbVbalrAnonymousTypes#34](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#34)]

  A declaração anterior é equivalente à linha mais longa de código que define o tipo anônimo `car2` .

  [!code-vb[VbVbalrAnonymousTypes#35](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#35)]

- De nomes de membro XML.

  [!code-vb[VbVbalrAnonymousTypes#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#12)]

  O tipo resultante para `anon` teria uma propriedade, `Book` , do tipo <xref:System.Collections.IEnumerable> (Of XElement).

- De uma função que não tem parâmetros, como `SomeFunction` no exemplo a seguir.

  ```vb
  Dim sc As New SomeClass
  Dim anon1 = New With {Key sc.SomeFunction()}
  ```

  A variável `anon2` no código a seguir é um tipo anônimo que tem uma propriedade, um caractere chamado `First` . Esse código exibirá uma letra "E", a letra retornada pela função <xref:System.Linq.Enumerable.First%2A> .

  [!code-vb[VbVbalrAnonymousTypes#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#13)]

## <a name="inference-failures"></a>Falhas de inferência

#### <a name="name-inference-will-fail-in-many-circumstances-including-the-following"></a>A inferência de nome falhará em muitas circunstâncias, incluindo o seguinte:

- A inferência deriva da invocação de um método, um construtor ou uma propriedade com parâmetros que requer argumentos. A declaração anterior de `anon1` falha se `someFunction` tiver um ou mais argumentos.

  ```vb
  ' Not valid.
  ' Dim anon3 = New With {Key sc.someFunction(someArg)}
  ```

  A atribuição a um novo nome de propriedade resolve o problema.

  ```vb
  ' Valid.
  Dim anon4 = New With {Key .FunResult = sc.someFunction(someArg)}
  ```

- A inferência deriva de uma expressão complexa.

  ```vb
  Dim aString As String = "Act "
  ' Not valid.
  ' Dim label = New With {Key aString & "IV"}
  ```

  O erro pode ser resolvido atribuindo o resultado da expressão a um nome de propriedade.

  [!code-vb[VbVbalrAnonymousTypes#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#14)]

- A inferência de várias propriedades produz duas ou mais propriedades que têm o mesmo nome. Referindo-se às declarações nos exemplos anteriores, você não pode listar ambas `product.Name` e `car1.Name` como propriedades do mesmo tipo anônimo. Isso ocorre porque o identificador inferido para cada um deles seria `Name` .

  ```vb
  ' Not valid.
  ' Dim anon5 = New With {Key product.Name, Key car1.Name}
  ```

  O problema pode ser resolvido atribuindo os valores a nomes de propriedade distintos.

  [!code-vb[VbVbalrAnonymousTypes#36](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#36)]

  Observe que as alterações no caso (alterações entre letras maiúsculas e minúsculas) não tornam os dois nomes distintos.

  ```vb
  Dim price = 0
  ' Not valid, because Price and price are the same name.
  ' Dim anon7 = New With {Key product.Price, Key price}
  ```

- O tipo inicial e o valor de uma propriedade dependem de outra propriedade que ainda não foi estabelecida. Por exemplo, `.IDName = .LastName` não é válido em uma declaração de tipo anônimo, a menos que `.LastName` já esteja inicializado.

  ```vb
  ' Not valid.
  ' Dim anon8 = New With {Key .IDName = .LastName, Key .LastName = "Jones"}
  ```

  Neste exemplo, você pode corrigir o problema revertendo a ordem em que as propriedades são declaradas.

  [!code-vb[VbVbalrAnonymousTypes#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#15)]

- Um nome de Propriedade do tipo anônimo é o mesmo que o nome de um membro de <xref:System.Object> . Por exemplo, a declaração a seguir falha porque `Equals` é um método de <xref:System.Object> .

  ```vb
  ' Not valid.
  ' Dim relationsLabels1 = New With {Key .Equals = "equals", Key .Greater = _
  '                       "greater than", Key .Less = "less than"}
  ```

  Você pode corrigir o problema alterando o nome da propriedade:

  [!code-vb[VbVbalrAnonymousTypes#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#16)]

## <a name="see-also"></a>Confira também

- [Inicializadores de objeto: tipos nomeados e anônimos](object-initializers-named-and-anonymous-types.md)
- [Inferência de Tipo de Variável Local](../variables/local-type-inference.md)
- [Tipos anônimos](anonymous-types.md)
- [Chave](../../../language-reference/modifiers/key.md)
