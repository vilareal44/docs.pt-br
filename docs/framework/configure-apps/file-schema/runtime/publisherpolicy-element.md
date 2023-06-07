---
title: Elemento <publisherPolicy>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/assemblyBinding/publisherPolicy
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/assemblyBinding/dependentAssembly/publisherPolicy
- http://schemas.microsoft.com/.NetConfiguration/v2.0#publisherPolicy
helpviewer_keywords:
- publisherPolicy element
- container tags, <publisherPolicy> element
- <publisherPolicy> element
ms.assetid: 4613407e-d0a8-4ef2-9f81-a6acb9fdc7d4
ms.openlocfilehash: 89fa8a991cc7d0352eb0a13cdfd3a6063ea468e7
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73115844"
---
# <a name="publisherpolicy-element"></a>Elemento \<publisherPolicy>
Especifica se o runtime aplica a política do editor.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<assemblyBinding>**](assemblybinding-element-for-runtime.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<dependentAssembly>**](dependentassembly-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<publisherPolicy>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<publisherPolicy apply="yes|no"/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`apply`|Especifica se a política do Publicador deve ser aplicada.|  
  
## <a name="apply-attribute"></a>aplicar atributo  
  
|Valor|Descrição|  
|-----------|-----------------|  
|`yes`|Aplica a política do Publicador. Essa é a configuração padrão.|  
|`no`|Não aplica a política do Publicador.|  
  
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
 Quando um fornecedor de componentes libera uma nova versão de um assembly, o fornecedor pode incluir uma política de editor para que os aplicativos que usam a versão antiga agora usem a nova versão. Para especificar se a política de editor deve ser aplicada a um determinado assembly, coloque o **\<publisherPolicy>** elemento no **\<dependentAssembly>** elemento.  
  
 A configuração padrão para o atributo **aplicar** é **Sim**. A definição do atributo **aplicar** como **não** substitui as configurações **Sim** anteriores para um assembly.  
  
 A permissão é necessária para que um aplicativo ignore explicitamente a política do editor usando o [\<publisherPolicy apply="no"/>](publisherpolicy-element.md) elemento no arquivo de configuração do aplicativo. A permissão é concedida definindo o <xref:System.Security.Permissions.SecurityPermissionFlag> sinalizador no <xref:System.Security.Permissions.SecurityPermission> . Para obter mais informações, consulte [permissão de segurança de redirecionamento de associação de assembly](../../assembly-binding-redirection-security-permission.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir desativa a política do Publicador para o assembly, `myAssembly` .  
  
```xml  
<configuration>  
   <runtime>  
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">  
         <dependentAssembly>  
            <assemblyIdentity name="myAssembly"  
                                    publicKeyToken="32ab4ba45e0a69a1"  
                                    culture="neutral" />  
            <publisherPolicy apply="no"/>  
         </dependentAssembly>  
      </assemblyBinding>  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- [Esquema de configurações do runtime](index.md)
- [Esquema de arquivos de configuração](../index.md)
- [Como o runtime localiza assemblies](../../../deployment/how-the-runtime-locates-assemblies.md)
- [Redirecionando versões de assembly](../../redirect-assembly-versions.md)
