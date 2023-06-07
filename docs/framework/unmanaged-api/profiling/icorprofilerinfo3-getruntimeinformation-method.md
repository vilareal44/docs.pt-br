---
title: Método ICorProfilerInfo3::GetRuntimeInformation
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo3.GetRuntimeInformation Method
api_location:
- Mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo3::GetRuntimeInformation
helpviewer_keywords:
- GetRuntimeInformation method [.NET Framework profiling]
- ICorProfilerInfo3::GetRuntimeInformation method [.NET Framework profiling]
ms.assetid: 4400fb8c-0407-4791-8557-f011fd2aee51
topic_type:
- apiref
ms.openlocfilehash: b8e503af11fa1d02aac2ec83edde0ffbd562d8e5
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84496393"
---
# <a name="icorprofilerinfo3getruntimeinformation-method"></a>Método ICorProfilerInfo3::GetRuntimeInformation
Fornece informações de versão sobre o Common Language Runtime (CLR) cujo perfil está sendo criado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetRuntimeInformation(  
       [out] USHORT *pClrInstanceId,  
       [out] COR_PRF_RUNTIME_TYPE *pRuntimeType,  
       [out] USHORT *pMajorVersion,  
       [out] USHORT *pMinorVersion,  
       [out] USHORT *pBuildNumber,  
       [out] USHORT *pQFEVersion,  
       [in]  ULONG  cchVersionString,  
       [out] ULONG  *pcchVersionString,  
       [out, size_is(cchVersionString), length_is(*pcchVersionString)]  
                   WCHAR  szVersionString[]);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pClrInstanceId`  
 fora A ID representativa de uma instância CLR em execução em um processo. Isso é o mesmo que os `ClrInstanceID` relatórios de eventos de inicialização do ETW (rastreamento de eventos para Windows).  
  
 `pRuntimeType`  
 fora O tipo de tempo de execução. Esse parâmetro retorna `COR_PRF_DESKTOP_CLR` para a versão de área de trabalho do CLR ou `COR_PRF_CORE_CLR` para a versão principal do CLR usado no Silverlight.  
  
 `pMajorVersion`  
 fora O número de versão principal do CLR.  
  
 `pMinorVersion`  
 fora O número de versão secundária do CLR.  
  
 `pBuildVersion`  
 fora O número de versão de compilação do CLR.  
  
 `pQFEVersion`  
 fora O número de versão do CLR associado a uma atualização de software.  
  
 `cchVersionString`  
 no O comprimento, em caracteres, do buffer que `szVersionString` aponta para.  
  
 `pcchVersionString`  
 fora O comprimento, em caracteres, de `szVersionString` .  
  
 `szVersionString`  
 fora A cadeia de caracteres da versão do CLR.  
  
## <a name="remarks"></a>Comentários  
 Você pode passar NULL para qualquer parâmetro. No entanto, `pcchVersionString` não pode ser nulo, a menos que `szVersionString` também seja nulo.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo3](icorprofilerinfo3-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Criação de perfil](index.md)
