---
ms.openlocfilehash: 2e13268d4983af5d2fdd6d12b4d9e67d61d07f3d
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621920"
---
### <a name="profiling-aspnet-mvc4-apps-can-lead-to-fatal-execution-engine-error"></a>Aplicativos ASP.NET MVC4 de criação de perfil podem causar Erro Fatal do Mecanismo de Execução

#### <a name="details"></a>Detalhes

Os criadores de perfil que usam NGEN/assemblies de perfil podem causar pane em aplicativos ASP.NET MVC4 de criação de perfil na inicialização com o "Erro Fatal do Mecanismo de Execução".

#### <a name="suggestion"></a>Sugestão

Esse problema foi corrigido no .NET Framework 4.5.2. Como alternativa, o criador de perfil pode evitar esse problema especificando <code>COR_PRF_DISABLE_ALL_NGEN_IMAGES</code> em sua máscara de evento.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime|
