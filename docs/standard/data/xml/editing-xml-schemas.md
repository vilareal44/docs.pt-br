---
title: Esquemas XML de edição
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
ms.assetid: fa09c8e5-c2b9-49d2-bb0d-40330cd13e4d
ms.openlocfilehash: b309c390ede3afc38122188337fa0dc3336e3ad5
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84292053"
---
# <a name="editing-xml-schemas"></a>Esquemas XML de edição

Editar um esquema XML é um dos recursos mais importantes do modelo de objeto (SOM) de esquema. Todas as propriedades de pre-esquema- compilação de SOM podem ser usadas para alterar os valores existentes em um esquema XML. O esquema XML pode então ser recompilado para refletir as alterações.

A primeira etapa em editar um esquema carregado no SOM é passar pelo esquema. Você deve estar familiarizado com o atravessamento de um esquema usando o SOM API antes de tentar editar um esquema. Você também deve estar familiarizado com as propriedades pré-compilação e de POST-esquema- compilação de POST-Esquema- compilação - Infoset (PSCI).

## <a name="editing-an-xml-schema"></a>Editando um esquema XML

Nesta seção, dois exemplos de código são fornecidos, ambos editar o esquema de cliente criado no tópico [Compilar esquemas XML](building-xml-schemas.md). O primeiro exemplo de código a seguir adiciona um novo elemento de `PhoneNumber` para o elemento de `Customer` e o segundo exemplo de código a seguir adiciona um novo atributo de `Title` para o elemento de `FirstName` . O primeiro exemplo também usa a coleção de <xref:System.Xml.Schema.XmlSchema.Elements%2A?displayProperty=nameWithType> de POST-esquema- compilação como os meios para percorrer o esquema de cliente quando o segundo exemplo de código usar a coleção de <xref:System.Xml.Schema.XmlSchema.Items%2A?displayProperty=nameWithType> de pre-esquema- compilação.

### <a name="phonenumber-element-example"></a>Exemplo do elemento de PhoneNumber

Isso primeiro exemplo de código a seguir adiciona um novo elemento de `PhoneNumber` para o elemento de `Customer` do cliente. O exemplo de código editar o esquema de cliente nas seguintes etapas.

1. Adiciona o esquema de cliente a um novo objeto de <xref:System.Xml.Schema.XmlSchemaSet> e compilá-lo em seguida. Todos os avisos de validação de esquema e leitura ou compilação encontrada erros o esquema são tratados pelo delegado de <xref:System.Xml.Schema.ValidationEventHandler> .

2. Recupera o objeto compilada de <xref:System.Xml.Schema.XmlSchema> de <xref:System.Xml.Schema.XmlSchemaSet> iterando sobre a propriedade de <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> . Porque o esquema é compilado, as propriedades de (PSCI) de POST-Esquema- compilação - Infoset são acessíveis.

3. Cria o elemento de `PhoneNumber` usando a classe de <xref:System.Xml.Schema.XmlSchemaElement> , a restrição de tipo simples de `xs:string` usando <xref:System.Xml.Schema.XmlSchemaSimpleType> e <xref:System.Xml.Schema.XmlSchemaSimpleTypeRestriction> classe, adiciona um aspecto padrão da propriedade de <xref:System.Xml.Schema.XmlSchemaSimpleTypeRestriction.Facets%2A> de restrição, e adicione a restrição à propriedade de <xref:System.Xml.Schema.XmlSchemaSimpleType.Content%2A> do tipo simples e de tipo simples a <xref:System.Xml.Schema.XmlSchemaElement.SchemaType%2A> do elemento de `PhoneNumber` .

4. Efetua iterações sobre cada <xref:System.Xml.Schema.XmlSchemaElement> na coleção de <xref:System.Xml.Schema.XmlSchemaObjectTable.Values%2A> de coleção de <xref:System.Xml.Schema.XmlSchema.Elements%2A?displayProperty=nameWithType> de POST-esquema- compilação.

5. Se <xref:System.Xml.Schema.XmlSchemaElement.QualifiedName%2A> do elemento é `"Customer"`, obtém o tipo complexo do elemento de `Customer` usando a classe de <xref:System.Xml.Schema.XmlSchemaComplexType> e a partícula a sequência de tipo complexo usando a classe de <xref:System.Xml.Schema.XmlSchemaSequence> .

