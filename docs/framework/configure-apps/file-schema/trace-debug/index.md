---
title: Esquema de configurações de rastreamento e depuração
ms.date: 03/30/2017
helpviewer_keywords:
- tracing [.NET Framework], trace and debug settings schema
- configuration schema [.NET Framework], trace and debug settings
- configuration settings [.NET Framework], tracing
- schema configuration settings
- configuration settings [.NET Framework], debugging
- trace listeners, trace and debug settings schema
- configuration sections [.NET Framework]
- elements [.NET Framework], trace and debug settings
ms.assetid: 277ca5f6-e1c4-41b6-a47f-3a67ce5b94ac
ms.openlocfilehash: 037d08b33e9aa6a64d236b36ebcf821b604b03df
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "69927127"
---
# <a name="trace-and-debug-settings-schema"></a>Esquema de configurações de rastreamento e depuração
As configurações de rastreamento e depuração especificam ouvintes de rastreamento que coletam, armazenam e roteiam mensagens e o nível em que uma opção de rastreamento é definida.  
  
 A tabela a seguir descreve a função de cada elemento de configurações de rastreamento e depuração.  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<add>](add-element-for-listeners-for-source.md)|Adiciona um ouvinte na coleção `Listeners` de uma origem de rastreamento.|  
|[\<add>](add-element-for-listeners-for-trace.md)|Adiciona um ouvinte na coleção `Listeners`.|  
|[\<add>](add-element-for-sharedlisteners.md)|Adiciona um ouvinte na coleção `sharedListeners`.|  
|[\<add>](add-element-for-switches.md)|Especifica o nível em que uma opção de rastreamento é definida.|  
|[\<assert>](assert-element.md)|Especifica se uma caixa de mensagem deve ser exibida ao chamar o método <xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType>; também especifica o nome do arquivo no qual as mensagens serão gravadas.|  
|[\<clear>](clear-element-for-listeners-for-source.md)|Limpa a coleção `Listeners` de uma origem de rastreamento.|  
|[\<clear>](clear-element-for-listeners-for-trace.md)|Limpa a coleção `Listeners` do rastreamento.|  
|[\<filter>](filter-element-for-add-for-listeners-for-source.md)|Adiciona um filtro a um ouvinte na coleção `Listeners` de uma origem de rastreamento.|  
|[\<filter>](filter-element-for-add-for-listeners-for-trace.md)|Adiciona um filtro a um ouvinte na coleção `Listeners` do rastreamento.|  
|[\<filter>](filter-element-for-add-for-sharedlisteners.md)|Adiciona um filtro a um ouvinte na coleção `sharedListeners`.|  
|[\<listeners>](listeners-element-for-source.md)|Especifica os ouvintes da coleção `Listeners` de uma origem de rastreamento.|  
|[\<listeners>](listeners-element-for-trace.md)|Especifica os ouvintes da coleção `Listeners` do rastreamento.|  
|[\<performanceCounters>](performancecounters-element.md)|Especifica o tamanho da memória global compartilhada por contadores de desempenho.|  
|[\<remove>](remove-element-for-listeners-for-trace.md)|Remove um ouvinte da coleção `Listeners` do rastreamento.|  
|[\<remove>](remove-element-for-listeners-for-source.md)|Remove um ouvinte da coleção `Listeners` de uma origem de rastreamento.|  
|[\<sharedListeners>](sharedlisteners-element.md)|Contém os ouvintes que podem ser referenciados por qualquer elemento de origem ou de rastreamento.|  
|[\<sources>](sources-element.md)|Contém as origens de rastreamento que iniciam as mensagens de rastreamento.|  
|[\<source>](source-element.md)|Especifica uma origem de rastreamento que inicia as mensagens de rastreamento.|  
|[\<switches>](switches-element.md)|Contém opções de rastreamento e o nível em que as opções de rastreamento são definidas.|  
|[\<system.diagnostics>](system-diagnostics-element.md)|Especifica os ouvintes de rastreamento que coletam, armazenam e roteiam mensagens e o nível em que uma opção de rastreamento é definida.|  
|[\<trace>](trace-element.md)|Contém os ouvintes que coletam, armazenam e roteiam mensagens de rastreamento.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.Diagnostics.Trace>
- <xref:System.Diagnostics.TraceSource>
- <xref:System.Diagnostics.Debug>
- [Esquema do arquivo de configuração](../index.md)
