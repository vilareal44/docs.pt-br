---
ms.openlocfilehash: acb5b467fc8f0692d8fa1b3b8263fd27308cc124
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617102"
---
### <a name="webutilityhtmlencode-and-webutilityhtmldecode-round-trip-bmp-correctly"></a>WebUtility.HtmlEncode e WebUtility.HtmlDecode vão e voltam corretamente ao BMP

#### <a name="details"></a>Detalhes

Nos aplicativos destinados ao .NET Framework 4.5, os caracteres fora do BMP (Basic Multilingual Plane) vão e voltam corretamente quando são passados para os métodos <xref:System.Net.WebUtility.HtmlDecode(System.String)>.

#### <a name="suggestion"></a>Sugestão

Essa alteração não deve ter efeito sobre os aplicativos atuais, mas para restaurar o comportamento original, defina o `targetFramework` atributo do `<httpRuntime>` elemento como uma cadeia de caracteres diferente de "4,5". Você também pode definir os atributos `unicodeEncodingConformance` e `unicodeDecodingConformance` do elemento de configuração `<webUtility>` para controlar esse comportamento independentemente da versão de destino do .NET Framework.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.5         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Net.WebUtility.HtmlEncode(System.String)?displayProperty=nameWithType>
- <xref:System.Net.WebUtility.HtmlEncode(System.String,System.IO.TextWriter)?displayProperty=nameWithType>
