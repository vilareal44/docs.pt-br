---
title: Método ICorProfilerInfo2::GetClassIDInfo2
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetClassIDInfo2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetClassIDInfo2
helpviewer_keywords:
- GetClassIDInfo2 method [.NET Framework profiling]
- ICorProfilerInfo2::GetClassIDInfo2 method [.NET Framework profiling]
ms.assetid: 0141d582-d066-4d49-8d1f-ae82129a1960
topic_type:
- apiref
ms.openlocfilehash: a33e51969dc0579d976f08470ebc6e2bcca04dd7
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84497160"
---
# <a name="icorprofilerinfo2getclassidinfo2-method"></a>Método ICorProfilerInfo2::GetClassIDInfo2
Obtém o módulo pai e o token de metadados para a definição genérica aberta da classe especificada, a `ClassID` de sua classe pai e o `ClassID` argumento para cada tipo, se presente, da classe.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetClassIDInfo2(  
    [in]  ClassID classId,  
    [out] ModuleID *pModuleId,  
    [out] mdTypeDef *pTypeDefToken,  
    [out] ClassID *pParentClassId,  
    [in]  ULONG32 cNumTypeArgs,  
    [out] ULONG32 *pcNumTypeArgs,  
    [out] ClassID typeArgs[]);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `classId`  
 no A ID da classe para a qual as informações serão recuperadas.  
  
 `pModuleId`  
 fora Aponta para a ID do módulo pai para a definição genérica aberta da classe especificada.  
  
 `pTypeDefToken`  
 fora Ponteiro para o token de metadados para a definição genérica aberta da classe especificada.  
  
 `pParentClassId`  
 fora Ponteiro para a ID da classe pai.  
  
 `cNumTypeArgs`  
 no O tamanho da `typeArgs` matriz.  
  
 `pcNumTypeArgs`  
 fora Aponta para o número total de elementos disponíveis.  
  
 `typeArgs`  
 fora Uma matriz de `ClassID` valores, cada um representando a ID de um argumento de tipo da classe. Quando o método retornar, `typeArgs` conterá alguns ou todos os `ClassID` valores disponíveis.  
  
## <a name="remarks"></a>Comentários  
 O `GetClassIDInfo2` método é semelhante ao método [ICorProfilerInfo:: GetClassIDInfo](icorprofilerinfo-getclassidinfo-method.md) , mas `GetClassIDInfo2` Obtém informações adicionais sobre um tipo genérico.  
  
 O código do criador de perfil pode chamar [ICorProfilerInfo:: GetModuleMetaData](icorprofilerinfo-getmodulemetadata-method.md) para obter uma interface de [metadados](../metadata/index.md) para um determinado módulo. O token de metadados que é retornado para o local referenciado por `pTypeDefToken` pode ser usado para acessar os metadados da classe.  
  
 Depois de `GetClassIDInfo2` retornar, você deve verificar se o `typeArgs` buffer foi grande o suficiente para conter todos os `ClassID` valores. Para fazer isso, compare o valor que `pcNumTypeArgs` aponta com o valor do `cNumTypeArgs` parâmetro. Se `pcNumTypeArgs` apontar para um valor maior que `cNumTypeArgs` , aloque um `typeArgs` buffer maior, atualize `cNumTypeArgs` com o tamanho novo, maior e chame `GetClassIDInfo2` novamente.  
  
 Como alternativa, você pode primeiro chamar `GetClassIDInfo2` com um buffer de comprimento zero `typeArgs` para obter o tamanho de buffer correto. Em seguida, você pode definir o `typeArgs` tamanho do buffer para o valor retornado em `pcNumTypeArgs` e chamar `GetClassIDInfo2` novamente.  
  
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
