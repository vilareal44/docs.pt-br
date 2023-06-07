---
title: Executando operações de cadeia de caracteres que não levam em conta a cultura
ms.date: 08/22/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- case mappings
- custom case mappings
- culture, custom sorting rules
- custom sorting rules
- culture-insensitive string operations, options for performing
- culture, custom case mappings
- culture-insensitive string operations, method overloads
ms.assetid: 579ef891-1f83-4c63-9ebd-2f40406b5b91
ms.openlocfilehash: 79ff899e2964ae2c1e90b7178616c612dddf6d86
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287500"
---
# <a name="performing-culture-insensitive-string-operations"></a>Executando operações de cadeia de caracteres que não levam em conta a cultura
A maioria dos métodos do .NET Framework que executam operações de cadeia de caracteres sensíveis à cultura fornece, por padrão, sobrecargas de método que permitem que você especifique explicitamente a cultura a ser usada passando um parâmetro <xref:System.Globalization.CultureInfo>. Essas sobrecargas permitem que você elimine variações culturais em mapeamentos de caso e em regras de classificação, e garante resultados insensíveis à cultura.  
  
 Esta seção fornece os seguintes tópicos para demonstrar como executar operações de cadeia de caracteres insensíveis à cultura usando métodos do .NET Framework que são sensíveis à cultura por padrão.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Executando comparações de cadeias de caracteres que não levam em conta a cultura](performing-culture-insensitive-string-comparisons.md)  
 Descreve como usar os métodos <xref:System.String.Compare%2A?displayProperty=nameWithType> e <xref:System.String.CompareTo%2A?displayProperty=nameWithType> para executar comparações de cadeia de caracteres insensíveis à cultura.  
  
 [Executando alterações de maiúsculas e minúsculas que não levam em conta a cultura](performing-culture-insensitive-case-changes.md)  
 Descreve como usar os métodos <xref:System.String.ToUpper%2A?displayProperty=nameWithType>, <xref:System.String.ToLower%2A?displayProperty=nameWithType>, <xref:System.Char.ToUpper%2A?displayProperty=nameWithType> e <xref:System.Char.ToLower%2A?displayProperty=nameWithType> para executar alterações na capitalização insensíveis à cultura.  
  
 [Executando operações de cadeia de caracteres que não levam em conta a cultura em coleções](performing-culture-insensitive-string-operations-in-collections.md)  
 Descreve como usar a classe <xref:System.Collections.CaseInsensitiveComparer>, <xref:System.Collections.CaseInsensitiveHashCodeProvider>, <xref:System.Collections.SortedList>, <xref:System.Collections.ArrayList.Sort%2A?displayProperty=nameWithType> e <xref:System.Collections.Specialized.CollectionsUtil.CreateCaseInsensitiveHashtable%2A?displayProperty=nameWithType> para executar operações insensíveis à cultura em coletas.  
  
 [Executando operações de cadeia de caracteres que não levam em conta a cultura em matrizes](performing-culture-insensitive-string-operations-in-arrays.md)  
 Descreve como usar os métodos <xref:System.Array.Sort%2A?displayProperty=nameWithType> e <xref:System.Array.BinarySearch%2A?displayProperty=nameWithType> para executar operações insensíveis à cultura em matrizes.  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Operações de cadeia de caracteres que não diferenciam culturas](culture-insensitive-string-operations.md)  
 Descreve por que você deve estar ciente da cultura ao executar operações em cadeias de caracteres, e fornece diretrizes para quando executar operações sensíveis à cultura e quando executar operações insensíveis à cultura.

## <a name="see-also"></a>Veja também

- [Classificação de tabelas de peso (para .NET em sistemas Windows)](https://www.microsoft.com/download/details.aspx?id=10921)
- [Tabela do elemento de ordenação Unicode padrão (para .NET Core em Linux e macOS)](https://www.unicode.org/Public/UCA/latest/allkeys.txt)
