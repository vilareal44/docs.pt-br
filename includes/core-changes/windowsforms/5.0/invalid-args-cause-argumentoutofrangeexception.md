---
ms.openlocfilehash: f42da00487735acdcc60f876c75e1dfd17103008
ms.sourcegitcommit: 1cbd77da54405ea7dba343ac0334fb03237d25d2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84702424"
---
### <a name="winforms-properties-now-throw-argumentoutofrangeexception"></a>As propriedades do WinForms agora geram ArgumentOutOfRangeException

Algumas propriedades de Windows Forms agora lançam um <xref:System.ArgumentOutOfRangeException> para argumentos inválidos, onde anteriormente não.

#### <a name="change-description"></a>Descrição da alteração

Anteriormente, essas propriedades lançavam várias exceções, como <xref:System.NullReferenceException> , <xref:System.IndexOutOfRangeException> ou, <xref:System.ArgumentException> quando passaram argumentos fora do intervalo. A partir do .NET 5,0, essas propriedades agora lançam <xref:System.ArgumentOutOfRangeException> argumentos When passados que estão fora do intervalo.

Lançar um <xref:System.ArgumentOutOfRangeException> está em conformidade com o comportamento do tempo de execução do .net. Ele também melhora a experiência de depuração ao comunicar-se claramente qual argumento é inválido.

#### <a name="version-introduced"></a>Versão introduzida

.NET 5,0

#### <a name="recommended-action"></a>Ação recomendada

- Atualize o código para evitar a passagem de argumentos inválidos.
- Se necessário, manipule um <xref:System.ArgumentOutOfRangeException> ao definir a propriedade.

#### <a name="category"></a>Categoria

Windows Forms

#### <a name="affected-apis"></a>APIs afetadas

A tabela a seguir lista as propriedades e os parâmetros afetados:

> [!div class="mx-tdBreakAll"]
> | Propriedade | Nome do parâmetro | Versão adicionada |
> |-|-|-|
> | <xref:System.Windows.Forms.ListBox.IntegerCollection.Item(System.Int32)?displayProperty=nameWithType> | `index` | 5,0 Preview 5 |
> | <xref:System.Windows.Forms.TreeNode.ImageIndex?displayProperty=nameWithType> | `value` | 5,0 Preview 6 |
> | <xref:System.Windows.Forms.TreeNode.SelectedImageIndex?displayProperty=nameWithType> | `value` | 5,0 Preview 6 |

<!-- 

#### Affected APIs

- `P:System.Windows.Forms.ListBox.IntegerCollection.Item(System.Int32)`
- `P:System.Windows.Forms.TreeNode.ImageIndex`
- `P:System.Windows.Forms.TreeNode.SelectedImageIndex`

-->
