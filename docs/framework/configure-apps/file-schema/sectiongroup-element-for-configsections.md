---
title: Elemento <sectionGroup> para <configSections>
ms.date: 05/01/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/configSections/sectionGroup
helpviewer_keywords:
- sectionGroup Element
- <sectionGroup> Element
ms.assetid: 6c27f9e2-809c-4bc9-aca9-72f90360e7a3
ms.openlocfilehash: eb221027470fe6e485f8fcc4b939b71e4f219712
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "77215258"
---
# <a name="sectiongroup-element-for-configsections"></a>Elemento \<sectionGroup> para \<configSections>

Define um namespace para seções de configuração.

[**\<configuration>**](configuration-element.md)\
&nbsp;&nbsp;[**\<configSections>**](configsections-element-for-configuration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<sectionGroup>**

## <a name="syntax"></a>Sintaxe

```xml
<sectionGroup name="section group name">
  <!-- Configuration sections -->
</sectionGroup>
```

## <a name="attribute"></a>Atributo

|           | Descrição |
| --------- | ----------- |
| **name**  | Atributo obrigatório.<br><br>Especifica o nome do grupo de seções que você está definindo. |

## <a name="parent-element"></a>Elemento pai

|     | Descrição |
| --- | ----------- |
| [**\<configSections>** Elementos](configsections-element-for-configuration.md) | Contém as declarações de namespace e seção de configuração. |

## <a name="child-elements"></a>Elementos filho

|     | Descrição |
| --- | ----------- |
| [**\<section>**](section-element.md) | Contém uma declaração de seção de configuração. |

## <a name="remarks"></a>Comentários

Declarar um grupo de seções cria uma marca de contêiner para seções de configuração e garante que não haja conflitos de nomenclatura com seções de configuração definidas por outra pessoa. Você pode aninhar **\<sectionGroup>** elementos dentro uns dos outros.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como declarar um grupo de seções e declarar seções dentro de um grupo de seções:

```xml
<configuration>
  <configSections>
    <sectionGroup name="mySectionGroup">
      <section name="mySection"
               type="System.Configuration.NameValueSectionHandler,System" />
    </sectionGroup>
  </configSections>
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
