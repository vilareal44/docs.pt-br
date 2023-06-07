---
title: Método IMetaDataDispenser::OpenScope
ms.date: 03/30/2017
api_name:
- IMetaDataDispenser.OpenScope
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataDispenser::OpenScope
helpviewer_keywords:
- IMetaDataDispenser::OpenScope method [.NET Framework metadata]
- OpenScope method, IMetaDataDispenser interface [.NET Framework metadata]
ms.assetid: 65063ad5-e0d9-4c01-8f8b-9a5950109fa6
topic_type:
- apiref
ms.openlocfilehash: 8d9de753f1c44338a96e990def80643d591f2a8b
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007462"
---
# <a name="imetadatadispenseropenscope-method"></a>Método IMetaDataDispenser::OpenScope
Abre um arquivo existente em disco e mapeia seus metadados na memória.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT OpenScope (  
    [in]  LPCWSTR     szScope,
    [in]  DWORD       dwOpenFlags,
    [in]  REFIID      riid,
    [out] IUnknown    **ppIUnk  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `szScope`  
 no O nome do arquivo a ser aberto. O arquivo deve conter metadados de Common Language Runtime (CLR).  
  
 `dwOpenFlags`  
 no Um valor da enumeração [CorOpenFlags](coropenflags-enumeration.md) para especificar o modo (leitura, gravação e assim por diante) para abertura.  
  
 `riid`  
 no A IID da interface de metadados desejada a ser retornada; o chamador usará a interface para importar (ler) ou emitir (gravar) metadados.  
  
 O valor de `riid` deve especificar uma das interfaces "Import" ou "Emit". Os valores válidos são IID_IMetaDataEmit, IID_IMetaDataImport, IID_IMetaDataAssemblyEmit, IID_IMetaDataAssemblyImport, IID_IMetaDataEmit2 ou IID_IMetaDataImport2.  
  
 `ppIUnk`  
 fora O ponteiro para a interface retornada.  
  
## <a name="remarks"></a>Comentários  
 A cópia na memória dos metadados pode ser consultada usando métodos de uma das interfaces "Import" ou adicionadas ao uso de métodos de uma das interfaces "Emit".  
  
 Se o arquivo de destino não contiver metadados CLR, o `OpenScope` método falhará.  
  
 No .NET Framework versão 1,0 e 1,1, se um escopo for aberto com `dwOpenFlags` definido como ofRead, ele será elegível para compartilhamento. Ou seja, se as chamadas subsequentes forem `OpenScope` aprovadas no nome de um arquivo que foi aberto anteriormente, o escopo existente será reutilizado e um novo conjunto de estruturas de dados não será criado. No entanto, podem surgir problemas devido a esse compartilhamento.  
  
 No .NET Framework versão 2,0, os escopos abertos com `dwOpenFlags` set como ofRead não são mais compartilhados. Use o valor ofReadOnly para permitir que o escopo seja compartilhado. Quando um escopo é compartilhado, as consultas que usam interfaces de metadados de "leitura/gravação" falharão.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataDispenser](imetadatadispenser-interface.md)
- [Interface IMetaDataDispenserEx](imetadatadispenserex-interface.md)
- [Interface IMetaDataAssemblyEmit](imetadataassemblyemit-interface.md)
- [Interface IMetaDataAssemblyImport](imetadataassemblyimport-interface.md)
- [Interface IMetaDataEmit](imetadataemit-interface.md)
- [Interface IMetaDataEmit2](imetadataemit2-interface.md)
- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
