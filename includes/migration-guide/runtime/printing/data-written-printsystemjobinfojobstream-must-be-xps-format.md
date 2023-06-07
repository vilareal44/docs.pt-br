---
ms.openlocfilehash: a007022bf32ffe76861f6f9016a7edace17b0f61
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619805"
---
### <a name="data-written-to-printsystemjobinfojobstream-must-be-in-xps-format"></a>Dados gravados em PrintSystemJobInfo.JobStream devem estar no formato XPS

#### <a name="details"></a>Detalhes

A propriedade <xref:System.Printing.PrintSystemJobInfo.JobStream> expõe o fluxo de um trabalho de impressão. O usuário pode enviar dados brutos para o sistema operacional subjacente que imprime os componentes gravando-os neste fluxo. A partir do .NET Framework 4.5 no Windows 8 e em versões posteriores do sistema operacional Windows, os dados gravados nesse fluxo devem estar no formato XPS como um fluxo de pacote.

#### <a name="suggestion"></a>Sugestão

Para mostrar o conteúdo impresso, você tem duas opções:<ul><li>Use a classe <xref:System.Windows.Xps.XpsDocumentWriter> para produzir conteúdo de impressão. Essa é a alternativa recomendada.</li><li>Garanta que os dados enviados ao fluxo retornado pela propriedade <xref:System.Printing.PrintSystemJobInfo.JobStream> estão no formato XPS como um fluxo de pacote.</li></ul>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Printing.PrintSystemJobInfo.JobStream?displayProperty=nameWithType></li></ul>|
