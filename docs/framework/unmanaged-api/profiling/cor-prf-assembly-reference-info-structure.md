---
title: Estrutura COR_PRF_ASSEMBLY_REFERENCE_INFO
ms.date: 03/30/2017
dev_langs:
- cpp
ms.assetid: c8c1d916-8d1a-4f82-8128-9fd3732383fc
ms.openlocfilehash: 861b31e5621e9a7b40403d249c6a5c8c4ac25db8
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501034"
---
# <a name="cor_prf_assembly_reference_info-structure"></a>Estrutura COR_PRF_ASSEMBLY_REFERENCE_INFO
[Com suporte no .NET Framework 4.5.2 e versões posteriores]  
  
 Fornece ao Common Language Runtime informações sobre uma referência de assembly que deve ser considerada ao realizar um exame de fechamento de referência de assembly.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef struct _COR_PRF_ASSEMBLY_REFERENCE_INFO {  
    void* pbPublicKeyOrToken;  
    ULONG cbPublicKeyOrToken;  
    LPCWSTR szName;  
    ASSEMBLYMETADATA* pMetaData;  
    void* pbHashValue;  
    ULONG cbHashValue;  
    DWORD dwAssemblyRefFlags;  
} COR_PRF_EX_CLAUSE_INFO;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`pbPublicKeyOrToken`|Um ponteiro para a chave pública ou token do assembly.|  
|`cbPublicKeyOrToken`|O número de bytes na chave pública ou token.|  
|`szName`|O nome do assembly referenciado.|  
|`pMetaData`|Um ponteiro para os metadados do assembly.|  
|`pbHashValue`|Um ponteiro para um BLOB (objeto binário grande) de hash.|  
|`cbHashValue`|O número de bytes no BLOB de hash.|  
|`dwAssemblyRefFlags`|Os sinalizadores do assembly.|  
  
## <a name="remarks"></a>Comentários  
 A estrutura `COR_PRF_EX_CLAUSE_INFO` é preenchida pelo criador de perfil ao declarar as referências adicionais de assembly que o Common Language Runtime deve considerar quando realiza um exame de fechamento de referência de assembly.  
  
 Se o criador de perfil se registrar para o método de retorno de chamada [ICorProfilerCallback6:: GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md) , o tempo de execução passará o caminho e o nome do assembly a ser carregado, juntamente com um ponteiro para um objeto de interface [ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md) para esse método. O criador de perfil pode então chamar o método [ICorProfilerAssemblyReferenceProvider:: AddAssemblyReference](icorprofilerassemblyreferenceprovider-addassemblyreference-method.md) com um `COR_PRF_ASSEMBLY_REFERENCE_INFO` objeto para cada assembly de destino que planeja fazer referência a partir do assembly especificado no retorno de chamada [ICorProfilerCallback6:: GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md) .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Estruturas de criação de perfil](profiling-structures.md)
- [Método GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md)
- [Método AddAssemblyReference](icorprofilerassemblyreferenceprovider-addassemblyreference-method.md)
