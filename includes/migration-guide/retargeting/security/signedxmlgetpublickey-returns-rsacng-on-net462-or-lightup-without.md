---
ms.openlocfilehash: 23e278d38d6904d8afe927e6b54c388d443e41f5
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85616017"
---
### <a name="signedxmlgetpublickey-returns-rsacng-on-net462-or-lightup-without-retargeting-change"></a>SignedXml.GetPublicKey retorna RSACng em net462 (ou lightup) sem redirecionar a alteração

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.6.2, o tipo concreto de objeto retornado pelo método <xref:System.Security.Cryptography.Xml.SignedXml.GetPublicKey%2A?displayProperty=nameWithType> foi alterado (sem um quirk) de uma implementação de CryptoServiceProvider para uma implementação de Cng. Isso ocorreu porque a implementação foi alterada: do uso de `certificate.PublicKey.Key` para o uso do `certificate.GetAnyPublicKey` interno, que encaminha para <xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPublicKey%2A?displayProperty=nameWithType>.

#### <a name="suggestion"></a>Sugestão

Começando com aplicativos em execução no .NET Framework 4.7.1, é possível usar a implementação de CryptoServiceProvider usada por padrão no .NET Framework 4.6.1 e versões anteriores adicionando a seguinte opção de configuração à seção [runtime](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md) do arquivo de configuração do aplicativo:

```xml
<AppContextSwitchOverrides value="Switch.System.Security.Cryptography.Xml.SignedXmlUseLegacyCertificatePrivateKey=true" />
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.6.2       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Security.Cryptography.Xml.SignedXml.CheckSignatureReturningKey(System.Security.Cryptography.AsymmetricAlgorithm@)?displayProperty=nameWithType>
