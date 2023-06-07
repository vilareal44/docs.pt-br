---
title: Expressão é um valor e, por isso, não pode ser o destino de uma atribuição
ms.date: 07/20/2015
f1_keywords:
- bc30068
- vbc30068
helpviewer_keywords:
- BC30068
ms.assetid: d65141e1-f31e-4ac5-a3b8-0b2e02a71ebf
ms.openlocfilehash: 9e4dbaf2f2800454c673cd58ddec4cf0f6e5c6b9
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409501"
---
# <a name="expression-is-a-value-and-therefore-cannot-be-the-target-of-an-assignment"></a>Expressão é um valor e, por isso, não pode ser o destino de uma atribuição

Uma instrução tenta atribuir um valor a uma expressão. Você pode atribuir um valor somente a uma variável, propriedade ou elemento de matriz gravável em tempo de execução. O exemplo a seguir ilustra como esse erro pode ocorrer.

```vb
Dim yesterday As Integer
ReadOnly maximum As Integer = 45
yesterday + 1 = DatePart(DateInterval.Day, Now)
' The preceding line is an ERROR because of an expression on the left.
maximum = 50
' The preceding line is an ERROR because maximum is declared ReadOnly.
```

Exemplos semelhantes podem ser aplicados a propriedades e elementos de matriz.

**Acesso indireto.** O acesso indireto por meio de um tipo de valor também pode gerar esse erro. Considere o exemplo de código a seguir, que tenta definir o valor de <xref:System.Drawing.Point> acessando-o indiretamente por meio de <xref:System.Windows.Forms.Control.Location%2A> .

```vb
' Assume this code runs inside Form1.
Dim exitButton As New System.Windows.Forms.Button()
exitButton.Text = "Exit this form"
exitButton.Location.X = 140
' The preceding line is an ERROR because of no storage for Location.
```

A última instrução do exemplo anterior falha porque ela cria apenas uma alocação temporária para a <xref:System.Drawing.Point> estrutura retornada pela <xref:System.Windows.Forms.Control.Location%2A> propriedade. Uma estrutura é um tipo de valor e a estrutura temporária não é mantida após a execução da instrução. O problema é resolvido declarando e usando uma variável para <xref:System.Windows.Forms.Control.Location%2A> , que cria uma alocação mais permanente para a <xref:System.Drawing.Point> estrutura. O exemplo a seguir mostra o código que pode substituir a última instrução do exemplo anterior.

```vb
Dim exitLocation as New System.Drawing.Point(140, exitButton.Location.Y)
exitButton.Location = exitLocation
```

**ID do erro:** BC30068

## <a name="to-correct-this-error"></a>Para corrigir este erro

- Se a instrução atribuir um valor a uma expressão, substitua a expressão por uma única variável, propriedade ou elemento de matriz gravável.

- Se a instrução fizer acesso indireto por meio de um tipo de valor (geralmente uma estrutura), crie uma variável para conter o tipo de valor.

- Atribua a estrutura apropriada (ou outro tipo de valor) à variável.

- Use a variável para acessar a propriedade para atribuir um valor a ela.

## <a name="see-also"></a>Confira também

- [Operadores e expressões](../../programming-guide/language-features/operators-and-expressions/index.md)
- [Instruções](../../programming-guide/language-features/statements.md)
- [Solucionando problemas de procedimentos](../../programming-guide/language-features/procedures/troubleshooting-procedures.md)
