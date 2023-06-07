---
ms.openlocfilehash: c3e39e49747be709977d7fba3c39b59f5575c40d
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619907"
---
### <a name="xmlschemaexception-now-sets-line-positions-properly"></a>XmlSchemaException agora define posições de linhas corretamente

#### <a name="details"></a>Detalhes

Se o valor <xref:System.Xml.Linq.LoadOptions.SetLineInfo> é passado para o método Load e um erro de validação ocorre, as propriedades <xref:System.Xml.Schema.XmlSchemaException.LineNumber> e <xref:System.Xml.Schema.XmlSchemaException.LinePosition> contêm agora a linha de informações.

#### <a name="suggestion"></a>Sugestão

O código de tratamento de exceção que supõe que <xref:System.Xml.Schema.XmlSchemaException.LineNumber> e <xref:System.Xml.Schema.XmlSchemaException.LinePosition> não serão definidas deverá ser atualizado, uma vez que essas propriedades agora serão definidas corretamente quando SetLineInfo for usado durante o carregamento de XML.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Xml.Linq.LoadOptions.SetLineInfo?displayProperty=nameWithType></li></ul>|
