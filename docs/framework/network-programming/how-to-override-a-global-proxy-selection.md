---
title: Como substituir uma seleção de proxy global
description: Siga este exemplo para substituir a seleção de proxy global enviando uma WebRequest para uma URL, que substitui a seleção por um servidor proxy.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 0da481a9-b414-4230-beb0-e3ceba882fe5
ms.openlocfilehash: 91f64775e2f401be9b740fe9e4c41e1087eb9617
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502503"
---
# <a name="how-to-override-a-global-proxy-selection"></a>Como substituir uma seleção de proxy global
Este exemplo envia uma **WebRequest** para `www.contoso.com` que substitui a seleção de proxy global por um servidor proxy chamado `alternateproxy` na porta 80.  
  
## <a name="example"></a>Exemplo  
  
```csharp  
WebRequest req = WebRequest.Create("http://www.contoso.com/");  
req.Proxy = new WebProxy("http://alternateproxy:80/");  
```  
  
```vb  
Dim req As WebRequest = WebRequest.Create("http://www.contoso.com/")  
req.Proxy = New WebProxy("http://alternateproxy:80/")  
```  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Este exemplo requer:  
  
- Uma [ `using` diretiva](../../csharp/language-reference/keywords/using-directive.md) para o namespace **System.net** .  
  
## <a name="see-also"></a>Confira também

- [Usando protocolos de aplicativo](using-application-protocols.md)
- [Acessando a Internet por meio de um proxy](accessing-the-internet-through-a-proxy.md)
