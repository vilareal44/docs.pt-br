---
title: O tipo de '<variablename>' não pode ser inferido porque os limites de loop e a variável step não são ampliados com o mesmo tipo
ms.date: 07/20/2015
f1_keywords:
- bc30982
- vbc30982
helpviewer_keywords:
- BC30982
ms.assetid: 741e85d9-a747-42ad-a1e1-a3f1928aaff5
ms.openlocfilehash: 74b690ce3dee87e481c629a254e629be4b40f8cd
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84387004"
---
# <a name="type-of-variablename-cannot-be-inferred-because-the-loop-bounds-and-the-step-variable-do-not-widen-to-the-same-type"></a>O tipo de '\<variablename>' não pode ser inferido porque os limites de loop e a variável step não são ampliados com o mesmo tipo

Você escreveu um `For...Next` loop no qual o compilador não pode inferir um tipo de dados para a variável de controle de loop porque as seguintes condições são verdadeiras:

- O tipo de dados da variável de controle de loop não é especificado com uma `As` cláusula.

- Os limites do loop e a variável Step contêm pelo menos dois tipos de dados.

- Não existem conversões padrão entre os tipos de dados.

 Portanto, o compilador não pode inferir o tipo de dados da variável de controle de um loop.

 No exemplo a seguir, a variável Step é um caractere e os limites do loop são inteiros. Como não há conversão padrão entre caracteres e inteiros, esse erro é relatado.

```vb
Dim stepVar = "1"c
Dim m = 0
Dim n = 20

' Not valid.
' For i = 1 To 10 Step stepVar
    ' Loop processing
' Next
```

**ID do erro:** BC30982

## <a name="to-correct-this-error"></a>Para corrigir este erro

- Altere os tipos dos limites do loop e da variável Step conforme necessário para que pelo menos um deles seja um tipo que os outros ampliam. No exemplo anterior, altere o tipo de `stepVar` para `Integer` .

  ```vb
  Dim stepVar = 1
  ```

  -ou-

  ```vb
  Dim stepVar As Integer = 1
  ```

- Use funções de conversão explícitas para converter os limites do loop e a variável Step para os tipos apropriados. No exemplo anterior, aplique a `Val` função a `stepVar` .

  ```vb
  For i = 1 To 10 Step Val(stepVar)
      ' Loop processing
  Next
  ```

## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Conversion.Val%2A>
- [Instrução For...Next](../statements/for-next-statement.md)
- [Conversões implícitas e explícitas](../../programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
- [Inferência de Tipo de Variável Local](../../programming-guide/language-features/variables/local-type-inference.md)
- [Instrução Option Infer](../statements/option-infer-statement.md)
- [Funções de conversão do tipo](../functions/type-conversion-functions.md)
- [Conversões de Widening e Narrowing](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
