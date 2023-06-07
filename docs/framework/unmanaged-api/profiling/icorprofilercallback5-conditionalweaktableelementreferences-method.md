---
title: ICorProfilerCallback5::Método ConditionalWeakTableElementReferences
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback5.ConditionalWeakTableReferences
api_location:
- Mscorwks.dll
api_type:
- COM
f1_keywords:
- ConditionalWeakTableElementReferences
helpviewer_keywords:
- ConditionalWeakTableElementReferences method, ICorProfilerCallback5 interface [.NET Framework profiling]
- ICorProfilerCallback5::ConditionalWeakTableElementReferences method [.NET Framework profiling]
ms.assetid: 532c7a02-a9de-4cea-bb2b-7f470da594de
topic_type:
- apiref
ms.openlocfilehash: 17fbc99b30921f795c1f7ff882ec73432aade8c6
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499240"
---
# <a name="icorprofilercallback5conditionalweaktableelementreferences-method"></a>ICorProfilerCallback5::Método ConditionalWeakTableElementReferences

Identifica o fechamento transitivo de objetos referenciados por estas raízes por meio de referências diretas do campo de membro e de dependências `ConditionalWeakTable`.

## <a name="syntax"></a>Sintaxe

```cpp
HRESULT ConditionalWeakTableElementReferences(
     [in]                     ULONG    cRootRefs,
     [in, size_is(cRootRefs)] ObjectID keyRefIds[],
     [in, size_is(cRootRefs)] ObjectID valueRefIds[],
     [in, size_is(cRootRefs)] GCHandleID rootIds[]
);
```

## <a name="parameters"></a>Parâmetros

`cRootRefs`\
[in] O número de elementos nas matrizes `keyRefIds`, `valueRefIds` e `rootIds`.

`keyRefIds`\
[in] Uma matriz de IDs de objeto, cada uma contendo o `ObjectID` para o elemento principal do par do identificador dependente.

`valueRefIds`\
[in] Uma matriz de IDs de objeto, cada uma contendo o `ObjectID` para o elemento secundário do par do identificador dependente. ( `keyRefIds[i]` mantém `valueRefIds[i]` vivo.)

`rootIds`\
[in] Uma matriz de valores `GCHandleID` que apontam para um número inteiro que contém informações adicionais sobre a raiz de coleta de lixo.

Nenhum dos valores `ObjectID` retornados pelo método `ConditionalWeakTableElementReferences` é válido durante o retorno de chamada em si, porque o coletor de lixo pode estar no processo de mover objetos de locais antigos para novos. Portanto, os criadores de perfis não devem tentar inspecionar objetos durante uma chamada `ConditionalWeakTableElementReferences`. Ao `GarbageCollectionFinished`, todos os objetos foram movidos para os novos locais e a inspeção pode ser feita.

## <a name="example"></a>Exemplo

O exemplo de código a seguir demonstra como implementar [ICorProfilerCallback5](icorprofilercallback5-interface.md) e usar esse método.

```cpp
HRESULT Callback5Impl::ConditionalWeakTableElementReferences(
    ULONG      cRootRefs,
    ObjectID   keyRefIds[],
    ObjectID   valueRefIds[],
    GCHandleID rootIds[])
{
    printf("Callback5Impl::ConditionalWeakTableElementReferences called\n");
    for (unsigned int i = 0; i < cRootRefs; ++i)
    {
        // Save dependency to XML for later retrieval
        PersistDependencyToXml(rootIds[i], keyRefIds[i], valueRefIds[i]);
        // or store dependency to an internal map
        m_cwt_deps->add_dep(rootIds[i], keyRefIds[i], valueRefIds[i]);
        // or add arc to object graph
        m_obj_graph->add_arc(keyRefIds[i], valueRefIds[i], rootIds[i]);
    }
    return S_OK;
}
```

## <a name="remarks"></a>Comentários

Um criador de perfil para o .NET Framework 4,5 ou versões posteriores implementa a interface [ICorProfilerCallback5](icorprofilercallback5-interface.md) e registra as dependências especificadas pelo `ConditionalWeakTableElementReferences` método. `ICorProfilerCallback5`fornece o conjunto completo de dependências entre objetos dinâmicos representados por `ConditionalWeakTable` entradas. Essas dependências e as referências de campo de membro especificadas pelo método [ICorProfilerCallback:: Objectreferenciations](icorprofilercallback-objectreferences-method.md) habilitam um criador de perfil gerenciado para gerar o grafo de objeto completo de objetos dinâmicos.

## <a name="requirements"></a>Requisitos

**Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).

**Cabeçalho:** CorProf. idl, CorProf. h

**.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]

## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback5](icorprofilercallback5-interface.md)
