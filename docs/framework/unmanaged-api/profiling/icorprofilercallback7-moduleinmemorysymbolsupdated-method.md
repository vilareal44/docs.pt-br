---
title: 'Método ICorProfilerCallback7:: ModuleInMemorySymbolsUpdated'
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback7.ModuleInMemorySymbolsUpdated
api_location:
- mscorwks.dll
- corprof.idl
api_type:
- COM
ms.assetid: f362a896-3247-4894-9727-e48dbbcd2c78
ms.openlocfilehash: c7e53816c2f571fe6ff68b517ed827459a0f1562
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499084"
---
# <a name="icorprofilercallback7moduleinmemorysymbolsupdated-method"></a>Método ICorProfilerCallback7:: ModuleInMemorySymbolsUpdated
[Com suporte no .NET Framework 4.6.1 e versões posteriores]  
  
 Notifica o criador de perfil sempre que o fluxo de símbolos associado a um módulo na memória é atualizado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT ModuleInMemorySymbolsUpdated(  
     ModuleID moduleId  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 [in] `moduleId`  
 O identificador do módulo na memória cujo fluxo de símbolos é atualizado.  
  
## <a name="remarks"></a>Comentários  
 Esse retorno de chamada é controlado pela definição do sinalizador de máscara de evento [COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED](cor-prf-high-monitor-enumeration.md) ao chamar o método [ICorProfilerCallback5:: SetEventMask2](icorprofilerinfo5-seteventmask2-method.md) .  
  
> [!NOTE]
> Esse evento não é gerado atualmente para símbolos criados implicitamente ou modificados via <xref:System.Reflection.Emit> APIs.  
  
 Mesmo quando os símbolos são fornecidos antecipadamente em uma chamada para uma das sobrecargas dos métodos gerenciados <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> que incluem um `rawSymbolStore` argumento para especificar os símbolos para o assembly, o tempo de execução pode não associar realmente os dados simbólicos ao módulo até que o retorno de chamada [ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md) tenha ocorrido. Esse evento fornece uma oportunidade posterior para coletar símbolos para esses módulos.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v461plus](../../../../includes/net-current-v461plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Método ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md)
- [Método SetEventMask2](icorprofilerinfo5-seteventmask2-method.md)
- [Interface ICorProfilerCallback7](icorprofilercallback7-interface.md)
