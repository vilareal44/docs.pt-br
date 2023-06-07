---
title: ICorProfilerInfo7::ReadInMemorySymbols
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo7.ReadInMemorySymbols
api_location:
- CorProf.idl
- CorProf.h
- CorGuids.lib
api_type:
- COM
ms.assetid: 1745a0b9-8332-4777-a670-b549bff3b901
ms.openlocfilehash: 6732457220d795bbf8ae54277ef9f5c07cf96359
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84495353"
---
# <a name="icorprofilerinfo7readinmemorysymbols"></a>ICorProfilerInfo7::ReadInMemorySymbols
[Com suporte no .NET Framework 4.6.1 e versões posteriores]  
  
 Lê bytes de um fluxo de símbolo na memória.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT ReadInMemorySymbols(  
        [in] ModuleID moduleId,  
        [in] DWORD symbolsReadOffset,  
        [out] BYTE* pSymbolBytes,  
        [in] DWORD countSymbolBytes,  
        [out] DWORD* pCountSymbolBytesRead  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `moduleId`  
 no O identificador do módulo que contém o fluxo na memória.  
  
 `symbolsReadOffset`  
 no O deslocamento dentro do fluxo na memória no qual começar a ler os bytes.  
  
 `pSymbolBytes`  
 fora Um ponteiro para o buffer para o qual os dados serão copiados. O buffer deve ter `countSymbolBytes` espaço disponível.  
  
 `countSymbolBytes`  
 no O número de bytes a serem copiados.  
  
 `pCountSymbolBytesRead`  
 fora Quando o método retorna, contém o número real de bytes lidos.  
  
## <a name="return-value"></a>Valor Retornado  
 `S_OK`, se um número diferente de zero de bytes tiver sido lido.  
  
 `CORPROF_E_MODULE_IS_DYNAMIC`, se o módulo foi criado usando <xref:System.Reflection.Emit> .  
  
## <a name="remarks"></a>Comentários  
 O `ReadInMemorySymbols` método tenta ler os `countSymbolBytes` dados começando no deslocamento `symbolsReadOffset` dentro do fluxo na memória. Os dados são copiados para o `pSymbolBytes` , que deve ter `countSymbolBytes` espaço disponível.     `pCountSymbolsBytesRead`contém o número real de bytes lidos, que pode ser menor que `countSymbolBytes` se o final do fluxo for atingido.  
  
> [!NOTE]
> A implementação atual não oferece suporte a Reflection. Emit. Se o módulo foi criado usando Reflection. Emit, o método retornará `CORPROF_E_MODULE_IS_DYNAMIC` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v461plus](../../../../includes/net-current-v461plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo7](icorprofilerinfo7-interface.md)
