---
title: Elemento <settings> (Configurações de Rede)
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#settings
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/settings
helpviewer_keywords:
- settings element
- <settings> element
ms.assetid: 189ce989-c39b-427d-b004-6b82a668b931
ms.openlocfilehash: d510c445c585a36005ed415b14188efc4be03984
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74089112"
---
# <a name="settings-element-network-settings"></a>Elemento \<settings> (Configurações de Rede)
Configura as opções de rede básicaspara o namespace <xref:System.Net?displayProperty=nameWithType>.  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.net>**](system-net-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<settings>**

## <a name="syntax"></a>Sintaxe  
  
```xml  
<settings>  
  <httpListener>...</httpListener>  
  <httpWebRequest>...</httpWebRequest>  
  <ipv6>...</ipv6>  
  <performanceCounters>...</performanceCounters>  
  <servicePointManager>...</servicePointManager>  
  <socket>...</socket>  
  <webProxyScript>...</webProxyScript>  
</settings>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum.  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[httpListener](httplistener-element-network-settings.md)|Personaliza os parâmetros usados pela <xref:System.Net.HttpListener> classe.|  
|[httpWebRequest](httpwebrequest-element-network-settings.md)|Personaliza os parâmetros de solicitação da Web.|  
|[protocolo](ipv6-element-network-settings.md)|Habilita o suporte a IPv6 (protocolo IP versão 6).|  
|[\<performanceCounter>Elemento (configurações de rede)](performancecounter-element-network-settings.md)|Habilita contadores de desempenho de rede.|  
|[servicePointManager](servicepointmanager-element-network-settings.md)|Configura conexões com recursos de rede.|  
|[SSA](socket-element-network-settings.md)|Especifica se as operações de soquete usam portas de conclusão.|  
|[\<webProxyScript>Elemento (configurações de rede)](webproxyscript-element-network-settings.md)|Configura as características do script usado para descobrir proxies da Web.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[system.net](system-net-element-network-settings.md)|Contém configurações que especificam como o .NET Framework se conecta à rede.|  
  
## <a name="remarks"></a>Comentários  
  
## <a name="configuration-files"></a>Arquivos de configuração  
 Esse elemento pode ser usado no arquivo de configuração do aplicativo ou no arquivo de configuração do computador (Machine. config).  
  
## <a name="see-also"></a>Confira também

- <xref:System.Net?displayProperty=nameWithType>
- [Esquema de configurações de rede](index.md)
