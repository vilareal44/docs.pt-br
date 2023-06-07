---
title: Esquema de configurações Web
ms.date: 03/30/2017
helpviewer_keywords:
- Web.config configuration file [ASP.NET]
- ASP.NET configuration system, Web settings schema
- schema Web settings
- Web settings, schema [ASP.NET]
- configuration files [ASP.NET]
- configuration schema [.NET Framework], Web settings
ms.assetid: ae1ac356-267d-4753-8d7a-7a04eb45a9be
ms.openlocfilehash: 030841330ff37cddb0c9e3e466a55a4be098e784
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74088784"
---
# <a name="web-settings-schema"></a>Esquema de configurações Web
As configurações de Web especificam as configurações do ASP.NET em nível de execução que se aplicam ao comportamento de todo o processo gerenciado pela camada de hospedagem do ASP.NET. Essas configurações são diferentes das configurações de tipo de domínio do aplicativo que são especificadas no arquivo Web.config de um aplicativo ASP.NET.  
  
As configurações da Web estão contidas em arquivos Aspnet.config, que estão localizados nas pastas de instalação das versões do .NET Framework. Por exemplo, o arquivo Aspnet. config para .NET Framework 2,0 está na seguinte pasta:  
  
`C:\Windows\Microsoft.NET\Framework\v2.0.50727\`  
  
As configurações da Web não são usadas em nenhum outro arquivo de configuração, como o arquivo machine.config, o Web.config raiz ou os arquivos Web.config do nível do aplicativo.  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.web>**](system-web-element-web-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<applicationPool>**](applicationpool-element-web-settings.md)

|Elemento|Descrição|  
|-------------|-----------------|  
|[\<system.web>](system-web-element-web-settings.md)|Contém informações que a camada de hospedagem do ASP.NET usa.|  
|[\<applicationPool>](applicationpool-element-web-settings.md)|Especifica as configurações de CPU e do ASP.NET em nível de execução que se aplicam ao comportamento de todo o processo gerenciado pela camada de hospedagem do ASP.NET.|  
  
## <a name="see-also"></a>Confira também

- [Esquema do arquivo de configuração](../index.md)
