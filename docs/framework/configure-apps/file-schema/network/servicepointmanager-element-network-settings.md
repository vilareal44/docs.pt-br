---
title: Elemento <servicePointManager> (Configurações de Rede)
description: O <servicePointManager> elemento de configurações de rede configura conexões com opções de recursos de rede no .NET Framework.
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#servicePointManager
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/settings/servicePointManager
helpviewer_keywords:
- servicePointManager element
- <servicePointManager> element
ms.assetid: 6e5def51-3646-4ef6-a7bd-c69151321bec
ms.openlocfilehash: eb27716ec7c2936f32a7e4d4c983d1e175c4d044
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504518"
---
# <a name="servicepointmanager-element-network-settings"></a>Elemento \<servicePointManager> (Configurações de Rede)
Configura conexões com recursos de rede.  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.net>**](system-net-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<settings>**](settings-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<servicePointManager>**

## <a name="syntax"></a>Sintaxe  
  
```xml  
<servicePointManager  
  checkCertificateName="true|false"  
  checkCertificateRevocationList="true|false"  
  encryptionPolicy="AllowNoEncryption|NoEncryption|RequireEncryption"  
  expect100Continue="true|false"  
  useNagleAlgorithm="true|false"  
  enableDnsRoundRobin="true|false"  
  dnsRefreshTimeout="time"  
/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|**Atributo**|**Descrição**|  
|-------------------|---------------------|  
|`checkCertificateName`|Especifica se o sistema deve verificar se o nome no certificado corresponde ao nome de host do servidor antes de usar o certificado. O valor padrão é `true`.|  
|`checkCertificateRevocationList`|Especifica se o sistema deve verificar se o certificado foi revogado antes de usar o certificado. O valor padrão é `false`.|  
|`dnsRefreshTimeout`|Especifica por quanto tempo as resoluções de DNS (serviço de nomes de domínio) são armazenadas em cache em conjunto com a opção Round Robin do DNS, em milissegundos. O valor padrão é de 120.000 milissegundos (dois minutos).|  
|`enableDnsRoundRobin`|Especifica se as resoluções DNS de nomes de host com vários endereços IP (Internet Protocol) retornam todos os endereços ou apenas o primeiro. O valor padrão é `false`.|  
|`encryptionPolicy`|Especifica a política de criptografia aplicada a uma sessão SSL/TLS em uma <xref:System.Net.ServicePointManager> instância. Os valores possíveis são equivalentes aos valores da <xref:System.Net.Security.EncryptionPolicy> enumeração. O uso de <xref:System.Security.Authentication.CipherAlgorithmType.Null> é necessário quando a política de criptografia é definida como `NoEncryption` . O valor padrão é `RequireEncryption`.|  
|`expect100Continue`|Especifica se os métodos POST devem esperar receber uma `100-continue` resposta do servidor. O valor padrão é `true`.|  
|`useNagleAlgorithm`|Especifica se as conexões controladas pelo Gerenciador de ponto de serviço usam o algoritmo Nagle. O valor padrão é `true`.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|**Elemento**|**Descrição**|  
|-----------------|---------------------|  
|[Configurações](settings-element-network-settings.md)|Configura as opções de rede básicaspara o namespace <xref:System.Net>.|  
  
## <a name="remarks"></a>Comentários  
  
## <a name="configuration-files"></a>Arquivos de configuração  
 Esse elemento pode ser usado no arquivo de configuração do aplicativo ou no arquivo de configuração do computador (Machine. config).  
  
## <a name="see-also"></a>Confira também

- <xref:System.Net.ServicePointManager>
- <xref:System.Net.Security.EncryptionPolicy>
- [Esquema de configurações de rede](index.md)
