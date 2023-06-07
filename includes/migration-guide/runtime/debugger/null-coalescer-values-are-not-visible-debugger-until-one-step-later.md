---
ms.openlocfilehash: 907c4aa5573c392a68afad0a4d937eadcd556440
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619793"
---
### <a name="null-coalescer-values-are-not-visible-in-debugger-until-one-step-later"></a>Valores nulos de união não são visíveis no depurador até uma etapa posterior

#### <a name="details"></a>Detalhes

Um bug no .NET Framework 4.5 faz com que os valores definidos por meio de uma operação de união nula não fiquem visíveis no depurador imediatamente após a operação de atribuição ser executada na versão de 64 bits do Framework.

#### <a name="suggestion"></a>Sugestão

Colocar um tempo adicional no depurador fará com que o valor do local/campo seja atualizado corretamente. Além disso, esse problema foi corrigido no .NET Framework 4.6 e pode ser resolvido com o upgrade para essa versão do .NET Framework.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime|
