---
ms.openlocfilehash: fa5cf2280cdd9535962568a6272d047d261eeba5
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621016"
---
### <a name="rsacngverifyhash-now-returns-false-for-any-verification-failure"></a>RSACng.VerifyHash agora retorna False para qualquer falha de verificação

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.6.2, esse método retornará **False** se a assinatura em si estiver incorretamente formatada. Agora, ele retornará false para qualquer falha de verificação. No .NET Framework 4.6 e 4.6.1, o método gera <xref:System.Security.Cryptography.CryptographicException?displayProperty=fullName> se a assinatura em si está incorretamente formatada.

#### <a name="suggestion"></a>Sugestão

Qualquer código cuja execução dependa da identificação de <xref:System.Security.Cryptography.CryptographicException?displayProperty=fullName> deverá ser executado se a validação falhar e o método retornar **False**.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.6.2|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Security.Cryptography.RSACng.VerifyHash(System.Byte[],System.Byte[],System.Security.Cryptography.HashAlgorithmName,System.Security.Cryptography.RSASignaturePadding)?displayProperty=nameWithType></li></ul>|
