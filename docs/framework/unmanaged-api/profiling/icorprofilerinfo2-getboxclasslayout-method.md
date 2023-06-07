---
title: Método ICorProfilerInfo2::GetBoxClassLayout
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetBoxClassLayout
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetBoxClassLayout
helpviewer_keywords:
- GetBoxClassLayout method [.NET Framework profiling]
- ICorProfilerInfo2::GetBoxClassLayout method [.NET Framework profiling]
ms.assetid: 624672b5-1189-488a-85d2-3e12b49617c1
topic_type:
- apiref
ms.openlocfilehash: 630b67a64716f26577bbc376970e4f76216f4da5
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84497340"
---
# <a name="icorprofilerinfo2getboxclasslayout-method"></a>Método ICorProfilerInfo2::GetBoxClassLayout
Obtém informações sobre onde o tipo de valor especificado está localizado quando ele está em caixa.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetBoxClassLayout(  
    [in] ClassID classId,  
    [out] ULONG32 *pBufferOffset);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `classId`  
 no A ID da classe que descreve o tipo de valor que está em caixa.  
  
 `pBufferOffset`  
 fora Um inteiro que é o deslocamento, relativo ao ponteiro da ID do objeto em caixa, do tipo de valor.  
  
## <a name="remarks"></a>Comentários  
 O `pBufferOffset` valor é o local do tipo de valor dentro de uma caixa. Depois `pBufferOffset` que é aplicado a um objeto em caixa, o layout de classe do tipo de valor pode ser usado para interpretar o valor do objeto.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
- [Interface ICorProfilerInfo2](icorprofilerinfo2-interface.md)
