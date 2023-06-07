---
ms.openlocfilehash: e4d9efe7d2a06a1e501b070c23184dcd913dba71
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621029"
---
### <a name="persian-calendar-now-uses-the-hijri-solar-algorithm"></a>O calendário persa agora usa o algoritmo solar islâmico

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.6, a classe <xref:System.Globalization.PersianCalendar?displayProperty=fullName> usa o algoritmo solar islâmico. Converter datas entre o <xref:System.Globalization.PersianCalendar?displayProperty=fullName> e outros calendários pode produzir um resultado um pouco diferente começando com o .NET Framework 4.6 para datas anteriores a 1800 ou posteriores a 2023 (gregoriano). Além disso, agora <xref:System.Globalization.PersianCalendar.MinSupportedDateTime?displayProperty=nameWithType> é <code>March 22, 0622</code> em vez de <code>March 21, 0622</code>.

#### <a name="suggestion"></a>Sugestão

Lembre-se de que algumas datas anteriores ou posteriores podem ser um pouco diferentes quando se usa o PersianCalendar no .NET Framework 4.6. Além disso, ao serializar datas entre processos que podem ser executados em diferentes versões do .NET Framework, não armazene-as como cadeias de caracteres de data PersianCalendar (uma vez que esses valores podem ser diferentes).

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.6|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Globalization.PersianCalendar?displayProperty=nameWithType></li></ul>|
