---
title: Interface ICorProfilerInfo7
ms.date: 03/30/2017
ms.assetid: cf37c462-73c5-412a-a7f8-bb26ca746313
ms.openlocfilehash: 0e9f76717aeff27e863245faac241927e7495076
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84495483"
---
# <a name="icorprofilerinfo7-interface"></a>Interface ICorProfilerInfo7
[Com suporte no .NET Framework 4.6.1 e versões posteriores]  
  
 Uma subclasse de [ICorProfilerInfo6](icorprofilerinfo6-interface.md) que fornece um método para aplicar metadados definidos recentemente a um módulo e que fornece acesso a um fluxo de símbolo na memória.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método ApplyMetaData](icorprofilerinfo7-applymetadata-method.md)|Aplica os metadados recentemente definidos pelos `IMetadataEmit::Define*` métodos a um módulo especificado.|  
|[Método GetInMemorySymbolsLength](icorprofilerinfo7-getinmemorysymbolslength-method.md)|Retorna o comprimento de um fluxo de símbolo na memória.|  
|[ReadInMemorySymbols](icorprofilerinfo7-readinmemorysymbols.md)|Lê bytes de um fluxo de símbolo na memória.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v461plus](../../../../includes/net-current-v461plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Criação de perfil de interfaces](profiling-interfaces.md)
