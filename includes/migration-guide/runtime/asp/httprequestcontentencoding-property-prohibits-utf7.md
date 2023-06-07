---
ms.openlocfilehash: 7d3568fef933758c40e47cefa86c24d31d4119fc
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619763"
---
### <a name="httprequestcontentencoding-property-prohibits-utf7"></a>A propriedade HttpRequest.ContentEncoding proíbe a UTF7

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.5, a codificação UTF-7 está proibida nos corpos de <xref:System.Web.HttpRequest?displayProperty=fullName>. Os dados para aplicativos que dependem de dados de entrada UTF-7 não serão decodificados corretamente em alguns casos.

#### <a name="suggestion"></a>Sugestão

De modo ideal, os aplicativos devem ser atualizados para não usar a codificação UTF-7 em <xref:System.Web.HttpRequest?displayProperty=fullName>. De modo alternativo, o comportamento herdado pode ser restaurado usando o atributo <code>aspnet:AllowUtf7RequestContentEncoding</code> do elemento [appSettings](~/docs/framework/configure-apps/file-schema/appsettings/appsettings-element-for-configuration.md).

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Web.HttpRequest.ContentEncoding?displayProperty=nameWithType></li></ul>|
