---
ms.openlocfilehash: 4d410811095786b33580d25f6c6eab3ac2f27148
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85616018"
---
### <a name="resolveassemblyreference-task-now-warns-of-dependencies-with-the-wrong-architecture"></a>A tarefa ResolveAssemblyReference agora avisa sobre dependências com arquitetura incorreta

#### <a name="details"></a>Detalhes

A tarefa emite um aviso, MSB3270, que indica que uma referência ou qualquer uma de suas dependências não corresponde à arquitetura do aplicativo. Por exemplo, isso ocorrerá se um aplicativo que tenha sido compilado com a opção `AnyCPU` incluir uma referência x86. Esse cenário pode resultar em uma falha do aplicativo no tempo de execução (nesse caso, se o aplicativo for implantado como um processo x64).

#### <a name="suggestion"></a>Sugestão

Há duas áreas de impacto:

- A recompilação gerencia os avisos que não foram exibidos quando o aplicativo foi compilado com uma versão anterior do MSBuild. Entretanto, como o aviso identifica uma possível fonte de falha no runtime, ele deve ser investigado e resolvido.
- Se os avisos forem tratados como erros, o aplicativo não será compilado.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.5.1       |
| Type    | Redirecionando |
