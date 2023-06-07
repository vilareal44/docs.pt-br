---
ms.openlocfilehash: 4a65e721e5639f12445a9a44f46baa0d26898a88
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615592"
---
### <a name="il-ret-not-allowed-in-a-try-region"></a>IL ret não é permitido em uma região try

#### <a name="details"></a>Detalhes

Ao contrário do compilador Just-In-Time JIT64, o RyuJIT (usado no .NET Framework 4.6) não permite que uma instrução IL ret em uma região try. Retornar de uma região try não é permitido pela especificação ECMA-335, e nenhum compilador gerenciado conhecido gera tal IL. No entanto, o compilador JIT64 executará essa IL se ela for gerada usando emissão de reflexo.

#### <a name="suggestion"></a>Sugestão

Se um aplicativo estiver gerando uma IL que inclua um opcode ret em uma região try, o aplicativo poderá ser direcionado ao .NET Framework 4.5 para usar o JIT antigo e evitar essa interrupção. Como alternativa, a IL gerada pode ser atualizado para retornar após a região try.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.6         |
| Type    | Redirecionando |
