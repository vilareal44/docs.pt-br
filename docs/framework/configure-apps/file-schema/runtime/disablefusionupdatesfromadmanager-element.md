---
title: Elemento <disableFusionUpdatesFromADManager>
ms.date: 03/30/2017
helpviewer_keywords:
- disableFusionUpdatesFromADManager element
- <disableFusionUpdatesFromADManager> element
ms.assetid: 58d2866c-37bd-4ffa-abaf-ff35926a2939
ms.openlocfilehash: 4e7375fddaa98b45766b29d911d555f773edcafa
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73117451"
---
# <a name="disablefusionupdatesfromadmanager-element"></a>Elemento \<disableFusionUpdatesFromADManager>
Especifica se o comportamento padrão, que é permitir que o host de runtime substitua as definições de configuração de um domínio de aplicativo, está desabilitado.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<disableFusionUpdatesFromADManager>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<disableFusionUpdatesFromADManager enabled="0|1"/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|Habilitado|Atributo obrigatório.<br /><br /> Especifica se a capacidade padrão de substituir as configurações de fusão está desabilitada.|  
  
## <a name="enabled-attribute"></a>Atributo habilitado  
  
|Valor|Descrição|  
|-----------|-----------------|  
|0|Não desabilite a capacidade de substituir as configurações de fusão. Esse é o comportamento padrão, começando com o .NET Framework 4.|  
|1|Desabilite a capacidade de substituir as configurações de fusão. Isso reverte para o comportamento de versões anteriores do .NET Framework.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`runtime`|Contém informações sobre associação do assembly e coleta de lixo.|  
  
## <a name="remarks"></a>Comentários  
 Começando com o .NET Framework 4, o comportamento padrão é permitir que o <xref:System.AppDomainManager> objeto substitua as definições de configuração usando a <xref:System.AppDomainSetup.ConfigurationFile%2A> propriedade ou o <xref:System.AppDomainSetup.SetConfigurationBytes%2A> método do <xref:System.AppDomainSetup> objeto que é passado para sua implementação do <xref:System.AppDomainManager.InitializeNewDomain%2A?displayProperty=nameWithType> método, em sua subclasse de <xref:System.AppDomainManager> . Para o domínio de aplicativo padrão, as configurações alteradas substituem as configurações que foram especificadas pelo arquivo de configuração do aplicativo. Para outros domínios de aplicativo, eles substituem as definições de configuração que foram passadas para o <xref:System.AppDomainManager.CreateDomain%2A?displayProperty=nameWithType> <xref:System.AppDomain.CreateDomain%2A?displayProperty=nameWithType> método ou.  
  
 Você pode passar novas informações de configuração ou passar NULL ( `Nothing` em Visual Basic) para eliminar informações de configuração que foram passadas.  
  
 Não transmita informações de configuração para a <xref:System.AppDomainSetup.ConfigurationFile%2A> propriedade e o <xref:System.AppDomainSetup.SetConfigurationBytes%2A> método. Se você passar informações de configuração para ambos, as informações passadas para a <xref:System.AppDomainSetup.ConfigurationFile%2A> propriedade serão ignoradas, pois o <xref:System.AppDomainSetup.SetConfigurationBytes%2A> método substitui as informações de configuração do arquivo de configuração do aplicativo. Se você usar a <xref:System.AppDomainSetup.ConfigurationFile%2A> propriedade, poderá passar NULL ( `Nothing` em Visual Basic) para o <xref:System.AppDomainSetup.SetConfigurationBytes%2A> método para eliminar quaisquer bytes de configuração que foram especificados na chamada para o <xref:System.AppDomainManager.CreateDomain%2A?displayProperty=nameWithType> método ou <xref:System.AppDomain.CreateDomain%2A?displayProperty=nameWithType> .  
  
 Além das informações de configuração, você pode alterar as seguintes configurações no <xref:System.AppDomainSetup> objeto que é passado para a implementação do método:,,,,,,,,,, <xref:System.AppDomainManager.InitializeNewDomain%2A?displayProperty=nameWithType> <xref:System.AppDomainSetup.ApplicationBase%2A> <xref:System.AppDomainSetup.ApplicationName%2A> <xref:System.AppDomainSetup.CachePath%2A> <xref:System.AppDomainSetup.DisallowApplicationBaseProbing%2A> <xref:System.AppDomainSetup.DisallowBindingRedirects%2A> <xref:System.AppDomainSetup.DisallowCodeDownload%2A> <xref:System.AppDomainSetup.DisallowPublisherPolicy%2A> <xref:System.AppDomainSetup.DynamicBase%2A> <xref:System.AppDomainSetup.LoaderOptimization%2A> <xref:System.AppDomainSetup.PrivateBinPath%2A> <xref:System.AppDomainSetup.PrivateBinPathProbe%2A> , <xref:System.AppDomainSetup.ShadowCopyDirectories%2A> e <xref:System.AppDomainSetup.ShadowCopyFiles%2A> .  
  
 Como alternativa ao uso do `<disableFusionUpdatesFromADManager>` elemento, você pode desabilitar o comportamento padrão criando uma configuração do registro ou definindo uma variável de ambiente. No registro, crie um valor DWORD chamado `COMPLUS_disableFusionUpdatesFromADManager` em `HKCU\Software\Microsoft\.NETFramework` ou `HKLM\Software\Microsoft\.NETFramework` e defina o valor como 1. Na linha de comando, defina a variável de ambiente `COMPLUS_disableFusionUpdatesFromADManager` como 1.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como desabilitar a capacidade de substituir as configurações de fusão usando o `<disableFusionUpdatesFromADManager>` elemento.  
  
```xml  
<configuration>  
   <runtime>  
      <disableFusionUpdatesFromADManager enabled="1" />  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- [Esquema de configurações do runtime](index.md)
- [Esquema de arquivos de configuração](../index.md)
- [Como o runtime localiza assemblies](../../../deployment/how-the-runtime-locates-assemblies.md)
