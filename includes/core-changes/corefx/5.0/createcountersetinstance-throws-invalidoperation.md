---
ms.openlocfilehash: fabbc9a51f61a703ce97db50ab251e7a13ffe348
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84144469"
---
### <a name="countersetcreatecountersetinstance-now-throws-invalidoperationexception-if-instance-already-exists"></a>CounterSet. CreateCounterSetInstance agora lança InvalidOperationException se a instância já existe

A partir do .NET 5,0, <xref:System.Diagnostics.PerformanceData.CounterSet.CreateCounterSetInstance(System.String)?displayProperty=nameWithType> o lançará um <xref:System.InvalidOperationException> em vez de um <xref:System.ArgumentException> se o conjunto de contadores já existir.

#### <a name="change-description"></a>Descrição da alteração

No .NET Framework e no .NET Core 1,0 a 3,1, você pode criar uma instância do conjunto de contadores chamando <xref:System.Diagnostics.PerformanceData.CounterSet.CreateCounterSetInstance%2A> . No entanto, se o conjunto de contadores já existir, o método lançará uma <xref:System.ArgumentException> exceção.

No .NET 5,0 e versões posteriores, quando você chama <xref:System.Diagnostics.PerformanceData.CounterSet.CreateCounterSetInstance%2A> e o conjunto de contadores existe, uma <xref:System.InvalidOperationException> exceção é lançada.

#### <a name="version-introduced"></a>Versão introduzida

5,0 Preview 5

#### <a name="recommended-action"></a>Ação recomendada

Se você capturar <xref:System.ArgumentException> exceções em seu aplicativo ao chamar <xref:System.Diagnostics.PerformanceData.CounterSet.CreateCounterSetInstance%2A> , considere também capturar <xref:System.InvalidOperationException> exceções.

> [!NOTE]
> <xref:System.ArgumentException>Não é recomendável capturar exceções.

#### <a name="category"></a>Categoria

Bibliotecas principais do .NET

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Diagnostics.PerformanceData.CounterSet.CreateCounterSetInstance%2A?displayProperty=fullName>

<!--

#### Affected APIs

- `M:System.Diagnostics.PerformanceData.CounterSet.CreateCounterSetInstance(System.String)`

-->
