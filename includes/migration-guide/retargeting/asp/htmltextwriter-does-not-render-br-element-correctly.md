---
ms.openlocfilehash: 8f03e5166e7f1f598e9bba7fb8c550809f287b82
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615588"
---
### <a name="htmltextwriter-does-not-render-br-element-correctly"></a>HtmlTextWriter não renderiza o elemento `<br/>` corretamente

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.6, chamar <xref:System.Web.UI.HtmlTextWriter.RenderBeginTag(System.String)> e <xref:System.Web.UI.HtmlTextWriter.RenderEndTag> com um elemento `<BR />` vai inserir corretamente apenas um `<BR />` (em vez de dois)

#### <a name="suggestion"></a>Sugestão

Se um aplicativo depender da marca `<BR />` extra, <xref:System.Web.UI.HtmlTextWriter.RenderBeginTag(System.String)> deverá ser chamado uma segunda vez. Observe que essa alteração de comportamento afeta apenas aplicativos destinados ao .NET Framework 4.6 ou versões posteriores, de modo que outra opção é destinar a uma versão anterior do .NET Framework a fim de obter o comportamento antigo.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.6         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Web.UI.HtmlTextWriter.RenderBeginTag(System.String)?displayProperty=nameWithType>
- <xref:System.Web.UI.HtmlTextWriter.RenderBeginTag(System.Web.UI.HtmlTextWriterTag)?displayProperty=nameWithType>
