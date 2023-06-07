---
ms.openlocfilehash: f4ff938b2d0e9ead481fdf239aed8a6b918a99b0
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619888"
---
### <a name="listboxitem-isselected-binding-issue-with-observablecollectionlttgtmove"></a>Problema de associação de ListBoxItem IsSelected com ObservableCollection&lt;T&gt;.Move

#### <a name="details"></a>Detalhes

Chamar <xref:System.Collections.ObjectModel.ObservableCollection%601.Move(System.Int32,System.Int32)> ou <xref:System.Collections.ObjectModel.ObservableCollection%601.MoveItem(System.Int32,System.Int32)> em uma coleção associada a um <xref:System.Windows.Controls.ListBox?displayProperty=fullName> com itens selecionados pode gerar comportamento imprevisível em seleções ou cancelamentos de seleção futuros de itens <xref:System.Windows.Controls.ListBox?displayProperty=fullName>.

#### <a name="suggestion"></a>Sugestão

Chamar <xref:System.Collections.ObjectModel.Collection%601.Remove(%600)?displayProperty=fullName> e <xref:System.Collections.ObjectModel.Collection%601.Insert(System.Int32,%600)?displayProperty=fullName> em vez de <xref:System.Collections.ObjectModel.ObservableCollection%601.Move(System.Int32,System.Int32)> é uma solução alternativa para esse problema. Como alternativa, esse problema foi corrigido no .NET Framework 4.6 e pode ser resolvido com o upgrade para essa versão do .NET Framework.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Collections.ObjectModel.ObservableCollection%601.Move(System.Int32,System.Int32)?displayProperty=nameWithType></li><li><xref:System.Collections.ObjectModel.ObservableCollection%601.MoveItem(System.Int32,System.Int32)?displayProperty=nameWithType></li></ul>|
