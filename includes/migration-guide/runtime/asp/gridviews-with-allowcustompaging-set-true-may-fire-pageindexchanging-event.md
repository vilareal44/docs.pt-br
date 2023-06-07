---
ms.openlocfilehash: 3b329bf5ba2af4d3ab9c3e203e99daba8ca0d0c0
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619764"
---
### <a name="gridviews-with-allowcustompaging-set-to-true-may-fire-the-pageindexchanging-event-when-leaving-the-final-page-of-the-view"></a>GridViews com AllowCustomPaging definido como verdadeiro pode acionar o evento PageIndexChanging ao deixar a página final do modo de exibição

#### <a name="details"></a>Detalhes

Um bug no .NET Framework 4.5 faz com que <xref:System.Web.UI.WebControls.GridView.PageIndexChanging?displayProperty=fullName>, às vezes, não seja acionado para <xref:System.Web.UI.WebControls.GridView?displayProperty=fullName>s com <xref:System.Web.UI.WebControls.GridView.AllowCustomPaging?displayProperty=fullName> habilitado.

#### <a name="suggestion"></a>Sugestão

Esse problema foi corrigido no .NET Framework 4.6 e pode ser resolvido com o upgrade para essa versão do .NET Framework. Como solução alternativa, o aplicativo pode fazer um BindGrid explícito em qualquer <code>Page_Load</code> que respeite essas condições (o <xref:System.Web.UI.WebControls.GridView?displayProperty=fullName> está na última página e Last<xref:System.Web.UI.WebControls.GridView.PageSize?displayProperty=fullName> é diferente de <xref:System.Web.UI.WebControls.GridView.PageSize?displayProperty=fullName>). Como alternativa, o aplicativo pode ser modificado para permitir a paginação (em vez da paginação personalizada), uma vez que esse cenário não demonstra o problema.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Web.UI.WebControls.GridView.AllowCustomPaging?displayProperty=nameWithType></li></ul>|
