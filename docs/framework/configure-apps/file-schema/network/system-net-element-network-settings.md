---
title: Elemento <system.Net> (configurações de rede)
description: O elemento de configurações de rede <system.Net> contém configurações que especificam como o .NET Framework se conecta às opções de rede no .NET Framework.
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#system.Net
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.Net
helpviewer_keywords:
- system.Net element
- <system.Net> element
ms.assetid: 52de4d6c-b24d-44aa-ba7d-6b5061f1357e
ms.openlocfilehash: 9f18c7a3586948c03391d609f437e216a91bc27f
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504479"
---
# <a name="systemnet-element-network-settings"></a>Elemento \<system.Net> (Configurações de Rede)
Contém configurações que especificam como o .NET Framework se conecta à rede.  
  
[**\<configuration>**](../configuration-element.md)  
&nbsp;&nbsp;**\<system.net>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.net>
</system.net>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum.  
  
### <a name="child-elements"></a>Elementos filho  
  
|**Elemento**|**Descrição**|  
|-----------------|---------------------|  
|[authenticationModules](authenticationmodules-element-network-settings.md)|Especifica os módulos usados para autenticar solicitações da Internet.|  
|[connectionManagement](connectionmanagement-element-network-settings.md)|Especifica o número máximo de conexões com um host de Internet.|  
|[defaultProxy](defaultproxy-element-network-settings.md)|Configura o servidor proxy HTTP (Hypertext Transfer Protocol).|  
|[mailSettings](mailsettings-element-network-settings.md)|Configura as opções de envio de email do protocolo SMTP.|  
|[requestCaching](requestcaching-element-network-settings.md)|Controla o mecanismo de cache para solicitações de rede.|  
|[configurações](settings-element-network-settings.md)|Configura as opções básicas de rede para classes nos <xref:System.Net> namespaces filho relacionados.|  
|[webRequestModules](webrequestmodules-element-network-settings.md)|Especifica os módulos a serem usados para solicitar informações de hosts da Internet.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|**Elemento**|**Descrição**|  
|-----------------|---------------------|  
|[configuração](../configuration-element.md)|Contém configurações para todos os namespaces.|  
  
## <a name="remarks"></a>Comentários  
 O [\<system.net>](system-net-element-network-settings.md) elemento contém configurações para classes nos <xref:System.Net> namespaces filho relacionados. As configurações definem módulos de autenticação, gerenciamento de conexão, configurações de email, o servidor proxy e os módulos de solicitação da Internet para receber informações de hosts da Internet.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra uma configuração típica usada por <xref:System.Net> classes.  
  
```xml  
<configuration>  
  <system.net>  
    <authenticationModules>  
      <add type="System.Net.DigestClient" />  
      <add type="System.Net.NegotiateClient" />  
      <add type="System.Net.KerberosClient" />  
      <add type="System.Net.NtlmClient" />  
      <add type="System.Net.BasicClient" />  
    </authenticationModules>  
    <connectionManagement>  
      <add address="*" maxconnection="2" />  
    </connectionManagement>  
    <defaultProxy>  
      <proxy  
        usesystemdefault="true"  
        bypassonlocal="true"  
      />  
    </defaultProxy>  
    <webRequestModules>  
      <add prefix="http"  
           type="System.Net.HttpRequestCreator"  
      />  
      <add prefix="https"  
           type="System.Net.HttpRequestCreator"  
      />  
      <add prefix="file"  
           type="System.Net.FileWebRequestCreator"  
      />  
    </webRequestModules>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- [Esquema de configurações de rede](index.md)
