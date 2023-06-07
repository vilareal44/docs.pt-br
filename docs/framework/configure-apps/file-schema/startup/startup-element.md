---
title: Elemento <startup>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/startup
- http://schemas.microsoft.com/.NetConfiguration/v2.0#startup
helpviewer_keywords:
- container tags, <startup> element
- <startup> element
- startup element
ms.assetid: 536acfd8-f827-452f-838a-e14fa3b87621
ms.openlocfilehash: e936c069275bfa9f7ac81ef1c6fc6228828182a8
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79153719"
---
# <a name="startup-element"></a>Elemento \<startup>

Especifica Common Language Runtime informações de inicialização.

[**\<configuration>**](../configuration-element.md)  
&nbsp;&nbsp;**\<startup>**  

## <a name="syntax"></a>Sintaxe

```xml
<startup useLegacyV2RuntimeActivationPolicy="true|false" >
</startup>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

 As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|`useLegacyV2RuntimeActivationPolicy`|Atributo opcional.<br /><br /> Especifica se é para habilitar a política de ativação de tempo de execução .NET Framework 2,0 ou usar a política de ativação .NET Framework 4.|

## <a name="uselegacyv2runtimeactivationpolicy-attribute"></a>atributo useLegacyV2RuntimeActivationPolicy

|Valor|Descrição|
|-----------|-----------------|
|`true`|Habilite a política de ativação de tempo de execução .NET Framework 2,0 para o tempo de execução escolhido, que é associar técnicas herdadas de ativação de tempo de execução (como a [função CorBindToRuntimeEx](../../../unmanaged-api/hosting/corbindtoruntimeex-function.md)) ao tempo de execução escolhido do arquivo de configuração em vez de encappá-las na versão 2,0 do CLR. Portanto, se o CLR versão 4 ou posterior for escolhido no arquivo de configuração, os assemblies de modo misto criados com versões anteriores do .NET Framework serão carregados com a versão do CLR escolhida. Definir esse valor impede que o CLR versão 1,1 ou a versão 2,0 do CLR seja carregado no mesmo processo, desabilitando efetivamente o recurso lado a lado no processo.|
|`false`|Use a política de ativação padrão para o .NET Framework 4 e posterior, que é para permitir que técnicas herdadas de ativação de tempo de execução carreguem o CLR versão 1,1 ou 2,0 no processo. Definir esse valor impede que assemblies de modo misto sejam carregados no .NET Framework 4 ou posterior, a menos que tenham sido criados com o .NET Framework 4 ou posterior. Esse valor é o padrão.|

### <a name="child-elements"></a>Elementos filho

|Elemento|Descrição|
|-------------|-----------------|
|[\<requiredRuntime>](requiredruntime-element.md)|Especifica que o aplicativo dá suporte apenas à versão 1.0 do Common Language Runtime. Os aplicativos criados com o tempo de execução versão 1,1 ou posterior devem usar o **\<supportedRuntime>** elemento.|
|[\<supportedRuntime>](supportedruntime-element.md)|Especifica a quais versões do Common Language Runtime o aplicativo oferece suporte.|

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|

## <a name="remarks"></a>Comentários

 O **\<supportedRuntime>** elemento deve ser usado por todos os aplicativos criados usando a versão 1,1 ou posterior do tempo de execução. Os aplicativos criados para dar suporte apenas à versão 1,0 do tempo de execução devem usar o **\<requiredRuntime>** elemento.

 O código de inicialização de um aplicativo hospedado no Microsoft Internet Explorer ignora o **\<startup>** elemento e seus elementos filho.

## <a name="the-uselegacyv2runtimeactivationpolicy-attribute"></a>O atributo useLegacyV2RuntimeActivationPolicy

 Esse atributo será útil se seu aplicativo usar caminhos de ativação herdados, como a [função CorBindToRuntimeEx](../../../unmanaged-api/hosting/corbindtoruntimeex-function.md), e você quiser que esses caminhos ativem a versão 4 do CLR em vez de uma versão anterior, ou se seu aplicativo for criado com o .NET Framework 4, mas tiver uma dependência em um assembly de modo misto criado com uma versão anterior do .NET Framework. Nesses cenários, defina o atributo como `true` .

> [!NOTE]
> Definir o atributo para `true` impedir que o CLR versão 1,1 ou a versão 2,0 do CLR seja carregado no mesmo processo, desabilitando efetivamente o recurso lado a lado no processo (Confira [execução lado a lado para interoperabilidade com](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/8t8td04t(v=vs.100))).

## <a name="example"></a>Exemplo

 O exemplo a seguir mostra como especificar a versão de tempo de execução em um arquivo de configuração.

```xml
<!-- When used with version 1.0 of the .NET Framework runtime -->
<configuration>
   <startup>
      <requiredRuntime version="v1.0.3705" safemode="true"/>
   </startup>
</configuration>
<!-- When used with version 1.1 (or later) of the runtime -->
<configuration>
   <startup>
      <supportedRuntime version="v1.1.4322"/>
      <supportedRuntime version="v1.0.3705"/>
   </startup>
</configuration>
```

## <a name="see-also"></a>Confira também

- [Esquema de configurações de inicialização](index.md)
- [Esquema de arquivos de configuração](../index.md)
- [Como configurar um aplicativo para dar suporte a .NET Framework 4 ou versões posteriores](../../../migration-guide/how-to-configure-an-app-to-support-net-framework-4-or-4-5.md)
- [Execução lado a lado para interoperabilidade COM](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/8t8td04t(v=vs.100))
- [Execução lado a lado em processo](../../../deployment/in-process-side-by-side-execution.md)
