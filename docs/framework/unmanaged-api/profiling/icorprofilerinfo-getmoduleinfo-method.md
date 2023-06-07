---
title: Método ICorProfilerInfo::GetModuleInfo
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetModuleInfo
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::GetModuleInfo
helpviewer_keywords:
- GetModuleInfo method [.NET Framework profiling]
- ICorProfilerInfo::GetModuleInfo method [.NET Framework profiling]
ms.assetid: 5a90d16f-7929-4987-8f83-a631becf564d
topic_type:
- apiref
ms.openlocfilehash: 751f2ac44e543fed76c7031791bb57d75ed0fd48
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84498096"
---
# <a name="icorprofilerinfogetmoduleinfo-method"></a>Método ICorProfilerInfo::GetModuleInfo
Dada uma ID de módulo, retorna o nome do arquivo do módulo e a ID do assembly pai do módulo.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetModuleInfo(  
    [in]  ModuleID   moduleId,  
    [out] LPCBYTE    *ppBaseLoadAddress,  
    [in]  ULONG      cchName,  
    [out] ULONG      *pcchName,  
    [out, size_is(cchName), length_is(*pcchName)]  
          WCHAR      szName[] ,  
    [out] AssemblyID *pAssemblyId);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `moduleId`  
 no A ID do módulo para o qual as informações serão recuperadas.  
  
 `ppBaseLoadAddress`  
 fora O endereço base no qual o módulo é carregado.  
  
 `cchName`  
 no O comprimento, em caracteres, do `szName` buffer de retorno.  
  
 `pcchName`  
 fora Um ponteiro para o comprimento total do caractere do nome de arquivo do módulo que é retornado.  
  
 `szName`  
 fora Um buffer de caracteres largo fornecido pelo chamador. Quando o método retorna, esse buffer contém o nome do arquivo do módulo.  
  
 `pAssemblyId`  
 fora Um ponteiro para a ID do assembly pai do módulo.  
  
## <a name="remarks"></a>Comentários  
 Para módulos dinâmicos, o `szName` parâmetro é uma cadeia de caracteres vazia e o endereço base é 0 (zero).  
  
 Embora o `GetModuleInfo` método possa ser chamado assim que a ID do módulo existir, a ID do assembly pai não estará disponível até que o criador de perfil receba o retorno de chamada [ICorProfilerCallback:: ModuleAttachedToAssembly](icorprofilercallback-moduleattachedtoassembly-method.md) .  
  
 Quando `GetModuleInfo` retorna, você deve verificar se o `szName` buffer foi grande o suficiente para conter o nome de arquivo completo do módulo. Para fazer isso, compare o valor que `pcchName` aponta com o valor do `cchName` parâmetro. Se `pcchName` apontar para um valor maior que `cchName` , aloque um `szName` buffer maior, atualize `cchName` com o tamanho novo, maior e chame `GetModuleInfo` novamente.  
  
 Como alternativa, você pode primeiro chamar `GetModuleInfo` com um buffer de comprimento zero `szName` para obter o tamanho de buffer correto. Em seguida, você pode definir o tamanho do buffer para o valor retornado em `pcchName` e chamar `GetModuleInfo` novamente.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Criação de perfil](index.md)
- [Método GetModuleInfo2](icorprofilerinfo3-getmoduleinfo2-method.md)
