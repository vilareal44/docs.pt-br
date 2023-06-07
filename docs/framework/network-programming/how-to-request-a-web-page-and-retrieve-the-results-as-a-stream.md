---
title: Como solicitar uma página da Web e recuperar os resultados como um fluxo
description: Este exemplo mostra como solicitar uma página da Web e recuperar os resultados em um fluxo no .NET Framework.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d32b7f35-29d8-4fb7-ad71-d219edc5e359
ms.openlocfilehash: bd57f9af6be29c783d044e785ebb36aaa8592df2
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502477"
---
# <a name="how-to-request-a-web-page-and-retrieve-the-results-as-a-stream"></a>Como solicitar uma página da Web e recuperar os resultados como um fluxo

Este exemplo mostra como solicitar uma página da Web e recuperar os resultados em um fluxo.
  
## <a name="example"></a>Exemplo

```csharp
var myClient = new WebClient();
Stream response = myClient.OpenRead("https://docs.microsoft.com/dotnet/");
// The stream data is used here.
response.Close();
```

```vb
Dim myClient As New WebClient()
Dim response As Stream = myClient.OpenRead("https://docs.microsoft.com/dotnet/")
' The stream data is used here.
response.Close()
```

## <a name="compiling-the-code"></a>Compilando o código

 Este exemplo requer:

- Referências aos namespaces <xref:System.IO> e <xref:System.Net>.

## <a name="see-also"></a>Confira também

- [Solicitando dados](requesting-data.md)
