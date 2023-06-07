---
ms.openlocfilehash: 9f6703c77e17ac9376aee944b891f4635dc7632e
ms.sourcegitcommit: 0fa2b7b658bf137e813a7f4d09589d64c148ebf5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86309125"
---
### <a name="winforms-methods-now-throw-argumentexception"></a>Os métodos WinForms agora lançam ArgumentException

Alguns métodos de Windows Forms agora lançam um <xref:System.ArgumentException> para argumentos inválidos, onde anteriormente não fizeram isso.

#### <a name="change-description"></a>Descrição da alteração

Anteriormente, passar argumentos de um tipo inesperado ou incorreto para determinados métodos de Windows Forms resultaria em um estado indeterminado. A partir do .NET 5,0, esses métodos agora geram um <xref:System.ArgumentException> quando passaram argumentos inválidos.

Lançar um <xref:System.ArgumentException> está em conformidade com o comportamento do tempo de execução do .net. Ele também melhora a experiência de depuração ao comunicar-se claramente qual argumento é inválido.

#### <a name="version-introduced"></a>Versão introduzida

.NET 5,0

#### <a name="recommended-action"></a>Ação recomendada

- Atualize o código para evitar a passagem de argumentos inválidos.
- Se necessário, manipule um <xref:System.ArgumentException> ao chamar o método.

#### <a name="category"></a>Categoria

Windows Forms

#### <a name="affected-apis"></a>APIs afetadas

A tabela a seguir lista os métodos e parâmetros afetados:

| Método | Nome do parâmetro | Condição | Versão adicionada |
|-|-|-|-|
| <xref:System.Windows.Forms.TabControl.GetToolTipText(System.Object)?displayProperty=fullName> | `item` | O argumento não é do tipo <xref:System.Windows.Forms.TabPage> . | Preview 1 |
| <xref:System.Windows.Forms.DataFormats.GetFormat(System.String)?displayProperty=fullName> | `format` | Argumento é `null` , <xref:System.String.Empty?displayProperty=nameWithType> ou espaço em branco. | Versão prévia 5 |
| <xref:System.Windows.Forms.InputLanguageChangedEventArgs.%23ctor(System.Globalization.CultureInfo,System.Byte)> | `culture` | Não é possível recuperar um `InputLanguage` para a cultura especificada. | Versão prévia 7 |

<!-- 

#### Affected APIs

- `M:System.Windows.Forms.TabControl.GetToolTipText(System.Object)`
- `M:System.Windows.Forms.DataFormats.GetFormat(System.String)`
- `M:System.Windows.Forms.InputLanguageChangedEventArgs.%23ctor(System.Globalization.CultureInfo,System.Byte)`

-->
