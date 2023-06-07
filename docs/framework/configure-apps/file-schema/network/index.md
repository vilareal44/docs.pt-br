---
title: Esquema de configurações de rede
description: Saiba mais sobre o esquema para as configurações de rede que especificam como o .NET Framework se conecta à Internet e manipula URIs.
ms.date: 03/30/2017
helpviewer_keywords:
- elements [.NET Framework], network configuration elements
- sending data, network configuration elements
- receiving data, network configuration elements
- configuration settings [.NET Framework], networks
- Internet, network configuration elements
- network configuration elements
- network settings
- connections [.NET Framework], network configuration elements
- network resources, network configuration elements
ms.assetid: f1de5a0f-76c5-4833-819f-5222b8146340
ms.openlocfilehash: 6a22d7f1608db2e8909d0ead11e9110ec8a8a2c5
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504570"
---
# <a name="network-settings-schema"></a>Esquema de configurações de rede
As configurações de rede especificam como o .NET Framework se conecta à Internet.

As \<system.net> configurações especificam como o .NET Framework se conecta à rede. A tabela a seguir descreve a função de cada elemento de configuração filho no [ \<system.Net> elemento (configurações de rede)](system-net-element-network-settings.md).  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<authenticationModules>Elemento (configurações de rede)](authenticationmodules-element-network-settings.md)|Especifica os módulos usados para autenticar solicitações de Internet.|  
|[\<connectionManagement>Elemento (configurações de rede)](connectionmanagement-element-network-settings.md)|Especifica o número máximo de conexões com host da Internet.|  
|[\<defaultProxy>Elemento (configurações de rede)](defaultproxy-element-network-settings.md)|Especifica o servidor proxy usado para solicitações HTTP para a Internet.|  
|[\<mailSettings>Elemento (configurações de rede)](mailsettings-element-network-settings.md)|Contém configurações para opções de envio de email.|  
|[\<requestCaching>Elemento (configurações de rede)](requestcaching-element-network-settings.md)|Controla o mecanismo de cache para solicitações de rede.|  
|[\<webRequestModules>Elemento (configurações de rede)](webrequestmodules-element-network-settings.md)|Especifica os módulos usados para solicitar informações de hosts da Internet.|  
  
As \<uri> configurações especificam como o .NET Framework trata os endereços da Web expressos usando URIs (Uniform Resource Identifiers). A tabela a seguir descreve a função de cada elemento de configuração filho no [ \<uri> elemento (configurações de URI)](uri-element-uri-settings.md).  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<idn>Elemento (configurações de URI)](idn-element-uri-settings.md)|Especifica se a análise de IDN (Nome de Domínio Internacionalizado) será aplicada aos nomes de domínio.|  
|[\<iriParsing>Elemento (configurações de URI)](iriparsing-element-uri-settings.md)|Especifica se a análise de IRI (Identificador de Recurso Internacional) é aplicada a um <xref:System.Uri> e se as regras de análise de IRI devem ser aplicada.|  
|[\<schemeSettings>Elemento (configurações de URI)](schemesettings-element-uri-settings.md)|Especifica como um <xref:System.Uri> será analisado quanto a esquemas específicos.|  
  
## <a name="see-also"></a>Confira também

- [Configuring Internet Applications](../../../network-programming/configuring-internet-applications.md) (Configurando aplicativos da Internet)
- [Esquema do arquivo de configuração](../index.md)
