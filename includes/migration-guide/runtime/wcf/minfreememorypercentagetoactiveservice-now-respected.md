---
ms.openlocfilehash: f8e5dee9e97956cea78b7c8ec999af1afe9ac66b
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619807"
---
### <a name="minfreememorypercentagetoactiveservice-is-now-respected"></a>MinFreeMemoryPercentageToActiveService agora é respeitado

#### <a name="details"></a>Detalhes

Essa configuração estabelece a memória mínima que deve estar disponível no servidor para que um serviço WCF possa ser ativado. Ela foi projetada para evitar exceções <xref:System.OutOfMemoryException?displayProperty=fullName>. No .NET Framework 4.5, essa configuração não tinha nenhum efeito. No .NET Framework 4.5.1, essa configuração é observada.

#### <a name="suggestion"></a>Sugestão

Ocorrerá uma exceção se a memória livre disponível no servidor Web for menor que a porcentagem definida pela definição de configuração. Alguns serviços WCF iniciados com êxito e executados em um ambiente de memória restrito agora podem falhar.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5.1|
|Type|Runtime|
