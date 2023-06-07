---
ms.openlocfilehash: 80e61d4dd5b8d4754a39e95e37475fefff57fcbd
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617103"
---
### <a name="new-ambiguous-dispatcherinvoke-overloads-could-result-in-different-behavior"></a>Sobrecargas novas (ambíguas) do Dispatcher.Invoke podem resultar em comportamento diferente

#### <a name="details"></a>Detalhes

O .NET Framework 4.5 adiciona novas sobrecargas ao <xref:System.Windows.Threading.Dispatcher.Invoke%2A?displayProperty=nameWithType> que incluem um parâmetro do tipo <xref:System.Action>. Quando um código existente é recompilado, os compiladores podem resolver as chamadas aos métodos Dispatcher.Invoke que têm um parâmetro <xref:System.Delegate> como chamadas aos métodos Dispatcher.Invoke com um parâmetro <xref:System.Action>. Se uma chamada à sobrecarga do Dispatcher.Invoke com um parâmetro <xref:System.Delegate> for resolvida como uma chamada à sobrecarga do Dispatcher.Invoke com um parâmetro <xref:System.Action>, poderão ocorrer as seguintes diferenças no comportamento:

- Se ocorrer uma exceção, os eventos <xref:System.Windows.Threading.Dispatcher.UnhandledExceptionFilter> e <xref:System.Windows.Threading.Dispatcher.UnhandledException> não serão gerados. Em vez disso, as exceções são tratadas pelo evento <xref:System.Threading.Tasks.TaskScheduler.UnobservedTaskException?displayProperty=fullName>.
- Chamadas para alguns membros, como <xref:System.Windows.Threading.DispatcherOperation.Result>, são bloqueadas até que a operação seja concluída.

#### <a name="suggestion"></a>Sugestão

Para evitar ambiguidade (e possíveis diferenças no tratamento de exceções ou nos comportamentos de bloqueio), o código que chama o Dispatcher.Invoke pode passar um object[] vazio como um segundo parâmetro à chamada de Invoke para garantir a resolução da sobrecarga do método no .NET Framework 4.0.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.5         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Windows.Threading.Dispatcher.Invoke(System.Delegate,System.Object[])?displayProperty=nameWithType>
- <xref:System.Windows.Threading.Dispatcher.Invoke(System.Delegate,System.TimeSpan,System.Object[])?displayProperty=nameWithType>
- <xref:System.Windows.Threading.Dispatcher.Invoke(System.Delegate,System.TimeSpan,System.Windows.Threading.DispatcherPriority,System.Object[])?displayProperty=nameWithType>
- <xref:System.Windows.Threading.Dispatcher.Invoke(System.Delegate,System.Windows.Threading.DispatcherPriority,System.Object[])?displayProperty=nameWithType>
