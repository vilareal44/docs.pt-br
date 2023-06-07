---
ms.openlocfilehash: c1e85941c8c6c31c7d937d0671ad955fa6490783
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614272"
---
### <a name="rsacng-now-correctly-loads-rsa-keys-of-non-standard-key-size"></a>Agora, RSACng carrega corretamente as chaves RSA de tamanho não padrão

#### <a name="details"></a>Detalhes

Nas versões do .NET Framework anteriores a 4.6.2, os clientes com tamanhos de chave não padrão para certificados RSA não conseguiam acessá-las por meio dos métodos de extensão <xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPublicKey(System.Security.Cryptography.X509Certificates.X509Certificate2)?displayProperty=fullName> e <xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPrivateKey(System.Security.Cryptography.X509Certificates.X509Certificate2)?displayProperty=fullName>.  Uma <xref:System.Security.Cryptography.CryptographicException?displayProperty=fullName> com uma mensagem &quot;O tamanho de chave solicitado não é compatível&quot; é gerada. Esse problema foi corrigido no .NET Framework 4.6.2. Da mesma forma, <xref:System.Security.Cryptography.RSA.ImportParameters(System.Security.Cryptography.RSAParameters)> e <xref:System.Security.Cryptography.RSACng.ImportParameters(System.Security.Cryptography.RSAParameters)> agora funcionam com tamanhos de chave não padrão sem gerar um <xref:System.Security.Cryptography.CryptographicException?displayProperty=fullName>.

#### <a name="suggestion"></a>Sugestão

Se houver qualquer lógica de tratamento de exceções dependente do comportamento anterior, em que um <xref:System.Security.Cryptography.CryptographicException?displayProperty=fullName> é gerado quando tamanhos de chave não padrão são usados, considere remover essa lógica.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.6.2       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Security.Cryptography.RSA.ImportParameters(System.Security.Cryptography.RSAParameters)?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.RSACng.ImportParameters(System.Security.Cryptography.RSAParameters)?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPrivateKey(System.Security.Cryptography.X509Certificates.X509Certificate2)?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPublicKey(System.Security.Cryptography.X509Certificates.X509Certificate2)?displayProperty=nameWithType>
