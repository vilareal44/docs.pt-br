---
title: Método ICorProfilerInfo::IsArrayClass
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.IsArrayClass
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::IsArrayClass
helpviewer_keywords:
- IsArrayClass method [.NET Framework profiling]
- ICorProfilerInfo::IsArrayClass method [.NET Framework profiling]
ms.assetid: 7f230961-23a6-4d56-ad2d-7a876d65705f
topic_type:
- apiref
ms.openlocfilehash: 2a3f5bb0c54935e524cc955a5e11aac75b0c0923
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84497550"
---
# <a name="icorprofilerinfoisarrayclass-method"></a>Método ICorProfilerInfo::IsArrayClass
Determina se a classe especificada é uma classe de matriz.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT IsArrayClass(  
    [in]  ClassID        classId,  
    [out] CorElementType *pBaseElemType,  
    [out] ClassID        *pBaseClassId,  
    [out] ULONG          *pcRank);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `classId`  
 no A ID da classe a ser examinada.  
  
 `pBaseElemType`  
 fora Um ponteiro para um valor da enumeração CorElementType que indica o tipo dos elementos da matriz.  
  
 `pBaseClassId`  
 fora Um ponteiro para a ID de classe dos elementos da matriz, quando disponível.  
  
 `pcRank`  
 fora Um ponteiro para um inteiro que indica a classificação (ou seja, o número de dimensões) da matriz.  
  
## <a name="remarks"></a>Comentários  
 Se a classe especificada for uma classe de matriz, o `IsArrayClass` método retornará um S_OK HRESULT e valores para quaisquer parâmetros de saída não nulos. Caso contrário, ele retornará S_FALSE.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
