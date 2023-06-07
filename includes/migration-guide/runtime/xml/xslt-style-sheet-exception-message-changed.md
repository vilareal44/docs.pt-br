---
ms.openlocfilehash: 5c27e8acdf30533036546ba4cca9e06877bde362
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619912"
---
### <a name="xslt-style-sheet-exception-message-changed"></a>Mensagem de exceção da folha de estilos XSLT alterada

#### <a name="details"></a>Detalhes

No .NET Framework 4,5, o texto da mensagem de erro quando um arquivo XSLT é muito complexo é &quot; a folha de estilos muito complexa. &quot; Em versões anteriores, a mensagem de erro era &quot; erro de compilação de XSLT. &quot; O código do aplicativo que depende do texto da mensagem de erro não funcionará mais. No entanto, os tipos de exceção permanecem os mesmos, de modo que essa modificação não deve ter um impacto real.

#### <a name="suggestion"></a>Sugestão

Atualize qualquer código de aplicativo, dependendo da mensagem de exceção dessa condição de erro, para esperar a nova mensagem ou (ainda melhor) atualize o código para depender somente do tipo de exceção (<xref:System.Xml.Xsl.XsltException?displayProperty=fullName>), que não foi alterado.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Xml.Xsl.XslCompiledTransform.Load(System.String)?displayProperty=nameWithType></li><li><xref:System.Xml.Xsl.XslCompiledTransform.Load(System.Type)?displayProperty=nameWithType></li><li><xref:System.Xml.Xsl.XslCompiledTransform.Load(System.Xml.XmlReader)?displayProperty=nameWithType></li><li><xref:System.Xml.Xsl.XslCompiledTransform.Load(System.Xml.XPath.IXPathNavigable)?displayProperty=nameWithType></li><li><xref:System.Xml.Xsl.XslCompiledTransform.Load(System.Reflection.MethodInfo,System.Byte[],System.Type[])?displayProperty=nameWithType></li><li><xref:System.Xml.Xsl.XslCompiledTransform.Load(System.String,System.Xml.Xsl.XsltSettings,System.Xml.XmlResolver)?displayProperty=nameWithType></li><li><xref:System.Xml.Xsl.XslCompiledTransform.Load(System.Xml.XmlReader,System.Xml.Xsl.XsltSettings,System.Xml.XmlResolver)?displayProperty=nameWithType></li><li><xref:System.Xml.Xsl.XslCompiledTransform.Load(System.Xml.XPath.IXPathNavigable,System.Xml.Xsl.XsltSettings,System.Xml.XmlResolver)?displayProperty=nameWithType></li></ul>|
