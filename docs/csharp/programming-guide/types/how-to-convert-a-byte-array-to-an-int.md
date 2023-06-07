---
title: Como converter uma matriz de bytes em um guia de programação int-C#
description: Saiba como converter uma matriz de bytes em um int. Consulte exemplos de código e exiba recursos adicionais disponíveis.
ms.date: 07/20/2015
helpviewer_keywords:
- conversions [C#], byte array to int
- byte arrays [C#], converting to int
ms.assetid: d6ac20e2-448e-4aea-99b9-faf04c6f1e79
ms.openlocfilehash: 24037a5e212326cf5e670214a6774eff52bd8faf
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381561"
---
# <a name="how-to-convert-a-byte-array-to-an-int-c-programming-guide"></a>Como converter uma matriz de bytes em um int (guia de programação C#)

Este exemplo mostra como usar a classe <xref:System.BitConverter> para converter uma matriz de bytes em um [int](../../language-reference/builtin-types/integral-numeric-types.md) e de volta em uma matriz de bytes. Talvez você precise converter bytes em um tipo de dados interno depois de ler bytes da rede, por exemplo. Além do método [ToInt32 (byte \[ \] , Int32)](xref:System.BitConverter.ToInt32(System.Byte[],System.Int32)) no exemplo, a tabela a seguir lista os métodos na <xref:System.BitConverter> classe que convertem bytes (de uma matriz de bytes) em outros tipos internos.

|Tipo retornado|Método|
|-------------------|------------|
|`bool`|[ToBoolean (byte \[ \] , Int32)](xref:System.BitConverter.ToBoolean(System.Byte[],System.Int32))|
|`char`|[ToChar (byte \[ \] , Int32)](xref:System.BitConverter.ToChar(System.Byte[],System.Int32))|
|`double`|[ToDouble (byte \[ \] , Int32)](xref:System.BitConverter.ToDouble(System.Byte[],System.Int32))|
|`short`|[ToInt16 (byte \[ \] , Int32)](xref:System.BitConverter.ToInt16(System.Byte[],System.Int32))|
|`int`|[ToInt32 (byte \[ \] , Int32)](xref:System.BitConverter.ToInt32(System.Byte[],System.Int32))|
|`long`|[ToInt64 (byte \[ \] , Int32)](xref:System.BitConverter.ToInt64(System.Byte[],System.Int32))|
|`float`|[ToSingle (byte \[ \] , Int32)](xref:System.BitConverter.ToSingle(System.Byte[],System.Int32))|
|`ushort`|[ToUInt16 (byte \[ \] , Int32)](xref:System.BitConverter.ToUInt16(System.Byte[],System.Int32))|
|`uint`|[ToUInt32 (byte \[ \] , Int32)](xref:System.BitConverter.ToUInt32(System.Byte[],System.Int32))|
|`ulong`|[ToUInt64 (byte \[ \] , Int32)](xref:System.BitConverter.ToUInt64(System.Byte[],System.Int32))|

## <a name="example"></a>Exemplo

Este exemplo Inicializa uma matriz de bytes, reverte a matriz se a arquitetura do computador é little-endian (ou seja, o byte menos significativo é armazenado primeiro) e, em seguida, chama o método [ToInt32 (byte \[ \] , Int32)](xref:System.BitConverter.ToInt32(System.Byte[],System.Int32)) para converter quatro bytes na matriz em um `int` . O segundo argumento para [ToInt32 (byte \[ \] , Int32)](xref:System.BitConverter.ToInt32(System.Byte[],System.Int32)) especifica o índice de início da matriz de bytes.

> [!NOTE]
> A saída pode diferir dependendo da ordenação da arquitetura do seu computador.

[!code-csharp[csProgGuideTypes#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#22)]

## <a name="example"></a>Exemplo

Neste exemplo, o método <xref:System.BitConverter.GetBytes%28System.Int32%29> da classe <xref:System.BitConverter> é chamado para converter um `int` em uma matriz de bytes.

> [!NOTE]
> A saída pode diferir dependendo da ordenação da arquitetura do seu computador.

[!code-csharp[csProgGuideTypes#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#23)]

## <a name="see-also"></a>Veja também

- <xref:System.BitConverter>
- <xref:System.BitConverter.IsLittleEndian>
- [Types](./index.md)
