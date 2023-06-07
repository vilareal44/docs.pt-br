---
title: Método ICorProfilerInfo::GetFunctionInfo
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetFunctionInfo
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::GetFunctionInfo
helpviewer_keywords:
- ICorProfilerInfo::GetFunctionInfo method [.NET Framework profiling]
- GetFunctionInfo method [.NET Framework profiling]
ms.assetid: c42b5891-019d-46b3-b551-4606295b75b8
topic_type:
- apiref
ms.openlocfilehash: e7193526bb0da1d28da4bf6bde108fc4d3fba273
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503010"
---
# <a name="icorprofilerinfogetfunctioninfo-method"></a>Método ICorProfilerInfo::GetFunctionInfo
Obtém a classe pai e o token de metadados para a função especificada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetFunctionInfo(  
    [in]  FunctionID functionId,  
    [out] ClassID    *pClassId,  
    [out] ModuleID   *pModuleId,  
    [out] mdToken    *pToken);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `functionId`  
 no A ID da função para a qual obter a classe pai e o token de metadados.  
  
 `pClassId`  
 fora Um ponteiro para a classe pai da função.  
  
 `pModuleId`  
 fora Um ponteiro para o módulo no qual a classe pai da função é definida.  
  
 `pToken`  
 fora Um ponteiro para o token de metadados para a função.  
  
## <a name="remarks"></a>Comentários  
 O código do criador de perfil pode chamar [ICorProfilerInfo:: GetModuleMetaData](icorprofilerinfo-getmodulemetadata-method.md) para obter uma interface de metadados para um determinado módulo. O token de metadados que é retornado para o local referenciado por `pToken` pode ser usado para acessar os metadados para a função.  
  
 `ClassID`Talvez não seja possível obter o de uma função em uma classe genérica sem mais informações contextuais sobre o uso da função. Nesse caso, `pClassId` será 0. O código do criador de perfil deve usar [ICorProfilerInfo2:: GetFunctionInfo2](icorprofilerinfo2-getfunctioninfo2-method.md) com um valor COR_PRF_FRAME_INFO para fornecer mais contexto.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
