---
title: Regras para inferir tipos de nó e estrutura de esquema
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: d74ce896-717d-4871-8fd9-b070e2f53cb0
ms.openlocfilehash: 381c5fbd3823514de98b38840b8259a417e48fb8
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289077"
---
# <a name="rules-for-inferring-schema-node-types-and-structure"></a>Regras para inferir tipos de nó e estrutura de esquema
Este tópico descreve como o processo de inferência de esquema converte os tipos de nós em um documento XML a estrutura do idioma da definição de esquema XML (XSD).  
  
## <a name="element-inference-rules"></a>Regras de inferência de elemento  
 Esta seção descreve as regras de inferência para declarações elemento. Há oito estruturas das declarações de elemento que serão inferidas:  
  
1. Elemento do tipo simples  
  
2. Elemento vazio  
  
3. Elemento vazio com atributos  
  
4. Elemento com atributos e conteúdo simples  
  
5. Elemento com uma sequência de elementos filho  
  
6. Elemento com uma sequência de elementos filho e atributos  
  
7. Elemento com uma sequência das opções de elementos filho  
  
8. Elemento com uma sequência das opções de elementos filho e atributos  
  
> [!NOTE]
> Todas as declarações de `complexType` são inferidas como tipos anônimos. O único elemento global é inferido o elemento raiz; todos os outros elementos são locais.  
  
 Para saber mais sobre o processo de inferência de esquema, consulte [Inferência de esquemas de documentos XML](inferring-schemas-from-xml-documents.md).  
  
### <a name="simple-typed-element"></a>Elemento tipado simples  
 A tabela a seguir mostra XML conectado ao método de <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A> , e o esquema XML gerado. O elemento negrito mostra o esquema inferido para o elemento de tipo simples.  
  
 Para saber mais sobre o processo de inferência de esquema, consulte [Inferência de esquemas de documentos XML](inferring-schemas-from-xml-documents.md).  
  
|XML|Esquema|  
|---------|------------|  
|`<?xml version="1.0"?>`<br /><br /> `<root>text</root>`|`<?xml version="1.0" encoding="utf-8"?>`<br /><br /> `<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xml`<br /><br /> `ns:xs="http://www.w3.org/2001/XMLSchema">`<br /><br /> `<xs:element name="root" type="xs:string" />`<br /><br /> `</xs:schema>`|  
  
### <a name="empty-element"></a>Elemento vazio  
 A tabela a seguir mostra XML conectado ao método de <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A> , e o esquema XML gerado. O elemento negrito mostra o esquema inferido para o elemento vazio.  
  
 Para saber mais sobre o processo de inferência de esquema, consulte [Inferência de esquemas de documentos XML](inferring-schemas-from-xml-documents.md).  
  
|XML|Esquema|  
|---------|------------|  
|`<?xml version="1.0"?>`<br /><br /> `<empty/>`|`<?xml version="1.0" encoding="utf-8"?>`<br /><br /> `<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xml`<br /><br /> `ns:xs="http://www.w3.org/2001/XMLSchema">`<br /><br /> `<xs:element name="empty" />`<br /><br /> `</xs:schema>`|  
  
### <a name="empty-element-with-attributes"></a>Elemento vazio com atributos  
 A tabela a seguir mostra XML conectado ao método de <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A> , e o esquema XML gerado. Os elementos negritos mostram o esquema inferido para o elemento vazio com atributos.  
  
 Para saber mais sobre o processo de inferência de esquema, consulte [Inferência de esquemas de documentos XML](inferring-schemas-from-xml-documents.md).  
  
|XML|Esquema|  
|---------|------------|  
|`<?xml version="1.0"?>`<br /><br /> `<empty attribute1="text"/>`|`<?xml version="1.0" encoding="utf-8"?>`<br /><br /> `<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xml`<br /><br /> `ns:xs="http://www.w3.org/2001/XMLSchema">`<br /><br /> `<xs:element name="empty">`<br /><br /> `<xs:complexType>`<br /><br /> `<xs:attribute name="attribute1" type="xs:string" use="required" />`<br /><br /> `</xs:complexType>`<br /><br /> `</xs:element>`<br /><br /> `</xs:schema>`|  
  
### <a name="element-with-attributes-and-simple-content"></a>Elemento com atributos e conteúdo simples  
 A tabela a seguir mostra XML conectado ao método de <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A> , e o esquema XML gerado. Os elementos negritos mostram o esquema inferido para um elemento com atributos e conteúdo simples.  
  
 Para saber mais sobre o processo de inferência de esquema, consulte [Inferência de esquemas de documentos XML](inferring-schemas-from-xml-documents.md).  
  
