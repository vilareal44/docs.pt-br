---
ms.openlocfilehash: e2ae329d027d605e6331afe422e550990fab1042
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614298"
---
### <a name="default-signedxml-and-signedxms-algorithms-changed-to-sha256"></a>Algoritmos SignedXML e SignedXMS padrão alterados para SHA256

#### <a name="details"></a>Detalhes

No .NET Framework 4.7 e anteriores, SignedXML e SignedCMS usam SHA1 como padrão para algumas operações. A partir do .NET Framework 4.7.1, SHA256 é habilitado por padrão para essas operações. Essa alteração é necessária porque SHA1 não é mais considerado seguro.

#### <a name="suggestion"></a>Sugestão

Há dois novos valores de alternância de contexto para controlar se SHA1 (inseguro) ou SHA256 é usado por padrão:

- Switch.System.Security.Cryptography.Xml.UseInsecureHashAlgorithms
- Switch.System. Security. Cryptography. Pkcs. UseInsecureHashAlgorithms para aplicativos direcionados ao .NET Framework 4.7.1 e versões posteriores, se o uso de SHA256 for indesejável, você poderá restaurar o padrão para SHA1 adicionando a opção de configuração a seguir à seção de [tempo de execução](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md) do seu arquivo de configuração de aplicativo:

```xml
<AppContextSwitchOverrides value="Switch.System.Security.Cryptography.Xml.UseInsecureHashAlgorithms=true;Switch.System.Security.Cryptography.Pkcs.UseInsecureHashAlgorithms=true" />
```

Para aplicativos destinados ao .NET Framework 4.7 e a versões anteriores, é possível aceitar essa alteração adicionando a seguinte opção de configuração à seção do [runtime](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md) do arquivo de configuração de seu aplicativo:

```xml
<AppContextSwitchOverrides value="Switch.System.Security.Cryptography.Xml.UseInsecureHashAlgorithms=false;Switch.System.Security.Cryptography.Pkcs.UseInsecureHashAlgorithms=false" />
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.7.1       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Security.Cryptography.Pkcs.CmsSigner?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.Xml.SignedXml?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.Xml.Reference?displayProperty=nameWithType>
