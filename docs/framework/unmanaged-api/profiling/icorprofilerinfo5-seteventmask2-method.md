---
title: ICorProfilerInfo5::Método SetEventMask2
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- IcorProfilerInfo5.SetEventMask2
api_location:
- mscorwks.dll
api_type:
- COM
ms.assetid: 05dbbe2b-049c-4a60-be69-2ad7a949405e
topic_type:
- apiref
ms.openlocfilehash: 8027cdcde8281c363207e309bf65fcd90c03b626
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84495613"
---
# <a name="icorprofilerinfo5seteventmask2-method"></a>ICorProfilerInfo5::Método SetEventMask2
[Com suporte no .NET Framework 4.5.2 e versões posteriores]  
  
 Define um valor que especifica os tipos de eventos para os quais o criador de perfil deseja receber notificações de evento do CLR (Common Language runtime). Ele fornece mais funcionalidade do que o método [ICorProfilerInfo:: SetEventMask](icorprofilerinfo-seteventmask-method.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp
HRESULT SetEventMask2(        [in] DWORD dwEventsLow,        [in] DWORD dwEventsHigh  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `dwEventsLow`  
 [in] Um valor de 4 bytes que especifica as categorias de eventos. Cada bit controla uma capacidade, um comportamento ou um tipo de evento diferente. Os bits são descritos na enumeração [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md) .  
  
 `dwEventsHigh`  
 [in] Um valor de 4 bytes que especifica as categorias de eventos.  Cada bit controla uma capacidade, um comportamento ou um tipo de evento diferente. Os bits são descritos na enumeração [COR_PRF_HIGH_MONITOR](cor-prf-high-monitor-enumeration.md) .  
  
## <a name="remarks"></a>Comentários  
 O método `SetEventMask2` é usado para definir os retornos de chamada para os quais o criador de perfil se inscreve. Normalmente, você chama o método [GetEventMask2](icorprofilerinfo5-geteventmask2-method.md) para determinar quais bits estão definidos, executar uma lógica ou seus `pdwEventsLow` valores e `pdwEventsHigh` quaisquer novos bits que deseja definir e, em seguida, chamar o `SetEventMask2` método.  
  
 Esse método é a alternativa recomendada para o método [SetEventMask](icorprofilerinfo-seteventmask-method.md) .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo5](icorprofilerinfo5-interface.md)
- [Método GetEventMask2](icorprofilerinfo5-geteventmask2-method.md)
