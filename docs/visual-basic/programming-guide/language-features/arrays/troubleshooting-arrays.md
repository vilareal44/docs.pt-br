---
title: Solução de problemas de matrizes
ms.date: 07/20/2015
helpviewer_keywords:
- troubleshooting arrays
- arrays [Visual Basic], initialization errors
- troubleshooting Visual Basic, arrays
- arrays [Visual Basic], compilation errors
- arrays [Visual Basic], declaration errors
- arrays [Visual Basic], troubleshooting
ms.assetid: f4e971c7-c0a4-4ed7-a77a-8d71039f266f
ms.openlocfilehash: e633c5a00693f188270b1610abaf2decb656b00a
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84414589"
---
# <a name="troubleshooting-arrays-visual-basic"></a>Solucionando problemas de matrizes (Visual Basic)
Esta página lista alguns problemas comuns que podem ocorrer ao trabalhar com matrizes.  
  
## <a name="compilation-errors-declaring-and-initializing-an-array"></a>Erros de compilação declarando e inicializando uma matriz  
 Os erros de compilação podem surgir de mal-entendido das regras para declarar, criar e inicializar matrizes. As causas mais comuns de erros são as seguintes:  
  
- Fornecendo uma [nova cláusula Operator](../../../language-reference/operators/new-operator.md) depois de especificar comprimentos de dimensão na declaração de variável de matriz. As linhas de código a seguir mostram declarações inválidas desse tipo.  
  
     `Dim INVALIDsingleDimByteArray(2) As Byte = New Byte()`  
  
     `Dim INVALIDtwoDimShortArray(1, 1) As Short = New Short(,)`  
  
     `Dim INVALIDjaggedByteArray(1)() As Byte = New Byte()()`  
  
- Especificando comprimentos de dimensão para mais do que a matriz de nível superior de uma matriz denteada. A linha de código a seguir mostra uma declaração inválida desse tipo.  
  
     `Dim INVALIDjaggedByteArray(1)(1) As Byte`  
  
- Omitindo a `New` palavra-chave ao especificar os valores do elemento. A linha de código a seguir mostra uma declaração inválida desse tipo.  
  
     `Dim INVALIDoneDimShortArray() As Short = Short() {0, 1, 2, 3}`  
  
- Fornecendo uma `New` cláusula sem chaves ( `{}` ). As linhas de código a seguir mostram declarações inválidas desse tipo.  
  
     `Dim INVALIDsingleDimByteArray() As Byte = New Byte()`  
  
     `Dim INVALIDsingleDimByteArray() As Byte = New Byte(2)`  
  
     `Dim INVALIDtwoDimShortArray(,) As Short = New Short(,)`  
  
     `Dim INVALIDtwoDimShortArray(,) As Short = New Short(1, 1)`  
  
## <a name="accessing-an-array-out-of-bounds"></a>Acessando uma matriz fora dos limites  
 O processo de inicializar uma matriz atribui um limite superior e um limite inferior a cada dimensão. Cada acesso a um elemento da matriz deve especificar um índice válido, ou subscrito, para cada dimensão. Se qualquer índice estiver abaixo de seu limite inferior ou acima de seu limite superior, <xref:System.IndexOutOfRangeException> ocorrerá uma exceção. O compilador não pode detectar esse erro, portanto, um erro ocorre em tempo de execução.  
  
### <a name="determining-bounds"></a>Determinando limites  
 Se outro componente passar uma matriz para seu código, por exemplo, como um argumento de procedimento, você não saberá o tamanho dessa matriz ou os comprimentos de suas dimensões. Você sempre deve determinar o limite superior para cada dimensão de uma matriz antes de tentar acessar quaisquer elementos. Se a matriz tiver sido criada por alguns meios diferentes de uma `New` cláusula Visual Basic, o limite inferior poderá ser algo diferente de 0, e será mais seguro determinar o limite inferior também.  
  
### <a name="specifying-the-dimension"></a>Especificando a dimensão  
 Ao determinar os limites de uma matriz multidimensional, tome cuidado com o modo de especificar a dimensão. Os `dimension` parâmetros dos <xref:System.Array.GetLowerBound%2A> métodos e <xref:System.Array.GetUpperBound%2A> são baseados em 0, enquanto os `Rank` parâmetros da Visual Basic e das <xref:Microsoft.VisualBasic.Information.LBound%2A> <xref:Microsoft.VisualBasic.Information.UBound%2A> funções são baseados em 1.  
  
## <a name="see-also"></a>Confira também

- [Matrizes](index.md)
- [Como inicializar uma variável de matriz no Visual Basic](how-to-initialize-an-array-variable.md)
