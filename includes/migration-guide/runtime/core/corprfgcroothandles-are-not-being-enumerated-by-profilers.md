---
ms.openlocfilehash: 4f52535e54607cc824500f9c6a14964a1dec9d32
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619776"
---
### <a name="cor_prf_gc_root_handles-are-not-being-enumerated-by-profilers"></a>COR_PRF_GC_ROOT_HANDLEs não estão sendo enumerados por criadores de perfil

#### <a name="details"></a>Detalhes

No .NET Framework v4.5.1, a API de criação de perfil <code>RootReferences2()</code>, de maneira incorreta, nunca retorna <code>COR_PRF_GC_ROOT_HANDLE</code> (que, em vez disso, é retornada como <code>COR_PRF_GC_ROOT_OTHER</code>). Esse problema foi corrigido a partir do .NET Framework 4.6.

#### <a name="suggestion"></a>Sugestão

Esse problema foi corrigido no .NET Framework 4.6 e pode ser resolvido com o upgrade para essa versão do .NET Framework.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5.1|
|Type|Runtime|
