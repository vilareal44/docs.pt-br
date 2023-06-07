---
title: Compilando esquemas XML
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
ms.assetid: 8a5ea56c-0140-4b51-8997-875ae6a8e0cb
ms.openlocfilehash: adc1a10bb9acb14ee88589c13e28c49773e42100
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291585"
---
# <a name="building-xml-schemas"></a>Compilando esquemas XML
As classes no namespace <xref:System.Xml.Schema?displayProperty=nameWithType> mapeiam para as estruturas definidas na recomendação do W3C (World Wide Web Consortium) sobre esquemas XML e podem ser usadas para compilar esquemas XML na memória.  
  
## <a name="building-an-xml-schema"></a>Compilando um esquema XML  
 Nos exemplos de código a seguir, a API do SOM é usada para compilar um esquema XML cliente na memória.  
  
### <a name="creating-element-and-attributes"></a>Criando o elemento e os atributos  
 Os exemplos de código compilam o esquema cliente de baixo para cima, criando primeiro elementos e atributos filho, bem como seus tipos correspondentes, e depois os elementos de nível superior.  
  
 No exemplo de código a seguir, os elementos `FirstName` e `LastName`, bem como o atributo `CustomerId` do esquema cliente são criados usando as classes <xref:System.Xml.Schema.XmlSchemaElement> e <xref:System.Xml.Schema.XmlSchemaAttribute> do SOM. Com exceção das propriedades <xref:System.Xml.Schema.XmlSchemaElement.Name%2A> das classes <xref:System.Xml.Schema.XmlSchemaElement> e <xref:System.Xml.Schema.XmlSchemaAttribute>, que correspondem ao atributo "name" dos elementos `<xs:element />` e `<xs:attribute />` em um esquema XML, todos os outros atributos permitidos pelo esquema (`defaultValue`, `fixedValue`, `form` e assim por diante) têm propriedades correspondentes nas classes <xref:System.Xml.Schema.XmlSchemaElement> e <xref:System.Xml.Schema.XmlSchemaAttribute>.  
  
 [!code-cpp[XmlSchemaCreateExample#2](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaCreateExample/CPP/XmlSchemaCreateExample.cpp#2)]
 [!code-csharp[XmlSchemaCreateExample#2](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaCreateExample/CS/XmlSchemaCreateExample.cs#2)]
 [!code-vb[XmlSchemaCreateExample#2](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaCreateExample/VB/XmlSchemaCreateExample.vb#2)]  
  
### <a name="creating-schema-types"></a>Criando tipos de esquema  
 O conteúdo de elementos e de atributos é definido pelos respectivos tipos. Para criar elementos e atributos cujos tipos são um dos tipos internos do esquema, a propriedade <xref:System.Xml.Schema.XmlSchemaElement.SchemaTypeName%2A> das classes <xref:System.Xml.Schema.XmlSchemaElement> ou <xref:System.Xml.Schema.XmlSchemaAttribute> são definidas com o nome qualificado correspondente do tipo interno usando a classe <xref:System.Xml.XmlQualifiedName>. Para criar um tipo definido pelo usuário de elementos e atributos, um novo tipo simples ou complexo é criado usando a classe <xref:System.Xml.Schema.XmlSchemaSimpleType> ou <xref:System.Xml.Schema.XmlSchemaComplexType>.  
  
> [!NOTE]
> Para criar tipos simples ou complexos sem nome que sejam filhos anônimos de um elemento ou atributo (somente os tipos simples são aplicáveis a atributos), defina a propriedade <xref:System.Xml.Schema.XmlSchemaElement.SchemaType%2A> das classes <xref:System.Xml.Schema.XmlSchemaElement> ou <xref:System.Xml.Schema.XmlSchemaAttribute> como o tipo simples ou complexo sem nome, em vez da propriedade <xref:System.Xml.Schema.XmlSchemaElement.SchemaTypeName%2A> das classes <xref:System.Xml.Schema.XmlSchemaElement> ou <xref:System.Xml.Schema.XmlSchemaAttribute>.  
  
 Os esquemas XML permitem que tipos simples, anônimos e nomeados, sejam derivados pela restrição de outros tipos simples (internos ou definidos pelo usuário) ou construídos como uma lista ou união de outros tipos simples. A classe <xref:System.Xml.Schema.XmlSchemaSimpleTypeRestriction> é usada para criar um tipo simples restringindo o tipo interno `xs:string`. Você também pode usar as classes <xref:System.Xml.Schema.XmlSchemaSimpleTypeList> ou <xref:System.Xml.Schema.XmlSchemaSimpleTypeUnion> para criar tipos de lista ou de união. A propriedade <xref:System.Xml.Schema.XmlSchemaSimpleType.Content%2A?displayProperty=nameWithType> indica se é uma união, uma lista ou uma restrição de tipos simples.  
  
 No exemplo de código a seguir, o tipo do elemento `FirstName` é o tipo interno `xs:string`, o tipo do elemento `LastName` é um tipo simples nomeado que é uma restrição do tipo interno `xs:string`, com um valor de faceta `MaxLength` igual a 20, e o tipo do atributo `CustomerId` é o tipo interno `xs:positiveInteger`. O elemento `Customer` é um tipo complexo anônimo cuja partícula é a sequência dos elementos `FirstName` e `LastName` e cujos atributos contêm o atributo `CustomerId`.  
  
> [!NOTE]
> Você também pode usar as classes <xref:System.Xml.Schema.XmlSchemaChoice> ou <xref:System.Xml.Schema.XmlSchemaAll> como partícula do tipo complexo para replicar as semânticas `<xs:choice />` ou `<xs:all />`.  
  
 [!code-cpp[XmlSchemaCreateExample#3](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaCreateExample/CPP/XmlSchemaCreateExample.cpp#3)]
 [!code-csharp[XmlSchemaCreateExample#3](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaCreateExample/CS/XmlSchemaCreateExample.cs#3)]
 [!code-vb[XmlSchemaCreateExample#3](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaCreateExample/VB/XmlSchemaCreateExample.vb#3)]  
  
### <a name="creating-and-compiling-schemas"></a>Criando e compilando esquemas  
 Neste ponto, os elementos e atributos filho, seus tipos correspondentes e o elemento de nível superior `Customer` foram criados na memória usando a API do SOM. No exemplo de código a seguir, o elemento de esquema é criado usando a classe <xref:System.Xml.Schema.XmlSchema>, os elementos e os tipos de nível superior são adicionados a ele usando a propriedade <xref:System.Xml.Schema.XmlSchema.Items%2A?displayProperty=nameWithType> e o esquema completo é compilado usando a classe <xref:System.Xml.Schema.XmlSchemaSet> e criado no console.  
  
 [!code-cpp[XmlSchemaCreateExample#4](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaCreateExample/CPP/XmlSchemaCreateExample.cpp#4)]
 [!code-csharp[XmlSchemaCreateExample#4](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaCreateExample/CS/XmlSchemaCreateExample.cs#4)]
 [!code-vb[XmlSchemaCreateExample#4](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaCreateExample/VB/XmlSchemaCreateExample.vb#4)]  
  
 O método <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A?displayProperty=nameWithType> valida o esquema cliente em relação às regras de um esquema XML e disponibiliza as propriedades de pós-compilação de esquema.  
  
> [!NOTE]
> Todas as propriedades de pós-compilação de esquema na API do SOM diferem do conjunto de informações de pós-validação de esquema.  
  
 O objeto <xref:System.Xml.Schema.ValidationEventHandler> adicionado à classe <xref:System.Xml.Schema.XmlSchemaSet> é um delegado que chama o método de retorno de chamada `ValidationCallback` para tratar avisos e erros de validação de esquema.  
  
 Veja a seguir o exemplo de código completo e o esquema cliente criado no console.  
  
 [!code-cpp[XmlSchemaCreateExample#1](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaCreateExample/CPP/XmlSchemaCreateExample.cpp#1)]
 [!code-csharp[XmlSchemaCreateExample#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaCreateExample/CS/XmlSchemaCreateExample.cs#1)]
 [!code-vb[XmlSchemaCreateExample#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaCreateExample/VB/XmlSchemaCreateExample.vb#1)]  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<xs:schema xmlns:tns="http://www.tempuri.org" targetNamespace="http://www.tempuri.org" xmlns:xs="http://www.w3.org/2001/XMLSchema">  
   <xs:element name="Customer">  
      <xs:complexType>  
         <xs:sequence>  
            <xs:element name="FirstName" type="xs:string" />  
            <xs:element name="LastName" type="tns:LastNameType" />  
         </xs:sequence>  
         <xs:attribute name="CustomerId" type="xs:positiveInteger" use="required" />  
      </xs:complexType>  
   </xs:element>  
   <xs:simpleType name="LastNameType">  
      <xs:restriction base="xs:string">  
         <xs:maxLength value="20" />  
      </xs:restriction>  
   </xs:simpleType>  
</xs:schema>  
```  
  
## <a name="see-also"></a>Veja também

- [Visão geral do modelo de objeto de esquema XML](xml-schema-object-model-overview.md)
- [Lendo e gravando esquemas XML](reading-and-writing-xml-schemas.md)
- [Percorrer esquemas XML](traversing-xml-schemas.md)
- [Esquemas XML de edição](editing-xml-schemas.md)
- [Incluindo ou importando um esquema XML](including-or-importing-xml-schemas.md)
- [XmlSchemaSet para compilação de esquema](xmlschemaset-for-schema-compilation.md)
- [Compilação Infoset de pré esquema](post-schema-compilation-infoset.md)
