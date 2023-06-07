---
title: Método ICorProfilerInfo2::GetStaticFieldInfo
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetStaticFieldInfo
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetStaticFieldInfo
helpviewer_keywords:
- ICorProfilerInfo2::GetStaticFieldInfo method [.NET Framework profiling]
- GetStaticFieldInfo method [.NET Framework profiling]
ms.assetid: fc663e76-e23f-49a8-bdd5-52cdf1a3b2b3
topic_type:
- apiref
ms.openlocfilehash: e1dd6addd9053ffb6cf2ce23408673d8fca17cb5
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84496835"
---
# <a name="icorprofilerinfo2getstaticfieldinfo-method"></a>Método ICorProfilerInfo2::GetStaticFieldInfo
Obtém um valor que indica o tipo de estático que se aplica ao campo especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetStaticFieldInfo (  
    [in] ClassID               classId,  
    [in] mdFieldDef            fieldToken,  
    [out] COR_PRF_STATIC_TYPE  *pFieldInfo);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `classId`  
 no A ID da classe na qual o campo estático é definido.  
  
 `fieldToken`  
 no O token de metadados para o campo estático.  
  
 `pFieldInfo`  
 fora Um ponteiro para um valor da enumeração [COR_PRF_STATIC_TYPE](cor-prf-static-type-enumeration.md) que indica se o campo especificado é estático e, nesse caso, o tipo de estático que se aplica ao campo.  
  
## <a name="remarks"></a>Comentários  
 Essas informações podem ser usadas para determinar qual função chamar para obter o endereço do campo estático.  
  
 O código do criador de perfil ainda deve verificar os metadados de um campo estático para garantir que ele realmente tenha um endereço. Literais estáticos (ou seja, constantes) existem somente nos metadados e não têm um endereço.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
- [Interface ICorProfilerInfo2](icorprofilerinfo2-interface.md)
