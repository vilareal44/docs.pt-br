---
ms.openlocfilehash: 7d00145bac473bd1799643a723b9e7928a3cd9a2
ms.sourcegitcommit: 97ce5363efa88179dd76e09de0103a500ca9b659
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2020
ms.locfileid: "86281324"
---
### <a name="winforms-methods-now-throw-argumentnullexception"></a>Os métodos WinForms agora lançam ArgumentNullException

Alguns métodos de Windows Forms agora geram um <xref:System.ArgumentNullException> para argumentos nulos, onde anteriormente eles lançavam um <xref:System.NullReferenceException> .

#### <a name="change-description"></a>Descrição da alteração

Anteriormente, determinados métodos de Windows Forms lançavam um <xref:System.NullReferenceException> se passasse um argumento que era nulo. A partir do .NET 5,0, esses métodos agora geram um <xref:System.ArgumentNullException> para argumentos nulos, em vez disso.

Lançar um <xref:System.ArgumentNullException> está em conformidade com o comportamento do tempo de execução do .net. Ele também melhora a experiência de depuração, comunicando claramente que um argumento é nulo e qual é o argumento.

#### <a name="version-introduced"></a>Versão introduzida

.NET 5,0

#### <a name="recommended-action"></a>Ação recomendada

Se você chamar qualquer um desses métodos e o seu código atualmente capturar um <xref:System.NullReferenceException> para argumentos nulos, pegue um <xref:System.ArgumentNullException> em vez disso. Além disso, considere atualizar o código para evitar a passagem de argumentos nulos para os métodos listados.

#### <a name="category"></a>Categoria

Windows Forms

#### <a name="affected-apis"></a>APIs afetadas

A tabela a seguir lista os métodos e parâmetros afetados:

> [!div class="mx-tdBreakAll"]
> | Método | Nome do parâmetro | Versão adicionada |
> |-|-|-|
> | <xref:System.Windows.Forms.Control.ControlCollection.%23ctor(System.Windows.Forms.Control)> | `owner` | Preview 1 |
> | <xref:System.Windows.Forms.TabControl.GetToolTipText(System.Object)?displayProperty=nameWithType> | `item` | Preview 1 |
> | <xref:System.Windows.Forms.TableLayoutControlCollection.%23ctor(System.Windows.Forms.TableLayoutPanel)> | `container` | Preview 1 |
> | <xref:System.Windows.Forms.ToolStripRenderer.OnRenderArrow(System.Windows.Forms.ToolStripArrowRenderEventArgs)?displayProperty=nameWithType> | `e` | Preview 1 |
> | <xref:System.Windows.Forms.ToolStripRenderer.OnRenderItemCheck(System.Windows.Forms.ToolStripItemImageRenderEventArgs)?displayProperty=nameWithType> | `e` | Preview 1 |
> | <xref:System.Windows.Forms.ToolStripRenderer.OnRenderItemImage(System.Windows.Forms.ToolStripItemImageRenderEventArgs)?displayProperty=nameWithType> | `e` | Preview 1 |
> | <xref:System.Windows.Forms.ToolStripRenderer.OnRenderItemText(System.Windows.Forms.ToolStripItemTextRenderEventArgs)?displayProperty=nameWithType> | `e` | Preview 1 |
> | <xref:System.Windows.Forms.ToolStripRenderer.OnRenderStatusStripSizingGrip(System.Windows.Forms.ToolStripRenderEventArgs)?displayProperty=nameWithType> > | `e` | Preview 1 |
> | <xref:System.Windows.Forms.DataGridViewComboBoxEditingControl.ApplyCellStyleToEditingControl(System.Windows.Forms.DataGridViewCellStyle)?displayProperty=nameWithType> | `dataGridViewCellStyle` | Preview 2 |
> | <xref:System.Windows.Forms.RichTextBox.LoadFile(System.IO.Stream,System.Windows.Forms.RichTextBoxStreamType)?displayProperty=nameWithType> | `data` | Preview 2 |
> | <xref:System.Windows.Forms.ListBox.IntegerCollection.%23ctor(System.Windows.Forms.ListBox)> | `owner` | Versão prévia 5 |
> | <xref:System.Windows.Forms.ListBox.IntegerCollection.CopyTo(System.Array,System.Int32)?displayProperty=nameWithType> | `destination` | Versão prévia 5 |
> | <xref:System.Windows.Forms.ListViewGroup.System%23Runtime%23Serialization%23ISerializable%23GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)?displayProperty=nameWithType> | `info` | Versão prévia 5 |
> | <xref:System.Windows.Forms.VisualStyles.VisualStyleRenderer.%23ctor(System.String,System.Int32,System.Int32)> | `className` | Versão prévia 5 |
> | <xref:System.Windows.Forms.ListBox.ObjectCollection.%23ctor(System.Windows.Forms.ListBox)> | `owner` | Versão prévia 6 |
> | <xref:System.Windows.Forms.ListBox.ObjectCollection.%23ctor(System.Windows.Forms.ListBox,System.Object[])> | `owner`, `value` | Versão prévia 6 |
> | <xref:System.Windows.Forms.ListBox.ObjectCollection.%23ctor(System.Windows.Forms.ListBox,System.Windows.Forms.ListBox.ObjectCollection)> | `owner`, `value` | Versão prévia 6 |
> | <xref:System.Windows.Forms.ListBox.ObjectCollection.AddRange(System.Object[])?displayProperty=nameWithType> | `items` | Versão prévia 6 |
> | <xref:System.Windows.Forms.ListBox.ObjectCollection.AddRange(System.Windows.Forms.ListBox.ObjectCollection)?displayProperty=nameWithType> | `value` | Versão prévia 6 |
> | <xref:System.Windows.Forms.ListBox.ObjectCollection.CopyTo(System.Object[],System.Int32)?displayProperty=nameWithType> | `destination` | Versão prévia 6 |
> | <xref:System.Windows.Forms.ListBox.ObjectCollection.System%23Collections%23ICollection%23CopyTo(System.Array,System.Int32)?displayProperty=nameWithType> | `destination` | Versão prévia 6 |
> | <xref:System.Windows.Forms.ListView.SelectedIndexCollection.%23ctor(System.Windows.Forms.ListView)> | `owner` | Versão prévia 7 |
> | <xref:System.Windows.Forms.TreeNodeCollection.Find(System.String,System.Boolean)?displayProperty=nameWithType> | `key`está `null` ou vazio | Visualização 8 |

