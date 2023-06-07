---
title: Elemento <clear> para <appSettings>
ms.date: 05/01/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/appSettings/clear
helpviewer_keywords:
- clear Element
- <clear> Element
ms.assetid: 6d18c7be-27db-438b-8fb5-765d396b0b7b
ms.openlocfilehash: 266d32ccb8b322f0472e0f552f9c0fc877c9a78e
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "77214794"
---
# <a name="clear-element-for-appsettings"></a>Elemento \<clear> para \<appSettings>

Limpa as configurações de aplicativo personalizadas.

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<appSettings>**](appsettings-element-for-configuration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<clear>**

## <a name="syntax"></a>Sintaxe

```xml
<appSettings>
  <clear />
</appSettings>
```

## <a name="attributes"></a>Atributos

Nenhum

## <a name="parent-element"></a>Elemento pai

|     | Descrição |
| --- | ----------- |
| [**\<appSettings>**](appsettings-element-for-configuration.md) | Contém configurações de aplicativo personalizadas, como caminhos de arquivo, URLs de serviço Web XML ou qualquer outra informação de configuração de aplicativo personalizada. |

## <a name="child-elements"></a>Elementos filho

Nenhum

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como limpar as definições de configuração personalizadas:

```xml
<appSettings>
  <clear />
</appSettings>
```

## <a name="see-also"></a>Confira também

- [Esquema do arquivo de configuração para o .NET Framework](../index.md)
