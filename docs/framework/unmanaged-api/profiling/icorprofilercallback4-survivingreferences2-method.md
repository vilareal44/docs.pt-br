---
title: Método ICorProfilerCallback4::SurvivingReferences2
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback4.SurvivingReferences2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback4::SurvivingReferences2
helpviewer_keywords:
- ICorProfilerCallback4::SurvivingReferences2 method [.NET Framework profiling]
- SurvivingReferences2 method, ICorProfilerCallback4 interface [.NET Framework profiling]
ms.assetid: 02b51888-5d89-4e50-a915-45b7e329aad9
topic_type:
- apiref
ms.openlocfilehash: 208ce1d7ef8a1eab4f18a6d488f0cc480b5713d8
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499331"
---
# <a name="icorprofilercallback4survivingreferences2-method"></a>Método ICorProfilerCallback4::SurvivingReferences2
Relata o layout dos objetos no heap como resultado de uma coleta de lixo sem compactação. Esse método será chamado se o criador de perfil tiver implementado a interface [ICorProfilerCallback4](icorprofilercallback4-interface.md) . Esse retorno de chamada substitui o método [ICorProfilerCallback2:: SurvivingReferences](icorprofilercallback2-survivingreferences-method.md) , pois ele pode relatar intervalos maiores de objetos cujos comprimentos excedem o que pode ser expresso em ULONG.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SurvivingReferences2(  
    [in] ULONG  cSurvivingObjectIDRanges,  
    [in, size_is(cSurvivingObjectIDRanges)] ObjectID  
                objectIDRangeStart[] ,  
    [in, size_is(cSurvivingObjectIDRanges)] SIZE_T  
                cObjectIDRangeLength[] );  
```  
  
## <a name="parameters"></a>Parâmetros  
 `cSurvivingObjectIDRanges`  
 no O número de blocos de objetos contíguos que sobreviveram como resultado da coleta de lixo sem compactação. Ou seja, o valor de `cSurvivingObjectIDRanges` é o tamanho das `objectIDRangeStart` `cObjectIDRangeLength` matrizes e, que armazena um `ObjectID` e um comprimento, respectivamente, para cada bloco de objetos.  
  
 Os dois próximos argumentos de `SurvivingReferences2` são matrizes paralelas. Em outras palavras, `objectIDRangeStart` e `cObjectIDRangeLength` preocupação com o mesmo bloco de objetos contíguos.  
  
 `objectIDRangeStart`  
 no Uma matriz de `ObjectID` valores, cada um dos quais é o endereço inicial de um bloco de objetos dinâmicos contíguos na memória.  
  
 `cObjectIDRangeLength`  
 no Uma matriz de inteiros, cada qual é o tamanho de um bloco sobrevivente de objetos contíguos na memória.  
  
 Um tamanho é especificado para cada bloco que é referenciado na `objectIDRangeStart` matriz.  
  
## <a name="remarks"></a>Comentários  
 Os elementos das `objectIDRangeStart` `cObjectIDRangeLength` matrizes e devem ser interpretados da seguinte maneira para determinar se um objeto sobreviveram a coleta de lixo. Suponha que um `ObjectID` valor ( `ObjectID` ) esteja dentro do seguinte intervalo:  
  
 `ObjectIDRangeStart[i]` <= `ObjectID` < `ObjectIDRangeStart[i]` + `cObjectIDRangeLength[i]`  
  
 Para qualquer valor de `i` que esteja no seguinte intervalo, o objeto tem sobreviveram a coleta de lixo:  
  
 0 <=`i` < `cSurvivingObjectIDRanges`  
  
 Uma coleta de lixo não compactada recupera a memória ocupada por objetos "inativos", mas não compacta esse espaço livre. Como resultado, a memória é retornada para o heap, mas nenhum objeto "ao vivo" é movido.  
  
 As chamadas de Common Language Runtime (CLR) `SurvivingReferences2` para coletas de lixo não compactadas. Para compactar as coleções de lixo, [MovedReferences2](icorprofilercallback4-movedreferences2-method.md) é chamado em vez disso. Uma única coleta de lixo pode ser compactada para uma geração e não compactação para outra. Para uma coleta de lixo em qualquer geração específica, o criador de perfil receberá um retorno de chamada `SurvivingReferences2` ou um retorno de chamada [MovedReferences2](icorprofilercallback4-movedreferences2-method.md) , mas não ambos.  
  
 Vários `SurvivingReferences2` retornos de chamada podem ser recebidos durante uma determinada coleta de lixo, devido ao buffer interno limitado, a vários retornos de chamada durante a coleta de lixo do servidor e por outros motivos. No caso de vários retornos de chamada durante uma coleta de lixo, as informações são cumulativas; todas as referências que são relatadas em qualquer `SurvivingReferences2` retorno de chamada sobrevivem à coleta de lixo.  
  
 Se o criador de perfil implementar as interfaces [ICorProfilerCallback](icorprofilercallback-interface.md) e [ICorProfilerCallback4](icorprofilercallback4-interface.md) , o `SurvivingReferences2` método será chamado antes do método [ICorProfilerCallback2:: SurvivingReferences](icorprofilercallback2-survivingreferences-method.md) , mas somente se `SurvivingReferences2` retornar com êxito. Os profileres podem retornar um HRESULT que indica falha do `SurvivingReferences2` método para evitar chamar o segundo método.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
- [Interface ICorProfilerCallback2](icorprofilercallback2-interface.md)
- [Interface ICorProfilerCallback4](icorprofilercallback4-interface.md)
