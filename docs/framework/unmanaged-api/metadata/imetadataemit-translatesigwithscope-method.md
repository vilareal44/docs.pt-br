---
title: Método IMetaDataEmit::TranslateSigWithScope
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.TranslateSigWithScope
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::TranslateSigWithScope
helpviewer_keywords:
- TranslateSigWithScope method [.NET Framework metadata]
- IMetaDataEmit::TranslateSigWithScope method [.NET Framework metadata]
ms.assetid: 47915d33-b7bf-409e-b484-4ee1df15de22
topic_type:
- apiref
ms.openlocfilehash: 7ef6dbc46806febc6fba89b39a8b894377225c23
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84003940"
---
# <a name="imetadataemittranslatesigwithscope-method"></a>Método IMetaDataEmit::TranslateSigWithScope
Importa um assembly para o escopo atual e obtém uma nova assinatura de metadados para o escopo mesclado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT TranslateSigWithScope (
    [in]  IMetaDataAssemblyImport   *pAssemImport,
    [in]  const void                *pbHashValue,
    [in]  ULONG                     cbHashValue,
    [in]  IMetaDataImport           *import,
    [in]  PCCOR_SIGNATURE           pbSigBlob,
    [in]  ULONG                     cbSigBlob,  
    [in]  IMetaDataAssemblyEmit     *pAssemEmit,
    [in]  IMetaDataEmit             *emit,
    [out] PCOR_SIGNATURE            pvTranslatedSig,
    [in]  ULONG                     cbTranslatedSigMax,
    [out] ULONG                     *pcbTranslatedSig
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pAssemImport`  
 no A interface para importar assembly (onde a assinatura é definida).  
  
 `pbHashValue`  
 no O blob de hash para o assembly.  
  
 `cbHashValue`  
 no A contagem de bytes em `pbHashValue` .  
  
 `import`  
 no A interface para o escopo de metadados de importação.  
  
 `pbSigBlob`  
 no A assinatura a ser importada.  
  
 `cbSigBlob`  
 no O tamanho, em bytes, de `pbSigBlob` .  
  
 `pAssemEmit`  
 no A interface para o assembly de exportação.  
  
 `emit`  
 no A interface para o escopo de metadados de exportação.  
  
 `pvTranslatedSig`  
 fora O buffer para armazenar o blob de assinatura traduzido.  
  
 `cbTranslatedSigMax`  
 no A capacidade, em bytes, de `pvTranslatedSig` .  
  
 `pcbTranslatedSig`  
 fora O número de bytes reais na assinatura traduzida.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataAssemblyEmit](imetadataassemblyemit-interface.md)
- [Interface IMetaDataAssemblyImport](imetadataassemblyimport-interface.md)
- [Interface IMetaDataEmit](imetadataemit-interface.md)
- [Interface IMetaDataEmit2](imetadataemit2-interface.md)
- [Interface IMetaDataImport](imetadataimport-interface.md)
