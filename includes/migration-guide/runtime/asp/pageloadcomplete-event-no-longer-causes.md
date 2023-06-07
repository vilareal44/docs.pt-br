---
ms.openlocfilehash: 39d609c955596354d1af28b4ed19d367dab0462b
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619767"
---
### <a name="pageloadcomplete-event-no-longer-causes-systemwebuiwebcontrolsentitydatasource-control-to-invoke-data-binding"></a>O evento Page.LoadComplete não faz mais com que o controle System.Web.UI.WebControls.EntityDataSource invoque a associação de dados

#### <a name="details"></a>Detalhes

O evento <xref:System.Web.UI.Page.LoadComplete> não faz mais com que o controle <xref:System.Web.UI.WebControls.EntityDataSource?displayProperty=fullName> chame a associação de dados para alterações par criar/atualizar/excluir parâmetros. Essa alteração elimina um processamento irrelevante para o banco de dados, impede que os valores dos controles sejam redefinidos e gera um comportamento consistente com outros controles de dados, como <xref:System.Web.UI.WebControls.SqlDataSource?displayProperty=fullName> e <xref:System.Web.UI.WebControls.ObjectDataSource?displayProperty=fullName>. Essa alteração gera um comportamento diferente em um evento improvável no qual os aplicativos dependam da associação de dados no evento <xref:System.Web.UI.Page.LoadComplete>.

#### <a name="suggestion"></a>Sugestão

Se houver necessidade de vinculação de dados, invoque manualmente a associação de dados em um evento que seja anterior no post-back.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime|
