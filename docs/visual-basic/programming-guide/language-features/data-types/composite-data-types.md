---
title: Tipos de dados compostos
ms.date: 04/25/2017
helpviewer_keywords:
- classes [Visual Basic], composite data types
- composite types [Visual Basic]
- composite data types [Visual Basic]
- data types [Visual Basic], composite
- arrays [Visual Basic], composite data types
- structures [Visual Basic], composite data types
- classes [Visual Basic], composite types
- types [Visual Basic], composite
ms.assetid: 62970f2e-52c0-4369-8963-613820f1f434
ms.openlocfilehash: 3e8df5ccfeca4bc0a19237ba6d59e9d0747080ea
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84394292"
---
# <a name="composite-data-types-visual-basic"></a>Tipos de dados compostos (Visual Basic)
Além dos tipos de dados elementares Visual Basic suprimentos, você também pode montar itens de tipos diferentes para criar *tipos de dados compostos* , como estruturas, matrizes e classes. Você pode criar tipos de dados compostos de tipos elementares e de outros tipos compostos. Por exemplo, você pode definir uma matriz de elementos de estrutura ou uma estrutura com membros de matriz.  
  
## <a name="data-types"></a>Tipos de dados  
 Um tipo composto é diferente do tipo de dados de qualquer um de seus componentes. Por exemplo, uma matriz de `Integer` elementos não é do `Integer` tipo de dados.  
  
 Um tipo de dados de matriz normalmente é representado usando o tipo de elemento, parênteses e vírgulas, conforme necessário. Por exemplo, uma matriz unidimensional de `String` elementos é representada como `String()` e uma matriz bidimensional de `Boolean` elementos é representada como `Boolean(,)` .  
  
## <a name="structure-types"></a>Tipos de estrutura  
 Não há nenhum tipo de dados único composto por todas as estruturas. Em vez disso, cada definição de uma estrutura representa um tipo de dados exclusivo, mesmo que duas estruturas definam elementos idênticos na mesma ordem. No entanto, se você criar duas ou mais instâncias da mesma estrutura, Visual Basic as considerará como sendo do mesmo tipo de dados.  
  
## <a name="tuples"></a>Tuplas

Uma tupla é uma estrutura leve que contém dois ou mais campos cujos tipos são predefinidos. As tuplas têm suporte a partir do Visual Basic 2017. As tuplas são usadas com mais frequência para retornar vários valores de uma única chamada de método sem precisar passar argumentos por referência ou empacotamento dos campos retornados em uma classe ou estrutura mais pesada. Consulte o tópico [tuplas](tuples.md) para obter mais informações sobre tuplas.

## <a name="array-types"></a>Tipos de matriz  
 Não há nenhum tipo de dados único composto por todas as matrizes. O tipo de dados de uma determinada instância de uma matriz é determinado pelo seguinte:  
  
- O fato de ser uma matriz  
  
- A classificação (número de dimensões) da matriz  
  
- O tipo de elemento da matriz  
  
 Em particular, o comprimento de uma determinada dimensão não faz parte do tipo de dados da instância. O exemplo a seguir ilustra isto.  
  
```vb  
Dim arrayA( ) As Byte = New Byte(12) {}  
Dim arrayB( ) As Byte = New Byte(100) {}  
Dim arrayC( ) As Short = New Short(100) {}  
Dim arrayD( , ) As Short  
Dim arrayE( , ) As Short = New Short(4, 10) {}  
```  
  
 No exemplo anterior, as variáveis de matriz `arrayA` e `arrayB` são consideradas do mesmo tipo de dados – `Byte()` — mesmo que elas sejam inicializadas para diferentes comprimentos. Variáveis `arrayB` e `arrayC` não são do mesmo tipo porque seus tipos de elementos são diferentes. Variáveis `arrayC` e `arrayD` não são do mesmo tipo porque suas classificações são diferentes. Variáveis `arrayD` e `arrayE` têm o mesmo tipo — `Short(,)` — porque suas classificações e os tipos de elementos são os mesmos, embora `arrayD` ainda não tenham sido inicializados.  
  
 Para obter mais informações sobre matrizes, consulte [matrizes](../arrays/index.md).  
  
## <a name="class-types"></a>Tipos de classe  
 Não há nenhum tipo de dados único que abranja todas as classes. Embora uma classe possa herdar de outra classe, cada uma é um tipo de dados separado. Várias instâncias da mesma classe são do mesmo tipo de dados. Se você atribuir uma variável de instância de classe a outra, não apenas elas terão o mesmo tipo de dados, elas apontarão para a mesma instância de classe na memória.  
  
 Para obter mais informações sobre classes, consulte [objetos e classes](../objects-and-classes/index.md).  
  
## <a name="see-also"></a>Confira também

- [Tipos de dados](index.md)
- [Tipos de dados elementares](elementary-data-types.md)
- [Tipos genéricos no Visual Basic](generic-types.md)
- [Tipos de valor e referência](value-types-and-reference-types.md)
- [Conversões de tipo no Visual Basic](type-conversions.md)
- [Estruturas](structures.md)
- [Solução de problemas de tipos de dados](troubleshooting-data-types.md)
- [Como: Manter mais de um valor em uma variável](how-to-hold-more-than-one-value-in-a-variable.md)
