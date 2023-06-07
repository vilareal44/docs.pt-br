---
ms.openlocfilehash: f955e270f709ddf6eea2e44bbcf386e372b9f6e3
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621034"
---
### <a name="targetframeworkname-for-default-app-domain-no-longer-defaults-to-null-if-not-set"></a>TargetFrameworkName para o domínio de aplicativo padrão não será mais padronizado como nulo se não for definido

#### <a name="details"></a>Detalhes

O <xref:System.AppDomainSetup.TargetFrameworkName?displayProperty=fullName> era nulo anteriormente no domínio de aplicativo padrão, a menos que fosse explicitamente definido. A partir da 4.6, a propriedade <xref:System.AppDomainSetup.TargetFrameworkName?displayProperty=fullName> para o domínio de aplicativo padrão terá um valor padrão derivado do TargetFrameworkAttribute (se um estiver presente). Os domínios de aplicativo não padrão continuarão herdando o respectivo <xref:System.AppDomainSetup.TargetFrameworkName?displayProperty=fullName> do domínio de aplicativo padrão (que não será padronizado para nulo na 4.6), a menos que sejam explicitamente substituídos.

#### <a name="suggestion"></a>Sugestão

O código deve ser atualizado para não depender do <xref:System.AppDomainSetup.TargetFrameworkName> que padroniza para nulo. Se for necessário que essa propriedade continue sendo avaliada como nula, ela poderá ser explicitamente definida para esse valor.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.6|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.AppDomainSetup.TargetFrameworkName?displayProperty=nameWithType></li></ul>|
