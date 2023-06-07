---
ms.openlocfilehash: 78faa5f4008b41bac75c94ce09a58c8227e5b485
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614275"
---
### <a name="currentculture-and-currentuiculture-flow-across-tasks"></a>Fluxo CurrentCulture e CurrentUICulture atravessam tarefas

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.6, <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> e <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName> são armazenados no <xref:System.Threading.ExecutionContext?displayProperty=fullName> do thread, que atravessa operações assíncronas. Isso significa que as alterações em <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> ou <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName> serão refletidas em tarefas que posteriormente serão executadas de forma assíncrona. Isso é diferente do comportamento das versões anteriores do .NET Framework (que redefiniria <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> e <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName> em todas as tarefas assíncronas).

#### <a name="suggestion"></a>Sugestão

Aplicativos afetados por essa alteração podem contorná-la definindo explicitamente o <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> ou <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName> desejado como a primeira operação em uma Tarefa assíncrona. Como alternativa, o comportamento antigo (de não atravessar <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>/<xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName>) pode ser aceito definindo a seguinte opção de compatibilidade:

```csharp
AppContext.SetSwitch("Switch.System.Globalization.NoAsyncCurrentCulture", true);
```

Esse problema foi corrigido pelo WPF no .NET Framework 4.6.2. Ele também foi corrigido no .NET Framework 4.6 e 4.6.1 por meio de [KB 3139549](https://support.microsoft.com/kb/3139549). Os aplicativos direcionados ao .NET Framework 4.6 ou posterior terão o comportamento correto automaticamente nos aplicativos WPF – <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>/<xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=fullName>. Isso seria preservado entre as operações de Dispatcher.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.6         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=nameWithType>
- <xref:System.Threading.Thread.CurrentCulture?displayProperty=nameWithType>
- <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=nameWithType>
- <xref:System.Threading.Thread.CurrentUICulture?displayProperty=nameWithType>
