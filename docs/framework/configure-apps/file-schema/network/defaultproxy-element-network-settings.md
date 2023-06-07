---
title: Elemento <defaultProxy> (Configurações de Rede)
description: O <defaultProxy> elemento de configurações de rede configura o servidor proxy http (Hypertext Transfer Protocol) no .NET Framework.
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#defaultProxy
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/defaultProxy
helpviewer_keywords:
- defaultProxy element
- <defaultProxy> element
ms.assetid: 9d663c4b-07b4-4f6f-9b12-efbd3630354f
ms.openlocfilehash: 85004d49ce7605b050709a3019592ec696a7bada
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141625"
---
# <a name="defaultproxy-element-network-settings"></a>Elemento \<defaultProxy> (Configurações de Rede)
Configura o servidor proxy HTTP (Hypertext Transfer Protocol).  
  
[**\<configuration>**](../configuration-element.md)  
&nbsp;&nbsp;[**\<system.net>**](system-net-element-network-settings.md)  
&nbsp;&nbsp;&nbsp;&nbsp;**\<defaultProxy>**  
  
## <a name="syntax"></a>Syntax  
  
```xml  
<defaultProxy  
  enabled="True|False"  
  useDefaultCredentials="True|False">  
    <bypasslist>...</bypasslist>  
    <proxy>...</proxy>  
    <module>...</module>  
</defaultProxy>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|**Elemento**|**Descrição**|  
|-----------------|---------------------|  
|`enabled`|Especifica se um proxy Web é usado. O valor padrão é `True`.|  
|`useDefaultCredentials`|Especifica se as credenciais padrão para este host são usadas para acessar o proxy Web. O valor padrão é `False`.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|**Elemento**|**Descrição**|  
|-----------------|---------------------|  
|[bypasslist](bypasslist-element-network-settings.md)|Fornece um conjunto de expressões regulares que descrevem endereços que não usam o proxy.|  
|[modulo](module-element-network-settings.md)|Adiciona um novo módulo de proxy ao aplicativo.|  
|[acionista](proxy-element-network-settings.md)|Define um servidor proxy.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|**Elemento**|**Descrição**|  
|-----------------|---------------------|  
|[system.net](system-net-element-network-settings.md)|Contém configurações que especificam como o .NET Framework se conecta à rede.|  
  
## <a name="remarks"></a>Comentários  
 Se o elemento defaultProxy estiver vazio, as configurações de proxy do Internet Explorer serão usadas. Esse comportamento é diferente da versão 1,1 do .NET Framework.  
  
 Uma exceção é gerada se o elemento [Module](module-element-network-settings.md) especifica um tipo não público, o tipo não é derivado da <xref:System.Net.IWebProxy> classe, uma exceção do construtor sem parâmetros desse objeto ocorreu ou uma exceção ocorreu ao recuperar o proxy padrão especificado pelo sistema. A <xref:System.Exception.InnerException%2A> Propriedade na exceção deve ter mais informações sobre a causa raiz do erro.  
  
## <a name="configuration-files"></a>Arquivos de configuração  
 Esse elemento pode ser usado no arquivo de configuração do aplicativo ou no arquivo de configuração do computador (Machine.config).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa os padrões do proxy do Internet Explorer, especifica o endereço de proxy e ignora o proxy para acesso local e contoso.com.  
  
```xml  
<configuration>  
  <system.net>  
    <defaultProxy>  
      <proxy  
        usesystemdefault="True"  
        proxyaddress="http://192.168.1.10:3128"  
        bypassonlocal="True"  
      />  
      <bypasslist>  
        <add address="[a-z]+\.contoso\.com$" />  
      </bypasslist>  
    </defaultProxy>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Net.WebProxy?displayProperty=nameWithType>
- [Esquema de configurações de rede](index.md)
