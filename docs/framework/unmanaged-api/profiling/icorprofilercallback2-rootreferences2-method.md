---
title: Método ICorProfilerCallback2::RootReferences2
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback2.RootReferences2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback2::RootReferences2
helpviewer_keywords:
- RootReferences2 method [.NET Framework profiling]
- ICorProfilerCallback2::RootReferences2 method [.NET Framework profiling]
ms.assetid: 55a2f907-d216-42eb-8f2f-e5d59c2eebd6
topic_type:
- apiref
ms.openlocfilehash: 2ce58113f40c8eb67a89b6ab6c9bb8f755975bd5
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499747"
---
# <a name="icorprofilercallback2rootreferences2-method"></a>Método ICorProfilerCallback2::RootReferences2
Notifica o criador de perfil sobre referências de raiz após a ocorrência de uma coleta de lixo. Esse método é uma extensão do método [ICorProfilerCallback:: RootReferences](icorprofilercallback-rootreferences-method.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT RootReferences2(  
    [in] ULONG  cRootRefs,  
    [in, size_is(cRootRefs)] ObjectID rootRefIds[],  
    [in, size_is(cRootRefs)] COR_PRF_GC_ROOT_KIND rootKinds[],  
    [in, size_is(cRootRefs)] COR_PRF_GC_ROOT_FLAGS rootFlags[],  
    [in, size_is(cRootRefs)] UINT_PTR rootIds[]);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `cRootRefs`  
 no O número de elementos nas `rootRefIds` `rootKinds` `rootFlags` matrizes,, e `rootIds` .  
  
 `rootRefIds`  
 no Uma matriz de IDs de objeto, cada uma das quais faz referência a um objeto estático ou a um objeto na pilha. Os elementos na `rootKinds` matriz fornecem informações para classificar os elementos correspondentes na `rootRefIds` matriz.  
  
 `rootKinds`  
 no Uma matriz de valores [COR_PRF_GC_ROOT_KIND](cor-prf-gc-root-kind-enumeration.md) que indicam o tipo da raiz da coleta de lixo.  
  
 `rootFlags`  
 no Uma matriz de valores [COR_PRF_GC_ROOT_FLAGS](cor-prf-gc-root-flags-enumeration.md) que descrevem as propriedades de uma raiz de coleta de lixo.  
  
 `rootIds`  
 no Uma matriz de valores UINT_PTR que apontam para um inteiro que contém informações adicionais sobre a raiz da coleta de lixo, dependendo do valor do `rootKinds` parâmetro.  
  
 Se o tipo da raiz for uma pilha, a ID raiz será para a função que contém a variável. Se essa ID raiz for 0, a função será uma função sem nome que é interna ao CLR. Se o tipo da raiz for um identificador, a ID raiz será para o identificador de coleta de lixo. Para os outros tipos de raiz, a ID é um valor opaco e deve ser ignorada.  
  
## <a name="remarks"></a>Comentários  
 As `rootRefIds` `rootKinds` matrizes,, e `rootFlags` `rootIds` são matrizes paralelas. Ou seja,,, `rootRefIds[i]` `rootKinds[i]` `rootFlags[i]` e `rootIds[i]` todas as preocupações com a mesma raiz.  
  
 Ambos `RootReferences` e `RootReferences2` são chamados para notificar o criador de perfil. Os profileres normalmente implementarão um método ou o outro, mas não ambos, porque as informações transmitidas `RootReferences2` são um superconjunto do passado `RootReferences` .  
  
 É possível que as entradas no `rootRefIds` sejam zero, o que implica que a referência raiz correspondente é nula e não se refere a um objeto no heap gerenciado.  
  
 As IDs de objeto retornadas por `RootReferences2` não são válidas durante o próprio retorno de chamada, pois a coleta de lixo pode estar no meio da movimentação de objetos de endereços antigos para novos endereços. Portanto, os criadores de perfis não devem tentar inspecionar objetos durante uma chamada `RootReferences2`. Quando [ICorProfilerCallback2:: GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md) é chamado, todos os objetos foram movidos para seus novos locais e podem ser inspecionados com segurança.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
- [Interface ICorProfilerCallback2](icorprofilercallback2-interface.md)
