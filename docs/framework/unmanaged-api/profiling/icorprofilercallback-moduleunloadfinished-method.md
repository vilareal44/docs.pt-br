---
title: Método ICorProfilerCallback::ModuleUnloadFinished
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.ModuleUnloadFinished
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::ModuleUnloadFinished
helpviewer_keywords:
- ModuleUnloadFinished method [.NET Framework profiling]
- ICorProfilerCallback::ModuleUnloadFinished method [.NET Framework profiling]
ms.assetid: 185e3327-9f9c-44bc-8a5c-febea9a6bb5b
topic_type:
- apiref
ms.openlocfilehash: fd35f47c004d1ffb235cefe1cd2a1eb2c1fffaef
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503309"
---
# <a name="icorprofilercallbackmoduleunloadfinished-method"></a>Método ICorProfilerCallback::ModuleUnloadFinished
Notifica o criador de perfil de que um módulo concluiu o descarregamento.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT ModuleUnloadFinished(  
    [in] ModuleID moduleId,  
    [in] HRESULT  hrStatus);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `moduleId`  
 no A ID do módulo que foi descarregado.  
  
 `hrStatus`  
 no Um HRESULT que indica se o módulo foi descarregado com êxito.  
  
## <a name="remarks"></a>Comentários  
 O valor de `moduleId` não é válido para uma solicitação de informações depois que o método [ICorProfilerCallback:: ModuleUnloadStarted](icorprofilercallback-moduleunloadstarted-method.md) retorna.  
  
 Algumas partes do descarregamento da classe podem continuar após o `ModuleUnloadFinished` retorno de chamada. Um HRESULT de falha em `hrStatus` indica uma falha. No entanto, um HRESULT de êxito em `hrStatus` indica apenas que a primeira parte do descarregamento do módulo foi bem-sucedida.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
