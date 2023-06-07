---
title: Esquema de configurações do runtime
ms.date: 03/30/2017
helpviewer_keywords:
- schema runtime settings
- configuration schema [.NET Framework], runtime settings
- runtime settings schema
ms.assetid: f04816ab-110d-4e28-9283-845d6d9a4a68
ms.openlocfilehash: d5af9f3299b48d431b43566c11610d745167b60b
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74431049"
---
# <a name="run-time-settings-schema"></a>Esquema de configurações do runtime

As configurações de tempo de execução são usadas pelo Common Language Runtime para configurar aplicativos direcionados ao .NET Framework.

## <a name="the-runtime-section-and-its-parent-and-child-elements"></a>A \<runtime> seção e seus elementos pai e filho

[\<configuration>](../configuration-element.md)\
&nbsp;&nbsp;[\<runtime>](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<alwaysFlowImpersonationPolicy>](alwaysflowimpersonationpolicy-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<AppContextSwitchOverrides>](appcontextswitchoverrides-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<appDomainManagerAssembly>](appdomainmanagerassembly-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<appDomainManagerType>](appdomainmanagertype-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<appDomainResourceMonitoring>](appdomainresourcemonitoring-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<assemblyBinding>](assemblybinding-element-for-runtime.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<dependentAssembly>](dependentassembly-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<assemblyIdentity>](assemblyidentity-element-for-runtime.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<bindingRedirect>](bindingredirect-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<codeBase>](codebase-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<publisherPolicy>](publisherpolicy-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<probing>](probing-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<qualifyAssembly>](qualifyassembly-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<supportPortability>](supportportability-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<bypassTrustedAppStrongNames>](bypasstrustedappstrongnames-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<CompatSortNLSVersion>](compatsortnlsversion-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<developmentMode>](developmentmode-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<disableCachingBindingFailures>](disablecachingbindingfailures-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<disableCommitThreadStack>](disablecommitthreadstack-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<disableFusionUpdatesFromADManager>](disablefusionupdatesfromadmanager-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<EnableAmPmParseAdjustment>](enableampmparseadjustment-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<enforceFIPSPolicy>](enforcefipspolicy-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<etwEnable>](etwenable-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<forcePerformanceCounterUniqueSharedMemoryReads>](forceperformancecounteruniquesharedmemoryreads-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<gcAllowVeryLargeObjects>](gcconcurrent-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<gcConcurrent>](gcconcurrent-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<GCCpuGroup>](gccpugroup-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<GCHeapAffinitizeMask>](gcheapaffinitizemask-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<GCHeapCount>](gcheapcount-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<GCLOHThreshold>](gclohthreshold-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<GCNoAffinitize>](gcnoaffinitize-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<gcServer>](gcserver-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<generatePublisherEvidence>](generatepublisherevidence-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<legacyCorruptedStateExceptionsPolicy>](legacycorruptedstateexceptionspolicy-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<legacyImpersonationPolicy>](legacyimpersonationpolicy-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<loadfromRemoteSources>](loadfromremotesources-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<NetFx40_LegacySecurityPolicy>](netfx40-legacysecuritypolicy-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<NetFx40_PInvokeStackResilience>](netfx40-pinvokestackresilience-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<NetFx45_CultureAwareComparerGetHashCode_LongStrings>](netfx45-cultureawarecomparergethashcode-longstrings-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<PreferComInsteadOfManagedRemoting>](prefercominsteadofmanagedremoting-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<relativeBindForResources>](relativebindforresources-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<shadowCopyVerifyByTimeStamp>](shadowcopyverifybytimestamp-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<Thread_UseAllCpuGroups>](thread-useallcpugroups-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<ThrowUnobservedTaskExceptions>](throwunobservedtaskexceptions-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<TimeSpan_LegacyFormatMode>](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<useLegacyJit>](uselegacyjit-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<UseRandomizedStringHashAlgorithm>](userandomizedstringhashalgorithm-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<UseSmallInternalThreadStacks>](usesmallinternalthreadstacks-element.md)\
&nbsp;&nbsp;[\<system.runtime.caching>](system-runtime-caching-element-cache-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<memoryCache>](memorycache-element-cache-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<namedCaches>](namedcaches-element-cache-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<add>](add-element-for-namedcaches.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<clear>](clear-element-for-namedcaches.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<remove>](remove-element-for-namedcaches.md)

## <a name="alphabetical-list-of-runtime-elements"></a>Lista alfabética de \<runtime> elementos

