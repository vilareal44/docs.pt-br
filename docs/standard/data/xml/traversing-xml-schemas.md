---
title: Percorrer esquemas XML
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
ms.assetid: cce69574-5861-4a30-b730-2e18d915d8ee
ms.openlocfilehash: 0951e83c3035de751801d194696eb64993260ef8
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289831"
---
# <a name="traversing-xml-schemas"></a>Percorrer esquemas XML

Percorrer um esquema XML que usa o modelo de objeto (SOM) API de esquema fornece acesso aos elementos, a atributos, e tipos armazenados no SOM. Percorrer um esquema XML carregado no SOM também é a primeira etapa em editar um esquema XML usando o SOM API.

## <a name="traversing-an-xml-schema"></a>Percorrer um esquema XML

As seguintes propriedades da classe de <xref:System.Xml.Schema.XmlSchema> fornecem acesso à coleção de todos os itens globais adicionados ao esquema XML.

|Propriedade|Tipo de objeto armazenado na coleção ou na matriz|
|--------------|---------------------------------------------------|
|<xref:System.Xml.Schema.XmlSchema.Elements%2A>|<xref:System.Xml.Schema.XmlSchemaElement>|
|<xref:System.Xml.Schema.XmlSchema.Attributes%2A>|<xref:System.Xml.Schema.XmlSchemaAttribute>|
|<xref:System.Xml.Schema.XmlSchema.AttributeGroups%2A>|<xref:System.Xml.Schema.XmlSchemaAttributeGroup>|
|<xref:System.Xml.Schema.XmlSchema.Groups%2A>|<xref:System.Xml.Schema.XmlSchemaGroup>|
|<xref:System.Xml.Schema.XmlSchema.Includes%2A>|<xref:System.Xml.Schema.XmlSchemaExternal>, <xref:System.Xml.Schema.XmlSchemaInclude>, <xref:System.Xml.Schema.XmlSchemaImport> ou <xref:System.Xml.Schema.XmlSchemaRedefine>|
|<xref:System.Xml.Schema.XmlSchema.Items%2A>|<xref:System.Xml.Schema.XmlSchemaObject> (provê acesso a todos os elementos, atributos, e tipos de nível globais).|
|<xref:System.Xml.Schema.XmlSchema.Notations%2A>|<xref:System.Xml.Schema.XmlSchemaNotation>|
|<xref:System.Xml.Schema.XmlSchema.SchemaTypes%2A>|<xref:System.Xml.Schema.XmlSchemaType>, <xref:System.Xml.Schema.XmlSchemaSimpleType>, <xref:System.Xml.Schema.XmlSchemaComplexType>|
|<xref:System.Xml.Schema.XmlSchema.UnhandledAttributes%2A>|<xref:System.Xml.XmlAttribute> (fornece acesso aos atributos que não pertencem ao namespace de esquema)|

> [!NOTE]
> Todas as propriedades listadas na tabela anterior, exceto para a propriedade de <xref:System.Xml.Schema.XmlSchema.Items%2A> , são as propriedades de (PSCI) de POST-Esquema- compilação - Infoset que não estão disponíveis até que o esquema foi criado. A propriedade de <xref:System.Xml.Schema.XmlSchema.Items%2A> é uma propriedade de pre-esquema- compilação que pode ser usada antes que o esquema foi criado para acessar e editar todos os elementos, atributos, e tipos de nível globais.
>
> A propriedade de <xref:System.Xml.Schema.XmlSchema.UnhandledAttributes%2A> fornece acesso a todos os atributos que não pertencem ao namespace de esquema. Esses atributos não são processadas pelo processador de esquema.

O exemplo de código a seguir demonstra o atravessamento do cliente criado no tópico [Criação de esquemas XML](building-xml-schemas.md) . O exemplo de código demonstra o atravessamento do esquema usando coleções descritos acima e grava todas os elementos e atributos no esquema no console.

O exemplo passa pelo esquema de cliente nas seguintes etapas.

1. Adiciona o esquema de cliente a um novo objeto de <xref:System.Xml.Schema.XmlSchemaSet> e compilá-lo em seguida. Todos os avisos de validação de esquema e leitura ou compilação encontrada erros o esquema são tratados pelo delegado de <xref:System.Xml.Schema.ValidationEventHandler> .

