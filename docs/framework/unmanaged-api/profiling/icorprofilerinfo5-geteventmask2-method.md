---
title: ICorProfilerInfo5::Método GetEventMask2
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- ICorProfilerInfo5.GetEventMask2
api_location:
- mscorwks.dll
api_type:
- COM
ms.assetid: f854b68f-009c-4ffb-89cd-ca874d1c0fb7
topic_type:
- apiref
ms.openlocfilehash: 758e5b71443b127c80c820eb8531056530e81b13
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84495691"
---
# <a name="icorprofilerinfo5geteventmask2-method"></a>ICorProfilerInfo5::Método GetEventMask2
[Com suporte no .NET Framework 4.5.2 e versões posteriores]  
  
 Obtém as categorias de eventos atuais para as quais o criador de perfil deseja receber notificações para o Common Language Runtime (CLR).  Ele fornece funcionalidade não fornecida pelo método [ICorProfilerInfo:: GetEventMask](icorprofilerinfo-geteventmask-method.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp
HRESULT GetEventMask2(  
        [out] DWORD* pdwEventsLow,  
        [out] DWORD* pdwEventsHigh  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pdwEventsLow`  
 [out] Um ponteiro para um valor de 4 bytes que especifica as categorias de eventos. Cada bit controla uma capacidade, um comportamento ou um tipo de evento diferente. Os bits são descritos na enumeração [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md) .  
  
 `pdwEventsHigh`  
 [out] Um ponteiro para um valor de 4 bytes que especifica as categorias de eventos.  Cada bit controla uma capacidade, um comportamento ou um tipo de evento diferente. Os bits são descritos na enumeração [COR_PRF_HIGH_MONITOR](cor-prf-high-monitor-enumeration.md) .  
  
## <a name="remarks"></a>Comentários  
 O método `GetEventMask2` é usado para determinar para quais retornos de chamada o criador de perfil fez assinatura. Normalmente, você executa um OR lógico dos `pdwEventsLow` valores e `pdwEventsHigh` e quaisquer novos bits que deseja definir e, em seguida, chama o método [SetEventMask2](icorprofilerinfo5-seteventmask2-method.md) .  
  
 Esse método é a alternativa recomendada para o método [GetEventMask](icorprofilerinfo-geteventmask-method.md) .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo5](icorprofilerinfo5-interface.md)
- [Método SetEventMask2](icorprofilerinfo5-seteventmask2-method.md)
