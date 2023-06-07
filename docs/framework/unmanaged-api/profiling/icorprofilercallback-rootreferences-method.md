---
title: Método ICorProfilerCallback::RootReferences
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.RootReferences
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::RootReferences
helpviewer_keywords:
- RootReferences method [.NET Framework profiling]
- ICorProfilerCallback::RootReferences method [.NET Framework profiling]
ms.assetid: dbdf853b-d1a4-4828-8ef7-53d121d8e6ae
topic_type:
- apiref
ms.openlocfilehash: b587f30a01623c6e9806bcd9d01058a0880be239
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499903"
---
# <a name="icorprofilercallbackrootreferences-method"></a>Método ICorProfilerCallback::RootReferences
Notifica o criador de perfil com informações sobre referências raiz após a coleta de lixo.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT RootReferences(  
    [in] ULONG    cRootRefs,  
    [in, size_is(cRootRefs)] ObjectID rootRefIds[] );  
```  
  
## <a name="parameters"></a>Parâmetros  
 `cRootRefs`  
 no O número de referências na `rootRefIds` matriz.  
  
 `rootRefIds`  
 no Uma matriz de IDs de objeto que fazem referência a um objeto estático ou a um objeto na pilha.  
  
## <a name="remarks"></a>Comentários  
 Ambos `RootReferences` e [ICorProfilerCallback2:: RootReferences2](icorprofilercallback2-rootreferences2-method.md) são chamados para notificar o criador de perfil. Os profileres normalmente implementarão um ou outro, mas não ambos, porque as informações transmitidas `RootReferences2` são um superconjunto do passado `RootReferences` .  
  
 É possível que a `rootRefIds` matriz contenha um objeto nulo. Por exemplo, todas as referências de objeto declaradas na pilha são tratadas como raízes pelo coletor de lixo e sempre serão relatadas.  
  
 As IDs de objeto retornadas por `RootReferences` não são válidas durante o próprio retorno de chamada, pois a coleta de lixo pode estar no meio da movimentação de objetos de endereços antigos para novos endereços. Portanto, os profileres não devem tentar inspecionar objetos durante uma `RootReferences` chamada. Quando [ICorProfilerCallback2:: GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md) é chamado, todos os objetos foram movidos para seus novos locais e podem ser inspecionados com segurança.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
