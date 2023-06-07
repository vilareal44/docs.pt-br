---
title: <remove>elemento para NameValueSectionHandler e DictionarySectionHandler
ms.date: 05/01/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/sectionName/remove
helpviewer_keywords:
- remove Element
- <remove> Element
ms.assetid: 8d8af7f5-26c9-4db9-bbe4-b2a4e6949568
ms.openlocfilehash: d1e4f3478f6afd6a20c01c6b57a137020ee88f5f
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "77214764"
---
# <a name="remove-element-for-namevaluesectionhandler-and-dictionarysectionhandler"></a>\<remove>elemento para NameValueSectionHandler e DictionarySectionHandler

Remove uma configuração definida anteriormente.

[**\<configuration>**](configuration-element.md)\
&nbsp;&nbsp;[**\<sectionName>**](custom-element-2.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<remove>**

## <a name="syntax"></a>Sintaxe

```xml
<add remove="key" />
```

## <a name="attribute"></a>Atributo

|           | Descrição |
| --------- | ----------- |
| **chave**   | Atributo obrigatório.<br><br>Especifica o nome da configuração a ser removida. |

## <a name="parent-element"></a>Elemento pai

| Elemento | Descrição |
| ------- | ------------|
| [**\<sectionName>** Elementos](custom-element-2.md) | Define as configurações para seções de configuração personalizadas que usam as <xref:System.Configuration.NameValueSectionHandler> <xref:System.Configuration.DictionarySectionHandler> classes e. |

## <a name="child-elements"></a>Elementos filho

Nenhum

## <a name="remarks"></a>Comentários

Você pode usar o **\<remove>** elemento para remover as configurações de seu aplicativo que foram definidas em um nível mais alto na hierarquia do arquivo de configuração.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como usar o **\<remove>** elemento em um arquivo de configuração de aplicativo para remover as configurações definidas anteriormente no arquivo de configuração do computador.

O código do arquivo de configuração do computador a seguir declara a seção **\<mySection>** e adiciona duas configurações, `key1` e `key2` , a ela:

```xml
<!-- Machine.config file -->
<configuration>
  <configSections>
    <section name="mySection" type="System.Configuration.NameValueSectionHandler,System" />
  </configSections>
  <mySection>
    <add key="key1" value="value1" />
    <add key="key2" value="value2" />
  </mySection>
</configuration>
```

O código do arquivo de configuração de aplicativo a seguir remove a `key2` configuração de **\<mySection>** :

```xml
<!--Application configuration file -->
<configuration>
  <mySection>
    <remove key="key2" />
  </mySection>
</configuration>
```

## <a name="configuration-file"></a>Arquivo de configuração

Esse elemento pode ser usado no arquivo de configuração do aplicativo, no arquivo de configuração do computador (*Machine. config*) e nos arquivos *Web. config* que não estão no nível do diretório do aplicativo.

## <a name="see-also"></a>Confira também

- [Esquema do arquivo de configuração para o .NET Framework](index.md)
