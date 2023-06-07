---
title: Elemento <mailSettings> (Configurações de Rede)
description: O <mailSettings> elemento de configurações de rede configura as opções de envio de email no .NET Framework.
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#mailSettings
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/mailSettings
helpviewer_keywords:
- mailSettings element
- <mailSettings> element
ms.assetid: 54f0f153-17e5-4f49-afdc-deadb940c9c1
ms.openlocfilehash: ce7b8564e4ee5ea73d42259612c077420d36645b
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504557"
---
# <a name="mailsettings-element-network-settings"></a>Elemento \<mailSettings> (Configurações de Rede)
Configura as opções de envio de email.  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.net>**](system-net-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<mailSettings>**

## <a name="syntax"></a>Sintaxe  
  
```xml  
<mailSettings>
  <smtp>...</smtp>  
</mailSettings>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum.  
  
### <a name="child-elements"></a>Elementos filho  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|[\<smtp>Elemento (configurações de rede)](smtp-element-network-settings.md)|Configura opções de protocolo de transporte de email simples.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|**Elemento**|**Descrição**|  
|-----------------|---------------------|  
|[\<system.Net>Elemento (configurações de rede)](system-net-element-network-settings.md)|Contém configurações que especificam como o .NET Framework se conecta à rede.|  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir especifica os parâmetros de SMTP apropriados para enviar email usando as credenciais de rede padrão.  
  
```xml  
<configuration>  
  <system.net>  
    <mailSettings>  
      <smtp deliveryMethod="Network">  
        <network  
          host="localhost"  
          port="25"  
          defaultCredentials="true"  
        />  
      </smtp>  
    </mailSettings>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Net.Mail.SmtpClient>
- [Esquema de configurações de rede](index.md)
