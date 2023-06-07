---
title: Atributos que controlam a serialização XML
description: Este artigo contém atributos que você pode aplicar a classes e membros de classe para controlar como o XmlSerializer serializa ou desserializa uma instância de uma classe.
ms.date: 03/30/2017
helpviewer_keywords:
- classes, serializing
- XmlSerializer class, serializing
- XML serialization, attributes
- attributes [.NET Framework], XML serialization
- serialization, attributes
- XML Schema, serializing
ms.assetid: 414b820f-a696-4206-b576-2711d85490c7
ms.openlocfilehash: fbc42ff696107f4a1b06d3611fc97a09cc4a3542
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84276693"
---
# <a name="attributes-that-control-xml-serialization"></a>Atributos que controlam a serialização XML
Você pode aplicar os atributos na tabela a seguir para classes e membros de classe para controlar a maneira pela qual o <xref:System.Xml.Serialization.XmlSerializer> serializa ou desserializa uma instância da classe. Para entender como esses atributos controlam a serialização XML, consulte [Controlando a serialização XML usando atributos](controlling-xml-serialization-using-attributes.md).  
  
 Esses atributos também podem ser usados para controlar as mensagens SOAP literais de estilo geradas por um serviço Web XML. Para obter mais informações sobre como aplicar esses atributos a um método de serviços Web XML, consulte [Serialização XML com serviços Web XML](xml-serialization-with-xml-web-services.md).  
  
 Para obter mais informações sobre atributos, consulte [Atributos](../attributes/index.md).  
  
|Atributo|Aplica-se a|Especifica|  
|---------------|----------------|---------------|  
|<xref:System.Xml.Serialization.XmlAnyAttributeAttribute>|O valor do campo público, propriedade, parâmetro ou retorno que retorna uma matriz de objetos <xref:System.Xml.XmlAttribute>.|Ao desserializar, a matriz será preenchida com objetos <xref:System.Xml.XmlAttribute> que representam todos os atributos XML desconhecidos do esquema.|  
|<xref:System.Xml.Serialization.XmlAnyElementAttribute>|O valor do campo público, propriedade, parâmetro ou retorno que retorna uma matriz de objetos <xref:System.Xml.XmlElement>.|Ao desserializar, a matriz será preenchida com objetos <xref:System.Xml.XmlElement> que representam todos os elementos XML desconhecidos do esquema.|  
|<xref:System.Xml.Serialization.XmlArrayAttribute>|O campo público, propriedade, parâmetro ou valor de retorno que retorna uma matriz de objetos complexos.|Os membros da matriz serão gerados como membros de uma matriz XML.|  
|<xref:System.Xml.Serialization.XmlArrayItemAttribute>|O campo público, propriedade, parâmetro ou valor de retorno que retorna uma matriz de objetos complexos.|Os tipos derivados que podem ser inseridos em uma matriz. Geralmente aplicado em conjunto com um <xref:System.Xml.Serialization.XmlArrayAttribute>.|  
|<xref:System.Xml.Serialization.XmlAttributeAttribute>|Campo público, propriedade, parâmetro ou valor de retorno.|O membro será serializado como um atributo XML.|  
|<xref:System.Xml.Serialization.XmlChoiceIdentifierAttribute>|Campo público, propriedade, parâmetro ou valor de retorno.|O membro pode ter a ambiguidade removida usando uma enumeração.|  
|<xref:System.Xml.Serialization.XmlElementAttribute>|Campo público, propriedade, parâmetro ou valor de retorno.|O campo ou propriedade serão serializados como um elemento XML.|  
|<xref:System.Xml.Serialization.XmlEnumAttribute>|O campo público que é um identificador de enumeração.|O nome do elemento de um membro de enumeração.|  
|<xref:System.Xml.Serialization.XmlIgnoreAttribute>|Propriedades públicas e campos.|A propriedade ou campo devem ser ignorados quando a classe recipiente é serializada.|  
|<xref:System.Xml.Serialization.XmlIncludeAttribute>|Declarações públicas de classe derivada e valores de retorno de métodos públicos para documentos da linguagem WSDL.|A classe deve ser incluída ao gerar esquemas (para serem reconhecidos quando serializados).|  
|<xref:System.Xml.Serialization.XmlRootAttribute>|Declarações públicas de classe.|Controla a serialização XML do destino do atributo como um elemento raiz XML. Use o atributo para especificar ainda mais o namespace e o nome do elemento.|  
|<xref:System.Xml.Serialization.XmlTextAttribute>|Propriedades públicas e campos.|A propriedade ou o campo devem ser serializados como texto XML.|  
|<xref:System.Xml.Serialization.XmlTypeAttribute>|Declarações públicas de classe.|O nome e o namespace do tipo XML.|  
  
 Além desses atributos, que são todos encontrados no namespace <xref:System.Xml.Serialization>, você também pode aplicar o atributo <xref:System.ComponentModel.DefaultValueAttribute> a um campo. O **DefaultValueAttribute** definirá o valor que será atribuído automaticamente ao membro se nenhum valor for especificado.  
  
 Para controlar a serialização XML de SOAP codificada, consulte [Atributos que controlam a serialização SOAP codificada](attributes-that-control-encoded-soap-serialization.md).  
  
## <a name="see-also"></a>Confira também

- [Serialização de XML e SOAP](xml-and-soap-serialization.md)
- <xref:System.Xml.Serialization.XmlSerializer>
- [Controlando a serialização XML usando atributos](controlling-xml-serialization-using-attributes.md)
- [Como especificar um nome de elemento alternativo para um fluxo XML](how-to-specify-an-alternate-element-name-for-an-xml-stream.md)
- [Como serializar um objeto](how-to-serialize-an-object.md)
- [Como desserializar um objeto](how-to-deserialize-an-object.md)
