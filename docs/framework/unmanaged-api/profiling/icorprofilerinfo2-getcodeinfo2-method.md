---
title: Método ICorProfilerInfo2::GetCodeInfo2
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetCodeInfo2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetCodeInfo2
helpviewer_keywords:
- ICorProfilerInfo2::GetCodeInfo2 method [.NET Framework profiling]
- GetCodeInfo2 method [.NET Framework profiling]
ms.assetid: 532da6ee-7f0a-401b-a61e-fc47ec235d2e
topic_type:
- apiref
ms.openlocfilehash: 04ce9ebded4be7ac3b20a4ceb78dd02294bbff4a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502884"
---
# <a name="icorprofilerinfo2getcodeinfo2-method"></a>Método ICorProfilerInfo2::GetCodeInfo2
Obtém as extensões do código nativo associado ao especificado `FunctionID` .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetCodeInfo2(  
    [in]  FunctionID functionID,  
    [in]  ULONG32 cCodeInfos,  
    [out] ULONG32 *pcCodeInfos,  
    [out, size_is(cCodeInfos), length_is(*pcCodeInfos)]  
    COR_PRF_CODE_INFO codeInfos[]);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `functionID`  
 no A ID da função à qual o código nativo está associado.  
  
 `cCodeInfos`  
 no O tamanho da `codeInfos` matriz.  
  
 `pcCodeInfos`  
 fora Um ponteiro para o número total de estruturas de [COR_PRF_CODE_INFO](cor-prf-code-info-structure.md) disponíveis.  
  
 `codeInfos`  
 fora Um buffer fornecido pelo chamador. Depois que o método retorna, ele contém uma matriz de `COR_PRF_CODE_INFO` estruturas, cada uma delas descreve um bloco de código nativo.  
  
## <a name="remarks"></a>Comentários  
 As extensões são classificadas em ordem de aumento do deslocamento da MSIL (Microsoft Intermediate Language).  
  
 Depois de `GetCodeInfo2` retornar, você deve verificar se o `codeInfos` buffer foi grande o suficiente para conter todas as `COR_PRF_CODE_INFO` estruturas. Para fazer isso, compare o valor de `cCodeInfos` com o valor do `cchName` parâmetro. Se `cCodeInfos` dividido pelo tamanho de uma `COR_PRF_CODE_INFO` estrutura for menor que `pcCodeInfos` , aloque um buffer maior `codeInfos` , atualize `cCodeInfos` com o tamanho novo, maior e chame `GetCodeInfo2` novamente.  
  
 Como alternativa, você pode primeiro chamar `GetCodeInfo2` com um buffer de comprimento zero `codeInfos` para obter o tamanho de buffer correto. Em seguida, você pode definir o `codeInfos` tamanho do buffer para o valor retornado em `pcCodeInfos` , multiplicado pelo tamanho de uma `COR_PRF_CODE_INFO` estrutura e chamar `GetCodeInfo2` novamente.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Método GetCodeInfo3](icorprofilerinfo4-getcodeinfo3-method.md)
- [Interface ICorProfilerInfo2](icorprofilerinfo2-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Criação de perfil](index.md)
