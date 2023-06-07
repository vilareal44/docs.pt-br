---
ms.openlocfilehash: 00ffc782c9a15c0d88f797f52372b4658706b350
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619880"
---
### <a name="glyphruncomputeinkboundingbox-and-formattedtextextent-return-different-values-beginning-in-net-framework-45"></a>GlyphRun.ComputeInkBoundingBox() e FormattedText.Extent retornam valores diferentes começando no .NET Framework 4.5

#### <a name="details"></a>Detalhes

Foram feitas melhorias em <xref:System.Windows.Media.GlyphRun.ComputeInkBoundingBox> e <xref:System.Windows.Media.FormattedText.Extent> do .NET Framework 4.5 para resolver problemas em que as caixas eram muito pequenas para o glifos contidos em alguns casos no .NET Framework 4.0. Como resultado, algumas caixas delimitadoras serão maiores a partir do .NET Framework 4.5, resultando em diferenças sutis no layout da interface do usuário.

#### <a name="suggestion"></a>Sugestão

Lembre-se de que alguns tamanhos de caixa delimitadora do glifo aumentaram. Essas alterações geralmente aprimorarão a apresentação e os testes de caixa de clique, mas se o comportamento anterior (anterior ao .NET 4.5) for desejado, ele poderá ser aceito com a adição da seguinte entrada ao arquivo app.config:<pre><code class="lang-xml">&lt;appsettings&gt;&#13;&#10;&lt;add key=&quot;IncludeAllInkInBoundingBox&quot; value=&quot;false&quot;&gt;&#13;&#10;&lt;/appsettings&gt;&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Windows.Media.GlyphRun.ComputeInkBoundingBox?displayProperty=nameWithType></li><li><xref:System.Windows.Media.FormattedText.Extent?displayProperty=nameWithType></li></ul>|
