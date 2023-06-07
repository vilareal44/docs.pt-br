---
title: LINQ e cadeias de caracteres (C#)
description: O LINQ pode consultar e transformar cadeias de caracteres e coleções de cadeias de caracteres. Você pode combinar consultas LINQ com funções de cadeia de caracteres C# e expressões regulares.
ms.date: 07/20/2015
ms.assetid: dbe2d657-b3f3-487e-b645-21fb2d71cd7b
ms.openlocfilehash: c515a0c56ad6473f93c6339540e4ed0245bb5bd2
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165617"
---
# <a name="linq-and-strings-c"></a>LINQ e cadeias de caracteres (C#)

A LINQ pode ser usada para consultar e transformar as cadeias de caracteres e coleções de cadeias de caracteres. Ele pode ser especialmente útil com os dados semiestruturados em arquivos de texto. Consultas LINQ podem ser combinadas com expressões regulares e funções de cadeia de caracteres tradicionais. Por exemplo, você pode usar o método <xref:System.String.Split%2A?displayProperty=nameWithType> ou <xref:System.Text.RegularExpressions.Regex.Split%2A?displayProperty=nameWithType> para criar uma matriz de cadeias de caracteres que você pode consultar ou modificar usando o LINQ. Você pode usar o método <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> na cláusula `where` de uma consulta LINQ. E você pode usar o LINQ para consultar ou modificar os resultados de <xref:System.Text.RegularExpressions.MatchCollection> retornados por uma expressão regular.

Você também pode usar as técnicas descritas nessa seção para transformar dados de texto semiestruturados em XML. Para obter mais informações, consulte [como gerar XML a partir de arquivos CSV](how-to-generate-xml-from-csv-files.md).

Os exemplos nesta seção se enquadram em duas categorias:

## <a name="querying-a-block-of-text"></a>Consultando um bloco de texto

Consultar, analisar e modificar os blocos de texto dividindo-os em uma matriz de cadeias de caracteres menores consultáveis usando o método <xref:System.String.Split%2A?displayProperty=nameWithType> ou o método <xref:System.Text.RegularExpressions.Regex.Split%2A?displayProperty=nameWithType>. Você pode dividir o texto de origem em palavras, frases, parágrafos, páginas ou quaisquer outros critérios e, em seguida, executar divisões adicionais se elas forem necessárias em sua consulta.

- [Como contar ocorrências de uma palavra em uma cadeia de caracteres (LINQ) (C#)](how-to-count-occurrences-of-a-word-in-a-string-linq.md)  
  Mostra como usar a LINQ para consultas simples em texto.

- [Como consultar frases que contêm um conjunto especificado de palavras (LINQ) (C#)](how-to-query-for-sentences-that-contain-a-specified-set-of-words-linq.md)

  Mostra como dividir os arquivos de texto em limites arbitrários e como executar consultas em cada parte.

- [Como consultar caracteres em uma cadeia (LINQ) (C#)](how-to-query-for-characters-in-a-string-linq.md)

  Demonstra que uma cadeia de caracteres é de um tipo passível de consulta.

- [Como combinar consultas LINQ com expressões regulares (C#)](how-to-combine-linq-queries-with-regular-expressions.md)

  Mostra como usar expressões regulares em consultas LINQ para correspondência de padrões complexos em resultados de consulta filtrados.

## <a name="querying-semi-structured-data-in-text-format"></a>Consultando dados semiestruturados em formato de texto

Muitos tipos diferentes de arquivos de texto consistem em uma série de linhas, geralmente com formatação semelhante, como arquivos delimitados por tabulação ou vírgula ou linhas de comprimento fixo. Depois de ler um arquivo de texto na memória, você pode usar a LINQ para consultar e/ou modificar as linhas. As consultas LINQ também simplificam a tarefa de combinar dados de várias fontes.

- [Como localizar a diferença de conjunto entre duas listas (LINQ) (C#)](how-to-find-the-set-difference-between-two-lists-linq.md)

  Mostra como localizar todas as cadeias de caracteres que estão presentes em uma lista, mas não na outra.

- [Como classificar ou filtrar dados de texto por qualquer palavra ou campo (LINQ) (C#)](how-to-sort-or-filter-text-data-by-any-word-or-field-linq.md)

  Mostra como classificar linhas de texto com base em qualquer palavra ou campo.

- [Como reordenar os campos de um arquivo delimitado (LINQ) (C#)](how-to-reorder-the-fields-of-a-delimited-file-linq.md)

  Mostra como reordenar campos em uma linha em um arquivo .csv.

- [Como combinar e comparar coleções de cadeias de caracteres (LINQ) (C#)](how-to-combine-and-compare-string-collections-linq.md)

  Mostra como combinar listas de cadeias de caracteres de várias maneiras.

- [Como preencher coleções de objetos de várias fontes (LINQ) (C#)](how-to-populate-object-collections-from-multiple-sources-linq.md)

  Mostra como criar coleções de objetos usando vários arquivos de texto como fontes de dados.

- [Como unir conteúdo de arquivos diferentes (LINQ) (C#)](how-to-join-content-from-dissimilar-files-linq.md)
  
  Mostra como combinar cadeias de caracteres em duas listas em uma única cadeia de caracteres usando uma chave correspondente.

- [Como dividir um arquivo em vários arquivos usando grupos (LINQ) (C#)](how-to-split-a-file-into-many-files-by-using-groups-linq.md)
  
  Mostra como criar novos arquivos usando um único arquivo como uma fonte de dados.

- [Como computar valores de coluna em um arquivo de texto CSV (LINQ) (C#)](how-to-compute-column-values-in-a-csv-text-file-linq.md)
  
  Mostra como executar cálculos matemáticos em dados de texto em arquivos .csv.

## <a name="see-also"></a>Confira também

- [LINQ (Consulta Integrada à Linguagem) (C#)](index.md)
- [Como gerar XML de arquivos CSV](how-to-generate-xml-from-csv-files.md)
