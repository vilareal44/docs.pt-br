---
title: Método ICorProfilerInfo4::GetILToNativeMapping2
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo4.GetILToNativeMapping2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo4::GetILToNativeMapping2
helpviewer_keywords:
- ICorProfilerInfo4::GetILToNativeMapping2 method [.NET Framework profiling]
- GetILToNativeMapping2 method, ICorProfilerInfo4 interface [.NET Framework profiling]
ms.assetid: 756c1c25-08a7-4060-9798-dbeaa2f3bee5
topic_type:
- apiref
ms.openlocfilehash: 9a6ee58cda5e0b673b3ff1378240f89323e30194
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84496030"
---
# <a name="icorprofilerinfo4getiltonativemapping2-method"></a>Método ICorProfilerInfo4::GetILToNativeMapping2
Obtém um mapa de deslocamentos de MSIL (Microsoft Intermediate Language) para deslocamentos nativos do código contido na versão recompilada do JIT da função especificada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetILToNativeMapping(  
    [in] FunctionID functionId,  
    [in] ReJITID reJitId,  
    [in] ULONG32 cMap,  
    [out] ULONG32 *pcMap,  
    [out, size_is(cMap), length_is(*pcMap)]  
        COR_DEBUG_IL_TO_NATIVE_MAP map[]);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `functionId`  
 no A ID da função que contém o código.  
  
 `pReJitId`  
 no A identidade da função de compilação JIT recompilada. A identidade deve ser zero no .NET Framework 4,5.  
  
 `cMap`  
 no O tamanho máximo da `map` matriz.  
  
 `pcMap`  
 fora O número total de estruturas de COR_DEBUG_IL_TO_NATIVE_MAP disponíveis.  
  
 `map`  
 fora Uma matriz de `COR_DEBUG_IL_TO_NATIVE_MAP` estruturas, cada uma delas especifica os deslocamentos. Depois que o `GetILToNativeMapping2` método retornar, `map` conterá algumas ou todas as `COR_DEBUG_IL_TO_NATIVE_MAP` estruturas.  
  
## <a name="remarks"></a>Comentários  
 `GetILToNativeMapping2`é semelhante ao método [ICorProfilerInfo:: GetILToNativeMapping](icorprofilerinfo-getiltonativemapping-method.md) , exceto pelo fato de que ele permitirá que o criador de perfil ESPECIFIQUE a ID da função recompilada em versões futuras.  
  
> [!NOTE]
> O método [ICorProfilerFunctionControl:: SetILInstrumentedCodeMap](icorprofilerfunctioncontrol-setilinstrumentedcodemap-method.md) não está implementado no .NET Framework 4,5, portanto, as funções que foram recompiladas por JIT não podem ter um mapeamento de Il para nativo diferente da função compilada originalmente. Como tal, `GetILToNativeMapping2` não pode ser chamado com uma ID de compilação JIT diferente de zero no .NET Framework 4,5.  
  
 O `GetILToNativeMapping2` método retorna uma matriz de `COR_DEBUG_IL_TO_NATIVE_MAP` estruturas. Para transmitir que determinados intervalos de instruções nativas correspondem a regiões especiais de código (por exemplo, o prólogo), uma entrada na matriz pode ter seu `ilOffset` campo definido como um valor da enumeração [CorDebugIlToNativeMappingTypes](../debugging/cordebugiltonativemappingtypes-enumeration.md) .  
  
 Depois de `GetILToNativeMapping2` retornar, você deve verificar se o `map` buffer foi grande o suficiente para conter todas as `COR_DEBUG_IL_TO_NATIVE_MAP` estruturas. Para fazer isso, compare o valor de `cMap` com o valor do `pcMap` parâmetro. Se o `pcMap` valor, quando for multiplicado pelo tamanho de uma `COR_DEBUG_IL_TO_NATIVE_MAP` estrutura, for maior do que `cMap` , aloque um buffer maior `map` , atualize `cMap` com o novo tamanho, maior e chame `GetILToNativeMapping2` novamente.  
  
 Como alternativa, você pode primeiro chamar `GetILToNativeMapping2` com um buffer de comprimento zero `map` para obter o tamanho de buffer correto. Em seguida, você pode definir o tamanho do buffer para o valor retornado em `pcMap` e chamar `GetILToNativeMapping2` novamente.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Método GetILToNativeMapping](icorprofilerinfo-getiltonativemapping-method.md)
- [Interface ICorProfilerInfo4](icorprofilerinfo4-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Criação de perfil](index.md)
