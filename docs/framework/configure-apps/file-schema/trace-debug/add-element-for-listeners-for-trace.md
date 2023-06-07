---
title: <add>Elemento para <listeners> para<trace>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/trace/listeners/add
helpviewer_keywords:
- initializeData attribute
- <add> element for <listeners>
- add element for <listeners>
ms.assetid: 81e804a3-ef11-4d39-bbde-bfa012c179e2
ms.openlocfilehash: c64673908dc9afe67d97c08f93e5b9d9533bd34d
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79153666"
---
# <a name="add-element-for-listeners-for-trace"></a>\<add>Elemento para \<listeners> para\<trace>
Adiciona um ouvinte à coleção de **ouvintes** .  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.diagnostics>**](system-diagnostics-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<trace>**](trace-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<listeners>**](listeners-element-for-trace.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**

## <a name="syntax"></a>Sintaxe  
  
```xml  
<add name="name"
     type="trace listener class name, Version, Culture, PublicKeyToken"  
     initializeData="data"/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|**tipo**|Atributo obrigatório.<br /><br /> Especifica o tipo do ouvinte. Você deve usar uma cadeia de caracteres que atenda aos requisitos especificados na [especificação de nomes de tipo totalmente qualificados](../../../reflection-and-codedom/specifying-fully-qualified-type-names.md).|  
|**initializeData**|Atributo opcional.<br /><br /> A cadeia de caracteres passada para o construtor para a classe especificada.|  
|**name**|Atributo opcional.<br /><br /> Especifica o nome do ouvinte.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<filter>](filter-element-for-add-for-listeners-for-trace.md)|Adiciona um filtro a um ouvinte na `Listeners` coleção de um rastreamento.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`listeners`|Especifica um ouvinte que coleta, armazena e roteia mensagens. Os ouvintes direcionam a saída de rastreamento para um destino apropriado.|  
|`system.diagnostics`|Especifica o elemento raiz para a seção de configuração ASP.NET.|  
|`trace`|Contém os ouvintes que coletam, armazenam e roteiam mensagens de rastreamento.|  
  
## <a name="remarks"></a>Comentários  
 As <xref:System.Diagnostics.Debug> <xref:System.Diagnostics.Trace> classes e compartilham a mesma coleção de **ouvintes** . Se você adicionar um objeto de ouvinte à coleção em uma dessas classes, a outra classe usará o mesmo ouvinte. As classes de ouvinte derivam de <xref:System.Diagnostics.TraceListener> .  
  
 Se você não especificar o `name` atributo do ouvinte de rastreamento, o <xref:System.Diagnostics.TraceListener.Name%2A> do ouvinte de rastreamento assume como padrão uma cadeia de caracteres vazia (""). Se seu aplicativo tiver apenas um ouvinte, você poderá adicioná-lo sem especificar um nome e removê-lo especificando uma cadeia de caracteres vazia para o nome. No entanto, se seu aplicativo tiver mais de um ouvinte, você deverá especificar nomes exclusivos para cada ouvinte de rastreamento, o que permite identificar e gerenciar ouvintes de rastreamento individuais nas <xref:System.Diagnostics.Debug.Listeners%2A> <xref:System.Diagnostics.Trace.Listeners%2A> coleções e.  
  
> [!NOTE]
> Adicionar mais de um ouvinte de rastreamento do mesmo tipo e com o mesmo nome resulta em apenas um ouvinte de rastreamento desse tipo e nome que estão sendo adicionados à `Listeners` coleção. No entanto, você pode adicionar programaticamente vários ouvintes idênticos à `Listeners` coleção.  
  
 O valor do atributo **initializeData** depende do tipo de ouvinte que você criar. Nem todos os ouvintes de rastreamento exigem que você especifique **initializeData**.  
  
> [!NOTE]
> Ao usar o `initializeData` atributo, você pode obter o aviso do compilador "o atributo ' initializeData ' não está declarado." Esse aviso ocorre porque as definições de configuração são validadas em relação à classe base abstrata <xref:System.Diagnostics.TraceListener> , que não reconhece o `initializeData` atributo. Normalmente, você pode ignorar esse aviso para implementações de ouvinte de rastreamento que têm um construtor que usa um parâmetro.  
  
 A tabela a seguir mostra os ouvintes de rastreamento que estão incluídos com o .NET Framework e descreve o valor de seus atributos **initializeData** .  
  
|Classe de ouvinte de rastreamento|valor do atributo initializeData|  
|--------------------------|------------------------------------|  
|<xref:System.Diagnostics.ConsoleTraceListener?displayProperty=nameWithType>|O `useErrorStream` valor do <xref:System.Diagnostics.ConsoleTraceListener.%23ctor%2A> Construtor.  Defina o `initializeData` atributo como " `true` " para gravar rastreamento e depurar saída para <xref:System.Console.Error%2A?displayProperty=nameWithType> ; " `false` " para gravar <xref:System.Console.Out%2A?displayProperty=nameWithType> .|  
|<xref:System.Diagnostics.DelimitedListTraceListener?displayProperty=nameWithType>|O nome do arquivo no qual o <xref:System.Diagnostics.DelimitedListTraceListener> é gravado.|  
|<xref:System.Diagnostics.EventLogTraceListener?displayProperty=nameWithType>|O nome do nome de uma origem de log de eventos existente.|  
|<xref:System.Diagnostics.EventSchemaTraceListener?displayProperty=nameWithType>|O nome do arquivo no qual as <xref:System.Diagnostics.EventSchemaTraceListener> gravações são gravadas.|  
|<xref:System.Diagnostics.TextWriterTraceListener?displayProperty=nameWithType>|O nome do arquivo no qual as <xref:System.Diagnostics.TextWriterTraceListener> gravações são gravadas.|  
|<xref:System.Diagnostics.XmlWriterTraceListener?displayProperty=nameWithType>|O nome do arquivo no qual as <xref:System.Diagnostics.XmlWriterTraceListener> gravações são gravadas.|  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como usar **\<add>** elementos para adicionar os ouvintes `MyListener` e a `MyEventListener` coleção de **ouvintes** . `MyListener`Cria um arquivo chamado `MyListener.log` e grava a saída no arquivo. `MyEventListener`Cria uma entrada no log de eventos.  
  
```xml  
<configuration>  
   <system.diagnostics>  
      <trace autoflush="true" indentsize="0">  
         <listeners>  
            <add name="myListener" type="System.Diagnostics.TextWriterTraceListener, system, version=1.0.3300.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" initializeData="c:\myListener.log" />  
            <add name="MyEventListener"  
                 type="System.Diagnostics.EventLogTraceListener, system, version=1.0.3300.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"                 initializeData="MyConfigEventLog"/>  
            <add name="configConsoleListener"  
                 type="System.Diagnostics.ConsoleTraceListener, system, version=1.0.3300.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"/>  
         </listeners>  
      </trace>  
   </system.diagnostics>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Diagnostics.Trace>
- <xref:System.Diagnostics.Debug>
- <xref:System.Diagnostics.EventLogTraceListener>
- <xref:System.Diagnostics.ConsoleTraceListener>
- <xref:System.Diagnostics.TextWriterTraceListener>
- [Esquema de configurações de rastreamento e depuração](index.md)
- [Ouvintes de rastreamento](../../../debug-trace-profile/trace-listeners.md)
