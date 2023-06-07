---
title: Interface ICorProfilerInfo4
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo4
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo4
helpviewer_keywords:
- ICorProfilerInfo4 interface [.NET Framework profiling]
ms.assetid: 80a5308e-c22f-4201-ba89-31cc8562515b
topic_type:
- apiref
ms.openlocfilehash: 58d11e9084f53c69f2656b4f0ee6bc7d2cc4ae21
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84495860"
---
# <a name="icorprofilerinfo4-interface"></a>Interface ICorProfilerInfo4
Fornece métodos que os profileres de código usam para se comunicar com o Common Language Runtime (CLR) para controlar o monitoramento de eventos e informações de solicitação. . A `ICorProfilerInfo4` interface é uma extensão das outras `ICorProfilerInfo` interfaces. Ele fornece novos métodos para dar suporte à recompilação JIT (just-in-time), adicionada ao .NET Framework 4,5.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método EnumJITedFunctions2](icorprofilerinfo4-enumjitedfunctions2-method.md)|Retorna um enumerador para todas as funções que foram previamente compiladas e recompiladas por JIT.|  
|[Método EnumThreads](icorprofilerinfo4-enumthreads-method.md)|Obtém um enumerador que fornece métodos para iterar sequencialmente através da coleção de todos os threads gerenciados no processo de perfil.|  
|[Método GetCodeInfo3](icorprofilerinfo4-getcodeinfo3-method.md)|Obtém as extensões do código nativo associado à versão recompilada do JIT da função especificada.|  
|[Método GetFunctionFromIP2](icorprofilerinfo4-getfunctionfromip2-method.md)|Mapeia um ponteiro de instrução de código gerenciado para a versão recompilada do JIT de uma função especificada.|  
|[Método GetILToNativeMapping2](icorprofilerinfo4-getiltonativemapping2-method.md)|Obtém um mapa de deslocamentos de MSIL (Microsoft Intermediate Language) para deslocamentos nativos do código contido na versão recompilada do JIT da função especificada.|  
|[Método GetObjectSize2](icorprofilerinfo4-getobjectsize2-method.md)|Retorna o tamanho de um objeto especificado.|  
|[Método GetReJITIDs](icorprofilerinfo4-getrejitids-method.md)|Retorna uma matriz de IDs que identifica todas as versões recompiladas por JIT da função especificada que ainda estão alocadas.|  
|[Método InitializeCurrentThread](icorprofilerinfo4-initializecurrentthread-method.md)|Inicializa o thread atual antes das chamadas de API do criador de perfil subsequentes no mesmo thread, para que o deadlock possa ser evitado.|  
|[Método RequestReJIT](icorprofilerinfo4-requestrejit-method.md)|Solicita uma recompilação JIT de todas as instâncias das funções especificadas.|  
|[Método RequestRevert](icorprofilerinfo4-requestrevert-method.md)|Reverte todas as instâncias das funções especificadas para suas versões originais.|  
  
## <a name="remarks"></a>Comentários  
 O CLR implementa os métodos da `ICorProfilerInfo4` interface usando o modelo de thread livre. Cada método retorna um HRESULT para indicar êxito ou falha. Para obter uma lista de possíveis códigos de retorno, consulte o arquivo CorError. h.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
