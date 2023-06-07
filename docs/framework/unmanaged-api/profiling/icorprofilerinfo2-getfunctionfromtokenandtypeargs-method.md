---
title: Método ICorProfilerInfo2::GetFunctionFromTokenAndTypeArgs
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetFunctionFromTokenAndTypeArgs
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetFunctionFromTokenAndTypeArgs
helpviewer_keywords:
- ICorProfilerInfo2::GetFunctionFromTokenAndTypeArgs method [.NET Framework profiling]
- GetFunctionFromTokenAndTypeArgs method [.NET Framework profiling]
ms.assetid: ce8f6aa6-4ebf-4a86-b429-4bbc8af41a8f
topic_type:
- apiref
ms.openlocfilehash: 7f1276e1adeece086ca7b6791eb6e870faf4d010
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502867"
---
# <a name="icorprofilerinfo2getfunctionfromtokenandtypeargs-method"></a>Método ICorProfilerInfo2::GetFunctionFromTokenAndTypeArgs
Obtém o `FunctionID` de uma função usando o token de metadados especificado, que contém a classe e os `ClassID` valores de qualquer argumento de tipo.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetFunctionFromTokenAndTypeArgs(  
    [in] ModuleID moduleID,  
    [in] mdMethodDef funcDef,  
    [in] ClassID classId,  
    [in] ULONG32 cTypeArgs,  
    [in, size_is(cTypeArgs)] ClassID typeArgs[],  
    [out] FunctionID* pFunctionID);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `moduleID`  
 no A ID do módulo no qual a função reside.  
  
 `funcDef`  
 no Um `mdMethodDef` token de metadados que faz referência à função.  
  
 `classId`  
 no A ID da classe que contém a função.  
  
 `cTypeArgs`  
 no O número de parâmetros de tipo para a função fornecida. Esse valor deve ser zero para funções não genéricas.  
  
 `typeArgs`  
 no Uma matriz de `ClassID` valores, cada um dos quais é um argumento da função. O valor de `typeArgs` pode ser NULL se `cTypeArgs` for definido como zero.  
  
 `pFunctionID`  
 fora Um ponteiro para o `FunctionID` da função especificada.  
  
## <a name="remarks"></a>Comentários  
 Chamar o `GetFunctionFromTokenAndTypeArgs` método com `mdMethodRef` metadados em vez de um `mdMethodDef` token de metadados pode ter resultados imprevisíveis. Os chamadores devem resolver o `mdMethodRef` para um `mdMethodDef` ao passá-lo.  
  
 Se a função ainda não estiver carregada, a chamada fará com que o `GetFunctionFromTokenAndTypeArgs` carregamento ocorra, que é uma operação perigosa em muitos contextos. Por exemplo, chamar esse método durante o carregamento de módulos ou tipos pode levar a um loop infinito, pois o tempo de execução tenta carregar as coisas de forma circular.  
  
 Em geral, o uso de `GetFunctionFromTokenAndTypeArgs` é desencorajado. Se os profileres estiverem interessados em eventos para uma função específica, eles deverão armazenar o `ModuleID` e `mdMethodDef` dessa função e usar [ICorProfilerInfo2:: GetFunctionInfo2](icorprofilerinfo2-getfunctioninfo2-method.md) para verificar se um determinado `FunctionID` é o da função desejada.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
- [Interface ICorProfilerInfo2](icorprofilerinfo2-interface.md)
