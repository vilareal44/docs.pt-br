---
title: Elemento <bypasslist> (Configurações de Rede)
description: O <bypasslist> elemento de configurações de rede fornece um conjunto de expressões regulares que descrevem os endereços que não usam um proxy no .NET Framework.
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#bypasslist
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/defaultProxy/bypasslist
helpviewer_keywords:
- bypasslist element
- <bypasslist> element
ms.assetid: 124446b7-abb1-4e5e-a492-b64398f268f1
ms.openlocfilehash: 42b6ddf4c3d09bcf8ef0ada105cefedccc63b505
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504622"
---
# <a name="bypasslist-element-network-settings"></a>Elemento \<bypasslist> (Configurações de Rede)
Fornece um conjunto de expressões regulares que descrevem endereços que não usam um proxy.  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.net>**](system-net-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<defaultProxy>**](defaultproxy-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<bypasslist>**

## <a name="syntax"></a>Sintaxe  
  
```xml  
<bypasslist>
</bypasslist>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum.  
  
### <a name="child-elements"></a>Elementos filho  
  
|**Elemento**|**Descrição**|  
|-----------------|---------------------|  
|[adicionar](add-element-for-bypasslist-network-settings.md)|Adiciona um endereço IP ou nome DNS à lista de bypass de proxy.|  
|[formatação](clear-element-for-bypasslist-network-settings.md)|Limpa a lista de bypass.|  
|[remove](remove-element-for-bypasslist-network-settings.md)|Remove um endereço IP ou nome DNS da lista de bypass de proxy.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|**Elemento**|**Descrição**|  
|-----------------|---------------------|  
|[defaultProxy](defaultproxy-element-network-settings.md)|Configura o servidor proxy HTTP (Hypertext Transfer Protocol).|  
  
## <a name="remarks"></a>Comentários  
 A lista de bypass contém expressões regulares que descrevem URIs que as <xref:System.Net.WebRequest> instâncias acessam diretamente em vez de por meio do servidor proxy.  
  
 Tome cuidado ao especificar uma expressão regular para este elemento. A expressão regular "[a-z] + \\ . contoso \\ . com" corresponde a qualquer host no domínio contoso.com, mas também corresponde a qualquer host no domínio contoso.com.cpandl.com. Para corresponder apenas a um host no domínio contoso.com, use uma âncora ("$"): "[a-z] + \\ . contoso \\ . com $".  
  
 Para obter mais informações sobre expressões regulares, consulte. [.NET Framework expressões regulares](../../../../standard/base-types/regular-expressions.md).  
  
## <a name="configuration-files"></a>Arquivos de configuração  
 Esse elemento pode ser usado no arquivo de configuração do aplicativo ou no arquivo de configuração do computador (Machine. config).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir adiciona dois endereços à lista de bypass. O primeiro ignora o proxy para todos os servidores no domínio contoso.com; o segundo ignora o proxy para todos os servidores cujos endereços IP começam com 192,168.  
  
```xml  
<configuration>  
  <system.net>  
    <defaultProxy>  
      <bypasslist>  
        <add address="[a-z]+\.contoso\.com$" />  
        <add address="192\.168\.\d{1,3}\.\d{1,3}" />  
      </bypasslist>  
    </defaultProxy>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Net.WebProxy?displayProperty=nameWithType>
- [Esquema de configurações de rede](index.md)
