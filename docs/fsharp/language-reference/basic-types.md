---
title: Tipos Básicos
description: 'Descubra os tipos básicos fundamentais que são usados na linguagem F #.'
ms.date: 08/15/2020
ms.openlocfilehash: 659ac8424c62985affcca1741e1b2a74c9c3ee8d
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88557692"
---
# <a name="basic-types"></a>Tipos básicos

Este tópico lista os tipos básicos que são definidos na linguagem F #. Esses tipos são os mais fundamentais em F #, que formam a base de quase todos os programas em F #. Eles são um superconjunto de tipos primitivos .NET.

|Type|Tipo .NET|Descrição|Exemplo|
|----|---------|-----------|-------|
|`bool`|<xref:System.Boolean>|Os valores possíveis são `true` e `false`.|`true`/`false`|
|`byte`|<xref:System.Byte>|Valores de 0 a 255.|`1uy`|
|`sbyte`|<xref:System.SByte>|Valores de-128 a 127.|`1y`|
|`int16`|<xref:System.Int16>|Valores de-32768 a 32767.|`1s`|
|`uint16`|<xref:System.UInt16>|Valores de 0 a 65535.|`1us`|
|`int`|<xref:System.Int32>|Valores de-2.147.483.648 a 2.147.483.647.|`1`|
|`uint`|<xref:System.UInt32>|Valores de 0 a 4.294.967.295.|`1u`|
|`int64`|<xref:System.Int64>|Valores de-9.223.372.036.854.775.808 a 9.223.372.036.854.775.807.|`1L`|
|`uint64`|<xref:System.UInt64>|Valores de 0 a 18446744073709551615.|`1UL`|
|`nativeint`|<xref:System.IntPtr>|Um ponteiro nativo como um inteiro assinado.|`nativeint 1`|
|`unativeint`|<xref:System.UIntPtr>|Um ponteiro nativo como um inteiro não assinado.|`unativeint 1`|
|`decimal`|<xref:System.Decimal>|Um tipo de dados de ponto flutuante que tem pelo menos 28 dígitos significativos.|`1.0`|
|`float`, `double`|<xref:System.Double>|Um tipo de ponto flutuante de 64 bits.|`1.0`|
|`float32`, `single`|<xref:System.Single>|Um tipo de ponto flutuante de 32 bits.|`1.0f`|
|`char`|<xref:System.Char>|Valores de caracteres Unicode.|`'c'`|
|`string`|<xref:System.String>|Texto Unicode.|`"str"`|
|`unit`|não aplicável|Indica a ausência de um valor real. O tipo tem apenas um valor formal, que é indicado `()` . O valor de unidade, `()` , geralmente é usado como um espaço reservado onde um valor é necessário, mas nenhum valor real está disponível ou faz sentido.|`()`|

> [!NOTE]
> Você pode executar cálculos com números inteiros muito grandes para o tipo inteiro de 64 bits usando o tipo [bigint](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-bigint.html) . `bigint` Não é considerado um tipo básico; é uma abreviação do `System.Numerics.BigInteger` .

## <a name="see-also"></a>Confira também

- [Referência de linguagem F #](index.md)
