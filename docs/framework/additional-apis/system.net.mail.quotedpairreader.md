---
title: Classe QuotedPairReader (System.Net)
ms.date: 06/12/2020
ms.technology: dotnet-networking
topic_type:
- apiref
api_name:
- System.Net.Mail.QuotedPairReader
- System.Net.Mail.QuotedPairReader.CountQuotedChars
api_location:
- System.dll
api_type:
- Assembly
ms.openlocfilehash: 898c6e9d2d5dd02f3d5f9c096ad470b5dd445d1d
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051305"
---
# <a name="quotedpairreader-class"></a>Classe QuotedPairReader

Determina quais caracteres são colocados entre aspas (escape) em uma cadeia de caracteres entre aspas. Esta classe não pode ser herdada.

```csharp
internal static class QuotedPairReader
```

> [!WARNING]
> Essa classe é interna e não deve ser usada diretamente no seu código.
>
> A Microsoft não oferece suporte ao uso dessa classe em um aplicativo de produção em nenhuma circunstância.

## <a name="countquotedchars-method"></a>Método CountQuotedChars

Conta o número de caracteres entre aspas consecutivas, incluindo várias barras invertidas precedentes, na cadeia de caracteres especificada. Por exemplo, considerando a cadeia `a\\\b` de caracteres e um índice de `4` , o método retorna `4` , porque `b` está entre aspas e, portanto, as três barras invertidas precedentes.

```csharp
internal static int CountQuotedChars(string data, int index, bool permitUnicodeEscaping)
```

### <a name="parameters"></a>Parâmetros

- `data` <xref:System.String>

  A cadeia de dados na qual contar caracteres entre aspas consecutivas.

- `index` <xref:System.Int32>

  A posição na cadeia de caracteres especificada até e incluindo quais caracteres entre aspas consecutivas devem ser contados.

- `permitUnicodeEscaping` <xref:System.Boolean>

  `true`para permitir que os caracteres Unicode sejam ignorados; caso contrário, `false` .

### <a name="return-value"></a>Retornar valor

<xref:System.Int32?displayProperty=nameWithType>

`0`Se o caractere no índice especificado não for de escape; caso contrário, o número de caracteres entre aspas consecutivas até e incluindo o caractere em `index` .

### <a name="exceptions"></a>Exceções

<xref:System.FormatException?displayProperty=nameWithType>

Um caractere Unicode de escape foi encontrado, mas não é permitido.

## <a name="requirements"></a>Requisitos

**Namespace:** <xref:System.Net>

**Assembly:** Sistema (em System.dll)
