---
ms.openlocfilehash: 77e9d28d79a92cf1523e4ef5779d78394b00ae80
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621911"
---
### <a name="net-com-successfully-marshals-byref-safearray-parameters-on-events"></a>O .NET COM realiza marshaling bem-sucedido de parâmetros ByRef SafeArray em eventos

#### <a name="details"></a>Detalhes

No .NET Framework 4.7.2 e versões anteriores, um parâmetro ByRef [SafeArray](https://docs.microsoft.com/windows/desktop/api/oaidl/ns-oaidl-safearray) em um evento COM falharia realizar marshaling de volta para o código nativo.  Com essa alteração, agora o marshal do [SafeArray](https://docs.microsoft.com/windows/desktop/api/oaidl/ns-oaidl-safearray) foi realizado com êxito.<ul><li>[x] Quirked</li></ul>

#### <a name="suggestion"></a>Sugestão

Se a realização de marshal dos parâmetros ByRef SafeArray corretamente em eventos COM interromper a execução, será possível desabilitar esse código adicionando a seguinte opção de configuração à sua configuração de aplicativo:<pre><code class="lang-xml">&lt;appSettings&gt;&#13;&#10;&lt;add key=&quot;Switch.System.Runtime.InteropServices.DoNotMarshalOutByrefSafeArrayOnInvoke&quot; value=&quot;true&quot; /&gt;&#13;&#10;&lt;/appSettings&gt;&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.8|
|Type|Runtime|
