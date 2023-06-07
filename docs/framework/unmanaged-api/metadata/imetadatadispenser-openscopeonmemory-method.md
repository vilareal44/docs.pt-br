---
title: Método IMetaDataDispenser::OpenScopeOnMemory
ms.date: 03/30/2017
api_name:
- IMetaDataDispenser.OpenScopeOnMemory
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataDispenser::OpenScopeOnMemory
helpviewer_keywords:
- OpenScopeOnMemory method [.NET Framework metadata]
- IMetaDataDispenser::OpenScopeOnMemory method [.NET Framework metadata]
ms.assetid: 14218249-bdec-48ae-b5fc-9f57f7ca8501
topic_type:
- apiref
ms.openlocfilehash: 69e5e05012d2b44a76a986591ec990f66bf8ae20
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007319"
---
# <a name="imetadatadispenseropenscopeonmemory-method"></a>Método IMetaDataDispenser::OpenScopeOnMemory
Abre uma área de memória que contém os metadados existentes. Ou seja, esse método abre uma área especificada de memória na qual os dados existentes são tratados como metadados.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT OpenScopeOnMemory (  
    [in]  LPCVOID     pData,
    [in]  ULONG       cbData,
    [in]  DWORD       dwOpenFlags,
    [in]  REFIID      riid,
    [out] IUnknown    **ppIUnk  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pData`  
 no Um ponteiro que especifica o endereço inicial da área de memória.  
  
 `cbData`  
 no O tamanho da área de memória, em bytes.  
  
 `dwOpenFlags`  
 no Um valor da enumeração [CorOpenFlags](coropenflags-enumeration.md) para especificar o modo (leitura, gravação e assim por diante) para abertura.  
  
 `riid`  
 no A IID da interface de metadados desejada a ser retornada; o chamador usará a interface para importar (ler) ou emitir (gravar) metadados.  
  
 O valor de `riid` deve especificar uma das interfaces "Import" ou "Emit". Os valores válidos são IID_IMetaDataEmit, IID_IMetaDataImport, IID_IMetaDataAssemblyEmit, IID_IMetaDataAssemblyImport, IID_IMetaDataEmit2 ou IID_IMetaDataImport2.  
  
 `ppIUnk`  
 fora O ponteiro para a interface retornada.  
  
## <a name="remarks"></a>Comentários  
 A cópia na memória dos metadados pode ser consultada usando métodos de uma das interfaces "Import" ou adicionadas ao uso de métodos de uma das interfaces "Emit".  
  
 O `OpenScopeOnMemory` método é semelhante ao método [IMetaDataDispenser:: OpenScope](imetadatadispenser-openscope-method.md) , exceto que os metadados de interesse já existem na memória, em vez de em um arquivo no disco.  
  
 Se a área de destino da memória não contiver metadados de Common Language Runtime (CLR), o `OpenScopeOnMemory` método falhará.  
  
## <a name="requirements"></a>Requisitos  
 **Plataforma:** Consulte [requisitos do sistema](../../get-started/system-requirements.md).  
  
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
