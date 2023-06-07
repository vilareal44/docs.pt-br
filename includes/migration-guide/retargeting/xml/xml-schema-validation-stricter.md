---
ms.openlocfilehash: 4a22d78f2308aab544d7a7d2e4d05ddc1ad5457d
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617113"
---
### <a name="xml-schema-validation-is-stricter"></a>A validação do esquema XML está mais rigorosa

#### <a name="details"></a>Detalhes

No .NET Framework 4.5, a validação do esquema XML está mais rigorosa. Se você usar xsd:anyURI para validar um URI como um protocolo mailto, a validação falhará se houver espaços no URI. Nas versões anteriores do .NET Framework, a validação foi bem-sucedida. A alteração afeta somente aplicativos destinados ao .NET Framework 4.5.

#### <a name="suggestion"></a>Sugestão

Se for necessária uma validação menos rigorosa do .NET Framework 4.0, o aplicativo de validação poderá se destinar à versão 4.0 do .NET Framework. No entanto, ao direcionar ao .NET Framework 4.5, é necessário revisar o código para verificar se URIs inválidos (com espaços) não são esperados como valores de atributo com o tipo de dados anyURI.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.5         |
| Type    | Redirecionando |
