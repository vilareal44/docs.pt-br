---
title: Out (modificador genérico)
ms.date: 07/20/2015
f1_keywords:
- vb.VarianceOut
helpviewer_keywords:
- Out keyword [Visual Basic]
- covariance, Out keyword [Visual Basic]
ms.assetid: c4418369-1518-4a46-9a1e-054c61038eca
ms.openlocfilehash: 28ae7d6fd51170aa822072fc2f5357443f51a353
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84392087"
---
# <a name="out-generic-modifier-visual-basic"></a>Out (modificador genérico) (Visual Basic)

Para parâmetros de tipo genérico, a `Out` palavra-chave especifica que o tipo é covariant.

## <a name="remarks"></a>Comentários

A covariância permite que você use um tipo mais derivado do que aquele especificado pelo parâmetro genérico. Isso permite a conversão implícita de classes que implementam interfaces variantes e a conversão implícita de tipos delegados.

Para obter mais informações, consulte [Covariância e contravariância](../../programming-guide/concepts/covariance-contravariance/index.md).

## <a name="rules"></a>Regras

Você pode usar a palavra-chave `Out` em delegados e interfaces genéricas.

Em uma interface genérica, um parâmetro de tipo pode ser declarado covariante se ele satisfizer as condições a seguir:

- O parâmetro de tipo é usado apenas como um tipo de retorno dos métodos de interface e não é usado como um tipo de argumentos de método.

    > [!NOTE]
    > Há uma exceção a essa regra. Se, em uma interface covariante, você tiver um delegado genérico contravariante como um parâmetro de método, você poderá usar o tipo covariante como um parâmetro de tipo genérico para o delegado. Para obter mais informações sobre delegados genéricos covariantes e contravariantes, consulte [Variância em delegados](../../programming-guide/concepts/covariance-contravariance/variance-in-delegates.md) e [Usando variância para delegados genéricos Func e Action](../../programming-guide/concepts/covariance-contravariance/using-variance-for-func-and-action-generic-delegates.md).

- O parâmetro de tipo não é usado como uma restrição genérica para os métodos de interface.

Em um delegado genérico, um parâmetro de tipo pode ser declarado como covariant se for usado apenas como um tipo de retorno de método e não usado para argumentos de método.

A covariância e a contravariância têm suporte para tipos de referência, mas não para tipos de valor.

No Visual Basic, você não pode declarar eventos em interfaces covariantes sem especificar o tipo delegado. Além disso, as interfaces covariantes não podem ter classes aninhadas, enums ou estruturas, mas podem ter interfaces aninhadas.

## <a name="behavior"></a>Comportamento

Uma interface que tem um parâmetro de tipo covariante permite que seus métodos retornem tipos mais derivados do que aqueles especificados pelo parâmetro de tipo. Por exemplo, já que no .NET Framework 4, em <xref:System.Collections.Generic.IEnumerable%601>, o tipo T é covariante, você pode atribuir um objeto do tipo `IEnumerable(Of String)` a um objeto do tipo `IEnumerable(Of Object)` sem usar nenhum método de conversão especial.

Pode ser atribuído a um delegado covariante outro delegado do mesmo tipo, mas com um parâmetro de tipo genérico mais derivado.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como declarar, estender e implementar uma interface genérica covariante. Ele também mostra como usar a conversão implícita para classes que implementam uma interface covariante.

[!code-vb[vbVarianceKeywords#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvariancekeywords/vb/module1.vb#3)]

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como declarar, instanciar e invocar um delegado genérico covariante. Ele também mostra como você pode usar a conversão implícita para tipos delegados.

[!code-vb[vbVarianceKeywords#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvariancekeywords/vb/module1.vb#4)]

## <a name="see-also"></a>Confira também

- [Variação em interfaces genéricas](../../programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces.md)
- [Em](in-generic-modifier.md)
