---
title: Método ICorProfilerInfo3::GetModuleInfo2
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo3.GetModuleInfo2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo3::GetModuleInfo2
helpviewer_keywords:
- ICorProfilerInfo3::GetModuleInfo2 method [.NET Framework profiling]
- GetModuleInfo2 method [.NET Framework profiling]
ms.assetid: f1f6b8f3-dcfc-49e8-be76-ea50ea90d5a7
topic_type:
- apiref
ms.openlocfilehash: d2b7e93866bf0aa79849925234a4d6e4cc9b5b52
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502815"
---
# <a name="icorprofilerinfo3getmoduleinfo2-method"></a>Método ICorProfilerInfo3::GetModuleInfo2
Dada uma ID de módulo, retorna o nome do arquivo do módulo, a ID do assembly pai do módulo e um bitmask que descreve as propriedades do módulo.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetModuleInfo2(  
    [in]  ModuleID   moduleId,  
    [out] LPCBYTE    *ppBaseLoadAddress,  
    [in]  ULONG      cchName,  
    [out] ULONG      *pcchName,  
    [out, annotation("__out_ecount_part(cchName, *pcchName)")]  
          WCHAR      szName[] ,  
    [out] AssemblyID *pAssemblyId);  
    [out] DWORD                 *pdwModuleFlags);  
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
  
 `pdwModuleFlags`  
 fora Uma bitmask de valores da enumeração [COR_PRF_MODULE_FLAGS](cor-prf-module-flags-enumeration.md) que especificam as propriedades do módulo.  
  
## <a name="remarks"></a>Comentários  
 Para módulos dinâmicos, o `szName` parâmetro é o nome de metadados do módulo e o endereço base é 0 (zero). O nome dos metadados é o valor na coluna nome da tabela de módulos dentro dos metadados. Isso também é exposto como a <xref:System.Reflection.Module.ScopeName%2A?displayProperty=nameWithType> Propriedade do código gerenciado e como o `szName` parâmetro do método [IMetaDataImport:: GetScopeProps](../metadata/imetadataimport-getscopeprops-method.md) para o código de cliente de metadados não gerenciado.  
  
 Embora o `GetModuleInfo2` método possa ser chamado assim que a ID do módulo existir, a ID do assembly pai não estará disponível até que o criador de perfil receba o retorno de chamada [ICorProfilerCallback:: ModuleAttachedToAssembly](icorprofilercallback-moduleattachedtoassembly-method.md) .  
  
 Quando `GetModuleInfo2` retorna, você deve verificar se o `szName` buffer foi grande o suficiente para conter o nome de arquivo completo do módulo. Para fazer isso, compare o valor que `pcchName` aponta com o valor do `cchName` parâmetro. Se `pcchName` apontar para um valor maior que `cchName` , aloque um `szName` buffer maior, atualize `cchName` com o tamanho novo, maior e chame `GetModuleInfo2` novamente.  
  
 Como alternativa, você pode primeiro chamar `GetModuleInfo2` com um buffer de comprimento zero `szName` para obter o tamanho de buffer correto. Em seguida, você pode definir o tamanho do buffer para o valor retornado em `pcchName` e chamar `GetModuleInfo2` novamente.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Criação de perfil](index.md)
