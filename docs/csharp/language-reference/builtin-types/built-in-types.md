---
title: Tipos internos-referência C#
description: Aprenda tipos de referência e valor internos do C#
ms.date: 02/04/2020
helpviewer_keywords:
- types [C#], built-in
- built-in C# types
ms.assetid: 54f901f2-bf2f-472c-ae8d-73e8ecfc57fe
ms.openlocfilehash: 3366f718cd83a28f475fae9b4e65ce37fe7d8c7b
ms.sourcegitcommit: 1eae045421d9ea2bfc82aaccfa5b1ff1b8c9e0e4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84803198"
---
# <a name="built-in-types-c-reference"></a>Tipos internos (referência C#)

A tabela a seguir lista os tipos de [valores](value-types.md) internos do C#:

|Palavra-chave Type do C#|Tipo .NET|
|--------------|-------------------------|
|[`bool`](bool.md)|<xref:System.Boolean?displayProperty=nameWithType>|
|[`byte`](integral-numeric-types.md)|<xref:System.Byte?displayProperty=nameWithType>|
|[`sbyte`](integral-numeric-types.md)|<xref:System.SByte?displayProperty=nameWithType>|
|[`char`](char.md)|<xref:System.Char?displayProperty=nameWithType>|
|[`decimal`](floating-point-numeric-types.md)|<xref:System.Decimal?displayProperty=nameWithType>|
|[`double`](floating-point-numeric-types.md)|<xref:System.Double?displayProperty=nameWithType>|
|[`float`](floating-point-numeric-types.md)|<xref:System.Single?displayProperty=nameWithType>|
|[`int`](integral-numeric-types.md)|<xref:System.Int32?displayProperty=nameWithType>|
|[`uint`](integral-numeric-types.md)|<xref:System.UInt32?displayProperty=nameWithType>|
|[`long`](integral-numeric-types.md)|<xref:System.Int64?displayProperty=nameWithType>|
|[`ulong`](integral-numeric-types.md)|<xref:System.UInt64?displayProperty=nameWithType>|
|[`short`](integral-numeric-types.md)|<xref:System.Int16?displayProperty=nameWithType>|
|[`ushort`](integral-numeric-types.md)|<xref:System.UInt16?displayProperty=nameWithType>|

A tabela a seguir lista os tipos de [referência](../keywords/reference-types.md) interna do C#:

|Palavra-chave Type do C#|Tipo .NET|
|--------------|-------------------------|
|[`object`](reference-types.md#the-object-type)|<xref:System.Object?displayProperty=nameWithType>|
|[`string`](reference-types.md#the-string-type)|<xref:System.String?displayProperty=nameWithType>|
|[`dynamic`](reference-types.md#the-dynamic-type)|<xref:System.Object?displayProperty=nameWithType>|

Nas tabelas anteriores, cada palavra-chave do tipo C# da coluna à esquerda é um alias para o tipo .NET correspondente. Eles são intercambiáveis. Por exemplo, as declarações a seguir declaram variáveis do mesmo tipo:

```csharp
int a = 123;
System.Int32 b = 123;
```

A [`void`](void.md) palavra-chave representa a ausência de um tipo. Você o usa como o tipo de retorno de um método que não retorna um valor.

## <a name="see-also"></a>Confira também

- [Referência de C#](../index.md)
- [Valores padrão de tipos C#](default-values.md)
