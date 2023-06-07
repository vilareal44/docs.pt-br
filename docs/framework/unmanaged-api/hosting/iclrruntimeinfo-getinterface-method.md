---
title: Método ICLRRuntimeInfo::GetInterface
ms.date: 03/30/2017
api_name:
- ICLRRuntimeInfo.GetInterface
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICLRRuntimeInfo::GetInterface
helpviewer_keywords:
- GetInterface method [.NET Framework hosting]
- ICLRRuntimeInfo::GetInterface method [.NET Framework hosting]
ms.assetid: cc7b0e5b-48c3-4509-8ebb-611ddb1f7ec2
topic_type:
- apiref
ms.openlocfilehash: 9cf9d48bf50ffc1fc56270c13215acfef6d9c3af
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504050"
---
# <a name="iclrruntimeinfogetinterface-method"></a>Método ICLRRuntimeInfo::GetInterface
Carrega o CLR no processo atual e retorna ponteiros de interface de tempo de execução, como [ICLRRuntimeHost](iclrruntimehost-interface.md), [ICLRStrongName](iclrstrongname-interface.md)e [IMetaDataDispenserEx](../metadata/imetadatadispenser-interface.md).  
  
 Esse método substitui todas as `CorBindTo` funções * na seção [funções de Hospedagem de CLR preteridas](deprecated-clr-hosting-functions.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetInterface(  
[in]  REFCLSID rclsid,  
[in]  REFIID   riid,  
[out, iid_is(riid), retval] LPVOID *ppUnk);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `rclsid`  
 no A interface CLSID para a coclass.  
  
 `riid`  
 no O IID da interface solicitada `rclsid` .  
  
 `ppUnk`  
 fora Um ponteiro para a interface consultada.  
  
## <a name="return-value"></a>Valor Retornado  
 Esse método retorna os HRESULTs específicos a seguir, bem como os erros de HRESULT que indicam falha de método.  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|O método foi concluído com êxito.|  
|E_POINTER|`ppUnk` é nulo.|  
|E_OUTOFMEMORY|Não há memória suficiente disponível para lidar com a solicitação.|  
|CLR_E_SHIM_LEGACYRUNTIMEALREADYBOUND|Um tempo de execução diferente já estava associado à política de ativação do CLR versão 2 herdada.|  
  
## <a name="remarks"></a>Comentários  
 Esse método faz com que o CLR seja carregado, mas não inicializado.  
  
 A tabela a seguir mostra as combinações com suporte para o `rclsid` e o `riid` .  
  
|`rclsid`|`riid`|  
|--------------|------------|  
|CLSID_CorMetaDataDispenser|IID_IMetaDataDispenser, IID_IMetaDataDispenserEx|  
|CLSID_CorMetaDataDispenserRuntime|IID_IMetaDataDispenser, IID_IMetaDataDispenserEx|  
|CLSID_CorRuntimeHost|IID_ICorRuntimeHost|  
|CLSID_CLRRuntimeHost|IID_ICLRRuntimeHost|  
|CLSID_TypeNameFactory|IID_ITypeNameFactory|  
|CLSID_CLRDebuggingLegacy|IID_ICorDebug|  
|||  
|CLSID_CLRStrongName|IID_ICLRStrongName|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MetaHost. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRRuntimeInfo](iclrruntimeinfo-interface.md)
- [Interfaces de hospedagem](hosting-interfaces.md)
- [Hosting](index.md)
