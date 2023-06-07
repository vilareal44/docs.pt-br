---
title: Como recuperar uma WebResponse específica de protocolo que corresponde a uma WebRequest
description: Saiba como recuperar um WebResponse específico de protocolo que corresponde a um WebRequest no .NET Framework.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d8c90785-f16b-42a5-8439-ed2f731b2ba8
ms.openlocfilehash: 15b1912a7bd951df7f3c14eb96251c2bdf237b4f
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502451"
---
# <a name="how-to-retrieve-a-protocol-specific-webresponse-that-matches-a-webrequest"></a>Como recuperar uma WebResponse específica de protocolo que corresponde a uma WebRequest
Esse exemplo mostra como recuperar uma WebResponse específica de protocolo que corresponde a uma WebRequest.  
  
## <a name="example"></a>Exemplo  
  
```csharp  
WebRequest req = WebRequest.Create("http://www.contoso.com/");  
WebResponse resp = req.GetResponse();  
```  
  
```vb  
Dim req As WebRequest = WebRequest.Create("http://www.contoso.com")  
Dim resp As WebResponse = req.GetResponse()  
```  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Este exemplo requer:  
  
- Referências ao namespace **System.Net**.  
  
## <a name="see-also"></a>Confira também

- [Solicitando dados](requesting-data.md)
