---
ms.openlocfilehash: c996dafcf65b1fd428be908be346de603ead6e0b
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615593"
---
### <a name="certificate-eku-oid-validation"></a>Validação dos certificados de EKU do OID

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.6, as classes <xref:System.Net.Security.SslStream> ou <xref:System.Net.ServicePointManager> executarão a validação do EKU (uso avançado de chave) do OID (identificador de objeto). Uma extensão de EKU (uso avançado de chave) é uma coleção de OIDs (identificadores de objeto) que indicam os aplicativos que usam a chave. A validação do EKU do OID usa retornos de chamada de certificado remoto para garantir que o certificado remoto tenha o OID correto para a finalidade pretendida.

#### <a name="suggestion"></a>Sugestão

Se essa alteração for indesejável, você poderá desabilitar a validação de OID de EKU de certificado adicionando a seguinte opção ao [\<AppContextSwitchOverrides>](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) no [`](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md) de seu arquivo de configuração de aplicativo:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Net.DontCheckCertificateEKUs=true" />
</runtime>
```

> [!IMPORTANT]
> Essa configuração é fornecida apenas para compatibilidade com versões anteriores. Caso contrário, seu uso não é recomendado.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.6         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Net.Security.SslStream?displayProperty=nameWithType>
- <xref:System.Net.ServicePointManager?displayProperty=nameWithType>
- <xref:System.Net.Http.HttpClient?displayProperty=nameWithType>
- <xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType>
- <xref:System.Net.HttpWebRequest?displayProperty=nameWithType>
- <xref:System.Net.FtpWebRequest?displayProperty=nameWithType>
