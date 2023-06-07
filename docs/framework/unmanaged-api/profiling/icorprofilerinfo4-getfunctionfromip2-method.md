---
title: Método ICorProfilerInfo4::GetFunctionFromIP2
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo4.GetFunctionFromIP2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo4::GetFunctionFromIP2
helpviewer_keywords:
- ICorProfilerInfo4::GetFunctionFromIP2 method [.NET Framework profiling]
- GetFunctionFromIP2 method, ICorProfilerInfo4 interface [.NET Framework profiling]
ms.assetid: 46ff70f4-13e9-40a0-802a-0a40abcfc6a0
topic_type:
- apiref
ms.openlocfilehash: ea66474e809b3813faceef79a69dd8a639a72a3b
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502789"
---
# <a name="icorprofilerinfo4getfunctionfromip2-method"></a>Método ICorProfilerInfo4::GetFunctionFromIP2
Mapeia um ponteiro de instrução de código gerenciado para a versão recompilada do JIT de uma função.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetFunctionFromIP2(  
    [in]  LPCBYTE    ip,  
    [out] FunctionID *pFunctionId,  
    [out] ReJITID *pReJitId);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `ip`  
 no O ponteiro de instrução no código gerenciado.  
  
 `pFunctionId`  
 fora A ID da função.  
  
 `pReJitId`  
 fora A identidade da versão recompilada do JIT da função.  
  
## <a name="remarks"></a>Comentários  
 `GetFunctionFromIP2`é semelhante a `GetFunctionFromIP` , exceto pelo fato de que ele obtém a ID recompilada de JIT em vez da ID de função da função que contém o endereço IP especificado.  
  
> [!NOTE]
> `GetFunctionFromIP2`pode disparar uma coleta de lixo, enquanto `GetFunctionFromIP` não vai.  Para obter mais informações, consulte [CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT](corprof-e-unsupported-call-sequence-hresult.md).  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
