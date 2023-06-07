---
title: Tipos de nós XML
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 71d03b78-6898-4ce7-b0fc-1282573f31f7
ms.openlocfilehash: 5e11d61e16659ac1a8ca1b0b2c0d493ffdad5621
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84282058"
---
# <a name="types-of-xml-nodes"></a>Tipos de nós XML
Quando um documento XML é lido na memória como uma árvore de nós, os tipos de nós são decididos quando os nós são criados. A especificação DOM (Document Object Model) XML tem vários tipos de nós, determinados pelo W3C (World Wide Web Consortium) e listados na seção 1.1.1 (The DOM Structure Model). A tabela a seguir lista os tipos de nós, o objeto atribuído ao tipo de nó e uma breve descrição de cada tipo.  
  
|Tipo de nó DOM|Objeto|Descrição|  
|-------------------|------------|-----------------|  
|Document|<xref:System.Xml.XmlDocument>|O contêiner de todos os nós na árvore. Também é conhecido como diretório base, que nem sempre é o mesmo do elemento raiz.|  
|DocumentFragment|<xref:System.Xml.XmlDocumentFragment>|Um recipiente temporário que contém um ou mais nós sem nenhuma estrutura de árvore.|  
|DocumentType|<xref:System.Xml.XmlDocumentType>|Representa o nó `<!DOCTYPE…>`.|  
|EntityReference|<xref:System.Xml.XmlEntityReference>|Representa o texto de referência de entidade não expandido.|  
|Elemento|<xref:System.Xml.XmlElement>|Representa um nó de elemento.|  
|Attr|<xref:System.Xml.XmlAttribute>|Atributo de um elemento.|  
|ProcessingInstruction|<xref:System.Xml.XmlProcessingInstruction>|Nó de instrução de processamento.|  
|Comentário|<xref:System.Xml.XmlComment>|Um nó de comentário.|  
|Texto|<xref:System.Xml.XmlText>|Texto que pertence a um elemento ou a um atributo.|  
|CDATASection|<xref:System.Xml.XmlCDataSection>|Representa CDATA.|  
|Entidade|<xref:System.Xml.XmlEntity>|Representa as declarações `<!ENTITY…>` em um documento XML, de um subconjunto de DTDs (definição de tipo de documento) internas ou de DTDs externas e de entidades de parâmetro.|  
|Notation|<xref:System.Xml.XmlNotation>|Representa uma notação declarada na DTD.|  
  
 Embora um atributo (*attr*) seja listado na seção 1.2 (Fundamental Interfaces) do W3C DOM Level 1 como um nó, ele não é considerado um filho de nenhum nó de elemento.  
  
 A tabela a seguir mostra tipos de nós adicionais não definidos pelo W3C. No entanto, eles estão disponíveis para uso no modelo de objeto do Microsoft .NET Framework como enumerações **XmlNodeType**. Portanto, não há nenhuma coluna de tipo de nó DOM correspondente com esses tipos de nós.  
  
|Tipo de nó|Description|  
|---------------|-----------------|  
|<xref:System.Xml.XmlDeclaration>|Representa o nó de declaração `<?xml version="1.0"…>`.|  
|<xref:System.Xml.XmlSignificantWhitespace>|Representa o espaço em branco significativo, que é o espaço em branco com conteúdo misto.|  
|<xref:System.Xml.XmlWhitespace>|Representa o espaço em branco no conteúdo de um elemento.|  
|EndElement|Retornado quando **XmlReader** chega ao final de um elemento.<br /><br /> Exemplo de XML:**\</item>**<br /><br /> Para obter mais informações, consulte <xref:System.Xml.XmlNodeType>.|  
|EndEntity|Retornado quando **XmlReader** chega ao final da substituição de entidade como resultado de uma chamada para <xref:System.Xml.XmlReader.ResolveEntity%2A>. Para obter mais informações, consulte <xref:System.Xml.XmlNodeType>.|  
  
 Para exibir um exemplo de código que lê XML e usa um constructo de caso nos tipos de nós para imprimir informações sobre o nó e seu conteúdo, confira <xref:System.Xml.XmlSignificantWhitespace.NodeType%2A>.  
  
 Para saber mais sobre a hierarquia de objetos dos tipos de nós e o nome do objeto equivalente, confira [Hierarquia DOM (Document Object Model) XML](xml-document-object-model-dom-hierarchy.md). Para saber mais sobre objetos criados na árvore de nós, confira [Mapeando a hierarquia de objetos para dados XML](mapping-the-object-hierarchy-to-xml-data.md).  
  
## <a name="see-also"></a>Veja também

- [XML Document Object Model (DOM)](xml-document-object-model-dom.md)
