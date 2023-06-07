---
title: Método ICorProfilerInfo2::GetClassFromTokenAndTypeArgs
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetClassFromTokenAndTypeArgs
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetClassFromTokenAndTypeArgs
helpviewer_keywords:
- GetClassFromTokenAndTypeArgs method [.NET Framework profiling]
- ICorProfilerInfo2::GetClassFromTokenAndTypeArgs method [.NET Framework profiling]
ms.assetid: b25c88f0-71b9-443b-8eea-1c94db0a44b9
topic_type:
- apiref
ms.openlocfilehash: 702c5f9f2bc08c824bdc0607741a6afd65a3e89b
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84497251"
---
# <a name="icorprofilerinfo2getclassfromtokenandtypeargs-method"></a>Método ICorProfilerInfo2::GetClassFromTokenAndTypeArgs
Obtém o `ClassID` de um tipo usando o token de metadados especificado e os `ClassID` valores de qualquer argumento de tipo.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetClassFromTokenAndTypeArgs(  
    [in] ModuleID moduleID,  
    [in] mdTypeDef typeDef,  
    [in] ULONG32 cTypeArgs,  
    [in, size_is(cTypeArgs)] ClassID typeArgs[],  
    [out] ClassID* pClassID);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `moduleID`  
 no A ID do módulo no qual o tipo reside.  
  
 `typeDef`  
 no Um `mdTypeDef` token de metadados que faz referência ao tipo.  
  
 `cTypeArgs`  
 no O número de parâmetros de tipo para o tipo fornecido. Esse valor deve ser zero para tipos não genéricos.  
  
 `typeArgs`  
 no Uma matriz de `ClassID` valores, cada um dos quais é um argumento do tipo. O valor de `typeArgs` pode ser NULL se `cTypeArgs` for definido como zero.  
  
 `pClassID`  
 fora Um ponteiro para o `ClassID` do tipo especificado.  
  
## <a name="remarks"></a>Comentários  
 Chamar o `GetClassFromTokenAndTypeArgs` método com um `mdTypeRef` em vez de um `mdTypeDef` token de metadados pode ter resultados imprevisíveis; os chamadores devem resolver o `mdTypeRef` para um `mdTypeDef` ao passá-lo.  
  
 Se o tipo ainda não estiver carregado, chamar `GetClassFromTokenAndTypeArgs` irá disparar o carregamento, que é uma operação perigosa em muitos contextos. Por exemplo, chamar esse método durante o carregamento de módulos ou outros tipos pode levar a um loop infinito, pois o tempo de execução tenta carregar as coisas de forma circular.  
  
 Em geral, o uso de `GetClassFromTokenAndTypeArgs` é desencorajado. Se os profileres estiverem interessados em eventos para um tipo específico, eles deverão armazenar o `ModuleID` e `mdTypeDef` desse tipo e usar [ICorProfilerInfo2:: GetClassIDInfo2](icorprofilerinfo2-getclassidinfo2-method.md) para verificar se um determinado é o `ClassID` do tipo desejado.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
- [Interface ICorProfilerInfo2](icorprofilerinfo2-interface.md)
