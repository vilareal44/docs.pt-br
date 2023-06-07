---
ms.openlocfilehash: 83d3f24680531d1e447f047151a28c98ce0da04b
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619871"
---
### <a name="managed-browser-hosting-controls-from-the-net-framework-11-and-20-are-blocked"></a>Controles de host do navegador gerenciado do .NET Framework 1.1 e 2.0 estão bloqueados

#### <a name="details"></a>Detalhes

Hospedar esses controles é bloqueado no Internet Explorer.

#### <a name="suggestion"></a>Sugestão

O Internet Explorer falhará ao iniciar um aplicativo que usa o navegador gerenciado que hospeda controles. O comportamento anterior pode ser restaurado definindo o valor EnableIEHosting da subchave do registro <code>HKLM/SOFTWARE/MICROSOFT/.NETFramework</code> para <code>1</code> para sistemas x86 e para processos de 32 bits em sistemas de x64, e definindo o valor <code>EnableIEHosting</code> da subchave do registro <code>HKLM/SOFTWARE/Wow6432Node/Microsoft/.NETFramework</code> para <code>1</code> para processos de 64 bits em sistemas de x64.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime|