2. Recupera o objeto compilada de <xref:System.Xml.Schema.XmlSchema> de <xref:System.Xml.Schema.XmlSchemaSet> iterando sobre a propriedade de <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> . Porque o esquema é compilado, as propriedades de (PSCI) de POST-Esquema- compilação - Infoset são acessíveis.

3. Efetua iterações sobre cada <xref:System.Xml.Schema.XmlSchemaElement> na coleção de <xref:System.Xml.Schema.XmlSchemaObjectTable.Values%2A> de coleção de <xref:System.Xml.Schema.XmlSchema.Elements%2A?displayProperty=nameWithType> de POST-esquema- compilação que grava o nome de cada elemento no console.

4. Obtém o tipo complexo do elemento de `Customer` usando a classe de <xref:System.Xml.Schema.XmlSchemaComplexType> .

5. Se o tipo complexo tem quaisquer atributos, obtém <xref:System.Collections.IDictionaryEnumerator> para enumerar sobre cada <xref:System.Xml.Schema.XmlSchemaAttribute> e escreve seu nome no console.

6. Obtém a partícula a sequência de tipo complexo usando a classe de <xref:System.Xml.Schema.XmlSchemaSequence> .

7. Efetua iterações sobre cada <xref:System.Xml.Schema.XmlSchemaElement> na coleção de <xref:System.Xml.Schema.XmlSchemaSequence.Items%2A?displayProperty=nameWithType> que grava o nome de cada elemento filho no console.

A seguir está o exemplo de código completo.

[!code-cpp[XmlSchemaTraverseExample#1](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaTraverseExample/CPP/XmlSchemaTraverseExample.cpp#1)]
[!code-csharp[XmlSchemaTraverseExample#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaTraverseExample/CS/XmlSchemaTraverseExample.cs#1)]
[!code-vb[XmlSchemaTraverseExample#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaTraverseExample/VB/XmlSchemaTraverseExample.vb#1)]

A propriedade de <xref:System.Xml.Schema.XmlSchemaElement.ElementSchemaType%2A?displayProperty=nameWithType> pode ser <xref:System.Xml.Schema.XmlSchemaSimpleType>, ou <xref:System.Xml.Schema.XmlSchemaComplexType> se é um tipo simples definido pelo usuário ou um tipo complexo. Também pode ser <xref:System.Xml.Schema.XmlSchemaDatatype> se é um dos tipos de dados internos definidos na recomendação de Esquema XML do W3C. No esquema de cliente, <xref:System.Xml.Schema.XmlSchemaElement.ElementSchemaType%2A> do elemento de `Customer` é <xref:System.Xml.Schema.XmlSchemaComplexType>, e os elementos de `FirstName` e de `LastName` são <xref:System.Xml.Schema.XmlSchemaSimpleType>.

O exemplo de código no tópico [Criação de esquemas XML](building-xml-schemas.md) usou a coleção <xref:System.Xml.Schema.XmlSchemaComplexType.Attributes%2A?displayProperty=nameWithType> para adicionar o atributo `CustomerId` para o elemento `Customer`. Esta é uma propriedade de pre-esquema- compilação. A propriedade correspondente de POST-Esquema- compilação - Infoset é a coleção de <xref:System.Xml.Schema.XmlSchemaComplexType.AttributeUses%2A?displayProperty=nameWithType> , que contém todos os atributos do tipo complexo, incluindo aqueles que são herdadas com a derivação de tipo.

## <a name="see-also"></a>Veja também

- [Visão geral do modelo de objeto de esquema XML](xml-schema-object-model-overview.md)
- [Lendo e gravando esquemas XML](reading-and-writing-xml-schemas.md)
- [Compilando esquemas XML](building-xml-schemas.md)
- [Esquemas XML de edição](editing-xml-schemas.md)
- [Incluindo ou importando um esquema XML](including-or-importing-xml-schemas.md)
- [XmlSchemaSet para compilação de esquema](xmlschemaset-for-schema-compilation.md)
- [Compilação Infoset de pré esquema](post-schema-compilation-infoset.md)
