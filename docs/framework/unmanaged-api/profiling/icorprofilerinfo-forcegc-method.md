---
title: Método ICorProfilerInfo::ForceGC
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.ForceGC
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::ForceGC
helpviewer_keywords:
- ICorProfilerInfo::ForceGC method [.NET Framework profiling]
- ForceGC method [.NET Framework profiling]
ms.assetid: 0da1ef80-d242-4636-87d0-43e0470b342a
topic_type:
- apiref
ms.openlocfilehash: 9bc6619f3ef383c7bf60a310a87f056cfc43cddf
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84498668"
---
# <a name="icorprofilerinfoforcegc-method"></a>Método ICorProfilerInfo::ForceGC
Força a coleta de lixo a ocorrer dentro do Common Language Runtime (CLR).  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT ForceGC();  
```  
  
## <a name="remarks"></a>Comentários  
 O `ForceGC` método deve ser chamado somente de um thread que nunca tenha executado código gerenciado e não tenha nenhum retorno de chamada do criador de perfil em sua pilha. A implementação mais conveniente é criar um thread separado dentro do criador de perfil que chama `ForceGC` quando sinalizado.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
