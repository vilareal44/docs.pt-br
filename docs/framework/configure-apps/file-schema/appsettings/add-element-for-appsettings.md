---
title: Elemento <add> para <appSettings>
ms.date: 05/01/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/appSettings/add
helpviewer_keywords:
- add Element
- <add> Element
ms.assetid: 8734efdc-00f6-4a65-bba6-084c5bc65246
ms.openlocfilehash: 5c7de79ec626966e71d461dd3865b294a8979db2
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "77214806"
---
# <a name="add-element-for-appsettings"></a>Elemento \<add> para \<appSettings>

Adiciona uma configuração de aplicativo personalizada.

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<appSettings>**](appsettings-element-for-configuration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**

## <a name="syntax"></a>Sintaxe

```xml
<appSettings>
  <add key="Key of custom setting" value="Value of custom setting" />
</appSettings>
```

## <a name="attributes"></a>Atributos

|           | Descrição |
| --------- | ----------- |
| **chave**   | Atributo obrigatório.<br><br>Especifica o nome da chave a ser adicionada. |
| **value** | Atributo obrigatório.<br><br>Especifica o valor da chave a ser adicionada. |

## <a name="parent-element"></a>Elemento pai

|     | Descrição |
| --- | ----------- |
| [**\<appSettings>**](appsettings-element-for-configuration.md) | Contém configurações de aplicativo personalizadas, como caminhos de arquivo, URLs de serviço da Web em XML ou qualquer outra informação de configuração personalizada para um aplicativo. |

## <a name="child-elements"></a>Elementos filho

Nenhum

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como adicionar uma definição de configuração personalizada para o nome do aplicativo:

```xml
<appSettings>
  <add key="ApplicationName" value="MyApplication" />
</appSettings>
```

O exemplo a seguir usa o `<add>` elemento para definir duas configurações de compatibilidade em um aplicativo ASP.net:

```xml
<appSettings>
  <add key="AppContext.SetSwitch:Switch.System.Globalization.NoAsyncCurrentCulture" value="true" />
  <add key="AppContext.SetSwitch:Switch.System.Uri.DontEnableStrictRFC3986ReservedCharacterSets" value="true" />
</appSettings>
```

## <a name="see-also"></a>Confira também

- [Esquema do arquivo de configuração para o .NET Framework](../index.md)
