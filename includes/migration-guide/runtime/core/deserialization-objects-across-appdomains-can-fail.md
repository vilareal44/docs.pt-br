---
ms.openlocfilehash: 5c949b79eefa68ea6f8d4ad27c716c438e24f170
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619777"
---
### <a name="deserialization-of-objects-across-appdomains-can-fail"></a>Desserialização de objetos em domínios de aplicativo podem falhar

#### <a name="details"></a>Detalhes

Em alguns casos, quando um aplicativo usa dois ou mais domínios de aplicativo com bases de aplicativo diferentes, a tentativa de desserializar objetos no contexto da chamada lógica nos domínios de aplicativo aciona uma exceção.

#### <a name="suggestion"></a>Sugestão

Consulte [Mitigação: desserialização de objetos em domínios de aplicativos](~/docs/framework/migration-guide/mitigation-deserialization-of-objects-across-app-domains.md).

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5.1|
|Type|Runtime|
