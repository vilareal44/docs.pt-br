---
title: Método ICorProfilerInfo2::GetFunctionInfo2
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetFunctionInfo2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetFunctionInfo2
helpviewer_keywords:
- GetFunctionInfo2 method [.NET Framework profiling]
- ICorProfilerInfo2::GetFunctionInfo2 method [.NET Framework profiling]
ms.assetid: 0aa60f24-8bbd-4c83-83c5-86ad191b1d82
topic_type:
- apiref
ms.openlocfilehash: f5438ddc655f0f6a7c11d978a47b1bf9e2a13059
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84496995"
---
# <a name="icorprofilerinfo2getfunctioninfo2-method"></a>Método ICorProfilerInfo2::GetFunctionInfo2
Obtém a classe pai, o token de metadados e o `ClassID` de cada argumento de tipo, se presente, de uma função.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetFunctionInfo2(  
    [in]  FunctionID funcId,  
    [in]  COR_PRF_FRAME_INFO frameInfo,  
    [out] ClassID *pClassId,  
    [out] ModuleID *pModuleId,  
    [out] mdToken *pToken,  
    [in]  ULONG32 cTypeArgs,  
    [out] ULONG32 *pcTypeArgs,  
    [out] ClassID typeArgs[]);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `funcId`  
 no A ID da função para a qual obter a classe pai e outras informações.  
  
 `frameInfo`  
 no Um `COR_PRF_FRAME_INFO` valor que aponta para informações sobre um registro de ativação.  
  
 `pClassId`  
 fora Um ponteiro para a classe pai da função.  
  
 `pModuleId`  
 fora Um ponteiro para o módulo no qual a classe pai da função é definida.  
  
 `pToken`  
 fora Um ponteiro para o token de metadados para a função.  
  
 `cTypeArgs`  
 no O tamanho da `typeArgs` matriz.  
  
 `pcTypeArgs`  
 fora Um ponteiro para o número total de `ClassID` valores.  
  
 `typeArgs`  
 fora Uma matriz de `ClassID` valores, cada qual é a ID de um argumento de tipo da função. Quando o método retornar, `typeArgs` conterá alguns ou todos os `ClassID` valores.  
  
## <a name="remarks"></a>Comentários  
 O código do criador de perfil pode chamar [ICorProfilerInfo:: GetModuleMetaData](icorprofilerinfo-getmodulemetadata-method.md) para obter uma interface de [metadados](../metadata/index.md) para um determinado módulo. O token de metadados que é retornado para o local referenciado por `pToken` pode ser usado para acessar os metadados para a função.  
  
 Os argumentos ID de classe e tipo retornados por meio dos `pClassId` `typeArgs` parâmetros e dependem do valor que é passado no `frameInfo` parâmetro, conforme mostrado na tabela a seguir.  
  
|Valor do `frameInfo` parâmetro|Result|  
|----------------------------------------|------------|  
|Um `COR_PRF_FRAME_INFO` valor que foi obtido de um `FunctionEnter2` retorno de chamada|O `ClassID` , retornado no local referenciado por `pClassId` , e todos os argumentos de tipo, retornados na `typeArgs` matriz, será exato.|  
|Um `COR_PRF_FRAME_INFO` que foi obtido de uma fonte diferente de um `FunctionEnter2` retorno de chamada|Os `ClassID` argumentos exato e de tipo não podem ser determinados. Ou seja, o `ClassID` pode ser nulo e alguns argumentos de tipo podem retornar como <xref:System.Object> .|  
|Zero|Os `ClassID` argumentos exato e de tipo não podem ser determinados. Ou seja, o `ClassID` pode ser nulo e alguns argumentos de tipo podem retornar como <xref:System.Object> .|  
  
 Depois de `GetFunctionInfo2` retornar, você deve verificar se o `typeArgs` buffer foi grande o suficiente para conter todos os `ClassID` valores. Para fazer isso, compare o valor que `pcTypeArgs` aponta com o valor do `cTypeArgs` parâmetro. Se `pcTypeArgs` apontar para um valor maior do que `cTypeArgs` o dividido pelo tamanho de um `ClassID` valor, aloque um buffer maior `pcTypeArgs` , atualize `cTypeArgs` com o novo tamanho, o maior e a chamada `GetFunctionInfo2` novamente.  
  
 Como alternativa, você pode primeiro chamar `GetFunctionInfo2` com um buffer de comprimento zero `pcTypeArgs` para obter o tamanho de buffer correto. Em seguida, você pode definir o tamanho do buffer para o valor retornado em `pcTypeArgs` dividido pelo tamanho de um `ClassID` valor e chamar `GetFunctionInfo2` novamente.  
  
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
