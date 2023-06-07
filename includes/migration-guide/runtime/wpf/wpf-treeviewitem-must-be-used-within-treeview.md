---
ms.openlocfilehash: af8cb9435be078351e3430dc8ded3f7cac377948
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619904"
---
### <a name="wpf-treeviewitem-must-be-used-within-a-treeview"></a>TreeViewItem do WPF deve ser usado em um TreeView

#### <a name="details"></a>Detalhes

Foi introduzida na versão 4.5 uma alteração que restringe o uso de elementos <xref:System.Windows.Controls.TreeViewItem?displayProperty=fullName> fora de um <xref:System.Windows.Controls.TreeView?displayProperty=fullName>. Isso se manifesta sob as seguintes condições:<ul><li>O pai visual de <xref:System.Windows.Controls.TreeViewItem?displayProperty=fullName> não é um painel. (Um <xref:System.Windows.Controls.TreeViewItem?displayProperty=fullName> gerado para um <xref:System.Windows.Controls.TreeView?displayProperty=fullName> terá um painel como pai)</li><li>O <xref:System.Windows.Controls.TreeViewItem?displayProperty=fullName> é descendente de um <xref:System.Windows.Controls.VirtualizingStackPanel?displayProperty=fullName> que atua como o &quot;host de itens&quot; de um controle de lista (ListBox, DataGrid, ListView etc.). A virtualização não precisa ser habilitada.</li><li>O <xref:System.Windows.Controls.VirtualizingStackPanel?displayProperty=fullName> é a rolagem de itens (<code>ScrollUnit=&quot;Item&quot;</code>).</li><li>Alguém chama <code>VirtualizingStackPanel.MakeVisible(v)</code> para rolar um elemento <code>v</code> no modo de exibição. Isso pode ser feito explícita ou implicitamente de várias maneiras; talvez a maneira mais comum seja simplesmente clicar em <code>v</code> para fornecer a ele o foco do teclado.</li><li>A cadeia de pais visuais de <code>v</code> para o <xref:System.Windows.Controls.VirtualizingStackPanel?displayProperty=fullName> passa pelo <xref:System.Windows.Controls.TreeViewItem?displayProperty=fullName>.</li></ul>Em outras palavras, isso é visto quando um <xref:System.Windows.Controls.TreeViewItem?displayProperty=fullName> é usado fora de um <xref:System.Windows.Controls.TreeView?displayProperty=fullName> e o usuário clica em um descendente do <xref:System.Windows.Controls.TreeViewItem?displayProperty=fullName> para colocá-lo no modo de exibição. Se o <xref:System.Windows.Controls.TreeViewItem?displayProperty=fullName> não tiver descendentes focalizáveis, você nunca verá esse problema. Um exemplo de situação em que isso acontece é quando um <xref:System.Windows.Controls.TreeViewItem?displayProperty=fullName> é a raiz de um DataTemplate. Quando esse problema for atingido, haverá uma InvalidCastException que ocorrerá dentro da estrutura do WPF.

#### <a name="suggestion"></a>Sugestão

Um hotfix será disponibilizado para isso.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime|
