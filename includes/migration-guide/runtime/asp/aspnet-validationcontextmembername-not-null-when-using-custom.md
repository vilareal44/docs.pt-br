---
ms.openlocfilehash: 7002c74594993ac6bf28643ef3271da356190c66
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621907"
---
### <a name="aspnet-validationcontextmembername-is-not-null-when-using-custom-dataannotationsvalidationattribute"></a>ASP.NET ValidationContext.MemberName não é NULL ao usar o DataAnnotations.ValidationAttribute personalizado

#### <a name="details"></a>Detalhes

No .NET Framework 4.7.2 e versões anteriores, ao usar um <xref:System.ComponentModel.DataAnnotations.ValidationAttribute?displayProperty=nameWithType> personalizado, a propriedade <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName?displayProperty=nameWithType> retorna `null`. Na versão .NET Framework 4,8 anterior à atualização de outubro de 2019, ela retorna o nome do membro. Começando com [.NET Framework visualização de outubro de 2019 de Rollup de qualidade](https://devblogs.microsoft.com/dotnet/net-framework-october-2019-preview-of-quality-rollup/) para .NET Framework 4,8, ele retorna `null` por padrão, mas você pode optar por retornar o nome do membro em vez disso.

#### <a name="suggestion"></a>Sugestão

Adicione a configuração a seguir ao arquivo *web.config* para que a propriedade retorne o nome do membro em [.NET Framework visualização de outubro de 2019 de Rollup de qualidade](https://devblogs.microsoft.com/dotnet/net-framework-october-2019-preview-of-quality-rollup/) para .NET Framework 4,8 e versões posteriores:<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;appSettings&gt;&#13;&#10;...&#13;&#10;&lt;add key=&quot;aspnet:GetValidationMemberName&quot;  value=&quot;true&quot;/&gt;&#13;&#10;...&#13;&#10;&lt;/appSettings&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>Na versão .NET Framework 4,8 anterior à atualização de outubro de 2019, adicionar isso ao arquivo de *web.config* restaura o comportamento anterior e a propriedade retorna `null` .

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Unknown (desconhecido)|
|Versão|4.8|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName?displayProperty=nameWithType></li></ul>|