|XML|Esquema|  
|---------|------------|  
|`<?xml version="1.0"?>`<br /><br /> `<root attribute1="text">value</root>`|`<?xml version="1.0" encoding="utf-8"?>`<br /><br /> `<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xml`<br /><br /> `ns:xs="http://www.w3.org/2001/XMLSchema">`<br /><br /> `<xs:element name="root">`<br /><br /> `<xs:complexType>`<br /><br /> `<xs:simpleContent>`<br /><br /> `<xs:extension base="xs:string">`<br /><br /> `<xs:attribute name="attribute1" type="xs:string" use="required" />`<br /><br /> `</xs:extension>`<br /><br /> `</xs:simpleContent>`<br /><br /> `</xs:complexType>`<br /><br /> `</xs:element>`<br /><br /> `</xs:schema>`|  
  
### <a name="element-with-a-sequence-of-child-elements"></a>Elemento com uma sequência de elementos filho  
 A tabela a seguir mostra XML conectado ao método de <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A> , e o esquema XML gerado. Os elementos negritos mostram o esquema inferido para um elemento com uma sequência de elementos filhos.  
  
> [!NOTE]
> Se um elemento é apenas um elemento filho, ainda é tratado como uma sequência.  
  
 Para saber mais sobre o processo de inferência de esquema, consulte [Inferência de esquemas de documentos XML](inferring-schemas-from-xml-documents.md).  
  
|XML|Esquema|  
|---------|------------|  
|`<?xml version="1.0"?>`<br /><br /> `<root>`<br /><br /> `<subElement/>`<br /><br /> `</root>`|`<?xml version="1.0" encoding="utf-8"?>`<br /><br /> `<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xml`<br /><br /> `ns:xs="http://www.w3.org/2001/XMLSchema">`<br /><br /> `<xs:element name="root">`<br /><br /> `<xs:complexType>`<br /><br /> `<xs:sequence>`<br /><br /> `<xs:element name="subElement" />`<br /><br /> `</xs:sequence>`<br /><br /> `</xs:complexType>`<br /><br /> `</xs:element>`<br /><br /> `</xs:schema>`|  
  
### <a name="element-with-a-sequence-of-child-elements-and-attributes"></a>Elemento com uma sequência de elementos filho e atributos  
 A tabela a seguir mostra XML conectado ao método de <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A> , e o esquema XML gerado. Os elementos negritos mostram o esquema inferido para um elemento com uma sequência de elementos filho e atributos.  
  
> [!NOTE]
> Se um elemento é apenas um elemento filho, ainda é tratado como uma sequência.  
  
 Para saber mais sobre o processo de inferência de esquema, consulte [Inferência de esquemas de documentos XML](inferring-schemas-from-xml-documents.md).  
  
|XML|Esquema|  
|---------|------------|  
|`<?xml version="1.0"?>`<br /><br /> `<root attribute1="text">`<br /><br /> `<subElement1/>`<br /><br /> `<subElement2/>`<br /><br /> `</root>`|`<?xml version="1.0" encoding="utf-8"?>`<br /><br /> `<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xml`<br /><br /> `ns:xs="http://www.w3.org/2001/XMLSchema">`<br /><br /> `<xs:element name="root">`<br /><br /> `<xs:complexType>`<br /><br /> `<xs:sequence>`<br /><br /> `<xs:element name="subElement1" />`<br /><br /> `<xs:element name="subElement2" />`<br /><br /> `</xs:sequence>`<br /><br /> `<xs:attribute name="attribute1" type="xs:string" use="required" />`<br /><br /> `</xs:complexType>`<br /><br /> `</xs:element>`<br /><br /> `</xs:schema>`|  
  
### <a name="element-with-a-sequence-and-choices-of-child-elements"></a>Elemento com uma sequência e opções de elementos filho  
 A tabela a seguir mostra XML conectado ao método de <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A> , e o esquema XML gerado. Os elementos negritos mostram o esquema inferido para um elemento com uma sequência e uma opção de elementos filhos.  
  
> [!NOTE]
> O atributo de `maxOccurs` do elemento de `xs:choice` é definido como `"unbounded"` no esquema inferido.  
  
 Para saber mais sobre o processo de inferência de esquema, consulte [Inferência de esquemas de documentos XML](inferring-schemas-from-xml-documents.md).  
  
|XML|Esquema|  
|---------|------------|  
|`<?xml version="1.0"?>`<br /><br /> `<root>`<br /><br /> `<subElement1/>`<br /><br /> `<subElement2/>`<br /><br /> `<subElement1/>`<br /><br /> `</root>`|`<?xml version="1.0" encoding="utf-8"?>`<br /><br /> `<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xml`<br /><br /> `ns:xs="http://www.w3.org/2001/XMLSchema">`<br /><br /> `<xs:element name="root">`<br /><br /> `<xs:complexType>`<br /><br /> `<xs:sequence>`<br /><br /> `<xs:choice maxOccurs="unbounded">`<br /><br /> `<xs:element name="subElement1" />`<br /><br /> `<xs:element name="subElement2" />`<br /><br /> `</xs:choice>`<br /><br /> `</xs:sequence>`<br /><br /> `</xs:complexType>`<br /><br /> `</xs:element>`<br /><br /> `</xs:schema>`|  
  
