---
title: Elemento <system.xml.serialization>
description: Este artigo descreve o < elemento System. xml. Serialization >, que é o elemento de nível superior para controlar a serialização de XML.
ms.date: 03/30/2017
helpviewer_keywords:
- system.xml.serialization element
- XML serialization, configuration
- <system.xml.serialization> element
ms.assetid: 3ce45919-388a-418c-8968-6df0372c73ec
ms.openlocfilehash: f69e80592e9321de64421b977a63b83d8be2ad9e
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84289480"
---
# <a name="systemxmlserialization-element"></a>Elemento \<system.xml.serialization>

O elemento de nível superior para controlar a serialização XML. Para obter mais informações sobre arquivos de configuração, consulte [Esquema de arquivos de configuração](../../framework/configure-apps/file-schema/index.md).

\<configuration>\
\<system.xml.serialization>

## <a name="syntax"></a>Sintaxe

```xml
<system.xml.serialization>
</system.xml.serialization>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

Nenhum.

### <a name="child-elements"></a>Elementos filho

|Elemento|Descrição|
|-------------|-----------------|
|[\<dateTimeSerialization>Elementos](datetimeserialization-element.md)|Determina o modo de serialização de objetos <xref:System.DateTime>.|
|[\<schemaImporterExtensions>Elementos](schemaimporterextensions-element.md)|Contém tipos que são usados pelo <xref:System.Xml.Serialization.XmlSchemaImporter> para mapeamento de tipos XSD para tipos do .NET Framework.|

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|[\<configuration>Elementos](../../framework/configure-apps/file-schema/configuration-element.md)|O elemento raiz em todos os arquivos de configuração que é usado pelo common language runtime e aplicativos do .NET Framework.|

## <a name="example"></a>Exemplo

O exemplo de código a seguir ilustra como especificar o modo de serialização de um objeto <xref:System.DateTime> e a adição de tipos usada pelo <xref:System.Xml.Serialization.XmlSchemaImporter> ao mapear os tipos XSD para os tipos do .NET Framework.

```xml
<system.xml.serialization>
    <xmlSerializer checkDeserializeAdvances="false" />
    <dateTimeSerialization mode = "Local" />
    <schemaImporterExtensions>
        <add
        name = "MobileCapabilities"
        type = "System.Web.Mobile.MobileCapabilities,
        System.Web.Mobile, Version=2.0.0.0, Culture=neutral,
        PublicKeyToken=b03f5f6f11d40a3a" />
    </schemaImporterExtensions>
</system.xml.serialization>
```

## <a name="see-also"></a>Confira também

- <xref:System.Xml.Serialization.XmlSchemaImporter>
- <xref:System.Xml.Serialization.Configuration.DateTimeSerializationSection.DateTimeSerializationMode>
- [Esquema de arquivos de configuração](../../framework/configure-apps/file-schema/index.md)
- [\<dateTimeSerialization>Elementos](datetimeserialization-element.md)
- [\<schemaImporterExtensions>Elementos](schemaimporterextensions-element.md)
- [\<add>Elemento para\<schemaImporterExtensions>](add-element-for-schemaimporterextensions.md)
