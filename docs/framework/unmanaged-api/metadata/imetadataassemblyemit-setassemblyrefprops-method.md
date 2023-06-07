---
title: Método IMetaDataAssemblyEmit::SetAssemblyRefProps
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyEmit.SetAssemblyRefProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyEmit::SetAssemblyRefProps
helpviewer_keywords:
- SetAssemblyRefProps method [.NET Framework metadata]
- IMetaDataAssemblyEmit::SetAssemblyRefProps method [.NET Framework metadata]
ms.assetid: 70a32bf3-9051-4f96-ae87-11356d06a073
topic_type:
- apiref
ms.openlocfilehash: fb381a872cbeb787da0c6920f2cdeef434fb33ea
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008086"
---
# <a name="imetadataassemblyemitsetassemblyrefprops-method"></a>Método IMetaDataAssemblyEmit::SetAssemblyRefProps
Modifica a estrutura de `AssemblyRef` metadados especificada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetAssemblyRefProps (  
    [in] mdAssemblyRef              ar,  
    [in] const void                 *pbPublicKeyOrToken,  
    [in] ULONG                      cbPublicKeyOrToken,  
    [in] LPCWSTR                    szName,
    [in] const ASSEMBLYMETADATA     *pMetaData,
    [in] const void                 *pbHashValue,  
    [in] ULONG                      cbHashValue,  
    [in] DWORD                      dwAssemblyRefFlags  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `ar`  
 no O token de metadados que especifica a `AssemblyRef` estrutura de metadados a ser modificada.  
  
 `pbPublicKeyOrToken`  
 no A chave pública do Publicador do assembly referenciado.  
  
 `cbPublicKeyOrToken`  
 no O tamanho em bytes de `pbPublicKeyOrToken` .  
  
 `szName`  
 no O nome de texto legível por humanos do assembly.  
  
 `pMetaData`  
 no Um ponteiro para uma instância ASSEMBLYMETADATA que contém a versão, a plataforma e as informações de localidade para o assembly.  
  
 `pbHashValue`  
 no Um ponteiro para os dados de hash associados ao assembly.  
  
 `cbHashValue`  
 no O tamanho em bytes de `pbHashValue` .  
  
 `dwAssemblyRefFlags`  
 no Uma combinação de bits de valores [AssemblyRefFlags](assemblyrefflags-enumeration.md) que especifica atributos do assembly referenciado.  
  
## <a name="remarks"></a>Comentários  
 Para criar uma `AssemblyRef` estrutura de metadados, use o método [IMetaDataAssemblyEmit::D efineassemblyref](imetadataassemblyemit-defineassemblyref-method.md) .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataAssemblyEmit](imetadataassemblyemit-interface.md)