### <a name="element-with-a-sequence-and-choice-of-child-elements-and-attributes"></a>Elemento com uma sequência e uma escolha dos elementos filho e atributos  
 A tabela a seguir mostra XML conectado ao método de <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A> , e o esquema XML gerado. Os elementos negritos mostram o esquema inferido para um elemento com uma sequência e uma escolha dos elementos filho e atributos.  
  
> [!NOTE]
> O atributo de `maxOccurs` do elemento de `xs:choice` é definido como `"unbounded"` no esquema inferido.  
  
 Para saber mais sobre o processo de inferência de esquema, consulte [Inferência de esquemas de documentos XML](inferring-schemas-from-xml-documents.md).  
  
|XML|Esquema|  
|---------|------------|  
|`<?xml version="1.0"?>`<br /><br /> `<root attribute1="text">`<br /><br /> `<subElement1/>`<br /><br /> `<subElement2/>`<br /><br /> `<subElement1/>`<br /><br /> `</root>`|`<?xml version="1.0" encoding="utf-8"?>`<br /><br /> `<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xml`<br /><br /> `ns:xs="http://www.w3.org/2001/XMLSchema">`<br /><br /> `<xs:element name="root">`<br /><br /> `<xs:complexType>`<br /><br /> `<xs:sequence>`<br /><br /> `<xs:choice maxOccurs="unbounded">`<br /><br /> `<xs:element name="subElement1" />`<br /><br /> `<xs:element name="subElement2" />`<br /><br /> `</xs:choice>`<br /><br /> `</xs:sequence>`<br /><br /> `<xs:attribute name="attribute1" type="xs:string" use="required" />`<br /><br /> `</xs:complexType>`<br /><br /> `</xs:element>`<br /><br /> `</xs:schema>`|  
  
### <a name="attribute-processing"></a>Processamento de atributos  
 Sempre que um novo atributo é encontrado dentro de um nó, é adicionado à definição inferido do nó com `use="required"`. Na próxima vez que o mesmo nó é encontrado na instância, o processo de inferência comparará atributos de instância atual com que já inferidas. Se alguma de inferidos já está ausente na instância, `use="optional"` é adicionado à definição de atributo. Novos atributos são adicionados às declarações existentes com `use="optional"`.  
  
### <a name="occurrence-constraints"></a>Restrições de clique  
 Durante o processo de inferência de esquema, os atributos de `minOccurs` e de `maxOccurs` são gerados, para componentes inferidos de um esquema, com os valores `"0"` ou `"1"` e `"1"` ou `"unbounded"`. Os valores `"1"` e `"unbounded"` são usados somente quando valores `"0"` e `"1"` não podem validar o documento XML (por exemplo, se `MinOccurs="0"` não descreve exatamente um elemento, `minOccurs="1"` é usado).  
  
### <a name="mixed-content"></a>Conteúdo Misto  
 Se um elemento contém conteúdo misturado (por exemplo intercalado texto com elementos), o atributo de `mixed="true"` é gerado para a definição de tipo complexo inferido.  
  
## <a name="other-node-type-inference-rules"></a>Outras regras de inferência de tipo de nó  
 A tabela a seguir descreve as regras de inferência para a instrução de processamento, o comentário, a referência de entidade, o CDATA, o tipo de documento, e os nós de namespace.  
  
|Tipo de nó|Tradução|  
|---------------|-----------------|  
|Instrução de processamento|Ignorado.|  
|Comentário|Ignorado.|  
|Referência de entidade|A classe de <xref:System.Xml.Schema.XmlSchemaInference> não trata referências a entidades. Se um documento XML contém referências a entidades, você precisará usar um leitor que expande as entidades. Por exemplo, você pode passar <xref:System.Xml.XmlTextReader> com a propriedade de <xref:System.Xml.XmlTextReader.EntityHandling%2A> definida como <xref:System.Xml.EntityHandling.ExpandEntities> como um parâmetro. Se as referências a entidades são localizadas e o leitor não expande entidades, uma exceção é throw.|  
|CDATA|Todas as seções de `<![CDATA[ … ]]` em um documento XML serão inferidas como `xs:string`.|  
|Tipo de documento|Ignorado.|  
|Namespaces|Ignorado.|  
  
 Para saber mais sobre o processo de inferência de esquema, consulte [Inferência de esquemas de documentos XML](inferring-schemas-from-xml-documents.md).  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.Schema.XmlSchemaInference>
- [SOM (Schema Object Model) XML](xml-schema-object-model-som.md)
- [Inferindo um esquema XML](inferring-an-xml-schema.md)
- [Inferindo esquemas de documentos XML](inferring-schemas-from-xml-documents.md)
- [Regras para inferir tipos simples](rules-for-inferring-simple-types.md)
