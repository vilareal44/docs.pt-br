---
title: Analisando outras cadeias de caracteres no .NET
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Char data type, parsing strings
- enumerations [.NET Framework], parsing strings
- base types, parsing strings
- parsing strings, other strings
- Boolean data type, parsing strings
ms.assetid: d139bc00-3c4e-4d78-ac9a-5c951b258d28
ms.openlocfilehash: a3503e0e499c6010fcc3d8669fa5c1eaf2dbf570
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84277538"
---
# <a name="parsing-other-strings-in-net"></a>Analisando outras cadeias de caracteres no .NET
Além das cadeias de caracteres numéricas e <xref:System.DateTime>, também é possível analisar cadeias de caracteres que representam os tipos <xref:System.Char><xref:System.Boolean> e<xref:System.Enum> em tipos de dados.  
  
## <a name="char"></a>Char  
 O método de análise estática associado ao tipo de dados **Char** é útil para converter uma cadeia de caracteres que contém um único caractere em seu valor Unicode. O exemplo de código a seguir analisa uma cadeia de caracteres em um caractere Unicode.  
  
 [!code-cpp[Conceptual.String.Parse#2](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.parse/cpp/parse.cpp#2)]
 [!code-csharp[Conceptual.String.Parse#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.parse/cs/parse.cs#2)]
 [!code-vb[Conceptual.String.Parse#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.parse/vb/parse.vb#2)]  
  
## <a name="boolean"></a>Boolean  
 O tipo de dados **Booliano** contém um método **Parse** que pode ser usado para converter uma cadeia de caracteres que representa um valor Booliano em um tipo **Booliano** real. Esse método não diferencia maiúsculas de minúsculas e consegue analisar com êxito uma cadeia de caracteres que contém “True” ou “False”. O método **Parse** associado ao tipo **Booliano** também pode analisar cadeias de caracteres cercadas por espaços em branco. Se qualquer outra cadeia de caracteres for passada, um <xref:System.FormatException> será gerado.  
  
 O exemplo de código a seguir usa o método **Parse** para converter uma cadeia de caracteres em um valor Booliano.  
  
 [!code-cpp[Conceptual.String.Parse#3](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.parse/cpp/parse.cpp#3)]
 [!code-csharp[Conceptual.String.Parse#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.parse/cs/parse.cs#3)]
 [!code-vb[Conceptual.String.Parse#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.parse/vb/parse.vb#3)]  
  
## <a name="enumeration"></a>Enumeração  
 Você pode usar o método estático **Parse** para inicializar um tipo de enumeração para o valor de uma cadeia de caracteres. Esse método aceita o tipo de enumeração que você estiver analisando, a cadeia de caracteres a analisar e um sinalizador Booliano opcional que indica se a análise diferencia maiúsculas de minúsculas ou não. A cadeia de caracteres que você está analisando pode conter diversos valores separados por vírgulas, que podem ser antecedidos ou seguidos por um ou mais espaços vazios (também chamados de espaços em branco). Quando a cadeia de caracteres contém diversos valores, o valor do objeto retornado é o valor de todos os valores especificados combinado com uma operação OR bit a bit.  
  
 O exemplo a seguir usa o método **Parse** para converter uma representação de cadeia de caracteres em um valor de enumeração. A enumeração <xref:System.DayOfWeek> é inicializada para **Quinta-feira** de uma cadeia de caracteres.  
  
 [!code-cpp[Conceptual.String.Parse#4](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.parse/cpp/parse.cpp#4)]
 [!code-csharp[Conceptual.String.Parse#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.parse/cs/parse.cs#4)]
 [!code-vb[Conceptual.String.Parse#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.parse/vb/parse.vb#4)]  
  
## <a name="see-also"></a>Veja também

- [Analisando cadeias de caracteres](parsing-strings.md)
- [Formatar tipos](formatting-types.md)
- [Conversão de tipo no .NET](type-conversion.md)
