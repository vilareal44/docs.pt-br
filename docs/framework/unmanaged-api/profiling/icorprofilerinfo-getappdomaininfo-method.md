---
title: Método ICorProfilerInfo::GetAppDomainInfo
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetAppDomainInfo
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::GetAppDomainInfo
helpviewer_keywords:
- ICorProfilerInfo::GetAppDomainInfo method [.NET Framework profiling]
- GetAppDomainInfo method [.NET Framework profiling]
ms.assetid: a6bf5a04-e03e-44f0-917a-96f6a6d3cc96
topic_type:
- apiref
ms.openlocfilehash: 5e5510ec098b2c1aa3b768830b812328287fd31a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503023"
---
# <a name="icorprofilerinfogetappdomaininfo-method"></a>Método ICorProfilerInfo::GetAppDomainInfo
Aceita uma ID de domínio do aplicativo. Retorna um nome de domínio de aplicativo e a ID do processo que o contém.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetAppDomainInfo(  
    [in]  AppDomainID appDomainId,  
    [in]  ULONG       cchName,  
    [out] ULONG       *pcchName,  
    [out, size_is(cchName), length_is(*pcchName)]  
          WCHAR       szName[] ,  
    [out] ProcessID   *pProcessId);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `appDomainId`  
 no A ID do domínio do aplicativo.  
  
 `cchName`  
 no O comprimento, em caracteres, do `szName` buffer de retorno.  
  
 `pcchName`  
 fora Um ponteiro para o tamanho total do caractere do nome de domínio do aplicativo.  
  
 `szName`  
 fora Um buffer de caracteres largo fornecido pelo chamador. Quando o método retornar, `szName` conterá o nome de domínio do aplicativo completo ou parcial.  
  
 `pProcessId`  
 fora Um ponteiro para a ID do processo que contém o domínio do aplicativo.  
  
## <a name="remarks"></a>Comentários  
 Depois que esse método retornar, você deverá verificar se o `szName` buffer era grande o suficiente para conter o nome completo do domínio do aplicativo. Para fazer isso, compare o valor que `pcchName` aponta com o valor do `cchName` parâmetro. Se `pcchName` apontar para um valor maior que `cchName` , aloque um `szName` buffer maior, atualize `cchName` com o tamanho novo, maior e chame `GetAppDomainInfo` novamente.  
  
 Como alternativa, você pode primeiro chamar `GetAppDomainInfo` com um buffer de comprimento zero `szName` para obter o tamanho de buffer correto. Em seguida, você pode definir o tamanho do buffer para o valor retornado em `pcchName` e chamar `GetAppDomainInfo` novamente.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Criação de perfil](index.md)
