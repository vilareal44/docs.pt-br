---
title: Método ICorProfilerInfo::SetILFunctionBody
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.SetILFunctionBody
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::SetILFunctionBody
helpviewer_keywords:
- ICorProfilerInfo::SetILFunctionBody method [.NET Framework profiling]
- SetILFunctionBody method [.NET Framework profiling]
ms.assetid: b159c712-00f4-4fc7-a990-40bf9f642e8f
topic_type:
- apiref
ms.openlocfilehash: 462fc7222243f8cad4e1d03d1717eedace549836
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502932"
---
# <a name="icorprofilerinfosetilfunctionbody-method"></a>Método ICorProfilerInfo::SetILFunctionBody
Substitui o corpo da função especificada no módulo especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetILFunctionBody(  
    [in] ModuleID    moduleId,  
    [in] mdMethodDef methodid,  
    [in] LPCBYTE     pbNewILMethodHeader);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `moduleId`  
 no A ID do módulo no qual a função reside.  
  
 `methodid`  
 no O token da função para a qual substituir o corpo.  
  
 `pbNewILMethodHeader`  
 no O novo cabeçalho da função.  
  
## <a name="remarks"></a>Comentários  
 O `SetILFunctionBody` método substitui o endereço virtual relativo da função nos metadados para que ele aponte para o novo corpo da função e ajusta todas as estruturas de dados internas conforme necessário.  
  
 O `SetILFunctionBody` método pode ser chamado somente nas funções que nunca foram compiladas por um compilador JIT (just-in-time).  
  
 Use o método [ICorProfilerInfo:: GetILFunctionBodyAllocator](icorprofilerinfo-getilfunctionbodyallocator-method.md) para alocar espaço para o novo método a fim de garantir que o buffer seja compatível.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
