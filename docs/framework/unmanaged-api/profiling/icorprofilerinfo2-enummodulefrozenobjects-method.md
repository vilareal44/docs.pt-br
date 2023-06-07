---
title: Método ICorProfilerInfo2::EnumModuleFrozenObjects
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.EnumModuleFrozenObjects
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::EnumModuleFrozenObjects
helpviewer_keywords:
- EnumModuleFrozenObjects method [.NET Framework profiling]
- ICorProfilerInfo2::EnumModuleFrozenObjects method [.NET Framework profiling]
ms.assetid: 920b6483-7064-4d64-8613-fcc38ccf9b1e
topic_type:
- apiref
ms.openlocfilehash: 1fe44f8f84c079e920c8c82fb9d52d1980d3b852
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84497199"
---
# <a name="icorprofilerinfo2enummodulefrozenobjects-method"></a>Método ICorProfilerInfo2::EnumModuleFrozenObjects
Obtém um enumerador que permite a iteração sobre os objetos congelados no módulo especificado. Este método é obsoleto.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT EnumModuleFrozenObjects(  
    [in] ModuleID moduleID,  
    [out] ICorProfilerObjectEnum** ppEnum);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `moduleID`  
 no A ID do módulo que contém os objetos congelados a serem enumerados.  
  
 `ppEnum`  
 fora Um ponteiro para o endereço de uma interface [ICorProfilerObjectEnum](icorprofilerobjectenum-interface.md) , que enumera os objetos congelados.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:** 3,5, 3,0 sp1, 3,0, 2,0 SP1, 2,0  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
- [Interface ICorProfilerInfo2](icorprofilerinfo2-interface.md)
