---
ms.openlocfilehash: 863e7035827537e0f943af05c2f0232029b99db8
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617106"
---
### <a name="entity-framework-version-must-match-the-net-framework-version"></a>A versão do Entity Framework deve corresponder à versão do .NET Framework

#### <a name="details"></a>Detalhes

A versão Entity Framework (EF) deve ser correspondida com a versão .NET Framework. O Entity Framework 5 é recomendado para o .NET Framework 4.5. Há alguns problemas conhecidos com o EF 4.x em um projeto do .NET Framework 4.5 em relação a <xref:System.ComponentModel.DataAnnotations>. No .NET Framework 4,5, elas foram movidas para um assembly diferente, portanto, há problemas para determinar quais anotações usar.

#### <a name="suggestion"></a>Sugestão

Atualização para o Entity Framework 5 do .NET Framework 4.5

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Principal       |
| Versão | 4.5         |
| Type    | Redirecionando |
