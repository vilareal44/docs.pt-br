---
ms.openlocfilehash: 115cd6be935ae12a1293f948a0f899c6c3ff0d78
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608474"
---
### <a name="winhttphandler-removed-from-net-runtime"></a>WinHttpHandler removido do tempo de execução do .NET

A `WinHttpHandler` classe foi removida do assembly *System.Net.Http.dll* . Agora, ele está disponível apenas como um [pacote NuGet](https://www.nuget.org/packages/System.Net.Http.WinHttpHandler/)fora de banda (OOB).

#### <a name="version-introduced"></a>Versão introduzida

5.0

#### <a name="change-description"></a>Descrição da alteração

Nas versões anteriores do .NET, a <xref:System.Net.Http.WinHttpHandler> classe está disponível como parte das principais bibliotecas do .net. A partir do .NET 5,0, a <xref:System.Net.Http.WinHttpHandler> classe só está disponível como um [pacote NuGet](https://www.nuget.org/packages/System.Net.Http.WinHttpHandler/)instalado separadamente.

#### <a name="recommended-action"></a>Ação recomendada

Instale o [pacote NuGet System .net. http. WinHttpHandler](https://www.nuget.org/packages/System.Net.Http.WinHttpHandler/). Ou, se você não precisar de recursos específicos do WinHTTP, use <xref:System.Net.Http.SocketsHttpHandler> em vez disso.

#### <a name="category"></a>Categoria

Rede

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Net.Http.WinHttpHandler?displayProperty=fullName>

<!--

#### Affected APIs

- `T:System.Net.Http.WinHttpHandler`

-->
