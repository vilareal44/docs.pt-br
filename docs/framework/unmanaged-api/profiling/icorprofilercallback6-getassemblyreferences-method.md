---
title: ICorProfilerCallback6::Método GetAssemblyReferences
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- ICorProfilerCallback6.GetAssemblyReferences
api_location:
- mscorwks.dll
- corprof.idl
api_type:
- COM
ms.assetid: 8b391afb-d79f-41bd-94ce-43ce62c6b5fc
topic_type:
- apiref
ms.openlocfilehash: 5deacbff740ebb1dcc8cb8d1fb7e4eb0d4bdcc30
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499214"
---
# <a name="icorprofilercallback6getassemblyreferences-method"></a>ICorProfilerCallback6::Método GetAssemblyReferences
[Com suporte no .NET Framework 4.5.2 e versões posteriores]  
  
 Notifica o criador de perfis de que um assembly está em um estágio inicial de carregamento, quando o Common Language Runtime realiza um exame de fechamento da referência do assembly.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp
HRESULT GetAssemblyReferences(        [in, string] const WCHAR* wszAssemblyPath,  
        [in] ICorProfilerAssemblyReferenceProvider* pAsmRefProvider  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `wszAssemblyPath`  
 [in] O caminho e o nome do assembly cujos metadados serão modificados.  
  
 `pAsmRefProvider`  
 no Um ponteiro para o endereço de uma interface [ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md) que especifica as referências de assembly a serem adicionadas.  
  
## <a name="return-value"></a>Valor Retornado  
 Os valores retornados desse retorno de chamada são ignorados.  
  
## <a name="remarks"></a>Comentários  
 Esse retorno de chamada é controlado pela definição do sinalizador de máscara de evento [COR_PRF_HIGH_ADD_ASSEMBLY_REFERENCES](cor-prf-high-monitor-enumeration.md) ao chamar o método [ICorProfilerCallback5:: SetEventMask2](icorprofilerinfo5-seteventmask2-method.md) . Se o criador de perfil se registrar para o método de retorno de chamada [ICorProfilerCallback6:: GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md) , o tempo de execução passará o caminho e o nome do assembly a ser carregado, juntamente com um ponteiro para um objeto de interface [ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md) para esse método. O criador de perfil pode então chamar o método [ICorProfilerAssemblyReferenceProvider:: AddAssemblyReference](icorprofilerassemblyreferenceprovider-addassemblyreference-method.md) com um `COR_PRF_ASSEMBLY_REFERENCE_INFO` objeto para cada assembly de destino que planeja fazer referência a partir do assembly especificado no `GetAssemblyReferences` retorno de chamada.  
  
 Use o retorno de chamada `GetAssemblyReferences` apenas se for necessário ao criador de perfis modificar os metadados de um assembly para adicionar referências de assembly. (Mas observe que a modificação real dos metadados de um assembly é feita no método de retorno de chamada [ICorProfilerCallback:: ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md).) O criador de perfil deve implementar o `GetAssemblyReferences` método de retorno de chamada para informar ao Common Language Runtime (CLR) que as referências de assembly serão adicionadas quando o módulo for carregado.  Isso ajuda a garantir que decisões de compartilhamento de assembly tomadas pelo CLR durante esse estágio inicial permanecem válidas apesar de o criador de perfis planejar modificar as referências de metadados do assembly mais tarde.  Isso pode evitar algumas instâncias nas quais as modificações de metadados do criador de perfis causam um erro de `SECURITY_E_INCOMPATIBLE_SHARE`.  
  
 O criador de perfil usa o objeto [ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md) fornecido por esse método para adicionar referências de assembly ao suplemento de referência de assembly CLR.  O objeto [ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md) deve ser usado somente de dentro deste retorno de chamada. Chamadas para o método [ICorProfilerAssemblyReferenceProvider:: AddAssemblyReference](icorprofilerassemblyreferenceprovider-addassemblyreference-method.md) desse retorno de chamada não resultam em metadados modificados, mas apenas em uma movimentação de fechamento de referência de assembly modificada. O criador de perfil ainda terá que usar um objeto [IMetaDataAssemblyEmit](../metadata/imetadataassemblyemit-interface.md) para adicionar explicitamente referências de assembly de dentro do retorno de chamada [ICorProfilerCallback:: ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md) para o assembly de referência, mesmo que ele implemente o `GetAssemblyReferences` retorno de chamada.  
  
 O criador de perfil deve estar preparado para receber chamadas duplicadas para esse retorno de chamada para o mesmo assembly e deve responder de forma idêntica para cada uma dessas chamadas duplicadas (fazendo o mesmo conjunto de chamadas [ICorProfilerAssemblyReferenceProvider:: AddAssemblyReference](icorprofilerassemblyreferenceprovider-addassemblyreference-method.md) ).  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback6](icorprofilercallback6-interface.md)
- [Método ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md)
- [Estrutura COR_PRF_ASSEMBLY_REFERENCE_INFO](cor-prf-assembly-reference-info-structure.md)
- [Interface ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md)
