---
ms.openlocfilehash: 349854b0dec5a585990b9c5e7c0b575df5bf70f3
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615600"
---
### <a name="xsd-schema-validation-now-correctly-detects-violations-of-unique-constraints-if-compound-keys-are-used-and-one-key-is-empty"></a>A validação do Esquema XSD agora detecta corretamente as violações de restrições exclusivas se chaves compostas forem usadas e uma chave estiver vazia

#### <a name="details"></a>Detalhes

As versões do .NET Framework anteriores a 4.6 tinham um bug que fazia com que a validação de XSD não detectasse restrições exclusivas em chaves compostas se uma das chaves estivesse vazia. No .NET Framework 4.6, esse problema foi corrigido. Isso resultará na validação mais correta, mas também pode resultar na não validação de algum XML que anteriormente seria validado.

#### <a name="suggestion"></a>Sugestão

Se for necessária uma validação menos rigorosa do .NET Framework 4.0, o aplicativo de validação poderá ser destinado à versão 4.5 (ou anterior) do .NET Framework. No entanto, ao redirecionar para o .NET Framework 4.6, será necessário fazer uma revisão de código para garantir que não seja esperado que chaves compostas duplicadas (conforme é descrito na descrição desse problema) sejam validadas.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.6         |
| Type    | Redirecionando |
