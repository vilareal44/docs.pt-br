---
ms.openlocfilehash: 0470cefc05fb5da6a6195ee0a96f04feef01fd10
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619883"
---
### <a name="flowdocument-may-show-an-extra-line-of-text"></a>FlowDocument pode mostrar uma linha extra de texto

#### <a name="details"></a>Detalhes

Em alguns casos, um elemento <xref:System.Windows.Documents.FlowDocument> exibirá uma linha de texto extra ao ser executado no .NET Framework 4.5, em comparação a como ele foi exibido quando executado no .NET Framework 4.0. Não conhecemos casos em que a alteração fez com que qualquer texto fosse exibido de forma inadequada ou ilegível, mas textos omitidos de uma exibição de <xref:System.Windows.Documents.FlowDocument> podem aparecer.

#### <a name="suggestion"></a>Sugestão

Em alguns casos, diminuir a propriedade PageHeight do elemento de exibição em 1 pode restaurar o número anterior de linhas exibidas.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Windows.Documents.FlowDocument.%23ctor></li><li><xref:System.Windows.Documents.FlowDocument.%23ctor(System.Windows.Documents.Block)></li><li><xref:System.Windows.Controls.FlowDocumentReader.%23ctor></li><li><xref:System.Windows.Controls.FlowDocumentPageViewer.%23ctor></li><li><xref:System.Windows.Controls.Primitives.DocumentPageView.%23ctor></li></ul>|
