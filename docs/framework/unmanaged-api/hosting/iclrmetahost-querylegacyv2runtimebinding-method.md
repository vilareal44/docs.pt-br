---
title: Método ICLRMetaHost::QueryLegacyV2RuntimeBinding
ms.date: 03/30/2017
api_name:
- ICLRMetaHost.RequestRuntimeLoadedNotification
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRMetaHost::QueryLegacyV2RuntimeBinding
helpviewer_keywords:
- QueryLegacyV2RuntimeBinding method [.NET Framework hosting]
- ICLRMetaHost::QueryLegacyV2RuntimeBinding method [.NET Framework hosting]
ms.assetid: 9929817e-acc9-40b7-960c-598664e04b60
topic_type:
- apiref
ms.openlocfilehash: b270a6691d4e4ee4a5d0b42f424694eb7993e4e7
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504141"
---
# <a name="iclrmetahostquerylegacyv2runtimebinding-method"></a>Método ICLRMetaHost::QueryLegacyV2RuntimeBinding
Retorna uma interface que representa um tempo de execução ao qual a política de ativação herdada foi associada, por exemplo, usando o `useLegacyV2RuntimeActivationPolicy` atributo na entrada do arquivo de configuração do [ \<startup> elemento](../../configure-apps/file-schema/startup/startup-element.md) , usando o uso direto das APIs de ativação herdadas ou chamando o método [ICLRRuntimeInfo:: BindAsLegacyV2Runtime](iclrruntimeinfo-bindaslegacyv2runtime-method.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT QueryLegacyV2RuntimeBinding (  
    [in] REFIID riid,  
    [out, iid_is(riid), retval] LPVOID *ppUnk);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `riid`  
 no Obrigatório. no momento, o único valor válido para esse parâmetro é `IID_ICLRRuntimeInfo` .  
  
 `ppUnk`  
 fora Necessário. Quando esse método retorna, contém um ponteiro para a interface [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) que representa um tempo de execução que foi associado à política de ativação herdada.  
  
## <a name="return-value"></a>Valor Retornado  
 Esse método retorna os HRESULTs específicos a seguir, bem como os erros de HRESULT que indicam falha de método.  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|O método foi concluído com êxito e retornou um tempo de execução que foi associado à política de ativação herdada.|  
|S_FALSE|O método foi concluído com êxito, mas um tempo de execução herdado ainda não foi associado.|  
|E_NOINTERFACE|O método encontrou um tempo de execução que foi associado à política de ativação herdada, mas `riid` não tem suporte nesse tempo de execução.|  
  
## <a name="remarks"></a>Comentários  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MetaHost. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRMetaHost](iclrmetahost-interface.md)
- [Hosting](index.md)
