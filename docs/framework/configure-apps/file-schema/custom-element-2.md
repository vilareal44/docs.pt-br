---
title: Elemento personalizado para NameValueSectionHandler e DictionarySectionHandler
ms.date: 05/01/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/sectionName
helpviewer_keywords:
- custom element
ms.assetid: 2303031f-4c1d-4df4-bca1-e9bd96ca40dc
ms.openlocfilehash: e5c5c6cf5744aa385e6f6700cad623751a4d7427
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "77215491"
---
# <a name="custom-element-for-namevaluesectionhandler-and-dictionarysectionhandler"></a>Elemento personalizado para NameValueSectionHandler e DictionarySectionHandler

Define as configurações para seções de configuração personalizadas que usam as <xref:System.Configuration.NameValueSectionHandler> <xref:System.Configuration.DictionarySectionHandler> classes e.

[**\<configuration>**](configuration-element.md)\
&nbsp;&nbsp;**\<sectionName>**

## <a name="attributes"></a>Atributos

Nenhum

## <a name="parent-element"></a>Elemento pai

|     | Descrição |
| --- | ----------- |
| [**\<configuration>**](configuration-element.md) | O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework. |

## <a name="child-elements"></a>Elementos filho

|     | Descrição |
| --- | ----------- |
| [**\<add>**](add-element-for-custom-2.md)para <xref:System.Configuration.NameValueSectionHandler> e<xref:System.Configuration.DictionarySectionHandler>  | Adiciona configurações de aplicativo personalizadas. |
| [**\<remove>**](remove-element-for-custom-2.md)para <xref:System.Configuration.NameValueSectionHandler> e<xref:System.Configuration.DictionarySectionHandler> | Remove uma configuração definida anteriormente. |
| [**\<clear>**](clear-element-for-custom-2.md)para <xref:System.Configuration.NameValueSectionHandler> e<xref:System.Configuration.DictionarySectionHandler> | Limpa todas as configurações definidas anteriormente em uma seção. |

## <a name="remarks"></a>Comentários

O **\<sectionName>** elemento é um elemento personalizado definido por uma **\<section>** marca no **\<configSections>** elemento.

A tabela a seguir mostra o tipo de objeto que o método ConfigurationSettings. GetConfig retorna para cada manipulador de seção de configuração:

| Manipulador da seção de configuração                        | Tipo de retorno                                                |
| ---------------------------------------------------- | ---------------------------------------------------------- |
| <xref:System.Configuration.NameValueSectionHandler>  | <xref:System.Collections.Specialized.NameValueCollection>  |
| <xref:System.Configuration.DictionarySectionHandler> | <xref:System.Collections.IDictionary>                      |

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como declarar seções que usam as <xref:System.Configuration.DictionarySectionHandler> <xref:System.Configuration.NameValueSectionHandler> classes e.

O primeiro elemento personalizado é **\<dictionarySample>** , que contém as configurações lidas pela <xref:System.Configuration.DictionarySectionHandler> classe no `System.dll` assembly. O segundo elemento personalizado é **\<mySection>** , que contém as configurações lidas pela <xref:System.Configuration.NameValueSectionHandler> classe no `System.dll` assembly.

```xml
<configuration>
  <configSections>
    <section name="dictionarySample" type="System.Configuration.DictionarySectionHandler,System" />
    <sectionGroup name="mySectionGroup">
      <section name="mySection" type="System.Configuration.NameValueSectionHandler,System" />
    </sectionGroup>
  </configSections>
  <dictionarySample>
    <add key="myKey" value="myValue" />
  </dictionarySample>
  <mySectionGroup>
    <mySection>
      <add key="key1" value="value1" />
    </mySection>
  </mySectionGroup>
</configuration>
```

## <a name="configuration-file"></a>Arquivo de configuração

Esse elemento pode ser usado no arquivo de configuração do aplicativo, no arquivo de configuração do computador (*Machine. config*) e nos arquivos *Web. config* que não estão no nível do diretório do aplicativo.

## <a name="see-also"></a>Confira também

- [Esquema do arquivo de configuração para o .NET Framework](index.md)
