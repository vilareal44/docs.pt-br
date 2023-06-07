---
title: Elemento <performanceCounter> (Configurações de Rede)
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/settings/performanceCounters
- http://schemas.microsoft.com/.NetConfiguration/v2.0#performanceCounters
helpviewer_keywords:
- performanceCounter element
- <performanceCounter> element
ms.assetid: 3afa1586-e1b8-473d-8985-c3fc90cf561b
ms.openlocfilehash: 58a2bf5118a3a2cd9c33301eca5dcc751c2351bf
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74283087"
---
# <a name="performancecounter-element-network-settings"></a>Elemento \<performanceCounter> (Configurações de Rede)
Habilita ou desabilita os contadores de desempenho de rede.  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.net>**](system-net-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<settings>**](settings-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<performanceCounters>**

## <a name="syntax"></a>Sintaxe  
  
```xml  
<performanceCounters  
  enabled="true|false"  
/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`enabled`|Especifica se os contadores de desempenho de rede estão habilitados. O valor padrão é `false`.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[configurações](settings-element-network-settings.md)|Configura as opções de rede básicaspara o namespace <xref:System.Net>.|  
  
## <a name="remarks"></a>Comentários  
 Esse elemento pode ser usado no arquivo de configuração do aplicativo ou no arquivo de configuração do computador (Machine. config).  
  
 Contadores de desempenho de rede precisam ser habilitados no arquivo de configuração a ser usado. Todos os contadores de desempenho de rede são habilitados ou desabilitados com uma única configuração no arquivo de configuração. Contadores de desempenho de rede individuais não podem ser habilitados nem desabilitados. Para obter mais informações sobre os contadores de desempenho de rede específicos, consulte [Network Performance Counters](../../../debug-trace-profile/performance-counters.md#networking-performance-counters).  
  
 O valor padrão é que os contadores de desempenho de rede estão desabilitados.  
  
 A <xref:System.Net.Configuration.PerformanceCountersElement.Enabled%2A?displayProperty=nameWithType> propriedade pode ser usada para obter o valor atual do atributo **habilitado** dos arquivos de configuração aplicáveis.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como configurar o <xref:System.Net> e os namespaces relacionados para habilitar os contadores de desempenho de rede.  
  
```xml  
<configuration>  
  <system.net>  
    <settings>  
      <performanceCounters  
        enabled="true"  
      />  
    </settings>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Net.Configuration.PerformanceCountersElement?displayProperty=nameWithType>
- <xref:System.Net.Configuration.PerformanceCountersElement.Enabled%2A?displayProperty=nameWithType>
- [Esquema de configurações de rede](index.md)
- [Contadores de desempenho de rede](../../../debug-trace-profile/performance-counters.md#networking-performance-counters)
