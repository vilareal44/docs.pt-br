---
title: Método ICorProfilerInfo::SetILInstrumentedCodeMap
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.SetILInstrumentedCodeMap
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::SetILInstrumentedCodeMap
helpviewer_keywords:
- ICorProfilerInfo::SetILInstrumentedCodeMap method [.NET Framework profiling]
- SetILInstrumentedCodeMap method [.NET Framework profiling]
ms.assetid: bce1dcf8-b4ec-4e73-a917-f2df1ad49c8a
topic_type:
- apiref
ms.openlocfilehash: cac8e9570dab55af6b6e1fcf6f53b6a697727972
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502906"
---
# <a name="icorprofilerinfosetilinstrumentedcodemap-method"></a>Método ICorProfilerInfo::SetILInstrumentedCodeMap

Define um mapa de código para a função especificada usando as entradas de mapa da MSIL (Microsoft Intermediate Language) especificadas.

> [!NOTE]
> No .NET Framework versão 2,0, chamar `SetILInstrumentedCodeMap` em um `FunctionID` que representa uma função genérica em um domínio de aplicativo específico afetará todas as instâncias dessa função no domínio do aplicativo.

## <a name="syntax"></a>Sintaxe

```cpp
HRESULT SetILInstrumentedCodeMap(
    [in]  FunctionID functionId,
    [in]  BOOL       fStartJit,
    [in]  ULONG      cILMapEntries,
    [in, size_is(cILMapEntries)] COR_IL_MAP rgILMapEntries[]);
```

## <a name="parameters"></a>Parâmetros

`functionId`\
no A ID da função para a qual definir o mapa de código.

`fStartJit`\
no Um valor booliano que indica se a chamada para o `SetILInstrumentedCodeMap` método é a primeira para um específico `FunctionID` . Defina `fStartJit` como `true` na primeira chamada para `SetILInstrumentedCodeMap` para um determinado `FunctionID` e para `false` depois.

`cILMapEntries`\
no O número de elementos na `cILMapEntries` matriz.

`rgILMapEntries`\
no Uma matriz de estruturas COR_IL_MAP, cada uma delas especifica um deslocamento MSIL.

## <a name="remarks"></a>Comentários

Um criador de perfil geralmente insere instruções no código-fonte de um método para instrumentar esse método (por exemplo, para notificar quando uma determinada linha de origem for atingida). `SetILInstrumentedCodeMap`permite que um criador de perfil Mapeie as instruções originais do MSIL para seus novos locais. Um criador de perfil pode usar o método [ICorProfilerInfo:: GetILToNativeMapping](icorprofilerinfo-getiltonativemapping-method.md) para obter o deslocamento de MSIL original para um determinado deslocamento nativo.

O depurador assumirá que cada deslocamento antigo se refere a um deslocamento MSIL dentro do código MSIL original e não modificado, e que cada deslocamento novo se refere ao deslocamento MSIL dentro do novo código instrumentado. O mapa deve ser classificado em ordem crescente. Para que a depuração funcione corretamente, siga estas diretrizes:

- Não reordene o código MSIL instrumentado.

- Não remova o código MSIL original.

- Inclua entradas para todos os pontos de sequência do arquivo de banco de dados do programa (PDB) no mapa. O mapa não interpola as entradas ausentes. Portanto, considerando o seguinte mapa:

  (0 antigo, 0 novo)

  (5 antigos, 10 novos)

  (9 antigos, 20 novos)

  - Um deslocamento antigo de 0, 1, 2, 3 ou 4 será mapeado para o novo deslocamento 0.

  - Um deslocamento antigo de 5, 6, 7 ou 8 será mapeado para o novo deslocamento 10.

  - Um deslocamento antigo de 9 ou superior será mapeado para o novo deslocamento 20.

  - Um novo deslocamento de 0, 1, 2, 3, 4, 5, 6, 7, 8 ou 9 será mapeado para o deslocamento antigo 0.

  - Um novo deslocamento de 10, 11, 12, 13, 14, 15, 16, 17, 18 ou 19 será mapeado para o deslocamento antigo 5.

  - Um novo deslocamento de 20 ou mais será mapeado para o deslocamento antigo 9.

No .NET Framework 3,5 e versões anteriores, você aloca a `rgILMapEntries` matriz chamando o método [CoTaskMemAlloc](/windows/desktop/api/combaseapi/nf-combaseapi-cotaskmemalloc) . Como o tempo de execução se apropria dessa memória, o criador de perfil não deve tentar liberá-la.

## <a name="requirements"></a>Requisitos

**Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).

**Cabeçalho:** CorProf. idl, CorProf. h

**Biblioteca:** CorGuids.lib

**.NET Framework versões:**[!INCLUDE[net_current_v11plus](../../../../includes/net-current-v11plus-md.md)]

## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
