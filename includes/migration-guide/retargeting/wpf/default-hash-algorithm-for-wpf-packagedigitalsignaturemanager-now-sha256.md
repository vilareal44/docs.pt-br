---
ms.openlocfilehash: d3e0a61601f60a389b662f6f74934b6e6dc6e663
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614313"
---
### <a name="the-default-hash-algorithm-for-wpf-packagedigitalsignaturemanager-is-now-sha256"></a>O algoritmo de hash padrão para o PackageDigitalSignatureManager do WPF agora é SHA256

#### <a name="details"></a>Detalhes

O `System.IO.Packaging.PackageDigitalSignatureManager` fornece funcionalidade para assinaturas digitais em relação a pacotes do WPF.  No .NET Framework 4.7 e em versões anteriores, o algoritmo padrão (<xref:System.IO.Packaging.PackageDigitalSignatureManager.DefaultHashAlgorithm?displayProperty=nameWithType>) usado para assinar partes de um pacote era SHA1.  Devido a questões de segurança recentes com SHA1, esse padrão foi alterado para o SHA256 a partir do .NET Framework 4.7.1.  Esta alteração afeta a assinatura de todos os pacotes, incluindo documentos XPS.

#### <a name="suggestion"></a>Sugestão

O desenvolvedor que desejará utilizar essa alteração ao direcionar a uma versão do framework abaixo do .NET 4.7.1 ou que precisar da funcionalidade anterior ao direcionar ao .NET 4.7.1 ou posteriores poderá definir o seguinte sinalizador de AppContext adequadamente.  O valor true fará com que SHA1 seja usado como algoritmo padrão; false fará com que SHA256 seja usado.<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.MS.Internal.UseSha1AsDefaultHashAlgorithmForDigitalSignatures=true&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.7.1       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.IO.Packaging.PackageDigitalSignatureManager.DefaultHashAlgorithm?displayProperty=nameWithType>
