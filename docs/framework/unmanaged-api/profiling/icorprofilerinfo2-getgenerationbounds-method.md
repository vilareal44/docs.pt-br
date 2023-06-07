---
title: Método ICorProfilerInfo2::GetGenerationBounds
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetGenerationBounds
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetGenerationBounds
helpviewer_keywords:
- ICorProfilerInfo2::GetGenerationBounds method [.NET Framework profiling]
- GetGenerationBounds method [.NET Framework profiling]
ms.assetid: 9c37185f-d1e0-4a6e-8b99-707f7df61d88
topic_type:
- apiref
ms.openlocfilehash: 2e6e3a6432d6568532a5f5b9676b5f130eb83d0b
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502880"
---
# <a name="icorprofilerinfo2getgenerationbounds-method"></a>Método ICorProfilerInfo2::GetGenerationBounds
Obtém as regiões de memória, que são segmentos do heap, que compõem as várias gerações de coleta de lixo.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetGenerationBounds(  
    [in]  ULONG cObjectRanges,  
    [out] ULONG *pcObjectRanges,  
    [out, size_is(cObjectRanges), length_is(*pcObjectRanges)] COR_PRF_GC_GENERATION_RANGE ranges[]);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `cObjectRanges`  
 no O número de elementos alocados pelo chamador para a `ranges` matriz.  
  
 `pcObjectRanges`  
 fora Um ponteiro para um inteiro que especifica o número total de intervalos, alguns ou todos os quais serão retornados na `ranges` matriz.  
  
 `ranges`  
 fora Uma matriz de estruturas [COR_PRF_GC_GENERATION_RANGE](cor-prf-gc-generation-range-structure.md) , cada uma delas descreve um intervalo (ou seja, bloco) de memória na geração que está passando pela coleta de lixo.  
  
## <a name="remarks"></a>Comentários  
 O `GetGenerationBounds` método pode ser chamado de qualquer retorno de chamada do criador de perfil, desde que a coleta de lixo não esteja em andamento.

 A maior mudança das gerações ocorre durante as coletas de lixo. As gerações podem crescer entre as coleções, mas geralmente não se movimentam. Portanto, os lugares mais interessantes a serem chamados `GetGenerationBounds` são no `ICorProfilerCallback2::GarbageCollectionStarted` e no `ICorProfilerCallback2::GarbageCollectionFinished` .  
  
 Durante a inicialização do programa, alguns objetos são alocados pelo Common Language Runtime (CLR) em si, geralmente nas gerações 3 e 0. Portanto, quando o código gerenciado de tempo começa a ser executado, essas gerações já conterão objetos. As gerações 1 e 2 normalmente estarão vazias, exceto objetos fictícios gerados pelo coletor de lixo. (O tamanho dos objetos fictícios é de 12 bytes em implementações de 32 bits do CLR; o tamanho é maior em implementações de 64 bits.) Você também pode ver intervalos de geração 2 que estão dentro de módulos produzidos pelo gerador de imagem nativa (NGen. exe). Nesse caso, os objetos na geração 2 são *objetos congelados*, que são alocados quando NGen. exe é executado em vez de pelo coletor de lixo.  
  
 Essa função usa buffers alocados pelo chamador.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
- [Interface ICorProfilerInfo2](icorprofilerinfo2-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Criação de perfil](index.md)
