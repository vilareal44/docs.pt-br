---
title: Elemento <clear> para <namedCaches>
ms.date: 03/30/2017
helpviewer_keywords:
- <clear> element for <namedCaches>
- clear element for <namedCaches>
ms.assetid: ea01a858-65da-4348-800f-5e3df59d4d79
ms.openlocfilehash: bcc0e23f0c47ad3a98430e36da31d39612caa3c9
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70252754"
---
# <a name="clear-element-for-namedcaches"></a>Elemento \<clear> para \<namedCaches>
Limpa todas as `namedCache` entradas na `namedCaches` coleção para um cache de memória.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.runtime.caching>**](system-runtime-caching-element-cache-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<memoryCache>**](memorycache-element-cache-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<namedCaches>**](namedcaches-element-cache-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<clear>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<namedCaches>  
    <clear name="Default" />  
    <!-- child elements -->  
 </namedCaches>  
```  
  
## <a name="type"></a>Type  
 `Type`  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 `None`  
  
### <a name="child-elements"></a>Elementos filho  
 `None`  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<namedCaches>](namedcaches-element-cache-settings.md)|Contém uma coleção de definições de configuração para as <xref:System.Runtime.Caching.MemoryCache> instâncias nomeadas.|  
  
## <a name="remarks"></a>Comentários  
 O `clear` elemento limpa todas as `namedCache` entradas na coleção de cache nomeado para um cache de memória. Você pode usar o `clear` elemento antes de usar o `add` elemento para adicionar uma nova entrada de cache nomeado a fim de ter certeza de que não há outros caches nomeados na coleção.  
  
## <a name="see-also"></a>Confira também

- [\<namedCaches>Elemento (configurações de cache)](namedcaches-element-cache-settings.md)
