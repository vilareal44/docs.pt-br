---
title: Elemento personalizado para SingleTagSectionHandler
ms.date: 05/01/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/sectionName
helpviewer_keywords:
- custom element
ms.assetid: e62056c6-b351-40eb-afc0-cc13fc44e45e
ms.openlocfilehash: a40f35838655f6021af0b2e966335803ec8c16b4
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "80635393"
---
# <a name="custom-element-for-singletagsectionhandler"></a>Elemento personalizado para SingleTagSectionHandler

Define as configurações em uma seção de configuração personalizada que é definida por um \<section> elemento e usa a <xref:System.Configuration.SingleTagSectionHandler> classe.

[**\<configuration>**](configuration-element.md) &nbsp;&nbsp;*\<sectionName>*

## <a name="syntax"></a>Sintaxe

```xml
<sectionName key="value" key2="value2" />
```

## <a name="attributes"></a>Atributos

Atributos e valores de atributo são definidos pelo usuário.

## <a name="parent-element"></a>Elemento pai

|     | Descrição |
| --- | ----------- |
| [**\<configuration>**](configuration-element.md) | O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework. |

## <a name="child-elements"></a>Elementos filho

Nenhum

## <a name="remarks"></a>Comentários

O **\<sectionName>** elemento é um elemento personalizado definido por uma [**\<section>**](section-element.md) marca no [**\<configSections>**](configsections-element-for-configuration.md) elemento. O sistema de configuração retorna um <xref:System.Collections.IDictionary> objeto quando você chama <xref:System.Configuration.Configuration.GetSection(System.String)?displayProperty=nameWithType> .

## <a name="example"></a>Exemplo

O exemplo a seguir declara um elemento personalizado chamado **\<sampleSection>** que contém as configurações lidas pela <xref:System.Configuration.SingleTagSectionHandler> classe:

```xml
<configuration>
  <configSections>
    <section name="sampleSection"
             type="System.Configuration.SingleTagSectionHandler" />
  </configSections>
  <sampleSection setting1="Value1"
                 setting2="value two"
                 setting3="third value" />
</configuration>
```

## <a name="configuration-file"></a>Arquivo de configuração

Esse elemento pode ser usado no arquivo de configuração do aplicativo, no arquivo de configuração do computador (*Machine. config*) e nos arquivos *Web. config* que não estão no nível do diretório do aplicativo.

## <a name="see-also"></a>Confira também

- [Esquema do arquivo de configuração para o .NET Framework](index.md)
