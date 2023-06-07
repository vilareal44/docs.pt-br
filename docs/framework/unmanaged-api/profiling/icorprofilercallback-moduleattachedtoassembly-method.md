---
title: Método ICorProfilerCallback::ModuleAttachedToAssembly
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.ModuleAttachedToAssembly
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::ModuleAttachedToAssembly
helpviewer_keywords:
- ICorProfilerCallback::ModuleAttachedToAssembly method [.NET Framework profiling]
- ModuleAttachedToAssembly method [.NET Framework profiling]
ms.assetid: b595798a-5d40-4cac-ab4f-911c61d2c5d2
topic_type:
- apiref
ms.openlocfilehash: 4f494919d11e0f979cf1964c08106fbb9b9ed20b
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503387"
---
# <a name="icorprofilercallbackmoduleattachedtoassembly-method"></a>Método ICorProfilerCallback::ModuleAttachedToAssembly
Notifica o criador de perfil de que um módulo está sendo anexado a seu assembly pai.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT ModuleAttachedToAssembly(  
    [in] ModuleID   moduleId,  
    [in] AssemblyID AssemblyId);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `moduleId`  
 no A ID do módulo que está sendo anexado.  
  
 `AssemblyId`  
 no A ID do assembly pai ao qual o módulo está anexado.  
  
## <a name="remarks"></a>Comentários  
 Um módulo pode ser carregado por meio de uma tabela de endereços de importação (IAT), por meio de uma chamada para `LoadLibrary` ou por meio de uma referência de metadados. Como resultado, o carregador de Common Language Runtime (CLR) tem vários caminhos de código para determinar o assembly no qual um módulo reside. Portanto, é possível que, após [ICorProfilerCallback:: ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md) seja chamado, o módulo não saiba em qual assembly ele está e obter a ID do assembly pai não é possível. O `ModuleAttachedToAssembly` método é chamado quando o módulo é anexado a seu assembly pai e sua ID de assembly pai pode ser obtida.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
