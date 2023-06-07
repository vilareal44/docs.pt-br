---
title: Tipos acelerados por SIMD no .NET
description: Este artigo descreve os tipos de habilitação para SIMD no .NET e demonstra como usar operações de hardware SIMD em C# e .NET.
author: FIVIL
ms.author: tagoo
ms.date: 04/28/2020
ms.technology: dotnet-standard
ms.openlocfilehash: 5c1ad01ea15a9c4352cf7f87e5fba3bf74b4679c
ms.sourcegitcommit: 2987e241e2f76c9248d2146bf2761a33e2c7a882
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88228731"
---
# <a name="use-simd-accelerated-numeric-types"></a>Usar tipos numéricos acelerados por SIMD

O SIMD (instrução única, vários dados) fornece suporte de hardware para executar uma operação em vários dados, em paralelo, usando uma única instrução. No .NET, há um conjunto de tipos com aceleração SIMD no <xref:System.Numerics> namespace. As operações de SIMD podem ser paralelizadas no nível de hardware. Isso aumenta a taxa de transferência dos cálculos vetorizadas, que são comuns em aplicativos matemáticos, científicos e gráficos.

## <a name="net-simd-accelerated-types"></a>Tipos acelerados pelo .NET SIMD

Os tipos acelerados do .NET SIMD incluem os seguintes tipos:

- Os tipos <xref:System.Numerics.Vector2>, <xref:System.Numerics.Vector3> e <xref:System.Numerics.Vector4>, que representam vetores com 2, 3 e 4 valores de <xref:System.Single>.

- Dois tipos de matriz, <xref:System.Numerics.Matrix3x2> , que representam uma matriz 3x2 e <xref:System.Numerics.Matrix4x4> , que representa uma matriz 4x4 de <xref:System.Single> valores.

- O <xref:System.Numerics.Plane> tipo, que representa um plano em espaço tridimensional usando <xref:System.Single> valores.

- O <xref:System.Numerics.Quaternion> tipo, que representa um vetor que é usado para codificar rotações físicas tridimensionais usando <xref:System.Single> valores.

- O tipo <xref:System.Numerics.Vector%601>, que representa um vetor de um tipo numérico especificado e fornece um amplo conjunto de operadores que se beneficiam de suporte a SIMD. A contagem de uma <xref:System.Numerics.Vector%601> instância é fixa durante o tempo de vida de um aplicativo, mas seu valor <xref:System.Numerics.Vector%601.Count%2A?displayProperty=nameWithType> depende da CPU da máquina que executa o código.

  > [!NOTE]
  > O <xref:System.Numerics.Vector%601> tipo não está incluído na .NET Framework. Você deve instalar o pacote [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors) do NuGet para obter acesso a esse tipo.
  
Os tipos com aceleração SIMD são implementados de forma que possam ser usados com hardware ou compiladores JIT que não são acelerados por SIMD. Para aproveitar as instruções de SIMD, seus aplicativos de 64 bits devem ser executados pelo tempo de execução que usa o compilador **RyuJIT** . Um compilador **RyuJIT** está incluído no .NET Core e no .NET Framework 4,6 e posterior. O suporte a SIMD é fornecido apenas ao direcionar processadores de 64 bits.

## <a name="how-to-use-simd"></a>Como usar o SIMD?

Antes de executar algoritmos de SIMD personalizados, é possível verificar se o computador host dá suporte a SIMD usando <xref:System.Numerics.Vector.IsHardwareAccelerated?displayProperty=nameWithType> o, que retorna um <xref:System.Boolean> . Isso não garante que a aceleração SIMD esteja habilitada para um tipo específico, mas é um indicador de que há suporte para alguns tipos.

## <a name="simple-vectors"></a>Vetores simples

Os tipos mais primitivos de SIMD mais rápidos no .NET são <xref:System.Numerics.Vector2> , <xref:System.Numerics.Vector3> e <xref:System.Numerics.Vector4> tipos, que representam vetores com 2, 3 e 4 <xref:System.Single> valores. O exemplo a seguir usa <xref:System.Numerics.Vector2> para adicionar dois vetores.

```csharp
var v1 = new Vector2(0.1f, 0.2f);
var v2 = new Vector2(1.1f, 2.2f);
var vResult = v1 + v2;
```

Também é possível usar vetores do .net para calcular outras propriedades matemáticas de vetores, como `Dot product` , `Transform` `Clamp` e assim por diante.

```csharp
var v1 = new Vector2(0.1f, 0.2f);
var v2 = new Vector2(1.1f, 2.2f);
var vResult1 = Vector2.Dot(v1, v2);
var vResult2 = Vector2.Distance(v1, v2);
var vResult3 = Vector2.Clamp(v1, Vector2.Zero, Vector2.One);
```

## <a name="matrix"></a>Matriz

<xref:System.Numerics.Matrix3x2>, que representa uma matriz 3x2 e <xref:System.Numerics.Matrix4x4> , que representa uma matriz 4x4. Pode ser usado para cálculos relacionados à matriz. O exemplo a seguir demonstra a multiplicação de uma matriz em sua matriz de Transpose de correspondente usando o SIMD.

```csharp
var m1 = new Matrix4x4(
            1.1f, 1.2f, 1.3f, 1.4f,
            2.1f, 2.2f, 3.3f, 4.4f,
            3.1f, 3.2f, 3.3f, 3.4f,
            4.1f, 4.2f, 4.3f, 4.4f);

var m2 = Matrix4x4.Transpose(m1);
var mResult = Matrix4x4.Multiply(m1, m2);
```

## <a name="vectort"></a>Vetor\<T>

O <xref:System.Numerics.Vector%601> oferece a capacidade de usar vetores mais longos. A contagem de uma <xref:System.Numerics.Vector%601> instância é fixa, mas seu valor <xref:System.Numerics.Vector%601.Count%2A?displayProperty=nameWithType> depende da CPU da máquina que executa o código.

O exemplo a seguir demonstra como adicionar elementos de matrizes longos usando <xref:System.Numerics.Vector%601> .

```csharp
double[] SimdVectorProd(double[] left, double[] right)
{
    var offset = Vector<double>.Count;
    double[] result = new double[left.Length];
    int i = 0;
    for (i = 0; i < left.Length; i += offset)
    {
        var v1 = new Vector<double>(left, i);
        var v2 = new Vector<double>(right, i);
        (v1 * v2).CopyTo(result, i);
    }

    //remaining items
    for (; i < left.Length; ++i)
    {
        result[i] = left[i] * right[i];
    }

    return result;
}
```

## <a name="remarks"></a>Comentários

O SIMD tem mais probabilidade de remover um afunilamento e expor o próximo, por exemplo, a taxa de transferência de memória. Em geral, o benefício de desempenho do uso de SIMD varia dependendo do cenário específico e, em alguns casos, ele pode até mesmo executar pior do que um código equivalente não-SIMD mais simples.
