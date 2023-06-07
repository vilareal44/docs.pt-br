---
title: Elemento <assemblyIdentity> para <runtime>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/assemblyBinding/dependentAssembly/assemblyIdentity
- http://schemas.microsoft.com/.NetConfiguration/v2.0#assemblyIdentity
helpviewer_keywords:
- <assemblyIdentity> element
- container tags, <assemblyIdentity> element
- assemblyIdentity element
ms.assetid: cea4d187-6398-4da4-af09-c1abc6a349c1
ms.openlocfilehash: b026dafbde796bbd8726de56b532ed6710ba2290
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79154303"
---
# <a name="assemblyidentity-element-for-runtime"></a>Elemento \<assemblyIdentity> para \<runtime>
Contém informações de identificação sobre o assembly.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<assemblyBinding>**](assemblybinding-element-for-runtime.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<dependentAssembly>**](dependentassembly-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<assemblyIdentity>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
   <assemblyIdentity
name="assembly name"  
publicKeyToken="public key token"  
culture="assembly culture"/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`name`|Atributo obrigatório.<br /><br /> O nome do assembly|  
|`culture`|Atributo opcional.<br /><br /> Uma cadeia de caracteres que especifica o idioma e o país/região do assembly.|  
|`publicKeyToken`|Atributo opcional.<br /><br /> Um valor hexadecimal que especifica o nome forte do assembly.|  
|`processorArchitecture`|Atributo opcional.<br /><br /> Um dos valores "x86", "amd64", "MSIL" ou "IA64", especificando a arquitetura do processador para um assembly que contém código específico do processador. Os valores não diferenciam maiúsculas de minúsculas. Se o atributo for atribuído a qualquer outro valor, todo o `<assemblyIdentity>` elemento será ignorado. Consulte <xref:System.Reflection.ProcessorArchitecture>.|  
  
## <a name="processorarchitecture-attribute"></a>Atributo processorArchitecture  
  
|Valor|Descrição|  
|-----------|-----------------|  
|`amd64`|Somente arquitetura AMD x86-64.|  
|`ia64`|Somente a arquitetura Itanium da Intel.|  
|`msil`|Neutro em relação ao processador e bits por palavra.|  
|`x86`|Um processador x86 de 32 bits, nativo ou no ambiente Windows no Windows (WOW) em uma plataforma de 64 bits.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`assemblyBinding`|Contém informações sobre o redirecionamento de versão e os locais dos assemblies.|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`dependentAssembly`|Encapsula local do assembly e política de associação para cada assembly. Use um `<dependentAssembly>` elemento para cada assembly.|  
|`runtime`|Contém informações sobre associação do assembly e coleta de lixo.|  
  
## <a name="remarks"></a>Comentários  
 Cada **\<dependentAssembly>** elemento deve ter um **\<assemblyIdentity>** elemento filho.  
  
 Se o `processorArchitecture` atributo estiver presente, o `<assemblyIdentity>` elemento se aplicará somente ao assembly com a arquitetura de processador correspondente. Se o `processorArchitecture` atributo não estiver presente, o `<assemblyIdentity>` elemento poderá ser aplicado a um assembly com qualquer arquitetura de processador.  
  
 O exemplo a seguir mostra um arquivo de configuração para dois assemblies com o mesmo nome direcionado a duas arquiteturas de processador diferentes, e cujas versões não foram mantidas em sincronia. Quando o aplicativo é executado na plataforma x86, o primeiro `<assemblyIdentity>` elemento se aplica e o outro é ignorado. Se o aplicativo for executado em uma plataforma diferente de x86 ou ia64, ambos serão ignorados.  
  
```xml  
<configuration>  
   <runtime>  
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">  
         <dependentAssembly>  
            <assemblyIdentity name="MyAssembly"  
                  publicKeyToken="14a739be0244c389"  
                  culture="neutral"  
                  processorArchitecture="x86" />  
            <bindingRedirect oldVersion= "1.0.0.0"
                  newVersion="1.1.0.0" />  
         </dependentAssembly>  
         <dependentAssembly>  
            <assemblyIdentity name="MyAssembly"  
                  publicKeyToken="14a739be0244c389"  
                  culture="neutral"
                  processorArchitecture="ia64" />  
            <bindingRedirect oldVersion="1.0.0.0"
                  newVersion="2.0.0.0" />  
         </dependentAssembly>  
      </assemblyBinding>  
   </runtime>  
</configuration>  
```  
  
 Se um arquivo de configuração contiver um `<assemblyIdentity>` elemento sem `processorArchitecture` atributo e não contiver um elemento que corresponda à plataforma, o elemento sem o `processorArchitecture` atributo será usado.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como fornecer informações sobre um assembly.  
  
```xml  
<configuration>  
   <runtime>  
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">  
         <dependentAssembly>  
            <assemblyIdentity name="myAssembly"  
                              publicKeyToken="32ab4ba45e0a69a1"  
                              culture="neutral" />  
            <!--Redirection and codeBase policy for myAssembly.-->  
         </dependentAssembly>  
      </assemblyBinding>  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- [Esquema de configurações do runtime](index.md)
- [Esquema de arquivos de configuração](../index.md)
- [Redirecionando versões de assembly](../../redirect-assembly-versions.md)
