---
ms.openlocfilehash: 26a001ec2009a1a66dd9038b9bd3a42d7bcefb73
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619785"
---
### <a name="missing-target-framework-moniker-results-in-40-behavior"></a>Ausência do Moniker da Estrutura de Destino resulta no comportamento da versão 4.0

#### <a name="details"></a>Detalhes

Os aplicativos sem um <xref:System.Runtime.Versioning.TargetFrameworkAttribute?displayProperty=fullName> aplicado no nível de assembly serão executados automaticamente usando a semântica (quirks) do .NET Framework 4.0. Para garantir a alta qualidade, é recomendável que todos os binários sejam explicitamente atribuídos com um <xref:System.Runtime.Versioning.TargetFrameworkAttribute?displayProperty=fullName> indicando a versão do .NET Framework com a qual eles foram criados. Usar um moniker da estrutura de destino em um arquivo de projeto fará com que o MSBuild aplique automaticamente um <xref:System.Runtime.Versioning.TargetFrameworkAttribute?displayProperty=fullName>.

#### <a name="suggestion"></a>Sugestão

O <xref:System.Runtime.Versioning.TargetFrameworkAttribute?displayProperty=fullName> deve ser fornecido, seja por meio da adição do atributo diretamente ao assembly ou da especificação de uma estrutura de destino no [arquivo de projeto, seja por meio da GUI das propriedades do projeto do Visual Studio](https://devblogs.microsoft.com/visualstudio/visual-studio-managed-multi-targeting-part-1-concepts-target-framework-moniker-target-framework/).

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Principal|
|Versão|4.5|
|Type|Runtime|
