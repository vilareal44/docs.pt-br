---
title: Método ICorProfilerInfo3::GetAppDomainsContainingModule
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo3.GetAppDomainsContainingModule Method
api_location:
- Mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo3::GetAppDomainsContainingModule
helpviewer_keywords:
- GetAppDomainsContainingModule method [.NET Framework profiling]
- ICorProfilerInfo3::GetAppDomainsContainingModule method [.NET Framework profiling]
ms.assetid: 603b3881-ea94-4dca-95cd-91eebac873a0
topic_type:
- apiref
ms.openlocfilehash: 8615deb2e42b039120d97b3eb5af23beb31b0808
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502841"
---
# <a name="icorprofilerinfo3getappdomainscontainingmodule-method"></a>Método ICorProfilerInfo3::GetAppDomainsContainingModule
Obtém os identificadores dos domínios de aplicativo nos quais o módulo fornecido foi carregado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetAppDomainsContainingModule(  
            [in] ModuleID moduleId,  
            [in] ULONG32 cAppDomainIds,  
            [out] ULONG32 *pcAppDomainIds,  
            [out, size_is(cAppDomainIds), length_is(*pcAppDomainIds)]  
                    AppDomainID appDomainIds[]);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `moduleId`  
 no A ID do módulo carregado.  
  
 `cAppDomainIds`  
 no O tamanho da `appDomainIds` matriz.  
  
 `pcAppDomainIds`  
 fora Um ponteiro para o número total de elementos retornados.  
  
 `appDomainIds`  
 fora Uma matriz de valores de ID de domínio do aplicativo.  
  
## <a name="remarks"></a>Comentários  
 O método usa buffers alocados do chamador.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerFunctionEnum](icorprofilerfunctionenum-interface.md)
- [Interface ICorProfilerInfo3](icorprofilerinfo3-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Criação de perfil](index.md)
