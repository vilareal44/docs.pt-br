---
title: Como habilitar uma WebRequest a usar um proxy para se comunicar com a Internet
description: Saiba como criar uma instância de proxy global para permitir que qualquer WebRequest use um proxy para se comunicar com a Internet na .NET Framework.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 63c0ef2c-44b5-4c54-9804-ba0b9b001ac7
ms.openlocfilehash: 0fc33cea3f5a7fe4669b110e53e71afdb9561c23
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502529"
---
# <a name="how-to-enable-a-webrequest-to-use-a-proxy-to-communicate-with-the-internet"></a>Como habilitar uma WebRequest a usar um proxy para se comunicar com a Internet

Este exemplo cria uma instância de proxy global que permitirá que qualquer <xref:System.Net.WebRequest> use um proxy para se comunicar com a Internet. O exemplo supõe que o servidor proxy se chama `webproxy` e que ele se comunica na porta 80, a porta HTTP padrão.

## <a name="example"></a>Exemplo

```csharp
var proxyObject = new WebProxy("http://webproxy:80/");
GlobalProxySelection.Select = proxyObject;
```

```vb
Dim proxyObject As New WebProxy("http://webproxy:80/")
GlobalProxySelection.Select = proxyObject
```

## <a name="compiling-the-code"></a>Compilando o código

Este exemplo requer:

- Uma [ `using` diretiva](../../csharp/language-reference/keywords/using-directive.md) C# para o namespace **System.net** .
- Uma [ `Imports` instrução](../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md) Visual Basic para o namespace **System.net** .

## <a name="see-also"></a>Confira também

- [Usando protocolos de aplicativo](using-application-protocols.md)
- [Acessando a Internet por meio de um proxy](accessing-the-internet-through-a-proxy.md)
