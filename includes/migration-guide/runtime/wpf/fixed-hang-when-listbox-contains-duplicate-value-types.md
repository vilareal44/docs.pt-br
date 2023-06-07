---
ms.openlocfilehash: 5d5423d18091545ad9d50325900f5a9a4fff6dd9
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621917"
---
### <a name="fixed-a-hang-when-listbox-contains-duplicate-value-types"></a>Correção de uma parada quando ListBox contém tipos de valor duplicados

#### <a name="details"></a>Detalhes

Correção de um problema em que um <xref:System.Windows.Controls.ItemsControl> de virtualização pode parar de responder durante a rolagem quando a respectiva coleção Itens contém objetos com tipos de valor duplicados.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Principal|
|Versão|4.8|
|Type|Runtime|
