---
ms.openlocfilehash: 9c9c4ec55143ef991e9b69a389e0f3368263bb4e
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614303"
---
### <a name="nullreferenceexception-in-exception-handling-code-from-imagesourceconverterconvertfrom"></a>NullReferenceException no código de tratamento de exceções de ImageSourceConverter.ConvertFrom

#### <a name="details"></a>Detalhes

Um erro no código de manipulação de exceções para <xref:System.Windows.Media.ImageSourceConverter.ConvertFrom(System.ComponentModel.ITypeDescriptorContext,System.Globalization.CultureInfo,System.Object)> fez um <xref:System.NullReferenceException?displayProperty=fullName> incorreto ser gerado, em vez da exceção pretendida (<xref:System.IO.DirectoryNotFoundException?displayProperty=fullName> ou <xref:System.IO.FileNotFoundException?displayProperty=fullName>). Essa alteração corrige esse erro, de modo que o método agora gera a exceção certa. <p/>Por padrão todos os aplicativos direcionados ao .NET Framework 4.6.2 e anteriores continuam a gerar <xref:System.NullReferenceException?displayProperty=fullName> para manter a compatibilidade. Os desenvolvedores que direcionarem ao .NET Framework 4.7 e superiores, observação as exceções certas.

#### <a name="suggestion"></a>Sugestão

Os desenvolvedores que desejam reverter para receber <xref:System.NullReferenceException?displayProperty=fullName> quando tiverem o .NET Framework 4.7 ou mais recentes como destino podem adicionar/mesclar o seguinte ao arquivo App.config do aplicativo:

<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Media.ImageSourceConverter.OverrideExceptionWithNullReferenceException=true&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.7         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Windows.Media.ImageSourceConverter.ConvertFrom(System.ComponentModel.ITypeDescriptorContext,System.Globalization.CultureInfo,System.Object)?displayProperty=nameWithType>
