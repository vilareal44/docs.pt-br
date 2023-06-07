---
title: <clear>Elemento para <listeners> para<trace>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/trace/listeners/clear
helpviewer_keywords:
- clear element for <listeners> for <trace>
- <clear> element for <listeners> for <trace>
ms.assetid: b44732a8-271f-4a06-ba9e-fe3298d6f192
ms.openlocfilehash: 905dad8274fede80f4809ff3c7a014049f9df450
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79153536"
---
# <a name="clear-element-for-listeners-for-trace"></a>\<clear>Elemento para \<listeners> para\<trace>
Limpa a coleção `Listeners` do rastreamento.  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.diagnostics>**](system-diagnostics-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<trace>**](trace-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<listeners>**](listeners-element-for-trace.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<clear>**

## <a name="syntax"></a>Sintaxe  
  
```xml  
<clear/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum.  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`system.diagnostics`|Especifica os ouvintes de rastreamento que coletam, armazenam e roteiam mensagens e o nível em que uma opção de rastreamento é definida.|  
|`trace`|Contém os ouvintes que coletam, armazenam e roteiam mensagens de rastreamento.|  
|`listeners`|Contém ouvintes que coletam, armazenam e roteiam mensagens. Os ouvintes direcionam a saída de rastreamento para um destino apropriado.|  
  
## <a name="remarks"></a>Comentários  
 O `<clear>` elemento remove todos os ouvintes da `Listeners` coleção para rastreamento. Você pode usar o `<clear>` elemento antes de usar o `<add>` elemento para ter certeza de que não há outros ouvintes ativos na coleção.  
  
 Você pode limpar a `Listeners` coleção programaticamente chamando o <xref:System.Diagnostics.TraceListenerCollection.Clear%2A> método na <xref:System.Diagnostics.Trace.Listeners%2A?displayProperty=nameWithType> Propriedade ( `System.Diagnostics.Trace.Listeners.Clear()` ).  
  
 Esse elemento pode ser usado no arquivo de configuração da máquina (Machine. config) e no arquivo de configuração do aplicativo.  
  
> [!NOTE]
> O `<clear>` elemento remove o <xref:System.Diagnostics.DefaultTraceListener> da `Listeners` coleção, alterando o comportamento dos <xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType> <xref:System.Diagnostics.Trace.Assert%2A?displayProperty=nameWithType> métodos,, <xref:System.Diagnostics.Debug.Fail%2A?displayProperty=nameWithType> e <xref:System.Diagnostics.Trace.Fail%2A?displayProperty=nameWithType> . Chamar um `Assert` `Fail` método ou normalmente resulta na exibição de uma caixa de mensagem. No entanto, a caixa de mensagem não será exibida se o <xref:System.Diagnostics.DefaultTraceListener> não estiver na `Listeners` coleção.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como usar o `<clear>` elemento antes de usar o `<add>` elemento para adicionar o ouvinte `console` à `Listeners` coleção para rastreamento.  
  
```xml  
<configuration>  
  <system.diagnostics>  
    <trace autoflush="false" indentsize="4">  
      <listeners>  
        <clear/>  
        <add name="console"
          type="System.Diagnostics.ConsoleTraceListener" >  
          <filter type="System.Diagnostics.EventTypeFilter"
            initializeData="Error" />  
        </add>  
      </listeners>  
    </trace>  
  </system.diagnostics>  
</configuration>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Diagnostics.Trace.Listeners%2A>
- <xref:System.Diagnostics.Trace>
- <xref:System.Diagnostics.Debug>
- <xref:System.Diagnostics.TraceSource>
- [Esquema de configurações de rastreamento e depuração](index.md)
- [\<remove>](remove-element-for-listeners-for-trace.md)
- [Ouvintes de rastreamento](../../../debug-trace-profile/trace-listeners.md)
