---
title: Interface ICorProfilerInfo5
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo5
api_location:
- mscorwks.dll
api_type:
- COM
ms.assetid: 7bd48c34-37ed-4230-9eec-39a17280f05d
topic_type:
- apiref
ms.openlocfilehash: 82f6262c2c576b49be4e7fcaa14043950df4c67a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84495621"
---
# <a name="icorprofilerinfo5-interface"></a>Interface ICorProfilerInfo5
[Com suporte no .NET Framework 4.5.2 e versões posteriores]  
  
 Uma subclasse de [ICorProfilerInfo4](icorprofilerinfo4-interface.md) que fornece métodos para uso por infilers de código para se comunicar com o Common Language Runtime (CLR) para controlar o monitoramento de eventos.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método GetEventMask2](icorprofilerinfo5-geteventmask2-method.md)|Obtém as categorias de eventos atuais para os quais o criador de perfis deseja receber notificações do CLR.|  
|[Método SetEventMask2](icorprofilerinfo5-seteventmask2-method.md)|Define um valor que especifica os tipos de eventos para os quais o criador de perfil deseja receber notificações de evento do CLR.|  
  
## <a name="remarks"></a>Comentários  
 Os métodos disponíveis nesta interface destinam-se a substituir os métodos [ICorProfilerInfo:: GetEventMask](icorprofilerinfo-geteventmask-method.md) e [ICorProfilerInfo:: SetEventMask](icorprofilerinfo-seteventmask-method.md) .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Criação de perfil de interfaces](profiling-interfaces.md)
