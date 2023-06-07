---
ms.openlocfilehash: ae557ce57557d027dba35b7da213c08aee85f2c7
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621909"
---
### <a name="aspnet-fix-handling-of-inputattributes-and-labelattributes-for-webforms-checkbox-control"></a>ASP.NET Corrigir a manipulação de InputAttributes e LabelAttributes para controle CheckBox de WebForms

#### <a name="details"></a>Detalhes

Para aplicativos que direcionam o .NET Framework 4.7.2 e versões anteriores, <xref:System.Web.UI.WebControls.CheckBox.InputAttributes?displayProperty=nameWithType> e <xref:System.Web.UI.WebControls.CheckBox.LabelAttributes?displayProperty=nameWithType> programaticamente adicionados a um controle <xref:System.Web.UI.WebControls.CheckBox> de WebForms são perdidos após o postback. Para aplicativos que direcionam o .NET Framework 4.8 ou versões posteriores, eles são preservados após o postback.

#### <a name="suggestion"></a>Sugestão

Para o comportamento correto para restaurar atributos em postback, defina <code>targetFrameworkVersion</code> como 4.8 ou superior. Por exemplo:<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;system.web&gt;&#13;&#10;&lt;httpRuntime targetFramework=&quot;4.8&quot;/&gt;&#13;&#10;&lt;/system.web&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>Defini-la como mais baixa ou não definir preserva o comportamento antigo incorreto.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Unknown (desconhecido)|
|Versão|4.8|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Web.UI.WebControls.CheckBox?displayProperty=nameWithType></li></ul>|
