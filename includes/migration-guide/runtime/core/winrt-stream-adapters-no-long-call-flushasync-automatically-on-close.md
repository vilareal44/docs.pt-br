---
ms.openlocfilehash: 60937459b6f18e9abae87ad2dc97c3942325eedc
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619789"
---
### <a name="winrt-stream-adapters-no-long-call-flushasync-automatically-on-close"></a>Adaptadores de fluxo do WinRT não chamam mais FlushAsync automaticamente ao fechar

#### <a name="details"></a>Detalhes

Em aplicativos da Windows Store, os adaptadores de fluxo do Windows Runtime não chamam mais o método de FlushAsync do método Dispose.

#### <a name="suggestion"></a>Sugestão

Essa alteração deve ser transparente. Os desenvolvedores podem restaurar o comportamento anterior gravando um código como este:<pre><code class="lang-csharp">using (var stream = GetWindowsRuntimeStream() as Stream)&#13;&#10;{&#13;&#10;// do something&#13;&#10;await stream.FlushAsync();&#13;&#10;}&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Transparente|
|Versão|4.5.1|
|Type|Runtime|
