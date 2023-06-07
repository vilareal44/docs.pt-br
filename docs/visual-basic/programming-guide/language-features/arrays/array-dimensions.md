---
title: Dimensões da matriz
ms.date: 07/20/2015
helpviewer_keywords:
- dimensions, arrays
- arrays [Visual Basic], dimensions
- arrays [Visual Basic], rectangular
- arrays [Visual Basic], rank
- rectangular arrays
- ranking, arrays
ms.assetid: 385e911b-18c1-4e98-9924-c6d279101dd9
ms.openlocfilehash: f971f0c3693177adbcb8869d487e3ad41d49ddc2
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84413098"
---
# <a name="array-dimensions-in-visual-basic"></a>Dimensões de matriz no Visual Basic

Uma *dimensão* é uma direção na qual você pode variar a especificação dos elementos de uma matriz. Uma matriz que contém o total de vendas para cada dia do mês tem uma dimensão (o dia do mês). Uma matriz que contém o total de vendas por departamento para cada dia do mês tem duas dimensões (o número do departamento e o dia do mês). O número de dimensões que uma matriz tem é chamado de sua *classificação*.

> [!NOTE]
> Você pode usar a <xref:System.Array.Rank%2A> propriedade para determinar quantas dimensões uma matriz tem.

## <a name="working-with-dimensions"></a>Trabalhando com dimensões

Você especifica um elemento de uma matriz fornecendo um *índice* ou *subscrito* para cada uma de suas dimensões. Os elementos são contíguos ao longo de cada dimensão do índice 0 por meio do índice mais alto para essa dimensão.

As ilustrações a seguir mostram a estrutura conceitual de matrizes com classificações diferentes. Cada elemento nas ilustrações mostra os valores de índice que o acessam. Por exemplo, você pode acessar o primeiro elemento da segunda linha da matriz bidimensional especificando índices `(1, 0)` .

![Diagrama que mostra uma matriz unidimensional.](./media/array-dimensions/one-dimensional-array.gif)

![Diagrama que mostra uma matriz bidimensional.](./media/array-dimensions/two-dimensional-array.gif)

![Diagrama que mostra uma matriz tridimensional.](./media/array-dimensions/three-dimensional-array.gif)

### <a name="one-dimension"></a>Uma dimensão

Muitas matrizes têm apenas uma dimensão, como o número de pessoas de cada idade. O único requisito para especificar um elemento é a idade para a qual o elemento contém a contagem. Portanto, essa matriz usa apenas um índice. O exemplo a seguir declara uma variável para manter uma *matriz unidimensional* de contagens de idade para idades de 0 a 120.

```vb
Dim ageCounts(120) As UInteger
```

### <a name="two-dimensions"></a>Duas dimensões

Algumas matrizes têm duas dimensões, como o número de escritórios em cada andar de cada prédio em um campus. A especificação de um elemento requer o número de edifício e o chão, e cada elemento contém a contagem para essa combinação de edifício e andar. Portanto, tal matriz usa dois índices. O exemplo a seguir declara uma variável para conter uma *matriz bidimensional* de contagens de escritórios, para edifícios de 0 a 40 e andares de 0 a 5.

```vb
Dim officeCounts(40, 5) As Byte
```

Uma matriz bidimensional também é chamada de *matriz retangular*.

### <a name="three-dimensions"></a>Três dimensões

Algumas matrizes têm três dimensões, como valores em espaço tridimensional. Essa matriz usa três índices, que, nesse caso, representam as coordenadas x, y e z do espaço físico. O exemplo a seguir declara uma variável para conter uma *matriz tridimensional* de temperaturas de ar em vários pontos em um volume tridimensional.

```vb
Dim airTemperatures(99, 99, 24) As Single
```

### <a name="more-than-three-dimensions"></a>Mais de três dimensões

Embora uma matriz possa ter até 32 dimensões, é raro ter mais de três.

> [!NOTE]
> Quando você adiciona dimensões a uma matriz, o armazenamento total necessário para a matriz aumenta consideravelmente, portanto, use matrizes multidimensionais com cuidado.

## <a name="using-different-dimensions"></a>Usando dimensões diferentes

Suponha que você deseja acompanhar os valores de vendas de cada dia do mês atual. Você pode declarar uma matriz unidimensional com 31 elementos, uma para cada dia do mês, como mostra o exemplo a seguir.

```vb
Dim salesAmounts(30) As Double
```

Agora suponha que você deseja controlar as mesmas informações não apenas para todos os dias de um mês, mas também para todos os meses do ano. Você pode declarar uma matriz bidimensional com 12 linhas (para os meses) e 31 colunas (por dias), como mostra o exemplo a seguir.

```vb
Dim salesAmounts(11, 30) As Double
```

Agora suponha que você decida que sua matriz contém informações por mais de um ano. Se você quiser acompanhar os valores de vendas por 5 anos, poderá declarar uma matriz tridimensional com 5 camadas, 12 linhas e 31 colunas, como mostra o exemplo a seguir.

```vb
Dim salesAmounts(4, 11, 30) As Double
```

Observe que, como cada índice varia de 0 até seu máximo, cada dimensão de `salesAmounts` é declarada como um valor menor que o comprimento necessário para essa dimensão. Observe também que o tamanho da matriz aumenta com cada nova dimensão. Os três tamanhos nos exemplos anteriores são 31, 372 e 1.860 elementos, respectivamente.

> [!NOTE]
> Você pode criar uma matriz sem usar a `Dim` instrução ou a `New` cláusula. Por exemplo, você pode chamar o <xref:System.Array.CreateInstance%2A> método ou outro componente pode passar seu código uma matriz criada dessa maneira. Tal matriz pode ter um limite inferior diferente de 0. Você sempre pode testar o limite inferior de uma dimensão usando o <xref:System.Array.GetLowerBound%2A> método ou a `LBound` função.

## <a name="see-also"></a>Confira também

- [Matrizes](index.md)
- [Solução de problemas de matrizes](troubleshooting-arrays.md)
