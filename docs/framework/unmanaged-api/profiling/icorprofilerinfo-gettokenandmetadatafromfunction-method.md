---
title: Método ICorProfilerInfo::GetTokenAndMetadataFromFunction
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetTokenAndMetadataFromFunction
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::GetTokenAndMetadataFromFunction
helpviewer_keywords:
- ICorProfilerInfo::GetTokenAndMetadataFromFunction method [.NET Framework profiling]
- GetTokenAndMetadataFromFunction method [.NET Framework profiling]
ms.assetid: e525aa16-c923-4b16-833b-36f1f0dd70fc
topic_type:
- apiref
ms.openlocfilehash: 1cc05f4c10f4a5b042ff14c05f3c85a7b5935184
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84497888"
---
# <a name="icorprofilerinfogettokenandmetadatafromfunction-method"></a>Método ICorProfilerInfo::GetTokenAndMetadataFromFunction
Obtém o token de metadados e uma instância de interface de metadados que podem ser usados em relação ao token para a função especificada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetTokenAndMetaDataFromFunction(  
    [in]  FunctionID functionId,  
    [in]  REFIID     riid,  
    [out] IUnknown   **ppImport,  
    [out] mdToken    *pToken);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `functionId`  
 no A ID da função para a qual obter o token de metadados e a interface de metadados.  
  
 `riid`  
 no A ID de referência da interface de metadados para obter a instância do.  
  
 `ppImport`  
 fora Um ponteiro para o endereço da instância da interface de metadados que pode ser usada em relação ao token para a função especificada.  
  
 `pToken`  
 fora Um ponteiro para o token de metadados para a função especificada.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
