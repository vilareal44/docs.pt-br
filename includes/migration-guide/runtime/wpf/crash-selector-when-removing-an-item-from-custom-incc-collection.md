---
ms.openlocfilehash: 6da589057cebfbf3f67a46b8d49d3a61f037c4c7
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622222"
---
### <a name="crash-in-selector-when-removing-an-item-from-a-custom-incc-collection"></a>Seletor falha ao remover um item de uma coleção INCC personalizada

#### <a name="details"></a>Detalhes

Um <code>T:System.InvalidOperationException</code> pode ocorrer no seguinte cenário:<ul><li>O ItemsSource de um <code>T:System.Windows.Controls.Primitives.Selector</code> é uma coleção com uma implementação personalizada de <code>T:System.Collections.Specialized.INotifyCollectionChanged</code>.</li><li>O item selecionado é removido da coleção.</li><li>O <code>T:System.Collections.Specialized.NotifyCollectionChangedEventArgs</code> tem <code>P:System.Collections.Specialized.NotifyCollectionChangedEventArgs.OldStartingIndex</code> = -1, o que indica uma posição desconhecida.</li></ul>A pilha de chamadas da exceção começa em System.Windows.Threading.Dispatcher.VerifyAccess() em System.Windows.DependencyObject.GetValue(DependencyProperty dp) em System.Windows.Controls.Primitives.Selector.GetIsSelected(DependencyObject element). Essa exceção poderá ocorrer no .NET Framework 4.5 se o aplicativo tiver mais que um thread Dispatcher. No .NET Framework 4.7, a exceção também pode ocorrer em aplicativos com um único thread Dispatcher. O problema foi corrigido no .NET Framework 4.7.1.

#### <a name="suggestion"></a>Sugestão

Atualizar para o .NET Framework 4.7.1.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.7|
|Type|Runtime|
