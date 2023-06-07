---
title: <clear>elemento para NameValueSectionHandler e DictionarySectionHandler
ms.date: 05/01/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/sectionName/clear
helpviewer_keywords:
- clear Element
- <clear> Element
ms.assetid: ff2294ec-fb82-4b0c-933e-ae185433fc7b
ms.openlocfilehash: f6d860f35d22002030ffa3d09dd0d8a96116bf5e
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "77214751"
---
# <a name="clear-element-for-namevaluesectionhandler-and-dictionarysectionhandler"></a>\<clear>elemento para NameValueSectionHandler e DictionarySectionHandler

Limpa todas as configurações definidas anteriormente em uma seção.

[**\<configuration>**](configuration-element.md)\
&nbsp;&nbsp;[**\<sectionName>**](custom-element-2.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<clear>**

## <a name="syntax"></a>Sintaxe

```xml
<clear />
```

## <a name="attributes"></a>Atributos

Nenhum

## <a name="parent-element"></a>Elemento pai

|     | Descrição |
| --- | ------------|
| [**\<sectionName>** Elementos](custom-element-2.md) | Define as configurações para seções de configuração personalizadas que usam as <xref:System.Configuration.NameValueSectionHandler> <xref:System.Configuration.DictionarySectionHandler> classes e. |

## <a name="child-elements"></a>Elementos filho

Nenhum

## <a name="remarks"></a>Comentários

Você pode usar o **\<clear>** elemento para remover todas as configurações do aplicativo que foram definidas em um nível mais alto na hierarquia do arquivo de configuração.

## <a name="example"></a>Exemplo

Este exemplo define um arquivo de configuração de computador e um arquivo de configuração de aplicativo e mostra como usar o **\<clear>** elemento em um arquivo de configuração de aplicativo para limpar as seções definidas anteriormente no arquivo de configuração de computador.

O código do arquivo de configuração do computador a seguir declara a seção **\<mySection>** :

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

O código do arquivo de configuração de aplicativo a seguir remove todas as configurações de **\<mySection>** . O aplicativo não pode recuperar nenhuma das configurações que foram declaradas no na **\<mySection>** seção do arquivo de configuração do computador.

```xml
<!-- Application configuration file -->
<configuration>
  <mySection>
    <clear/>
  </mySection>
</configuration>
```

## <a name="configuration-file"></a>Arquivo de configuração

Esse elemento pode ser usado no arquivo de configuração do aplicativo, no arquivo de configuração do computador (*Machine. config*) e nos arquivos *Web. config* que não estão no nível do diretório do aplicativo.

## <a name="see-also"></a>Confira também

- [Esquema do arquivo de configuração para o .NET Framework](index.md)
