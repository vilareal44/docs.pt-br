---
title: Trabalhando com esquemas XML
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: bbbcc70c-bf9a-4f6a-af72-1bab5384a187
ms.openlocfilehash: f239d67d959c1f7a0bfebfaaaa49de9cf9c9a111
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84281681"
---
# <a name="working-with-xml-schemas"></a>Trabalhando com esquemas XML
Para definir a estrutura de um documento XML, bem como os relacionamentos de seus elementos, tipos de dados e restrições de conteúdo, você usa uma DTD (definição de tipo de documento) ou um esquema XSD (linguagem de definição de esquema XML). Embora um documento XML seja considerado bem-formado se atender a todos os requisitos sintáticos definidos pela Recomendação da linguagem XML 1.0 do W3C (World Wide Web Consortium), ele não é considerado válido a menos que seja bem-formado e esteja de acordo com as restrições definidas pela DTD ou o esquema. Portanto, embora todos os documentos XML válidos sejam bem-formados, nem todos os documentos XML bem-formados são válidos.  
  
 Para saber mais sobre o XML, confira a [Recomendação W3C XML 1.0](https://www.w3.org/TR/REC-xml/). Para saber mais sobre o esquema XML, confira as recomendações [Parte 1 do Esquema W3C XML: recomendação de estruturas](https://www.w3.org/TR/xmlschema-1/) e a [Parte 2 do Esquema W3C XML: recomendação de tipos de dados](https://www.w3.org/TR/xmlschema-2/).  
  
## <a name="in-this-section"></a>Nesta seção  
 [SOM (Schema Object Model) XML](xml-schema-object-model-som.md)  
 Discute o SOM (Schema Object Model) no namespace <xref:System.Xml.Schema?displayProperty=nameWithType> que fornece um conjunto de classes que permitem que você leia um esquema XSD (linguagem de definição de esquema XML) em um arquivo ou crie programaticamente um esquema na memória.  
  
 [XmlSchemaSet para compilação de esquema](xmlschemaset-for-schema-compilation.md)  
 Discute a classe <xref:System.Xml.Schema.XmlSchemaSet> que é um cache onde os esquemas XSD podem ser armazenados e validados.  
  
 [XmlSchemaValidator Envio- de validação](xmlschemavalidator-push-based-validation.md)  
 Discute a classe <xref:System.Xml.Schema.XmlSchemaValidator>, que fornece um mecanismo eficiente e de alto desempenho para validar dados XML em esquemas XSD de uma maneira baseada em push.  
  
 [Inferindo um esquema XML](inferring-an-xml-schema.md)  
 Discute como usar a classe <xref:System.Xml.Schema.XmlSchemaInference> para inferir um esquema XSD na estrutura de um documento XML.  
  
## <a name="reference"></a>Referência  
 <xref:System.Xml.Schema.XmlSchemaSet> &#124; <xref:System.Xml.Schema.XmlSchemaInference> &#124; <xref:System.Xml.XmlReader>  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Validando um documento XML no DOM](validating-an-xml-document-in-the-dom.md)  
 Discute como validar o XML no DOM (Document Object Model). Você pode validar o XML como é carregado no DOM ou validar um documento XML invalidado anteriormente no DOM.  
  
 [Validação de esquema usando XPathNavigator](schema-validation-using-xpathnavigator.md)  
 Discute como validar XML que está sendo navegado e editado usando a classe <xref:System.Xml.XPath.XPathNavigator>.
