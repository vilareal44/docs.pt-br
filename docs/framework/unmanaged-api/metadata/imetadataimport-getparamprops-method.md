---
title: Método IMetaDataImport::GetParamProps
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetParamProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetParamProps
helpviewer_keywords:
- IMetaDataImport::GetParamProps method [.NET Framework metadata]
- GetParamProps method [.NET Framework metadata]
ms.assetid: 4d5e5f00-bcab-4f41-b191-176511a186a7
topic_type:
- apiref
ms.openlocfilehash: c2abf2813c6e1a9db4264bded32d9cb9c58a2bcb
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84491050"
---
# <a name="imetadataimportgetparamprops-method"></a>Método IMetaDataImport::GetParamProps
Obtém valores de metadados para o parâmetro referenciado pelo token ParamDef especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetParamProps (  
   [in]  mdParamDef      tk,  
   [out] mdMethodDef     *pmd,  
   [out] ULONG           *pulSequence,  
   [out] LPWSTR          szName,  
   [in]  ULONG           cchName,  
   [out] ULONG           *pchName,  
   [out] DWORD           *pdwAttr,  
   [out] DWORD           *pdwCPlusTypeFlag,  
   [out] UVCP_CONSTANT   *ppValue,  
   [out] ULONG           *pcchValue  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `tk`  
 no Um token ParamDef que representa o parâmetro para o qual retornar metadados.  
  
 `pmd`  
 fora Um ponteiro para um token MethodDef que representa o método que usa o parâmetro.  
  
 `pulSequence`  
 fora A posição ordinal do parâmetro na lista de argumentos do método.  
  
 `szName`  
 fora Um buffer para armazenar o nome do parâmetro.  
  
 `cchName`  
 no O tamanho solicitado em caracteres largos de `szName` .  
  
 `pchName`  
 fora O tamanho retornado em caracteres largos de `szName` .  
  
 `pdwAttr`  
 fora Um ponteiro para qualquer sinalizador de atributo associado ao parâmetro. Esta é uma bitmask de `CorParamAttr` valores.  
  
 `pdwCPlusTypeFlag`  
 fora Um ponteiro para um sinalizador que especifica que o parâmetro é um <xref:System.ValueType> .  
  
 `ppValue`  
 fora Um ponteiro para uma cadeia de caracteres constante retornada pelo parâmetro.  
  
 `pcchValue`  
 fora O tamanho de `ppValue` , em caracteres largos, ou zero se não `ppValue` mantiver uma cadeia de caracteres.  
  
## <a name="remarks"></a>Comentários

Os valores de sequência em `pulSequence` começam com 1 para parâmetros. Um valor de retorno tem um número de sequência de 0.

## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
