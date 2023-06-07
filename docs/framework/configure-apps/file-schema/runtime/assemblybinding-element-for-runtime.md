---
title: Elemento <assemblyBinding> para <runtime>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/assemblyBinding
helpviewer_keywords:
- <assemblyBinding> element
- assemblyBinding element
- container tags, <assemblyBinding> element
ms.assetid: 964cbb35-ab49-4498-8471-209689e5dada
ms.openlocfilehash: 202b063ad3f0f9696cdc12aff434d61fe5a813e6
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79154316"
---
# <a name="assemblybinding-element-for-runtime"></a>Elemento \<assemblyBinding> para \<runtime>
Contém informações sobre o redirecionamento de versão e os locais dos assemblies.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<assemblyBinding>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
      <assemblyBinding
   xmlns="urn:schemas-microsoft-com:asm.v1" appliesTo="v1.0.3705">  
</assemblyBinding>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|**xmlns**|Atributo obrigatório.<br /><br /> Especifica o namespace XML necessário para a associação de assembly. Use a cadeia de caracteres "urn: schemas-microsoft-com: asm. v1" como o valor.|  
|**appliesTo**|Especifica a versão de tempo de execução à qual o redirecionamento de assembly .NET Framework se aplica. Esse atributo opcional usa um número de versão .NET Framework para indicar a qual versão ele se aplica. Se nenhum atributo **AppliesTo** for especificado, o **\<assemblyBinding>** elemento se aplicará a todas as versões do .NET Framework. O atributo **AppliesTo** foi introduzido na versão .NET Framework 1,1; Ele é ignorado pelo .NET Framework versão 1,0. Isso significa que todos os **\<assemblyBinding>** elementos são aplicados ao usar o .NET Framework versão 1,0, mesmo se um atributo **AppliesTo** for especificado.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<dependentAssembly>](dependentassembly-element.md)|Encapsula a política de associação e o local do assembly para um assembly. Use uma **\<dependentAssembly>** marca para cada assembly.|  
|[\<probing>](probing-element.md)|Especifica subdiretórios que o Common Language Runtime pesquisa ao carregar assemblies.|  
|[\<publisherPolicy>](publisherpolicy-element.md)|Especifica se o runtime aplica a política do editor.|  
|[\<qualifyAssembly>](qualifyassembly-element.md)|Especifica o nome completo do assembly que deve ser carregado dinamicamente quando um nome parcial é usado.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`runtime`|Contém informações sobre associação do assembly e coleta de lixo.|  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como redirecionar uma versão do assembly para outra e fornecer uma codebase.  
  
```xml  
<configuration>  
   <runtime>  
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">  
         <dependentAssembly>  
            <assemblyIdentity name="myAssembly"  
                              publicKeyToken="32ab4ba45e0a69a1"  
                              culture="neutral" />  
            <bindingRedirect oldVersion="1.0.0.0"  
                             newVersion="2.0.0.0"/>  
            <codeBase version="2.0.0.0"  
                      href="http://www.litwareinc.com/myAssembly.dll"/>  
         </dependentAssembly>  
      </assemblyBinding>  
   </runtime>  
</configuration>  
```  
  
 O exemplo a seguir mostra como usar o atributo **AppliesTo** para redirecionar a associação de um assembly .NET Framework.  
  
```xml  
<runtime>  
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1" appliesTo="v1.0.3705">  
      <dependentAssembly>
         <assemblyIdentity name="mscorcfg" publicKeyToken="b03f5f7f11d50a3a" culture=""/>  
         <bindingRedirect oldVersion="0.0.0.0-65535.65535.65535.65535" newVersion="1.0.3300.0"/>  
      </dependentAssembly>  
   </assemblyBinding>  
</runtime>  
```  
  
## <a name="see-also"></a>Confira também

- [Esquema de configurações do runtime](index.md)
- [Esquema de arquivos de configuração](../index.md)
- [Redirecionando versões de assembly](../../redirect-assembly-versions.md)
