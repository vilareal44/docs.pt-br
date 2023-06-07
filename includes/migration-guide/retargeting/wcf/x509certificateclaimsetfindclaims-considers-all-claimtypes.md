---
ms.openlocfilehash: abef47c64a7cfd99c5b50bf2100401a2d5fcc5c3
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614286"
---
### <a name="x509certificateclaimsetfindclaims-considers-all-claimtypes"></a>X509CertificateClaimSet.FindClaims considera todos os claimTypes

#### <a name="details"></a>Detalhes

Em aplicativos destinados ao .NET Framework 4.6.1, se um conjunto de declarações X509 for inicializado de um certificado que tenha várias entradas DNS em seu campo SAN, o método <xref:System.IdentityModel.Claims.X509CertificateClaimSet.FindClaims(System.String,System.String)?displayProperty=fullName> tentará corresponder o argumento claimType a todas as entradas DNS. Em aplicativos destinados a versões anteriores do .NET Framework, o método <xref:System.IdentityModel.Claims.X509CertificateClaimSet.FindClaims(System.String,System.String)?displayProperty=fullName> tenta corresponder o argumento claimType somente com a última entrada DNS.

#### <a name="suggestion"></a>Sugestão

Essa alteração afeta apenas aplicativos destinados ao .NET Framework 4.6.1. Essa alteração pode ser desabilitada (ou habilitada se destinada a versões anteriores a 4.6.1) com a opção de compatibilidade [DisableMultipleDNSEntries](~/docs/framework/migration-guide/mitigation-x509certificateclaimset-findclaims-method.md#mitigation).

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.6.1       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.IdentityModel.Claims.X509CertificateClaimSet.FindClaims(System.String,System.String)?displayProperty=nameWithType>
