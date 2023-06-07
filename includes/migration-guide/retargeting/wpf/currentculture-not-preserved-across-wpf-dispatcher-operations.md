---
ms.openlocfilehash: bd656478a55856e676853e57f3e7386ea0aa0211
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614300"
---
### <a name="currentculture-is-not-preserved-across-wpf-dispatcher-operations"></a>CurrentCulture não é preservada entre operações do Dispatcher do WPF

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.6, alterações em <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> ou <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName> feitas em um <xref:System.Windows.Threading.Dispatcher?displayProperty=fullName> serão perdidas ao final dessa operação do dispatcher. Da mesma forma, alterações em <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> ou <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName> feitas fora de uma operação de Dispatcher podem não ser refletidas quando a operação é executada. Em termos práticos, isso significa que alterações em <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> e <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName> podem não fluir entre retornos de chamada de UI do WPF e outro código em um aplicativo do WPF. Isso ocorre devido a uma alteração em <xref:System.Threading.ExecutionContext?displayProperty=fullName> que faz com que <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> e <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName> sejam armazenados no contexto de execução começando com aplicativos destinados ao .NET Framework 4.6. As operações de dispatcher do WPF armazenam o contexto de execução usado para começar a operação e restaurar o contexto anterior quando a operação é concluída. Como <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> e <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName> agora fazem parte desse contexto, alterações nelas em uma operação de dispatcher não são persistidas fora da operação.

#### <a name="suggestion"></a>Sugestão

Os aplicativos afetados por essa alteração poderão contornar esse problema ao armazenar o <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> ou <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName> desejado em um campo e verificar em todos os corpos da operação de Dispatcher (incluindo manipuladores de retorno de chamada do evento de interface do usuário) se o <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> e <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName> corretos foram definidos. Como alternativa, como a alteração de ExecutionContext subjacente a essa alteração do WPF afeta apenas aplicativos destinados ao .NET Framework 4.6 ou mais recente, essa interrupção poderá ser evitada destinando-os ao .NET Framework 4.5.2. Aplicativos destinados ao .NET Framework 4.6 ou posterior também podem contornar esse problema definindo a seguinte opção de compatibilidade:

<pre><code class="lang-csharp">AppContext.SetSwitch(&quot;Switch.System.Globalization.NoAsyncCurrentCulture&quot;, true);&#13;&#10;</code></pre>

Esse problema foi corrigido pelo WPF no .NET Framework 4.6.2. Ele também foi corrigido no .NET Framework 4.6 e 4.6.1 por meio de [KB 3139549](https://support.microsoft.com/kb/3139549). Os aplicativos direcionados ao .NET Framework 4.6 ou posterior terão o comportamento correto automaticamente nos aplicativos WPF – <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>/<xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName>. Isso seria preservado entre as operações de Dispatcher.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.6         |
| Type    | Redirecionando |
