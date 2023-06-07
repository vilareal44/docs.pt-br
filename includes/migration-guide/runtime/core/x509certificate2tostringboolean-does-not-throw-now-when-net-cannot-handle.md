---
ms.openlocfilehash: 51208762cea2a5688c5d43e5d444d4e014e5f0b4
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621035"
---
### <a name="x509certificate2tostringboolean-does-not-throw-now-when-net-cannot-handle-the-certificate"></a>Agora, o X509Certificate2.ToString(Boolean) não é gerado quando o .NET não pode tratar o certificado

#### <a name="details"></a>Detalhes

No .NET Framework 4.5.2 e nas versões anteriores, esse método era gerado quando <code>true</code> era passado para o parâmetro verbose e havia certificados instalados que não eram compatíveis com o .NET Framework. Agora, o método será bem-sucedido e retornará uma cadeia de caracteres válida que omita as partes inacessíveis do certificado.

#### <a name="suggestion"></a>Sugestão

Qualquer código dependente do <xref:System.Security.Cryptography.X509Certificates.X509Certificate2.ToString(System.Boolean)?displayProperty=nameWithType> deve ser atualizado para esperar que a cadeia de caracteres retornada possa excluir alguns dados do certificado (como chave pública, chave privada e extensões) em alguns casos nos quais a API teria sido gerada anteriormente.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.6|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Security.Cryptography.X509Certificates.X509Certificate2.ToString(System.Boolean)?displayProperty=nameWithType></li></ul>|