6. Adicionar o novo elemento de `PhoneNumber` a sequência que contém `FirstName` e elementos existentes de `LastName` usando a coleção de <xref:System.Xml.Schema.XmlSchemaSequence.Items%2A> de pre-esquema- compilação de sequência.

7. Finalmente, reprocessa e cria o objeto modificado de <xref:System.Xml.Schema.XmlSchema> usando os métodos de <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> e de <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> da classe de <xref:System.Xml.Schema.XmlSchemaSet> e grava no console.

A seguir está o exemplo de código completo.

[!code-cpp[XmlSchemaEditExample1#1](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaEditExample1/CPP/XmlSchemaEditExample1.cpp#1)]
[!code-csharp[XmlSchemaEditExample1#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaEditExample1/CS/XmlSchemaEditExample1.cs#1)]
[!code-vb[XmlSchemaEditExample1#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaEditExample1/VB/XmlSchemaEditExample1.vb#1)]

A seguir, o esquema modificado de cliente criado no tópico [Compilar esquemas XML](building-xml-schemas.md).

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema xmlns:tns="http://www.tempuri.org" targetNamespace="http://www.tempuri.org" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="Customer">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="FirstName" type="xs:string" />
        <xs:element name="LastName" type="tns:LastNameType" />
        <xs:element name="PhoneNumber">           <xs:simpleType>             <xs:restriction base="xs:string">               <xs:pattern value="\d{3}-\d{3}-\d(4)" />             </xs:restriction>           </xs:simpleType>         </xs:element>
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

### <a name="title-attribute-example"></a>Exemplo de atributo de título

O segundo exemplo de código, adicione um novo atributo de `Title` para o elemento de `FirstName` do cliente. No primeiro exemplo de código, o tipo de elemento de `FirstName` é `xs:string`. Para o elemento de `FirstName` tem um atributo juntamente com o conteúdo de cadeia de caracteres, seu tipo deve ser alterado para um tipo complexo com um modelo de conteúdo de conteúdo simples de extensão.

O exemplo de código editar o esquema de cliente nas seguintes etapas.

1. Adiciona o esquema de cliente a um novo objeto de <xref:System.Xml.Schema.XmlSchemaSet> e compilá-lo em seguida. Todos os avisos de validação de esquema e leitura ou compilação encontrada erros o esquema são tratados pelo delegado de <xref:System.Xml.Schema.ValidationEventHandler> .

2. Recupera o objeto compilada de <xref:System.Xml.Schema.XmlSchema> de <xref:System.Xml.Schema.XmlSchemaSet> iterando sobre a propriedade de <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> . Porque o esquema é compilado, as propriedades de (PSCI) de POST-Esquema- compilação - Infoset são acessíveis.

3. Cria um novo tipo complexo para o elemento de `FirstName` usando a classe de <xref:System.Xml.Schema.XmlSchemaComplexType> .

4. Cria uma nova extensão de conteúdo simples, com um tipo base de `xs:string`, usando as classes de <xref:System.Xml.Schema.XmlSchemaSimpleContent> e de <xref:System.Xml.Schema.XmlSchemaSimpleContentExtension> .

5. Cria o novo atributo de `Title` usando a classe de <xref:System.Xml.Schema.XmlSchemaAttribute> , com <xref:System.Xml.Schema.XmlSchemaAttribute.SchemaTypeName%2A> de `xs:string` e adicione o atributo a extensão de conteúdo simples.

6. Defina o modelo de conteúdo de conteúdo simples para a extensão de conteúdo simples e o modelo de conteúdo do tipo complexo para o conteúdo simples.

7. Adicionar o novo tipo complexo à coleção de <xref:System.Xml.Schema.XmlSchema.Items%2A?displayProperty=nameWithType> de pre-esquema- compilação.

8. Efetua iterações sobre cada <xref:System.Xml.Schema.XmlSchemaObject> na coleção de <xref:System.Xml.Schema.XmlSchema.Items%2A?displayProperty=nameWithType> de pre-esquema- compilação.

> [!NOTE]
> Como o elemento de `FirstName` não é um elemento global no esquema, não está disponível em coleções de <xref:System.Xml.Schema.XmlSchema.Items%2A?displayProperty=nameWithType> ou de <xref:System.Xml.Schema.XmlSchema.Elements%2A?displayProperty=nameWithType> . O exemplo de código posicionar o elemento de `FirstName` posicionando o primeiro elemento de `Customer` .
>
> O primeiro exemplo de código atravessou o esquema usando a coleção de <xref:System.Xml.Schema.XmlSchema.Elements%2A?displayProperty=nameWithType> de POST-esquema- compilação. Nesse exemplo, a coleção de <xref:System.Xml.Schema.XmlSchema.Items%2A?displayProperty=nameWithType> de pre-esquema- compilação é usada para percorrer o esquema. Quando ambas as coleções fornecem acesso aos elementos globais no esquema, iterar através da coleção de <xref:System.Xml.Schema.XmlSchema.Items%2A> é mais demorado porque você deve iterar todos os elementos globais no esquema e não tem nenhuma propriedades de PSCI. Coleções de PSCI (<xref:System.Xml.Schema.XmlSchema.Elements%2A?displayProperty=nameWithType>, <xref:System.Xml.Schema.XmlSchema.Attributes%2A?displayProperty=nameWithType>, <xref:System.Xml.Schema.XmlSchema.SchemaTypes%2A?displayProperty=nameWithType>, e assim por diante) fornecem acesso direto a seus elementos, atributos globais, tipos e suas propriedades de PSCI.

1. Se <xref:System.Xml.Schema.XmlSchemaObject> é um elemento, cujo <xref:System.Xml.Schema.XmlSchemaElement.QualifiedName%2A> é `"Customer"`, obtém o tipo complexo do elemento de `Customer` usando a classe de <xref:System.Xml.Schema.XmlSchemaComplexType> e a partícula a sequência de tipo complexo usando a classe de <xref:System.Xml.Schema.XmlSchemaSequence> .

2. Efetua iterações sobre cada <xref:System.Xml.Schema.XmlSchemaParticle> na coleção de <xref:System.Xml.Schema.XmlSchemaSequence.Items%2A?displayProperty=nameWithType> de pre-esquema- compilação.

3. Se <xref:System.Xml.Schema.XmlSchemaParticle> é um elemento, que é <xref:System.Xml.Schema.XmlSchemaElement.QualifiedName%2A> é `"FirstName"`, define <xref:System.Xml.Schema.XmlSchemaElement.SchemaTypeName%2A> do elemento de `FirstName` para o novo tipo complexo de `FirstName` .

4. Finalmente, reprocessa e cria o objeto modificado de <xref:System.Xml.Schema.XmlSchema> usando os métodos de <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> e de <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> da classe de <xref:System.Xml.Schema.XmlSchemaSet> e grava no console.

A seguir está o exemplo de código completo.

[!code-cpp[XmlSchemaEditExample2#1](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaEditExample2/CPP/XmlSchemaEditExample2.cpp#1)]
[!code-csharp[XmlSchemaEditExample2#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaEditExample2/CS/XmlSchemaEditExample2.cs#1)]
[!code-vb[XmlSchemaEditExample2#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaEditExample2/VB/XmlSchemaEditExample2.vb#1)]

A seguir, o esquema modificado de cliente criado no tópico [Compilar esquemas XML](building-xml-schemas.md).

```xml
<?xml version="1.0" encoding=" utf-8"?>
<xs:schema xmlns:tns="http://www.tempuri.org" targetNamespace="http://www.tempuri.org" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="Customer">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="FirstName" type="tns:FirstNameComplexType" />
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
  <xs:complexType name="FirstNameComplexType">     <xs:simpleContent>       <xs:extension base="xs:string">         <xs:attribute name="Title" type="xs:string" />       </xs:extension>     </xs:simpleContent>   </xs:complexType>
</xs:schema>
```

## <a name="see-also"></a>Veja também

- [Visão geral do modelo de objeto de esquema XML](xml-schema-object-model-overview.md)
- [Lendo e gravando esquemas XML](reading-and-writing-xml-schemas.md)
- [Compilando esquemas XML](building-xml-schemas.md)
- [Percorrer esquemas XML](traversing-xml-schemas.md)
- [Incluindo ou importando um esquema XML](including-or-importing-xml-schemas.md)
- [XmlSchemaSet para compilação de esquema](xmlschemaset-for-schema-compilation.md)
- [Compilação Infoset de pré esquema](post-schema-compilation-infoset.md)
