---
title: Enumeração METAHOST_CONFIG_FLAGS
ms.date: 03/30/2017
api_name:
- METAHOST_CONFIG_FLAGS
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- METAHOST_CONFIG_FLAGS
helpviewer_keywords:
- METAHOST_CONFIG_FLAGS enumeration [.NET Framework hosting]
ms.assetid: 6f1e389f-ed99-4d6a-a0ba-72d7d869a01d
topic_type:
- apiref
ms.openlocfilehash: 6b37fcfc04e3ec880c67f102ec12d7f3e4b06a43
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84493156"
---
# <a name="metahost_config_flags-enumeration"></a>Enumeração METAHOST_CONFIG_FLAGS
Descreve os possíveis sinalizadores retornados no `pdwConfigFlags` parâmetro do método [ICLRMetaHostPolicy:: GetRequestedRuntime](iclrmetahostpolicy-getrequestedruntime-method.md) , indicando a presença e a configuração do `useLegacyV2RuntimeActivationPolicy` atributo no [ \<startup> elemento](../../configure-apps/file-schema/startup/startup-element.md) do arquivo de configuração.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum {  
    METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_UNSET  = 0x00,  
    METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_TRUE   = 0x01,  
    METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_FALSE  = 0x02,  
    METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_MASK   = 0x03  
} METAHOST_CONFIG_FLAGS;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_UNSET`|O `useLegacyV2RuntimeActivationPolicy` atributo não estava presente no [ \<startup> elemento](../../configure-apps/file-schema/startup/startup-element.md).|  
|`METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_TRUE`|O `useLegacyV2RuntimeActivationPolicy` atributo estava presente e definido como `true` .|  
|`METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_FALSE`|O `useLegacyV2RuntimeActivationPolicy` atributo estava presente e definido como `false` .|  
|`METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_MASK`|Aplique essa máscara ao valor retornado em `pdwConfigFlags` para obter os valores relevantes para `useLegacyV2RuntimeActivationPolicy` .|  
  
## <a name="remarks"></a>Comentários  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MetaHost. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Hospedando enumerações](hosting-enumerations.md)
- [Método GetRequestedRuntime](iclrmetahostpolicy-getrequestedruntime-method.md)
- [\<startup>Elementos](../../configure-apps/file-schema/startup/startup-element.md)
