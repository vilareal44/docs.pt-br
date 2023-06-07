---
ms.openlocfilehash: 35fc4782ebbba33f5fc6668712af9d89d00cafe9
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614418"
---
### <a name="wpf-changing-a-primary-key-when-displaying-ado-data-in-a-masterdetail-scenario"></a>WPF alterando uma chave primária ao exibir dados ADO em um cenário de detalhes/mestre

#### <a name="details"></a>Detalhes

Suponha que você tenha uma coleção ADO de itens do tipo `Order`, com uma relação denominada &quot;OrderDetails&quot;, relacionando-a a uma coleção de itens do tipo `Detail` por meio da chave primária &quot;OrderID&quot;. No seu aplicativo WPF, você pode vincular um controle de lista aos detalhes de determinado pedido:

```xaml
<ListBox ItemsSource="{Binding Path=OrderDetails}" >
```

em que o DataContext é um `Order`. O WPF Obtém o valor da `OrderDetails` Propriedade – uma coleção D de todos os `Detail` itens cujo `OrderID` corresponde ao `OrderID` do item mestre. A alteração de comportamento surge quando você altera a chave primária `OrderID` do item mestre. O ADO altera automaticamente a `OrderID` de cada um dos registros afetados na coleção Detalhes (ou seja, os que foram copiados na coleção D).  Mas o que acontece com D?

- Comportamento antigo: a coleção D está desmarcada. O item mestre *não* gera uma notificação de alteração para a propriedade `OrderDetails`. O ListBox continua a usar a coleção D, que agora está vazia.
- Novo comportamento: a coleção D fica inalterada. Cada um dos seus itens gera uma notificação de alteração para a propriedade `OrderID`. O ListBox continua a usar a coleção D e exibe os detalhes com a nova `OrderID`. O WPF implementa o novo comportamento criando a coleção D de uma maneira diferente: chamando o método ADO <xref:System.Data.DataRowView.CreateChildView(System.Data.DataRelation,System.Boolean)?displayProperty=nameWithType> com o argumento `followParent` definido como `true`.

#### <a name="suggestion"></a>Sugestão

Um aplicativo obtém o novo comportamento usando o comutador AppContext a seguir.

```xml
<configuration>
  <runtime>
    <AppContextSwitchOverrides value="Switch.System.Windows.Data.DoNotUseFollowParentWhenBindingToADODataRelation=false"/>
  </runtime>
</configuration>
```

O padrão do comutador é `true` (comportamento antigo) para aplicativos direcionados para o .NET 4.7.1 ou anterior e `false` (novo comportamento) para aplicativos direcionados para o .NET 4.7.2 ou posterior.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.7.2       |
| Type    | Redirecionando |
