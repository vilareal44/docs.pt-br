---
title: <add>Elemento para <listeners> para<source>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/sources/source/listeners/add
helpviewer_keywords:
- initializeData attribute
- add element for <listeners> for <source>
- <add> element for <listeners> for <source>
ms.assetid: 4ce36ac1-81ef-48e8-b8b2-b5a5b0e2adcb
ms.openlocfilehash: 883eef32172c5a7f900197995b4c57c3d5a84e19
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79153679"
---
# <a name="add-element-for-listeners-for-source"></a>\<add>Elemento para \<listeners> para\<source>
Adiciona um ouvinte na coleção `Listeners` de uma origem de rastreamento.  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.diagnostics>**](system-diagnostics-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<sources>**](sources-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<source>**](source-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<listeners>**](listeners-element-for-source.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**

## <a name="syntax"></a>Sintaxe  
  
```xml  
<add name="name"
  type="TraceListenerClassName, Version, Culture, PublicKeyToken"  
  initializeData="data"/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`type`|Atributo obrigatório, a menos que você esteja fazendo referência a um ouvinte na `sharedListeners` coleção; nesse caso, você só precisa fazer referência a ele por nome (consulte o [exemplo](#example)).<br /><br /> Especifica o tipo do ouvinte. Você deve usar uma cadeia de caracteres que atenda aos requisitos especificados na [especificação de nomes de tipo totalmente qualificados](../../../reflection-and-codedom/specifying-fully-qualified-type-names.md).|  
|`initializeData`|Atributo opcional.<br /><br /> A cadeia de caracteres passada para o construtor para a classe especificada. Um <xref:System.Configuration.ConfigurationException> será gerado se a classe não tiver um construtor que usa uma cadeia de caracteres.|  
|`name`|Atributo opcional.<br /><br /> Especifica o nome do ouvinte.|  
|`traceOutputOptions`|Atributo opcional.<br /><br /> Especifica o <xref:System.Diagnostics.TraceListener.TraceOutputOptions%2A> valor da propriedade para o ouvinte de rastreamento.|  
|[atributos personalizados]|Atributos opcionais.<br /><br /> Especifica o valor para atributos específicos do ouvinte identificados pelo <xref:System.Diagnostics.TraceListener.GetSupportedAttributes%2A> método para esse ouvinte. <xref:System.Diagnostics.DelimitedListTraceListener.Delimiter%2A>é um exemplo de um atributo extra exclusivo para a <xref:System.Diagnostics.DelimitedListTraceListener> classe.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<filter>](filter-element-for-add-for-listeners-for-source.md)|Adiciona um filtro a um ouvinte na coleção `Listeners` de uma origem de rastreamento.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`system.diagnostics`|Especifica os ouvintes de rastreamento que coletam, armazenam e roteiam mensagens e o nível em que uma opção de rastreamento é definida.|  
|`sources`|Contém as origens de rastreamento que iniciam as mensagens de rastreamento.|  
|`source`|Especifica uma origem de rastreamento que inicia as mensagens de rastreamento.|  
|`listeners`|Especifica os ouvintes que coletam, armazenam e roteiam mensagens.|  
  
## <a name="remarks"></a>Comentários  
 As classes de ouvinte fornecidas com o .NET Framework derivam da <xref:System.Diagnostics.TraceListener> classe.  
  
 Se você não especificar o `name` atributo do ouvinte de rastreamento, a <xref:System.Diagnostics.TraceListener.Name%2A> Propriedade do ouvinte de rastreamento será padronizada como uma cadeia de caracteres vazia (""). Se seu aplicativo tiver apenas um ouvinte, você poderá adicioná-lo sem especificar um nome, e poderá removê-lo especificando uma cadeia de caracteres vazia para o nome. No entanto, se seu aplicativo tiver mais de um ouvinte, você deverá especificar um nome exclusivo para cada ouvinte de rastreamento, que permite identificar e gerenciar ouvintes de rastreamento individuais na <xref:System.Diagnostics.TraceSource.Listeners%2A?displayProperty=nameWithType> coleção.  
  
> [!NOTE]
> Adicionar mais de um ouvinte de rastreamento do mesmo tipo e com o mesmo nome resulta em apenas um ouvinte de rastreamento desse tipo e nome que estão sendo adicionados à `Listeners` coleção. No entanto, você pode adicionar programaticamente vários ouvintes idênticos à `Listeners` coleção.  
  
 O valor para o `initializeData` atributo depende do tipo de ouvinte que você criar. Nem todos os ouvintes de rastreamento exigem que você especifique `initializeData` .  
  
> [!NOTE]
> Ao usar o `initializeData` atributo, você pode obter o aviso do compilador "o atributo ' initializeData ' não está declarado." Esse aviso ocorre porque as definições de configuração são validadas em relação à classe base abstrata <xref:System.Diagnostics.TraceListener> , que não reconhece o `initializeData` atributo. Normalmente, você pode ignorar esse aviso para implementações de ouvinte de rastreamento que têm um construtor que usa um parâmetro.  
  
 A tabela a seguir mostra os ouvintes de rastreamento que estão incluídos com o .NET Framework e descreve o valor de seus `initializeData` atributos.  
  
|Classe de ouvinte de rastreamento|valor do atributo initializeData|  
|--------------------------|------------------------------------|  
|<xref:System.Diagnostics.ConsoleTraceListener?displayProperty=nameWithType>|O `useErrorStream` valor do <xref:System.Diagnostics.ConsoleTraceListener.%23ctor%2A> Construtor.  Defina o `initializeData` atributo como " `true` " para gravar rastreamento e depurar saída para o fluxo de erro padrão; defina-o como " `false` " para gravar no fluxo de saída padrão.|  
|<xref:System.Diagnostics.DelimitedListTraceListener?displayProperty=nameWithType>|O nome do arquivo no qual o <xref:System.Diagnostics.DelimitedListTraceListener> é gravado.|  
|<xref:System.Diagnostics.EventLogTraceListener?displayProperty=nameWithType>|O nome da origem de um log de eventos existente.|  
|<xref:System.Diagnostics.EventSchemaTraceListener?displayProperty=nameWithType>|O nome do arquivo no qual as <xref:System.Diagnostics.EventSchemaTraceListener> gravações são gravadas.|  
|<xref:System.Diagnostics.TextWriterTraceListener?displayProperty=nameWithType>|O nome do arquivo no qual as <xref:System.Diagnostics.TextWriterTraceListener> gravações são gravadas.|  
|<xref:System.Diagnostics.XmlWriterTraceListener?displayProperty=nameWithType>|O nome do arquivo no qual as <xref:System.Diagnostics.XmlWriterTraceListener> gravações são gravadas.|  
  
## <a name="configuration-file"></a>Arquivo de configuração  
 Esse elemento pode ser usado no arquivo de configuração da máquina (Machine. config) e no arquivo de configuração do aplicativo.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como usar `<add>` elementos para adicionar os ouvintes `console` e `textListener` à `Listeners` coleção para a origem do rastreamento `TraceSourceApp` . O `textListener` ouvinte grava a saída de rastreamento no arquivo myListener. log.  
  
```xml  
<configuration>  
  <system.diagnostics>  
    <sources>  
      <source name="TraceSourceApp" switchName="sourceSwitch"
        switchType="System.Diagnostics.SourceSwitch">  
        <listeners>  
          <add name="console"
            type="System.Diagnostics.ConsoleTraceListener"/>  
          <add name="textListener"/>  
          <remove name="Default"/>  
        </listeners>  
      </source>  
    </sources>  
    <sharedListeners>  
      <add name="textListener"
        type="System.Diagnostics.TextWriterTraceListener"
        initializeData="myListener.log"/>  
    </sharedListeners>  
    <switches>  
      <add name="sourceSwitch" value="Warning"/>  
    </switches>  
  </system.diagnostics>  
</configuration>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Diagnostics.TraceSource>
- <xref:System.Diagnostics.TraceListener>
- [Esquema de configurações de rastreamento e depuração](index.md)
- [Ouvintes de rastreamento](../../../debug-trace-profile/trace-listeners.md)
