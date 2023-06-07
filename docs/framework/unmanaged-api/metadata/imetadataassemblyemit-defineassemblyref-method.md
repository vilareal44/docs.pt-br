---
title: Método IMetaDataAssemblyEmit::DefineAssemblyRef
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyEmit.DefineAssemblyRef
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyEmit::DefineAssemblyRef
helpviewer_keywords:
- DefineAssemblyRef method [.NET Framework metadata]
- IMetaDataAssemblyEmit::DefineAssemblyRef method [.NET Framework metadata]
ms.assetid: 0b284b18-0084-4b3a-912a-5ebe9f29c88b
topic_type:
- apiref
ms.openlocfilehash: 612463bca18c23fac0b086adde2d208a0fbc5ae5
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008161"
---
# <a name="imetadataassemblyemitdefineassemblyref-method"></a>Método IMetaDataAssemblyEmit::DefineAssemblyRef
Cria uma `AssemblyRef` estrutura que contém metadados para o assembly ao qual este assembly faz referência e retorna o token de metadados associado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT DefineAssemblyRef (  
    [in]  void                *pbPublicKeyOrToken,  
    [in]  ULONG               cbPublicKeyOrToken,  
    [in]  LPCWSTR             szName,  
    [in]  ASSEMBLYMETADATA    pMetaData,  
    [in]  void                *pbHashValue,  
    [in]  ULONG               cbHashValue,  
    [in]  DWORD               dwAssemblyRefFlags,  
    [out] mdAssemblyRef       *pmdar  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pbPublicKeyOrToken`  
 no A chave pública do Publicador do assembly referenciado. A função auxiliar [StrongNameTokenFromAssembly](../strong-naming/strongnametokenfromassembly-function.md) pode ser usada para obter o hash da chave pública a ser passada como esse parâmetro.  
  
 `cbPublicKeyOrToken`  
 no O tamanho em bytes de `pbPublicKeyOrToken` .  
  
 `szName`  
 no O nome de texto legível por humanos do assembly. Esse valor não deve exceder 1024 caracteres.  
  
 `pMetaData`  
 no Uma instância de ASSEMBLYMETADATA que contém a versão, a plataforma e as informações de localidade do assembly referenciado.  
  
 `pbHashValue`  
 no Os dados de hash associados ao assembly referenciado. Opcional.  
  
 `cbHashValue`  
 no O tamanho em bytes de `pbHashValue` .  
  
 `dwAssemblyRefFlags`  
 no Uma combinação bits de valores [CorAssemblyFlags](corassemblyflags-enumeration.md) que influencia o comportamento do mecanismo de execução.  
  
 `pmdar`  
 fora Um ponteiro para o `AssemblyRef` token de metadados retornado.  
  
## <a name="remarks"></a>Comentários  
 Uma `AssemblyRef` estrutura de metadados deve ser definida para cada assembly referenciado por esse assembly.  
  
 Em tempo de execução, os detalhes de um assembly referenciado são passados para o resolvedor de assembly com uma indicação de que eles representam as informações "como criadas". Em seguida, o resolvedor de assembly aplica a política.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataAssemblyEmit](imetadataassemblyemit-interface.md)
