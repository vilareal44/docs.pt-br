---
title: Elemento <proxy> (Configurações de Rede)
description: O <proxy> elemento de configurações de rede define as opções de servidor proxy no .NET Framework. Este artigo inclui um exemplo.
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/defaultProxy/proxy
- http://schemas.microsoft.com/.NetConfiguration/v2.0#proxy
helpviewer_keywords:
- <proxy> element
- proxy element
ms.assetid: 37a548d8-fade-4ac5-82ec-b49b6c6cb22a
ms.openlocfilehash: 8ae30b8c29dcf3aaa183ff295c7ee8592322797f
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141775"
---
# <a name="proxy-element-network-settings"></a>Elemento \<proxy> (Configurações de Rede)
Define um servidor proxy.  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.net>**](system-net-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<defaultProxy>**](defaultproxy-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<proxy>**

## <a name="syntax"></a>Syntax  
  
```xml  
<proxy
  autoDetect="True|False|Unspecified"
  bypassonlocal="True|False|Unspecified"
  proxyaddress="uriString"
  scriptLocation="uriString"
  usesystemdefault="True|False|Unspecified"
/>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|**Atributo**|**Descrição**|  
|-------------------|---------------------|  
|`autoDetect`|Especifica se o proxy é detectado automaticamente. O valor padrão é `Unspecified`.|  
|`bypassonlocal`|Especifica se o proxy é ignorado para os recursos locais. Os recursos locais incluem o servidor local ( `http://localhost` , `http://loopback` ou `http://127.0.0.1` ) e um URI sem um ponto ( `http://webserver` ). O valor padrão é `Unspecified`.|  
|`proxyaddress`|Especifica o URI de proxy a ser usado.|  
|`scriptLocation`|Especifica o local do script de configuração. Não use o `bypassonlocal` atributo com este atributo. |  
|`usesystemdefault`|Especifica se as configurações de proxy do Internet Explorer devem ser usadas. Se definido como `True` , os atributos subsequentes substituirão as configurações de proxy do Internet Explorer. O valor padrão é `Unspecified`.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|**Elemento**|**Descrição**|  
|-----------------|---------------------|  
|[defaultProxy](defaultproxy-element-network-settings.md)|Configura o servidor proxy HTTP (Hypertext Transfer Protocol).|  
  
## <a name="text-value"></a>Valor de texto  
  
## <a name="remarks"></a>Comentários  
 O `proxy` elemento define um servidor proxy para um aplicativo. Se esse elemento estiver ausente no arquivo de configuração, o .NET Framework usará as configurações de proxy no Internet Explorer.  
  
 O valor do `proxyaddress` atributo deve ser um URI (Uniform Resource Indicator) bem formado.  
  
 O `scriptLocation` atributo refere-se à detecção automática de scripts de configuração de proxy. A <xref:System.Net.WebProxy> classe tentará localizar um script de configuração (geralmente chamado WPAD. dat) quando a opção **usar script de configuração automática** estiver selecionada no Internet Explorer. Se `bypassonlocal` é definido como qualquer valor, `scriptLocation` é ignorado.
  
 Use o `usesystemdefault` atributo para os aplicativos .NET Framework versão 1,1 que estão migrando para a versão 2,0.  
  
 Uma exceção será gerada se o `proxyaddress` atributo especificar um proxy padrão inválido. A <xref:System.Exception.InnerException%2A> Propriedade na exceção deve ter mais informações sobre a causa raiz do erro.  
  
## <a name="configuration-files"></a>Arquivos de configuração  
 Esse elemento pode ser usado no arquivo de configuração do aplicativo ou no arquivo de configuração do computador (Machine.config).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa os padrões do proxy do Internet Explorer, especifica o endereço de proxy e ignora o proxy para acesso local.  
  
```xml  
<configuration>  
  <system.net>  
    <defaultProxy>  
      <proxy  
        usesystemdefault="True"  
        proxyaddress="http://192.168.1.10:3128"  
        bypassonlocal="True"  
      />  
    </defaultProxy>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Net.WebProxy?displayProperty=nameWithType>
- [Esquema de configurações de rede](index.md)
