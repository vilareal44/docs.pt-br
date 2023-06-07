---
ms.openlocfilehash: 135d59de135c8416d384b221379f912c8a9172ad
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621908"
---
### <a name="aspnet-incorrect-multipart-handling-may-result-in-lost-form-data"></a>ASP.NET A manipulação incorreta com várias partes pode resultar na perda dos dados de formulário.

#### <a name="details"></a>Detalhes

Nos aplicativos que direcionam o .NET Framework 4.7.2 e versões anteriores, o ASP.Net pode analisar incorretamente valores de limite com várias partes, resultando na indisponibilidade dos dados de formulário durante a execução da solicitação. Os aplicativos que direcionam o .NET Framework 4.8 ou versões posteriores analisam corretamente os dados com várias partes para que os valores de formulário estejam disponíveis durante a execução da solicitação.

#### <a name="suggestion"></a>Sugestão

Começando com aplicativos em execução no .NET Framework 4.8, ao direcionar o .NET Framework 4.8 ou posterior usando o elemento <code>targetFrameworkVersion</code>, o comportamento padrão é alterado para retirar os delimitadores. Ao direcionar versões anteriores da estrutura ou não usar <code>targetFrameworkVersion</code>, ainda são retornados delimitadores à direita para alguns valores. Esse comportamento também pode ser explicitamente controlado com um <code>appSetting</code>:<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;appSettings&gt;&#13;&#10;...&#13;&#10;&lt;add key=&quot;aspnet:UseLegacyMultiValueHeaderHandling&quot;  value=&quot;true&quot;/&gt;&#13;&#10;...&#13;&#10;&lt;/appSettings&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Unknown (desconhecido)|
|Versão|4.8|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Web.HttpRequest.Form?displayProperty=nameWithType></li><li><xref:System.Web.HttpRequest.Files?displayProperty=nameWithType></li><li><xref:System.Web.HttpRequest.ContentEncoding?displayProperty=nameWithType></li></ul>|
