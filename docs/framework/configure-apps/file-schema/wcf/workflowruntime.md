---
title: <workflowRuntime>
ms.date: 03/30/2017
ms.assetid: 304c70fa-78d1-4d0f-b89f-0ca23d734c6f
ms.openlocfilehash: d12656b77fa219080382603fd04a542d2fa9064a
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70399083"
---
# \<workflowRuntime>
Especifica as configurações para uma instância do <xref:System.Workflow.Runtime.WorkflowRuntime> para hospedar serviços de Windows Communication Foundation baseado em fluxo de trabalho (WCF).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<workflowRuntime>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<workflowRuntime cachedInstanceExpiration="TimeSpan"
                 enablePerformanceCounters="Boolean"
                 name="String"
                 validateOnCreate="Boolean">
  <commonParameters>
    <add name="String"
         value="String" />
  </commonParameters>
  <services>
    <add type="String" />
  </services>
</workflowRuntime>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|cachedInstanceExpiration|Um <xref:System.TimeSpan> valor opcional que especifica a duração máxima em que uma instância de fluxo de trabalho pode permanecer na memória no estado ocioso antes de ser descarregada ou anulada de modo forçado. Se o WorkflowRuntime tiver `PersistenceService` que executar UnloadOnIdle, esse atributo será ignorado.|  
|enablePerformanceCounters|Um valor booliano opcional que especifica se os contadores de desempenho estão habilitados. Os contadores de desempenho fornecem informações sobre várias estatísticas relacionadas ao fluxo de trabalho, mas causam uma penalidade de desempenho quando o mecanismo de tempo de execução do fluxo de trabalho é iniciado e quando as instâncias de fluxo de trabalho estão em execução. O valor padrão é `true`.|  
|name|Uma cadeia de caracteres que contém o nome do mecanismo de tempo de execução do fluxo de trabalho. O nome é usado na saída para distinguir esse tempo de execução de outros tempos de execução que podem estar em execução no sistema, por exemplo, em contadores de desempenho.<br /><br /> O padrão é uma cadeia de caracteres vazia.|  
|validateOnCreate|Um valor booliano opcional que especifica se a validação da definição de fluxo de trabalho ocorrerá quando o WorkflowServiceHost for aberto.  Quando esse atributo é definido como `true` , a validação do fluxo de trabalho é executada toda vez que `WorkflowServiceHost.Open` é chamada. Se forem encontrados erros de validação, um <xref:System.Workflow.ComponentModel.Compiler.WorkflowValidationFailedException> erro será gerado.<br /><br /> Quando essa propriedade é definida como `false` , nenhuma validação de definição de fluxo de trabalho ocorrerá.<br /><br /> O valor padrão para essa propriedade é `true`.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|commonParameters|Uma coleção de parâmetros comuns usados por serviços. Normalmente, essa coleção inclui a cadeia de conexão do banco de dados que pode ser compartilhada por serviços duráveis.|  
|services|Uma coleção de serviços que serão adicionados ao <xref:System.Workflow.Runtime.WorkflowRuntime> mecanismo. Os elementos são do tipo <xref:System.Workflow.Runtime.Configuration.WorkflowRuntimeServiceElement> .  Os serviços especificados na coleção serão inicializados pelo mecanismo de tempo de execução do fluxo de trabalho e adicionados aos seus serviços quando o <xref:System.Workflow.Runtime.WorkflowRuntime> construtor apropriado for chamado. Portanto, os serviços especificados na coleção devem seguir determinadas regras sobre as assinaturas de seus construtores. Consulte <xref:System.Workflow.Runtime.Configuration.WorkflowRuntimeServiceElement> para obter mais informações.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<behavior>](behavior-of-endpointbehaviors.md)|Especifica um elemento de comportamento.|  
  
## <a name="remarks"></a>Comentários  
 Para obter mais informações sobre como usar um arquivo de configuração para controlar o comportamento de um <xref:System.Workflow.Runtime.WorkflowRuntime> objeto de um aplicativo host Windows Workflow Foundation, consulte [arquivos de configuração de fluxo de trabalho](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms732240(v=vs.90)).  
  
## <a name="example"></a>Exemplo  
  
```xml  
<serviceBehaviors>
   <behavior name="ServiceBehavior">
      <workflowRuntime name="WorkflowServiceHostRuntime"
                       validateOnCreate="true"
                       enablePerformanceCounters="true">
         <commonParameters>
            <add name="ConnectionString" value="Initial Catalog=WorkflowStore;Data Source=localhost;Integrated Security=SSPI;" />
            <add name="EnableRetries" value="True" />
         </commonParameters>
         <services>
             <add type="NetFx.Checkin.Scenario.WorkflowServices.WorkflowBasedServices.Common.TestPersistenceService.FilePersistenceService, NetFx.Checkin.Scenario.WorkflowServices.WorkflowBasedServices.Common"/>
         </services>
      </workflowRuntime>
   </behavior>
</serviceBehaviors>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.WorkflowRuntimeElement>
- <xref:System.Workflow.Runtime.Configuration.WorkflowRuntimeServiceElement>
- <xref:System.Workflow.Runtime.WorkflowRuntime>
- [Arquivos de configuração de fluxo de trabalho](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms732240(v=vs.90))
