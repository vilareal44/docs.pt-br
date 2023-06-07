---
title: Elemento <developmentMode>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/developmentMode
- http://schemas.microsoft.com/.NetConfiguration/v2.0#developmentMode
helpviewer_keywords:
- developmentMode element
- container tags, <developmentMode> element
- <developmentMode> element
ms.assetid: 60e79a8c-415a-497d-be29-b9d0fd9bdee3
ms.openlocfilehash: 4a062da31740edb8f0c7a4f4db8b09800c687587
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73117626"
---
# <a name="developmentmode-element"></a>Elemento \<developmentMode>
Especifica se o runtime pesquisa por assemblies em diretórios especificados pela variável de ambiente DEVPATH.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<developmentMode>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<developmentMode developerInstallation="true | false"/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|**developerInstallation**|Especifica se o runtime pesquisa por assemblies em diretórios especificados pela variável de ambiente DEVPATH.|  
  
## <a name="developerinstallation-attribute"></a>Atributo developerInstallation  
  
|Valor|Descrição|  
|-----------|-----------------|  
|**true**|Pesquisa assemblies em diretórios especificados pela variável de ambiente DEVPATH.|  
|**false**|Não pesquisa assemblies em diretórios especificados pela variável de ambiente DEVPATH. Esse é o padrão.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`runtime`|Contém informações sobre associação do assembly e coleta de lixo.|  
  
## <a name="remarks"></a>Comentários  
 Use essa configuração somente no momento do desenvolvimento. O tempo de execução não verifica as versões em assemblies de nome forte encontrados no DEVPATH. Ele simplesmente usa o primeiro assembly que encontra.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como fazer com que o tempo de execução procure por assemblies em diretórios especificados pela variável de ambiente DEVPATH.  
  
```xml  
<configuration>  
   <runtime>  
      <developmentMode developerInstallation="true"/>  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- [Esquema de configurações do runtime](index.md)
- [Esquema de arquivos de configuração](../index.md)
- [Como: Localizar assemblies usando DEVPATH](../../how-to-locate-assemblies-by-using-devpath.md)
