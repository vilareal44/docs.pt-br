---
title: Exportando esquemas de classes
ms.date: 03/30/2017
helpviewer_keywords:
- WCF, schema import and export
- schemas [WCF], exporting from classes
- schemas [WCF]
- XsdDataContractExporter class
- XsdDataContractImporter class
ms.assetid: bb57b962-70c1-45a9-93d5-e721e340a13f
ms.openlocfilehash: d356450af8ce6690e2142f3487e153bcde095324
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595512"
---
# <a name="exporting-schemas-from-classes"></a>Exportando esquemas de classes
Para gerar esquemas XSD (linguagem de definição de esquema XML) de classes usadas no modelo de contrato de dados, use a <xref:System.Runtime.Serialization.XsdDataContractExporter> classe. Este tópico descreve o processo de criação de esquemas.  
  
## <a name="the-export-process"></a>O processo de exportação  
 O processo de exportação de esquema começa com um ou mais tipos e produz um <xref:System.Xml.Schema.XmlSchemaSet> que descreve a projeção XML desses tipos.  
  
 O `XmlSchemaSet` faz parte do som (modelo de objeto de esquema) do .NET Framework que representa um conjunto de documentos de esquema XSD. Para criar documentos XSD a partir de um `XmlSchemaSet` , use a coleção de esquemas da <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> propriedade da `XmlSchemaSet` classe. Em seguida, Serialize cada <xref:System.Xml.Schema.XmlSchema> objeto usando o <xref:System.Xml.Serialization.XmlSerializer> .  
  
#### <a name="to-export-schemas"></a>Para exportar esquemas  
  
1. Crie uma instância de <xref:System.Runtime.Serialization.XsdDataContractExporter>.  
  
2. Opcional. Passe um <xref:System.Xml.Schema.XmlSchemaSet> no construtor. Nesse caso, o esquema gerado durante a exportação de esquema é adicionado a essa <xref:System.Xml.Schema.XmlSchemaSet> instância em vez de começar com um espaço em branco <xref:System.Xml.Schema.XmlSchemaSet> .  
  
3. Opcional. Chame um dos <xref:System.Runtime.Serialization.XsdDataContractExporter.CanExport%2A> métodos. O método determina se o tipo especificado pode ser exportado. O método tem as mesmas sobrecargas que o `Export` método na próxima etapa.  
  
4. Chame um dos <xref:System.Runtime.Serialization.XsdDataContractExporter.Export%2A> métodos. Há três sobrecargas que tomam um <xref:System.Type> , um <xref:System.Collections.Generic.List%601> de `Type` objetos ou um <xref:System.Collections.Generic.List%601> de <xref:System.Reflection.Assembly> objetos. No último caso, todos os tipos em todos os assemblies fornecidos são exportados.  
  
     Várias chamadas para o `Export` método resultam na adição de vários itens ao mesmo `XmlSchemaSet` . Um tipo não será gerado no `XmlSchemaSet` se ele já existir. Portanto, chamar `Export` várias vezes no mesmo `XsdDataContractExporter` é preferível a criar várias instâncias da `XsdDataContractExporter` classe. Isso evita que tipos de esquema duplicados sejam gerados.  
  
    > [!NOTE]
    > Se houver uma falha durante a exportação, o `XmlSchemaSet` estará em um estado imprevisível.  
  
5. Acesse o <xref:System.Xml.Schema.XmlSchemaSet> por meio da <xref:System.Runtime.Serialization.XsdDataContractExporter.Schemas%2A> propriedade.  
  
## <a name="export-options"></a>Opções de exportação  
 Você pode definir a <xref:System.Runtime.Serialization.XsdDataContractExporter.Options%2A> propriedade de <xref:System.Runtime.Serialization.XsdDataContractExporter> como uma instância da <xref:System.Runtime.Serialization.ExportOptions> classe para controlar vários aspectos do processo de exportação. Especificamente, você pode definir as seguintes opções:  
  
- <xref:System.Runtime.Serialization.ExportOptions.KnownTypes%2A>. Essa coleção de `Type` representa os tipos conhecidos para os tipos que estão sendo exportados. (Para obter mais informações, consulte [tipos conhecidos de contrato de dados](data-contract-known-types.md).) Esses tipos conhecidos são exportados em cada `Export` chamada, além dos tipos passados para o `Export` método.  
  
- <xref:System.Runtime.Serialization.ExportOptions.DataContractSurrogate%2A>. Um <xref:System.Runtime.Serialization.IDataContractSurrogate> pode ser fornecido por essa propriedade que personalizará o processo de exportação. Para obter mais informações, consulte [substitutos de contrato de dados](../extending/data-contract-surrogates.md). Por padrão, nenhum substituto é usado.  
  
## <a name="helper-methods"></a>Métodos auxiliares  
 Além de sua principal função de exportação de esquema, o `XsdDataContractExporter` fornece vários métodos auxiliares úteis que fornecem informações sobre tipos. Elas incluem:  
  
- Método <xref:System.Runtime.Serialization.XsdDataContractExporter.GetRootElementName%2A>. Esse método usa `Type` e retorna um <xref:System.Xml.XmlQualifiedName> que representa o nome do elemento raiz e o namespace que seriam usados se esse tipo fosse serializado como o objeto raiz.  
  
- Método <xref:System.Runtime.Serialization.XsdDataContractExporter.GetSchemaTypeName%2A>. Esse método usa `Type` e retorna um <xref:System.Xml.XmlQualifiedName> que representa o nome do tipo de esquema XSD que seria usado se esse tipo fosse exportado para o esquema. Para <xref:System.Xml.Serialization.IXmlSerializable> tipos representados como tipos anônimos no esquema, esse método retorna `null` .  
  
- Método <xref:System.Runtime.Serialization.XsdDataContractExporter.GetSchemaType%2A>. Esse método só funciona com <xref:System.Xml.Serialization.IXmlSerializable> tipos que são representados como tipos anônimos no esquema e retorna `null` para todos os outros tipos. Para tipos anônimos, esse método retorna um <xref:System.Xml.Schema.XmlSchemaType> que representa um determinado `Type` .  
  
 As opções de exportação afetam todos esses métodos.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Runtime.Serialization.XsdDataContractImporter>
- <xref:System.Runtime.Serialization.XsdDataContractExporter>
- [Exportação e importação de esquema](schema-import-and-export.md)
- [Importando o esquema para gerar classes](importing-schema-to-generate-classes.md)
