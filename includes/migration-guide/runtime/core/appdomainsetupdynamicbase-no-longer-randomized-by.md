---
ms.openlocfilehash: 08ad6fd4ccb6d5ddbbb4fa7ef1b325ce30689748
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621030"
---
### <a name="appdomainsetupdynamicbase-is-no-longer-randomized-by-userandomizedstringhashalgorithm"></a>AppDomainSetup.DynamicBase não é mais randomizado por UseRandomizedStringHashAlgorithm

#### <a name="details"></a>Detalhes

Antes do .NET Framework 4.6, o valor de <xref:System.AppDomainSetup.DynamicBase> seria randomizado entre domínios de aplicativos ou entre processos se UseRandomizedStringHashAlgorithm estivesse habilitado no arquivo de configuração do aplicativo. A partir do .NET Framework 4.6, <xref:System.AppDomainSetup.DynamicBase> retornará um resultado estável entre instâncias diferentes de um aplicativo em execução e entre diferentes domínios de aplicativo. Bases dinâmicas ainda serão diferentes para diferentes aplicativos. Essa alteração remove apenas o elemento de nomenclatura aleatório para instâncias diferentes do mesmo aplicativo.

#### <a name="suggestion"></a>Sugestão

Lembre-se de que habilitar <code>UseRandomizedStringHashAlgorithm</code> não fará com que <xref:System.AppDomainSetup.DynamicBase> seja randomizado. Se for necessária uma base aleatória, ela deverá ser produzida no código do aplicativo, e não por meio dessa API.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.6|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.AppDomainSetup.DynamicBase?displayProperty=nameWithType></li></ul>|