|Elemento|Descrição|
|-------------|-----------------|
|[\<add>](add-element-for-namedcaches.md)|Adiciona um cache nomeado à coleção de `namedCaches` para um cache de memória.|
|[\<alwaysFlowImpersonationPolicy>](alwaysflowimpersonationpolicy-element.md)|Especifica que a identidade do Windows sempre fluirá por pontos assíncronos, independentemente de como a representação tenha sido executada.|
|[\<AppContextSwitchOverrides>](appcontextswitchoverrides-element.md)|Define uma ou mais opções usadas pela classe <xref:System.AppContext> para fornecer um mecanismo de recusa de uma nova funcionalidade.|
|[\<appDomainManagerAssembly>](appdomainmanagerassembly-element.md)|Especifica o assembly que fornece o gerenciador do domínio do aplicativo para o domínio do aplicativo padrão no processo.|
|[\<appDomainManagerType>](appdomainmanagertype-element.md)|Especifica o tipo que serve como o gerenciador de domínio do aplicativo para o domínio do aplicativo padrão.|
|[\<appDomainResourceMonitoring>](appdomainresourcemonitoring-element.md)|Instrui o runtime a coletar estatísticas sobre todos os domínios de aplicativos no processo durante toda a vida do processo.|
|[\<assemblyBinding>](assemblybinding-element-for-runtime.md)|Contém informações sobre o redirecionamento de versão e os locais dos assemblies.|
|[\<assemblyIdentity>](assemblyidentity-element-for-runtime.md)|Contém informações de identificação sobre um assembly.|
|[\<bindingRedirect>](bindingredirect-element.md)|Redireciona uma versão do assembly para outra.|
|[\<bypassTrustedAppStrongNames>](bypasstrustedappstrongnames-element.md)|Especifica se uma verificação de nome forte para assemblies confiáveis deve ser ignorada.|
|[\<clear>](clear-element-for-namedcaches.md)|Limpa a coleção `namedCaches` de um cache de memória.|
|[\<codeBase>](codebase-element.md)|Especifica onde o runtime pode encontrar um assembly.|
|[\<CompatSortNLSVersion>](compatsortnlsversion-element.md)|Especifica que o runtime deve usar um comportamento de classificação herdado ao executar comparações de cadeias de caracteres|
|[\<dependentAssembly>](dependentassembly-element.md)|Encapsula local do assembly e política de associação para cada assembly.|
|[\<developmentMode>](developmentmode-element.md)|Especifica se o runtime pesquisa por assemblies em diretórios especificados pela variável de ambiente DEVPATH.|
|[\<disableCachingBindingFailures>](disablecachingbindingfailures-element.md)|Especifica se o cache de falhas de associação, que é o comportamento padrão no .NET Framework 2,0, está desabilitado.|
|[\<disableCommitThreadStack>](disablecommitthreadstack-element.md)|Especifica se a pilha completa de threads está confirmada quando um thread é iniciado.|
|[\<disableFusionUpdatesFromADManager>](disablefusionupdatesfromadmanager-element.md)|Especifica se o comportamento padrão, que é permitir que o host de runtime substitua as definições de configuração de um domínio de aplicativo, está desabilitado.|
|[\<EnableAmPmParseAdjustment>](enableampmparseadjustment-element.md)|Determina se os métodos de análise de data e hora usam um conjunto de regras ajustado para analisar sequências de datas que contêm somente um dia, mês, hora e designador AM/PM.|
|[\<enforceFIPSPolicy>](enforcefipspolicy-element.md)|Especifica se deve-se impor um requisito de configuração do computador em que os algoritmos de criptografia devem estar em conformidade com o FIPS (padrão norte-americano de processamento de informações).|
|[\<etwEnable>](etwenable-element.md)|Especifica se deseja-se habilitar o rastreamento de eventos para Windows (ETW) para eventos de Common Language Runtime.|
|[\<forcePerformanceCounterUniqueSharedMemoryReads>](forceperformancecounteruniquesharedmemoryreads-element.md)|Especifica se o PerfCounter.dll usa a configuração de registro CategoryOptions em um aplicativo do .NET Framework versão 1.1 para determinar se é preciso carregar dados do contador de desempenho da memória global ou da memória compartilhada especifica da categoria.|
|[\<gcAllowVeryLargeObjects>](gcallowverylargeobjects-element.md)|Em plataformas de 64 bits, habilita matrizes com mais de 2 gigabytes (GB) de tamanho total.|
|[\<gcConcurrent>](gcconcurrent-element.md)|Especifica se o runtime executa a coleta de lixo simultaneamente.|
|[\<GCCpuGroup>](gccpugroup-element.md)|Especifica se a coleta de lixo oferece suporte a vários grupos de CPU.|
|[\<GCHeapAffinitizeMask>](gcheapaffinitizemask-element.md)|Define a afinidade entre heaps de GC e processadores individuais.|
|[\<GCHeapCount>](gcheapcount-element.md)|Especifica o número de heaps/threads a serem usados para a coleta de lixo do servidor.  |
|[\<GCLOHThreshold>](gclohthreshold-element.md)|Especifica o tamanho do limite que faz com que os objetos entrem no LOH (heap de objeto grande).|
|[\<GCNoAffinitize>](gcnoaffinitize-element.md)|Especifica se os threads GC do servidor relacionar ou não devem ser usados com CPUs.|
|[\<gcServer>](gcserver-element.md)|Especifica se o Common Language Runtime executa a coleta de lixo do servidor.|
|[\<generatePublisherEvidence>](generatepublisherevidence-element.md)|Especifica se o runtime usa a política de editor de CAS (Segurança de Acesso do Código).|
|[\<legacyCorruptedStateExceptionsPolicy>](legacycorruptedstateexceptionspolicy-element.md)|Especifica se o runtime permite que o código gerenciado detecte violações de acesso e outras exceções de estado corrompido.|
|[\<legacyImpersonationPolicy>](legacyimpersonationpolicy-element.md)|Especifica que a identidade do Windows não flua entre pontos assíncronos, independentemente das configurações de fluxo para o contexto de execução no thread atual.|
|[\<loadfromRemoteSources>](loadfromremotesources-element.md)|Especifica se os assemblies de fontes remotas são carregados como confiança total.|
|[\<memoryCache>](memorycache-element-cache-settings.md)|Define um elemento usado para configurar um cache baseado na classe <xref:System.Runtime.Caching.MemoryCache>.|
|[\<namedCaches>](namedcaches-element-cache-settings.md)|Contém um conjunto de definições de configuração para a instância `namedCache`.|
|[\<NetFx40_LegacySecurityPolicy>](netfx40-legacysecuritypolicy-element.md)|Especifica se o runtime usa a política de CAS (Segurança de Acesso do Código) herdada.|
|[\<NetFx40_PInvokeStackResilience>](netfx40-pinvokestackresilience-element.md)|Especifica se o runtime corrige automaticamente declarações de invocação de plataforma incorretas em runtime, às custas de transições mais lentas entre o código gerenciado e não gerenciado.|
|[\<NetFx45_CultureAwareComparerGetHashCode_LongStrings>](netfx45-cultureawarecomparergethashcode-longstrings-element.md)|Especifica se o runtime usa uma quantidade fixa de memória para calcular códigos hash para o método <xref:System.StringComparer.GetHashCode%2A?displayProperty=nameWithType>.|
|[\<PreferComInsteadOfManagedRemoting>](prefercominsteadofmanagedremoting-element.md)|Especifica que o runtime usará a interoperabilidade COM em vez de comunicação remota entre limites de domínio de aplicativo.|
|[\<probing>](probing-element.md)|Especifica os subdiretórios que o runtime procura ao carregar assemblies.|
|[\<publisherPolicy>](publisherpolicy-element.md)|Especifica se o runtime aplica a política do editor.|
|[\<qualifyAssembly>](qualifyassembly-element.md)|Especifica o nome completo do assembly que deve ser carregado dinamicamente quando um nome parcial é usado.|
|[\<relativeBindForResources>](relativebindforresources-element.md)|Otimiza o teste para assemblies satélites.|
|[\<remove>](remove-element-for-namedcaches.md)|Remove uma entrada de cache nomeado da coleção de `namedCaches` para um cache de memória.|
|[\<runtime>](runtime-element.md)|Contém informações sobre a associação de assembly e o comportamento de coleta de lixo.|
|[\<shadowCopyTimeStampVerification>](shadowcopyverifybytimestamp-element.md)|Especifica se a cópia de sombra usa o comportamento de inicialização padrão introduzido no .NET Framework 4 ou reverte para o comportamento de inicialização de versões anteriores do .NET Framework.|
|[\<supportPortability>](supportportability-element.md)|Especifica que um aplicativo pode fazer referência ao mesmo assembly em duas implementações diferentes do .NET Framework, desabilitando o comportamento padrão que trata os assemblies como equivalentes para fins de portabilidade do aplicativo.|
|[\<system.runtime.caching>](system-runtime-caching-element-cache-settings.md)|Fornece informações de configuração para o cache de objeto na memória padrão.|
|[\<Thread_UseAllCpuGroups>](thread-useallcpugroups-element.md)|Especifica se o runtime distribui threads gerenciados entre todos os grupos de CPU.|
|[\<ThrowUnobservedTaskExceptions>](throwunobservedtaskexceptions-element.md)|Especifica se as exceções de tarefas sem tratamento devem encerrar um processo em execução.|
|[\<TimeSpan_LegacyFormatMode>](runtime-element.md)|Especifica se o runtime usa uma formatação herdada para valores de <xref:System.TimeSpan>.|
|[\<useLegacyJit>](uselegacyjit-element.md)|Determina se o Common Language Runtime usa o compilador JIT de 64 bits herdado para uma compilação just-in-time.|
|[\<UseRandomizedStringHashAlgorithm>](userandomizedstringhashalgorithm-element.md)|Especifica se o runtime calcula códigos hash para sequências com base no domínio do aplicativo.|
|[\<UseSmallInternalThreadStacks>](usesmallinternalthreadstacks-element.md)|As solicitações que o runtime usa para explicitar os tamanhos das pilhas ao criar certos threads usados internamente, em vez do tamanho de pilha padrão.|

## <a name="see-also"></a>Confira também

- [Esquema de arquivos de configuração](../index.md)
- [Para desabilitar a coleta de lixo simultânea](gcconcurrent-element.md#to-disable-background-garbage-collection)
- [Redirecionando versões de assembly](../../redirect-assembly-versions.md)
