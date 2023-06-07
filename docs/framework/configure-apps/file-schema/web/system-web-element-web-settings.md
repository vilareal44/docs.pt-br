---
title: Elemento <system.web> (Configurações da Web)
ms.date: 03/30/2017
helpviewer_keywords:
- Web.config configuration file [ASP.NET]
- system.Web element
- <system.Web> element
- ASP.NET configuration system
- configuration files [ASP.NET]
ms.assetid: 24c4cf4f-ad32-42b2-b040-8e4549e2855e
ms.openlocfilehash: b37b05bdf90630251cbfcf86751243a3a8b77663
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79152835"
---
# <a name="systemweb-element-web-settings"></a>Elemento \<system.web> (configurações da Web)
Contém informações sobre como a camada de hospedagem ASP.NET gerencia o comportamento de todo o processo.  
  
[**\<configuration>**](../configuration-element.md)  
&nbsp;&nbsp;**\<system.web>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.web>  
</system.web>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  

As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  

Nenhum.  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<applicationPool>](applicationpool-element-web-settings.md)|Especifica as definições de configuração para pools de aplicativos do IIS em um arquivo Aspnet. config.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<configuration>](../configuration-element.md)|Especifica o elemento raiz em cada arquivo de configuração que é usado pelo Common Language Runtime e .NET Framework aplicativos.|  
  
## <a name="remarks"></a>Comentários  

O `system.web` elemento e seu `applicationPool` elemento filho foram adicionados à .NET Framework a partir de .NET Framework 3,5 SP1. Quando você executa o IIS 7,0 ou versões posteriores no modo integrado, essa combinação de elementos permite que você configure como o ASP.NET gerencia threads e como ele enfileira solicitações quando o ASP.NET está hospedado em um pool de aplicativos do IIS. Se você executar o IIS 7,0 ou versões posteriores no modo clássico ou ISAPI, essas configurações serão ignoradas.  
  
## <a name="example"></a>Exemplo  

O exemplo a seguir mostra como configurar o comportamento do ASP.NET em todo o processo no arquivo Aspnet. config quando o ASP.NET está hospedado em um pool de aplicativos do IIS. O exemplo supõe que o IIS está sendo executado no modo integrado e que o aplicativo está usando o .NET Framework 3,5 SP1 ou uma versão posterior. Esse comportamento não ocorre em versões do .NET Framework anteriores ao .NET Framework 3,5 SP1. Os valores no exemplo são os valores padrão.  
  
```xml  
<configuration>  
  <system.web>  
    <applicationPool
        maxConcurrentRequestsPerCPU="5000"
        maxConcurrentThreadsPerCPU="0"
        requestQueueLimit="5000" />  
  </system.web>  
</configuration>  
```  
  
## <a name="element-information"></a>Informações do elemento  
  
|||  
|-|-|  
|Namespace||  
|Nome do Esquema||  
|Arquivo de validação||  
|Pode estar vazio||  
  
## <a name="see-also"></a>Confira também

- [\<applicationPool>Elemento (configurações da Web)](applicationpool-element-web-settings.md)
