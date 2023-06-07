---
title: Esquema de configurações de criptografia
ms.date: 03/30/2017
helpviewer_keywords:
- configuration schema [.NET Framework], cryptography
- elements [.NET Framework], cryptography
- schema configuration settings
- cryptography, settings schema
- cryptography, mapping algorithm names
- configuration sections [.NET Framework]
- configuration settings [.NET Framework], cryptography
ms.assetid: 1f55050a-b2a3-4868-a3c0-da20826150f3
ms.openlocfilehash: c632a15552c8ba5743aac1309098b7d7ef949bbd
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74088002"
---
# <a name="cryptography-settings-schema"></a>Esquema de configurações de criptografia
O esquema de configurações de criptografia contém elementos que especificam como mapear nomes de algoritmo amigáveis para classes que implementam algoritmos de criptografia.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<mscorlib>**](mscorlib-element-for-cryptography-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<cryptographySettings>**](cryptographysettings-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<cryptoNameMapping>**](cryptonamemapping-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<cryptoClasses>**](cryptoclasses-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<cryptoClass>**](cryptoclass-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<nameEntry>**](nameentry-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<oidMap>**](oidmap-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<oidEntry>**](oidentry-element.md)

|Elemento|Descrição|  
|-------------|-----------------|  
|[**\<cryptoClasses**>](cryptoclasses-element.md)|Contém uma lista de classes de criptografia que têm um mapeamento para um nome amigável no **\<nameEntry>** elemento.|  
|[**\<cryptoClass**>](cryptoclass-element.md)|Contém uma classe de criptografia que tem um mapeamento para um nome amigável no **\<nameEntry>** elemento.|  
|[**\<cryptographySettings**>](cryptographysettings-element.md)|Contém configurações de criptografia.|  
|[**\<cryptoNameMapping**>](cryptonamemapping-element.md)|Contém mapeamentos de classes para nomes amigáveis.|  
|[**\<mscorlib>** elemento para configurações de criptografia](mscorlib-element-for-cryptography-settings.md)|Contém o **\<cryptographySettings>** elemento.|  
|[**\<nameEntry>**](nameentry-element.md)|Mapeia um nome de classe para um nome de algoritmo amigável, o que permite que uma classe tenha vários nomes amigáveis.|  
|[**\<oidEntry>**](oidentry-element.md)|Mapeia um OID (identificador de objeto) do ASN.1 para um nome amigável.|  
|[**\<oidMap>**](oidmap-element.md)|Contém mapeamentos de OID do ASN.1 para classes.|  
  
## <a name="see-also"></a>Confira também

- [Esquema de arquivos de configuração](../index.md)
- [Serviços de Criptografia](../../../../standard/security/cryptographic-services.md)
