---
title: Método ICorProfilerInfo::GetAssemblyInfo
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetAssemblyInfo
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- GetAssemblyInfo Method
helpviewer_keywords:
- GetAssemblyInfo method [.NET Framework profiling]
- ICorProfilerInfo::GetAssemblyInfo method [.NET Framework profiling]
ms.assetid: 7a3c97c3-1e31-47b1-bf23-386785c509c4
topic_type:
- apiref
ms.openlocfilehash: 41083b2fcd61a9a726e835c3d5710308aa634600
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84498603"
---
# <a name="icorprofilerinfogetassemblyinfo-method"></a>Método ICorProfilerInfo::GetAssemblyInfo
Aceita uma ID de assembly e retorna o nome do assembly e a ID do seu módulo de manifesto.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetAssemblyInfo(  
    [in]  AssemblyID  assemblyId,  
    [in]  ULONG       cchName,  
    [out] ULONG       *pcchName,  
    [out, size_is(cchName), length_is(*pcchName)]  
          WCHAR       szName[] ,  
    [out] AppDomainID *pAppDomainId,  
    [out] ModuleID    *pModuleId);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `assemblyId`  
 no O identificador do assembly.  
  
 `cchName`  
 no O comprimento, em caracteres, de `szName` .  
  
 `pcchName`  
 fora Um ponteiro para o comprimento total do caractere do nome do assembly.  
  
 `szName`  
 fora Um buffer de caracteres largo fornecido pelo chamador. Quando a função retornar, ela conterá o nome do assembly.  
  
 `pAppDomainId`  
 fora Um ponteiro para a ID do domínio do aplicativo que contém o assembly.  
  
 `pModuleId`  
 fora Um ponteiro para a ID do módulo de manifesto do assembly.  
  
## <a name="remarks"></a>Comentários  
 Após esse método retornar, você deve verificar se o `szName` buffer foi grande o suficiente para conter o nome completo do assembly. Para fazer isso, compare o valor que `pcchName` aponta com o valor do `cchName` parâmetro. Se `pcchName` apontar para um valor maior que `cchName` , aloque um `szName` buffer maior, atualize `cchName` com o tamanho novo, maior e chame `GetAssemblyInfo` novamente.  
  
 Como alternativa, você pode primeiro chamar `GetAssemblyInfo` com um buffer de comprimento zero `szName` para obter o tamanho de buffer correto. Em seguida, você pode ajustar o tamanho do buffer com base no valor retornado em `pcchName` e chamar `GetAssemblyInfo` novamente.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Criação de perfil](index.md)
