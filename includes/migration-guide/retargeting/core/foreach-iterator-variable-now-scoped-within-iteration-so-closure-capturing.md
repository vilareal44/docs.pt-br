---
ms.openlocfilehash: 9a2d6a25a8ab1b8bf65b947557802e0805a7f826
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617107"
---
### <a name="foreach-iterator-variable-is-now-scoped-within-the-iteration-so-closure-capturing-semantics-are-different-in-c5"></a>A variável do iterador Foreach agora tem escopo na iteração, de modo que as semânticas de captura de fechamento são diferentes (no C#5)

#### <a name="details"></a>Detalhes

A partir do C# 5 (Visual Studio 2012), as variáveis do iterador `foreach` têm escopo dentro da iteração. Isso poderá causar interrupções se o código anteriormente dependia das variáveis para não ser incluído no fechamento de `foreach`. O sintoma dessa alteração será que uma variável do iterador passada a um delegado será tratada com o valor que ela tinha no momento em que o delegado foi criado, e não com o valor que ela tinha no momento em que o delegado foi invocado.

#### <a name="suggestion"></a>Sugestão

De maneira ideal, o código deve ser atualizado para esperar o novo comportamento do compilador. Se as semânticas antigas forem necessárias, a variável do iterador poderá ser substituída por uma variável separada que seja explicitamente colocada fora do escopo do loop.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Principal       |
| Versão | 4.5         |
| Type    | Redirecionando |
