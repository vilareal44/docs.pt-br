---
ms.openlocfilehash: 62702de022656e45466a45f4150e518226a3fecc
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621913"
---
### <a name="improvements-to-grid-star-rows-space-allocating-algorithm"></a>Melhorias no algoritmo de alocação de espaço de linhas de estrela da grade

#### <a name="details"></a>Detalhes

Correção de um bug no [algoritmo para alocar tamanhos](https://github.com/Microsoft/dotnet/blob/master/Documentation/compatibility/wpf-grid-allocation-of-space-to-star-columns.md)) em um <xref:System.Windows.Controls.Grid> introduzido no .NET Framework 4.7.  Em alguns casos, como uma grade com <code>Height=&quot;Auto&quot;</code> contendo linhas vazias, as linhas foram organizadas na posição incorreta, possivelmente fora da grade.

#### <a name="suggestion"></a>Sugestão

Para que o aplicativo se beneficie dessas alterações, ele deve ser executado no .NET Framework 4.8 ou posterior.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Principal|
|Versão|4.8|
|Type|Runtime|
