---
title: Elemento <alwaysFlowImpersonationPolicy>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/alwaysFlowImpersonationPolicy
- http://schemas.microsoft.com/.NetConfiguration/v2.0#alwaysFlowImpersonationPolicy
helpviewer_keywords:
- alwaysFlowImpersonationPolicy element
- <alwaysFlowImpersonationPolicy> element
ms.assetid: ee622801-9e46-470b-85ab-88c4b1dd2ee1
ms.openlocfilehash: 7c8ac37932a528ff0f000cbaab49124dec51b88c
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79154477"
---
# <a name="alwaysflowimpersonationpolicy-element"></a>Elemento \<alwaysFlowImpersonationPolicy>
Especifica que a identidade do Windows sempre fluirá por pontos assíncronos, independentemente de como a representação tenha sido executada.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<alwaysFlowImpersonationPolicy>**\  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<alwaysFlowImpersonationPolicy
  enabled="true|false"/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`enabled`|Atributo obrigatório.<br /><br /> Indica se a identidade do Windows flui entre pontos assíncronos.|  
  
## <a name="enabled-attribute"></a>Atributo habilitado  
  
|Valor|Descrição|  
|-----------|-----------------|  
|`false`|A identidade do Windows não flui em pontos assíncronos, a menos que a representação seja executada por meio de métodos gerenciados, como <xref:System.Security.Principal.WindowsIdentity.Impersonate%2A> . Este é o padrão.|  
|`true`|A identidade do Windows sempre flui entre pontos assíncronos, independentemente de como a representação foi executada.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`runtime`|Contém informações sobre associação do assembly e coleta de lixo.|  
  
## <a name="remarks"></a>Comentários  
 No .NET Framework versões 1,0 e 1,1, a identidade do Windows não flui entre pontos assíncronos. No .NET Framework versão 2,0, há um <xref:System.Threading.ExecutionContext> objeto que contém informações sobre o thread em execução no momento e o flui entre pontos assíncronos dentro de um domínio de aplicativo. O <xref:System.Security.Principal.WindowsIdentity> também flui como parte das informações que fluem pelos pontos assíncronos, desde que a representação tenha sido obtida usando métodos gerenciados, como <xref:System.Security.Principal.WindowsIdentity.Impersonate%2A> e não por outros meios, como invocação de plataforma para métodos nativos. Esse elemento é usado para especificar que a identidade do Windows flua entre pontos assíncronos, independentemente de como a representação foi obtida.  
  
 Você pode alterar esse comportamento padrão de duas maneiras:  
  
1. Em código gerenciado por thread.  
  
     Você pode suprimir o fluxo por thread modificando as <xref:System.Threading.ExecutionContext> <xref:System.Security.SecurityContext> configurações e usando o <xref:System.Threading.ExecutionContext.SuppressFlow%2A?displayProperty=nameWithType> <xref:System.Security.SecurityContext.SuppressFlowWindowsIdentity%2A?displayProperty=nameWithType> método, ou <xref:System.Security.SecurityContext.SuppressFlow%2A?displayProperty=nameWithType> .  
  
2. Na chamada à interface de hospedagem não gerenciada para carregar o Common Language Runtime (CLR).  
  
     Se uma interface de hospedagem não gerenciada (em vez de um executável gerenciado simples) for usada para carregar o CLR, você poderá especificar um sinalizador especial na chamada para a função de [função CorBindToRuntimeEx](../../../unmanaged-api/hosting/corbindtoruntimeex-function.md) . Para habilitar o modo de compatibilidade para todo o processo, defina o `flags` parâmetro para a [função CorBindToRuntimeEx](../../../unmanaged-api/hosting/corbindtoruntimeex-function.md) como `STARTUP_ALWAYSFLOW_IMPERSONATION` .  
  
## <a name="configuration-file"></a>Arquivo de configuração  
 Em um aplicativo .NET Framework, esse elemento pode ser usado somente no arquivo de configuração do aplicativo.  
  
 Para um aplicativo ASP.NET, o fluxo de representação pode ser configurado no arquivo Aspnet. config encontrado no \<Windows Folder> diretório \Microsoft.NET\Framework\vx.x.xxxx  
  
 O ASP.NET, por padrão, desabilita o fluxo de representação no arquivo Aspnet. config usando as seguintes definições de configuração:  
  
```xml
<configuration>  
   <runtime>  
      <legacyImpersonationPolicy enabled="true"/>  
      <alwaysFlowImpersonationPolicy enabled="false"/>  
   </runtime>  
</configuration>  
```  
  
 Em ASP.NET, se você quiser permitir o fluxo de representação em vez disso, deverá usar explicitamente as seguintes definições de configuração:  
  
```xml  
<configuration>  
   <runtime>  
      <legacyImpersonationPolicy enabled="false"/>  
      <alwaysFlowImpersonationPolicy enabled="true"/>  
   </runtime>  
</configuration>  
```  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como especificar que a identidade do Windows flui entre pontos assíncronos, mesmo quando a representação é obtida por meio de meios diferentes dos métodos gerenciados.  
  
```xml  
<configuration>  
  <runtime>  
    <alwaysFlowImpersonationPolicy enabled="true"/>  
  </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- [Esquema de configurações do runtime](index.md)
- [Esquema de arquivos de configuração](../index.md)
- [\<legacyImpersonationPolicy>Elementos](legacyimpersonationpolicy-element.md)
