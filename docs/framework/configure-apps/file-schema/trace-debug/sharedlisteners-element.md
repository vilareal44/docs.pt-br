---
title: Elemento <sharedListeners>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#sharedListeners
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/sharedListeners
helpviewer_keywords:
- <sharedListeners> element
- listeners
- shared listeners
- trace listeners, <sharedListeners> element
- sharedListeners element
ms.assetid: de200534-19dd-4156-86cf-c50521802c4c
ms.openlocfilehash: 69f15cc9583b397017ac30a0c567914495867c18
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79153315"
---
# <a name="sharedlisteners-element"></a>Elemento \<sharedListeners>
Contém os ouvintes que podem ser referenciados por qualquer elemento de origem ou de rastreamento.  Esses ouvintes não recebem nenhum rastreamento por padrão e não é possível recuperar esses ouvintes em tempo de execução. Os ouvintes identificados como ouvintes compartilhados podem ser adicionados a fontes ou rastreamentos por nome.  
  
[**\<configuration>**](../configuration-element.md)  
&nbsp;&nbsp;[**\<system.diagnostics>**](system-diagnostics-element.md)  
&nbsp;&nbsp;&nbsp;&nbsp;**\<sharedListeners>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<sharedListeners>
  <add>...</add>  
</sharedListeners>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum.  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<add>](add-element-for-listeners-for-trace.md)|Adiciona um ouvinte na coleção `sharedListeners`.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`Configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`system.diagnostics`|Especifica o elemento raiz para a seção de configuração ASP.NET.|  
  
## <a name="remarks"></a>Comentários  
 A adição de um ouvinte à coleção de ouvintes compartilhados não o torna um ouvinte ativo. Ele ainda deve ser adicionado a uma origem de rastreamento ou a um rastreamento adicionando-o à `Listeners` coleção para esse elemento Trace. As classes de ouvinte no .NET Framework derivam da <xref:System.Diagnostics.TraceListener> classe.  
  
 Esse elemento pode ser usado no arquivo de configuração da máquina (Machine. config) e no arquivo de configuração do aplicativo.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como usar o `<sharedListeners>` elemento para adicionar o ouvinte `console` à `Listeners` coleção para as <xref:System.Diagnostics.TraceSource> classes e <xref:System.Diagnostics.Trace> . O ouvinte de rastreamento do console grava informações de rastreamento no console por meio de chamadas para o <xref:System.Diagnostics.TraceSource> ou o <xref:System.Diagnostics.Trace> .  
  
```xml  
<configuration>  
  <system.diagnostics>  
    <sharedListeners>  
      <add name="console" type="System.Diagnostics.ConsoleTraceListener" >  
        <filter type="System.Diagnostics.EventTypeFilter"  
          initializeData="Warning" />  
      </add>  
    </sharedListeners>  
    <sources>  
      <source name="mySource" switchName="sourceSwitch"  >  
        <listeners>  
          <add name="console" />  
        </listeners>  
      </source>  
    </sources>  
    <switches>  
      <add name="sourceSwitch" value="Verbose"/>  
    </switches>  
    <trace>  
      <listeners>  
        <add name="console" />  
      </listeners>  
    </trace>  
  </system.diagnostics>  
</configuration>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Diagnostics.TraceListener>
- [Esquema de configurações de rastreamento e depuração](index.md)
- [Ouvintes de rastreamento](../../../debug-trace-profile/trace-listeners.md)
