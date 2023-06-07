---
ms.openlocfilehash: fdec6671cbf2dae0d72dfaec07f162058b38cf9d
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619908"
---
### <a name="xmltextreader-dtd-entity-expansion-is-limited-to-10000000-characters"></a>Expansão da entidade DTD de XmlTextReader é limitada a 10.000.000 caracteres

#### <a name="details"></a>Detalhes

A expansão da entidade DTD agora é limitada a 10.000.000 de caracteres. O carregamento de arquivos XML sem expansão de entidade DTD ou com expansão de entidade DTD limitada não é afetado. Arquivos com entidades DTD que se expandem para mais de 10.000.000 caracteres falham ao carregar e geram uma exceção.

#### <a name="suggestion"></a>Sugestão

Se o limite da expansão da entidade DTD for muito inferior a 10.000.000, o valor poderá ser substituído pela propriedade <xref:System.Xml.XmlReaderSettings.MaxCharactersFromEntities>. Um <xref:System.Xml.XmlReaderSettings?displayProperty=fullName> com o valor <xref:System.Xml.XmlReaderSettings.MaxCharactersFromEntities?displayProperty=fullName> correto pode ser passado para o <code>XmlReader.Create</code> que usa <xref:System.Xml.XmlReaderSettings?displayProperty=fullName> (por exemplo, <xref:System.Xml.XmlReader.Create(System.String,System.Xml.XmlReaderSettings)>).

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Xml.XmlTextReader?displayProperty=nameWithType></li><li><xref:System.Xml.XmlTextReader.%23ctor></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.IO.Stream)></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.IO.Stream,System.Xml.XmlNameTable)></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.IO.Stream,System.Xml.XmlNodeType,System.Xml.XmlParserContext)></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.IO.TextReader)></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.IO.TextReader,System.Xml.XmlNameTable)></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.String)></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.String,System.IO.Stream)></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.String,System.IO.Stream,System.Xml.XmlNameTable)></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.String,System.IO.TextReader)></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.String,System.IO.TextReader,System.Xml.XmlNameTable)></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.String,System.Xml.XmlNameTable)></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.String,System.Xml.XmlNodeType,System.Xml.XmlParserContext)></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.Xml.XmlNameTable)></li></ul>|
