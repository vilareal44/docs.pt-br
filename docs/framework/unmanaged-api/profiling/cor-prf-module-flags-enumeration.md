---
title: Enumeração COR_PRF_MODULE_FLAGS
ms.date: 03/30/2017
api_name:
- COR_PRF_MODULE_FLAGS
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_MODULE_FLAGS
helpviewer_keywords:
- COR_PRF_MODULE_FLAGS enumeration [.NET Framework profiling]
ms.assetid: 7bc3a938-0df1-4739-9ff1-89cff454b704
topic_type:
- apiref
ms.openlocfilehash: 12e7faa8d9fee7698de9d9734f522d818f225c84
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500813"
---
# <a name="cor_prf_module_flags-enumeration"></a>Enumeração COR_PRF_MODULE_FLAGS
Especifica as propriedades de um módulo.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum  
{  
    COR_PRF_MODULE_DISK             = 0x00000001,  
    COR_PRF_MODULE_NGEN             = 0x00000002,  
    COR_PRF_MODULE_DYNAMIC          = 0x00000004,  
    COR_PRF_MODULE_COLLECTIBLE      = 0x00000008,  
    COR_PRF_MODULE_RESOURCE         = 0x00000010,  
    COR_PRF_MODULE_FLAT_LAYOUT      = 0x00000020,  
    COR_PRF_MODULE_WINDOWS_RUNTIME  = 0x00000040  
}   COR_PRF_MODULE_FLAGS;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|COR_PRF_MODULE_DISK|O módulo foi carregado do disco.|  
|COR_PRF_MODULE_NGEN|O módulo foi gerado pelo gerador de imagem nativa (NGen. exe).|  
|COR_PRF_MODULE_DYNAMIC|O módulo foi criado por métodos no <xref:System.Reflection.Emit?displayProperty=nameWithType> namespace.|  
|COR_PRF_MODULE_COLLECTIBLE|O tempo de vida do módulo é gerenciado pelo coletor de lixo.|  
|COR_PRF_MODULE_RESOURCE|O módulo não contém metadados e é usado estritamente como um recurso. O equivalente gerenciado desse bit é o <xref:System.Reflection.Module.IsResource%2A?displayProperty=nameWithType> método.|  
|COR_PRF_MODULE_FLAT_LAYOUT|O layout do módulo na memória é simples, não mapeado. Se um módulo tiver esse conjunto de bits, os infileres que lêem informações diretamente do cabeçalho do arquivo executável portátil (PE) precisarão ser cuidadosos ao interpretar RVAs (endereços virtuais relativos) no cabeçalho.|  
|COR_PRF_MODULE_WINDOWS_RUNTIME|O sinalizador de tipo de conteúdo Windows Runtime é definido nos metadados para o assembly do módulo. Esse é o caso para todos os módulos de metadados do Windows (. winmd).|  
  
## <a name="remarks"></a>Comentários  
 Bits de COR_PRF_MODULE_FLAGS são retornados para o criador de perfil no `pdwModuleFlags` parâmetro de saída do método [ICorProfilerInfo3:: GetModuleInfo2](icorprofilerinfo3-getmoduleinfo2-method.md) . Algumas combinações de dois ou mais sinalizadores são possíveis, mas nem todas as combinações são possíveis.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Criando perfil de enumerações](profiling-enumerations.md)
