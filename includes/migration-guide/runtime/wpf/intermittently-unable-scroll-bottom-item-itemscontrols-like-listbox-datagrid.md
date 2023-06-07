---
ms.openlocfilehash: b92dc8a1c48e83846c3d9a1d86e66629f31b7722
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621918"
---
### <a name="intermittently-unable-to-scroll-to-bottom-item-in-itemscontrols-like-listbox-and-datagrid-when-using-custom-datatemplates"></a>Não é possível rolar intermitentemente para o item na parte inferior em ItemsControls (como ListBox e DataGrid) ao usar DataTemplates personalizados

#### <a name="details"></a>Detalhes

Em alguns casos, um bug no .NET Framework 4.5 está fazendo com que ItemsControls (como <xref:System.Windows.Controls.ListBox?displayProperty=fullName>, <xref:System.Windows.Controls.ComboBox?displayProperty=fullName>, <xref:System.Windows.Controls.DataGrid?displayProperty=fullName> etc.) não rolem para o item na parte inferior ao usar DataTemplates personalizados. Se houver uma tentativa de rolagem pela segunda vez (depois da rolagem de volta), ela funcionará.

#### <a name="suggestion"></a>Sugestão

Esse problema foi corrigido no .NET Framework 4.5.2 e pode ser solucionado com o upgrade para essa versão (ou uma versão posterior) do .NET Framework. Como alternativa, os usuários ainda podem arrastar as barras de rolagem para os itens finais nessas coleções, mas pode ser necessário tentar duas vezes para obter êxito.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime|
