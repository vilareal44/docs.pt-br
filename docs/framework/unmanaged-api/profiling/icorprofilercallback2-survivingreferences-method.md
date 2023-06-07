---
title: Método ICorProfilerCallback2::SurvivingReferences
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback2.SurvivingReferences
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback2::SurvivingReferences
helpviewer_keywords:
- ICorProfilerCallback2::SurvivingReferences method [.NET Framework profiling]
- SurvivingReferences method [.NET Framework profiling]
ms.assetid: f165200e-3a91-47f7-88fc-13ff10c8babc
topic_type:
- apiref
ms.openlocfilehash: 3681106bca94f1fefb2f24a1aa4254eb2b1b0531
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499734"
---
# <a name="icorprofilercallback2survivingreferences-method"></a>Método ICorProfilerCallback2::SurvivingReferences
Relata o layout dos objetos no heap como resultado de uma coleta de lixo sem compactação.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SurvivingReferences(  
    [in] ULONG  cSurvivingObjectIDRanges,  
    [in, size_is(cSurvivingObjectIDRanges)] ObjectID  
                objectIDRangeStart[] ,  
    [in, size_is(cSurvivingObjectIDRanges)] ULONG  
                cObjectIDRangeLength[] );  
```  
  
## <a name="parameters"></a>Parâmetros  
 `cSurvivingObjectIDRanges`  
 no O número de blocos de objetos contíguos que sobreviveram como resultado da coleta de lixo sem compactação. Ou seja, o valor de `cSurvivingObjectIDRanges` é o tamanho das `objectIDRangeStart` `cObjectIDRangeLength` matrizes e, que armazena um `ObjectID` e um comprimento, respectivamente, para cada bloco de objetos.  
  
 Os dois próximos argumentos de `SurvivingReferences` são matrizes paralelas. Em outras palavras, `objectIDRangeStart` e `cObjectIDRangeLength` preocupação com o mesmo bloco de objetos contíguos.  
  
 `objectIDRangeStart`  
 no Uma matriz de `ObjectID` valores, cada um dos quais é o endereço inicial de um bloco de objetos dinâmicos contíguos na memória.  
  
 `cObjectIDRangeLength`  
 no Uma matriz de inteiros, cada qual é o tamanho de um bloco sobrevivente de objetos contíguos na memória.  
  
 Um tamanho é especificado para cada bloco que é referenciado na `objectIDRangeStart` matriz.  
  
## <a name="remarks"></a>Comentários  
  
> [!IMPORTANT]
> Esse método relata tamanhos `MAX_ULONG` para objetos que são maiores que 4 GB em plataformas de 64 bits. Para objetos com mais de 4 GB, use o método [ICorProfilerCallback4:: SurvivingReferences2](icorprofilercallback4-survivingreferences2-method.md) em vez disso.  
  
 Os elementos das `objectIDRangeStart` `cObjectIDRangeLength` matrizes e devem ser interpretados da seguinte maneira para determinar se um objeto sobreviveram a coleta de lixo. Suponha que um `ObjectID` valor ( `ObjectID` ) esteja dentro do seguinte intervalo:  
  
 `ObjectIDRangeStart[i]` <= `ObjectID` < `ObjectIDRangeStart[i]` + `cObjectIDRangeLength[i]`  
  
 Para qualquer valor de `i` que esteja no seguinte intervalo, o objeto tem sobreviveram a coleta de lixo:  
  
 0 <=`i` < `cSurvivingObjectIDRanges`  
  
 Uma coleta de lixo não compactada recupera a memória ocupada por objetos "inativos", mas não compacta esse espaço livre. Como resultado, a memória é retornada para o heap, mas nenhum objeto "ao vivo" é movido.  
  
 As chamadas de Common Language Runtime (CLR) `SurvivingReferences` para coletas de lixo não compactadas. Para compactar as coleções de lixo, [ICorProfilerCallback:: MovedReferences](icorprofilercallback-movedreferences-method.md) é chamado em vez disso. Uma única coleta de lixo pode ser compactada para uma geração e não compactação para outra. Para uma coleta de lixo em qualquer geração específica, o criador de perfil receberá um retorno de chamada `SurvivingReferences` ou um `MovedReferences` retorno de chamada, mas não ambos.  
  
 Vários `SurvivingReferences` retornos de chamada podem ser recebidos durante uma determinada coleta de lixo, devido ao buffer interno limitado, a vários threads relatando no caso da coleta de lixo do servidor e por outros motivos. No caso de vários retornos de chamada durante uma coleta de lixo, as informações são cumulativas – todas as referências que são relatadas em qualquer `SurvivingReferences` retorno de chamada sobrevivem à coleta de lixo.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
- [Interface ICorProfilerCallback2](icorprofilercallback2-interface.md)
- [Método SurvivingReferences2](icorprofilercallback4-survivingreferences2-method.md)