<!-- 

#### Affected APIs

- `M:System.Windows.Forms.Control.ControlCollection.#ctor(System.Windows.Forms.Control)`
- `M:System.Windows.Forms.TabControl.GetToolTipText(System.Object)`
- `M:System.Windows.Forms.TableLayoutControlCollection.#ctor(System.Windows.Forms.TableLayoutPanel)`
- `M:System.Windows.Forms.ToolStripRenderer.OnRenderArrow(System.Windows.Forms.ToolStripArrowRenderEventArgs)`
- `M:System.Windows.Forms.ToolStripRenderer.OnRenderItemImage(System.Windows.Forms.ToolStripItemImageRenderEventArgs)`
- `M:System.Windows.Forms.ToolStripRenderer.OnRenderItemCheck(System.Windows.Forms.ToolStripItemImageRenderEventArgs)`
- `M:System.Windows.Forms.ToolStripRenderer.OnRenderItemText(System.Windows.Forms.ToolStripItemTextRenderEventArgs)`
- `M:System.Windows.Forms.ToolStripRenderer.OnRenderStatusStripSizingGrip(System.Windows.Forms.ToolStripRenderEventArgs)`
- `M:System.Windows.Forms.DataGridViewComboBoxEditingControl.ApplyCellStyleToEditingControl(System.Windows.Forms.DataGridViewCellStyle)`
- `M:System.Windows.Forms.RichTextBox.LoadFile(System.IO.Stream,System.Windows.Forms.RichTextBoxStreamType)`
- `M:System.Windows.Forms.ListViewGroup.System%23Runtime%23Serialization%23ISerializable%23GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)`
- `M:System.Windows.Forms.VisualStyles.VisualStyleRenderer.%23ctor(System.String,System.Int32,System.Int32)`
- `M:System.Windows.Forms.ListBox.IntegerCollection.%23ctor(System.Windows.Forms.ListBox)`
- `M:System.Windows.Forms.ListBox.IntegerCollection.CopyTo(System.Array,System.Int32)`
- `M:System.Windows.Forms.ListBox.ObjectCollection.%23ctor(System.Windows.Forms.ListBox)`
- `M:System.Windows.Forms.ListBox.ObjectCollection.%23ctor(System.Windows.Forms.ListBox,System.Object[])`
- `M:System.Windows.Forms.ListBox.ObjectCollection.%23ctor(System.Windows.Forms.ListBox,System.Windows.Forms.ListBox.ObjectCollection)`
- `M:System.Windows.Forms.ListBox.ObjectCollection.AddRange(System.Object[])`
- `M:System.Windows.Forms.ListBox.ObjectCollection.AddRange(System.Windows.Forms.ListBox.ObjectCollection)`
- `M:System.Windows.Forms.ListBox.ObjectCollection.CopyTo(System.Object[],System.Int32)`
- `M:System.Windows.Forms.ListBox.ObjectCollection.System%23Collections%23ICollection%23CopyTo(System.Array,System.Int32)`
- `M:System.Windows.Forms.ListView.SelectedIndexCollection.#ctor(System.Windows.Forms.ListView)`
- `M:System.Windows.Forms.TreeNodeCollection.Find(System.String,System.Boolean)`

-->
