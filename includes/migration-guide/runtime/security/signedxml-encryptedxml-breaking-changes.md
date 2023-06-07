---
ms.openlocfilehash: 8cc4f2ba2923774ef4e4e6861a89a7797ca988e1
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621015"
---
### <a name="signedxml-and-encryptedxml-breaking-changes"></a>Alterações de falha de SignedXml e EncryptedXml

#### <a name="details"></a>Detalhes

No .NET Framework 4.6.2, correções de segurança em <xref:System.Security.Cryptography.Xml.SignedXml?displayProperty=fullName> e <xref:System.Security.Cryptography.Xml.EncryptedXml?displayProperty=fullName> levam a comportamentos de tempo de execução diferentes. Por exemplo,<ul><li>Se um documento tiver vários elementos com o mesmo atributo <code>id</code> e uma assinatura tiver como destino um desses elementos como a raiz da assinatura, o documento agora será considerado inválido.</li><li>Documentos que usam algoritmos de transformação XPath não canônicos nas referências agora são considerados inválidos.</li><li>Documentos que usam algoritmos de transformação XSLT não canônicos nas referências agora são considerados inválidos.</li><li>Nenhum programa que usar assinaturas desanexadas de recursos externos poderá fazer isso.</li></ul>

#### <a name="suggestion"></a>Sugestão

Talvez os desenvolvedores queiram revisar o uso de <xref:System.Security.Cryptography.Xml.XmlDsigXsltTransform> e <xref:System.Security.Cryptography.Xml.XmlDsigXsltTransform>, bem como dos tipos derivados de <xref:System.Security.Cryptography.Xml.Transform>, uma vez que o receptor de um documento poderá não conseguir processá-lo.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.6.2|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Security.Cryptography.Xml.Transform?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.Xml.XmlDsigXPathTransform?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.Xml.XmlDsigXsltTransform?displayProperty=nameWithType></li></ul>|
