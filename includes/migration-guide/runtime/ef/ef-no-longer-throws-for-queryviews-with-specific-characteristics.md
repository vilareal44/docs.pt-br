---
ms.openlocfilehash: c6c897c13c84ce4b2be21da18e5702365518f286
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619799"
---
### <a name="ef-no-longer-throws-for-queryviews-with-specific-characteristics"></a>EF não gera mais para QueryViews com características específicas

#### <a name="details"></a>Detalhes

O Entity Framework já não gera uma exceção <xref:System.StackOverflowException?displayProperty=fullName> quando um aplicativo executa uma consulta que envolve QueryView com uma propriedade de navegação de 0..1 que tenta incluir as entidades relacionadas como parte da consulta. Por exemplo, chamando <code>.Include(e =&gt; e.RelatedNavProp)</code>.

#### <a name="suggestion"></a>Sugestão

Essa alteração afeta apenas o código que usa relacionamentos de QueryViews com 1-0..1 ao executar consultas que chamam .Include. Isso melhora a confiabilidade e deve ser transparente para quase todos os aplicativos. No entanto, se causa um comportamento inesperado, é possível desabilitá-lo adicionando a seguinte entrada à seção <code>&lt;appSettings&gt;</code> do arquivo de configuração do aplicativo:<pre><code class="lang-xml">&lt;add key=&quot;EntityFramework_SimplifyUserSpecifiedViews&quot; value=&quot;false&quot; /&gt;&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5.2|
|Type|Runtime|
