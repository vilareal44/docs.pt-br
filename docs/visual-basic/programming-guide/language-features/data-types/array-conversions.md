---
title: Conversões de matriz
ms.date: 07/20/2015
helpviewer_keywords:
- arrays [Visual Basic], converting type
- type conversion [Visual Basic], arrays
- conversions [Visual Basic], type
- arrays [Visual Basic], data types
- conversions [Visual Basic], data type
- object arrays [Visual Basic], converting type
- data type conversion [Visual Basic], array conversions
- conversions [Visual Basic], array types
- object arrays
ms.assetid: fceff7d2-a1b7-44c7-b9aa-8bd831d8a444
ms.openlocfilehash: 1d20b01200d3f967e3355dc6e9651291003d140e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401999"
---
# <a name="array-conversions-visual-basic"></a>Conversões de matriz (Visual Basic)
Você pode converter um tipo de matriz em um tipo de matriz diferente, desde que atenda às seguintes condições:  
  
- **Classificação igual.** As classificações das duas matrizes devem ser as mesmas, ou seja, devem ter o mesmo número de dimensões. No entanto, os comprimentos das respectivas dimensões não precisam ser os mesmos.  
  
- **Tipo de dados do elemento.** Os tipos de dados dos elementos de ambas as matrizes devem ser tipos de referência. Não é possível converter uma `Integer` matriz em uma `Long` matriz ou até mesmo em uma `Object` matriz, porque pelo menos um tipo de valor está envolvido. Para obter mais informações, consulte [tipos de valor e tipos de referência](value-types-and-reference-types.md).  
  
- **Convertibilidade.** Uma conversão, que pode ser ampliada ou estreita, deve ser possível entre os tipos de elemento das duas matrizes. Um exemplo que falha esse requisito é uma tentativa de conversão entre uma `String` matriz e uma matriz de uma classe derivada de <xref:System.Attribute?displayProperty=nameWithType> . Esses dois tipos não têm nada em comum, e não existe nenhuma conversão de nenhum tipo entre eles.  
  
 Uma conversão de um tipo de matriz para outro está ampliando ou estreitando dependendo se a conversão dos respectivos elementos está ampliando ou diminuindo. Para obter mais informações, consulte [Ampliando e restringindo conversões](widening-and-narrowing-conversions.md).  
  
## <a name="conversion-to-an-object-array"></a>Conversão para uma matriz de objeto  
 Quando você declara uma `Object` matriz sem inicializá-la, seu tipo de elemento é `Object` desde que permaneça não inicializado. Quando você o define como uma matriz de uma classe específica, ela assume o tipo dessa classe. No entanto, seu tipo subjacente ainda é `Object` , e você pode defini-lo posteriormente como outra matriz de uma classe não relacionada. Como todas as classes derivam de `Object` , você pode alterar o tipo de elemento da matriz de qualquer classe para qualquer outra classe.  
  
 No exemplo a seguir, não existe nenhuma conversão entre os tipos `student` e `String` , mas ambos derivam de `Object` , portanto, todas as atribuições são válidas.  
  
```vb  
' Assume student has already been defined as a class.  
Dim testArray() As Object  
' testArray is still an Object array at this point.  
Dim names() As String = New String(3) {"Name0", "Name1", "Name2", "Name3"}  
testArray = New student(3) {}  
' testArray is now of type student().  
testArray = names  
' testArray is now a String array.  
```  
  
### <a name="underlying-type-of-an-array"></a>Tipo subjacente de uma matriz  
 Se você declarar originalmente uma matriz com uma classe específica, seu tipo de elemento subjacente será essa classe. Se você defini-lo subsequentemente como uma matriz de outra classe, deverá haver uma conversão entre as duas classes.  
  
 No exemplo a seguir, `students` é uma `student` matriz. Como não existe nenhuma conversão entre `String` e `student` , a última instrução falhará.  
  
```vb  
Dim students() As student  
Dim names() As String = New String(3) {"Name0", "Name1", "Name2", "Name3"}  
students = New Student(3) {}  
' The following statement fails at compile time.  
students = names  
```  
  
## <a name="see-also"></a>Confira também

- [Tipos de dados](index.md)
- [Conversões de tipo no Visual Basic](type-conversions.md)
- [Conversões implícitas e explícitas](implicit-and-explicit-conversions.md)
- [Conversões entre cadeias de caracteres e outros tipos](conversions-between-strings-and-other-types.md)
- [Como converter um objeto em outro tipo no Visual Basic](how-to-convert-an-object-to-another-type.md)
- [Tipos de dados](../../../language-reference/data-types/index.md)
- [Funções de conversão do tipo](../../../language-reference/functions/type-conversion-functions.md)
- [Matrizes](../arrays/index.md)
