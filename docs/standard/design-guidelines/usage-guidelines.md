---
title: Diretrizes de uso
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- class library design guidelines [.NET Framework], usage guidelines
ms.assetid: 42215ffa-a099-4a26-b14e-fb2bdb6f95b7
ms.openlocfilehash: 03eaba3e52cb25619f65637efb4f414c22770440
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291338"
---
# <a name="usage-guidelines"></a>Diretrizes de uso

Esta seção contém diretrizes para o uso de tipos comuns em APIs acessíveis publicamente. Ele trata do uso direto de tipos de estrutura internos (por exemplo, atributos de serialização) e sobrecarga de operadores comuns.
  
A <xref:System.IDisposable?displayProperty=nameWithType> interface não é abordada nesta seção, mas é discutida na seção [padrão Dispose](../garbage-collection/implementing-dispose.md) .

> [!NOTE]
> Para obter diretrizes e informações adicionais sobre outros tipos de .NET Framework internos comuns, consulte os tópicos de referência para o seguinte: <xref:System.DateTime?displayProperty=nameWithType> , <xref:System.DateTimeOffset?displayProperty=nameWithType> ,,,,, <xref:System.ICloneable?displayProperty=nameWithType> <xref:System.IComparable%601?displayProperty=nameWithType> <xref:System.IEquatable%601?displayProperty=nameWithType> <xref:System.Nullable%601?displayProperty=nameWithType> <xref:System.Object?displayProperty=nameWithType> , <xref:System.Uri?displayProperty=nameWithType> .

## <a name="in-this-section"></a>Nesta seção

[Matrizes](arrays.md)  
[Atributos](attributes.md)  
[Coleções](guidelines-for-collections.md)  
[Serialização](serialization.md)  
[Uso de System. xml](system-xml-usage.md)  
[Operadores de igualdade](equality-operators.md)  

*Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

*Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*
  
## <a name="see-also"></a>Veja também

- [Diretrizes de design de estrutura](index.md)
