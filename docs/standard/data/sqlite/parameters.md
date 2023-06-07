---
title: Parâmetros
ms.date: 12/13/2019
description: Saiba como usar parâmetros SQL.
ms.openlocfilehash: b24610a5cb65e2b24171452acef9bf55b4995431
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768944"
---
# <a name="parameters"></a>Parâmetros

Os parâmetros são usados para proteger contra ataques de injeção de SQL. Em vez de concatenar a entrada do usuário com instruções SQL, use parâmetros para garantir que a entrada seja tratada apenas como um valor literal e nunca executada. No SQLite, os parâmetros são geralmente permitidos em qualquer lugar que um literal é permitido em instruções SQL.

Os parâmetros podem ser prefixados com um `:` , `@` ou `$` .

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/HelloWorldSample/Program.cs?name=snippet_Parameter)]

Consulte [tipos de dados](types.md) para obter detalhes sobre como os valores do .NET são mapeados para os valores do SQLite.

## <a name="truncation"></a>Truncation

Use a <xref:Microsoft.Data.Sqlite.SqliteParameter.Size> propriedade para truncar valores de texto e BLOB.

```csharp
// Truncate name to 30 characters
command.Parameters.AddWithValue("$name", name).Size = 30;
```

## <a name="alternative-types"></a>Tipos alternativos

Às vezes, talvez você queira usar um tipo SQLite alternativo. Faça isso definindo a <xref:Microsoft.Data.Sqlite.SqliteParameter.SqliteType> propriedade.

Os mapeamentos de tipo alternativo a seguir podem ser usados. Para os mapeamentos padrão, consulte [tipos de dados](types.md).

| Valor          | Sqlitetype | Comentários          |
| -------------- | ---------- | ---------------- |
| Char           | Integer    | UTF-16           |
| Datetime       | Real       | Valor do dia Juliano |
| DateTimeOffset | Real       | Valor do dia Juliano |
| Guid           | Blob       |                  |
| TimeSpan       | Real       | Em dias          |

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/DateAndTimeSample/Program.cs?name=snippet_SqliteType)]

## <a name="output-parameters"></a>Parâmetros de saída

O SQLite não dá suporte a parâmetros de saída. Em vez disso, retorna valores nos resultados da consulta.

## <a name="see-also"></a>Veja também

* [Tipos de dados](types.md)
