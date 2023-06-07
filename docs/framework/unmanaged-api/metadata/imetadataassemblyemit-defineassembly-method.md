---
title: Método IMetaDataAssemblyEmit::DefineAssembly
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyEmit.DefineAssembly
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyEmit::DefineAssembly
helpviewer_keywords:
- IMetaDataAssemblyEmit::DefineAssembly method [.NET Framework metadata]
- DefineAssembly method [.NET Framework metadata]
ms.assetid: a0637d66-74bf-4f2d-8137-9ff838bccece
topic_type:
- apiref
ms.openlocfilehash: 17c91200730431c4c6e230b8c1561ce7c4863868
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008177"
---
# <a name="imetadataassemblyemitdefineassembly-method"></a>Método IMetaDataAssemblyEmit::DefineAssembly
Cria uma `Assembly` estrutura que contém metadados para o assembly especificado e retorna o token de metadados associado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT DefineAssembly (  
    [in]  void                 *pbPublicKey,  
    [in]  ULONG                cbPublicKey,  
    [in]  ULONG                uHashAlgId,  
    [in]  LPCWSTR              szName,
    [in]  ASSEMBLYMETADATA     *pMetaData,  
    [in]  DWORD                dwAssemblyFlags,  
    [out] mdAssembly           *pmda  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pbPublicKey`  
 no A chave pública que identifica o editor do assembly ou NULL se o assembly não tiver um nome forte.  
  
 `cbPublicKey`  
 no O tamanho em bytes de `pbPublicKey` .  
  
 `uHashAlgId`  
 no O identificador do algoritmo de hash a ser usado para criptografar os arquivos no assembly ou nulo para especificar o algoritmo SHA-1.  
  
 `szName`  
 no O nome de texto legível por humanos do assembly. Esse valor não deve exceder 1024 caracteres.  
  
 `pMetaData`  
 no Um ponteiro para uma instância ASSEMBLYMETADATA que contém a versão, a plataforma e as informações de localidade para o assembly.  
  
 `dwAssemblyFlags`  
 no Uma combinação de valores [CorAssemblyFlags](corassemblyflags-enumeration.md) que descrevem os recursos do assembly.  
  
 `pmda`  
 fora Um ponteiro para o token de metadados.  
  
## <a name="remarks"></a>Comentários  
 Somente uma `Assembly` estrutura de metadados pode ser definida em um manifesto.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataAssemblyEmit](imetadataassemblyemit-interface.md)
