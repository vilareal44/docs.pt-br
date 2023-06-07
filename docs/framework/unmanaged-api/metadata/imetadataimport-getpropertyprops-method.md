---
title: Método IMetaDataImport::GetPropertyProps
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetPropertyProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetPropertyProps
helpviewer_keywords:
- GetPropertyProps method [.NET Framework metadata]
- IMetaDataImport::GetPropertyProps method [.NET Framework metadata]
ms.assetid: dc0ff3e6-7e7d-4f6c-948d-52b28f5cb78c
topic_type:
- apiref
ms.openlocfilehash: cac5aaa7ed13b6a48b36ad550da8b73d0deb2ee7
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84491037"
---
# <a name="imetadataimportgetpropertyprops-method"></a>Método IMetaDataImport::GetPropertyProps
Obtém os metadados para a propriedade representada pelo token especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetPropertyProps (  
   [in]  mdProperty        prop,  
   [out] mdTypeDef         *pClass,
   [out] LPCWSTR           szProperty,
   [in]  ULONG             cchProperty,
   [out] ULONG             *pchProperty,
   [out] DWORD             *pdwPropFlags,
   [out] PCCOR_SIGNATURE   *ppvSig,
   [out] ULONG             *pbSig,
   [out] DWORD             *pdwCPlusTypeFlag,
   [out] UVCP_CONSTANT     *ppDefaultValue,  
   [out] ULONG             *pcchDefaultValue,  
   [out] mdMethodDef       *pmdSetter,
   [out] mdMethodDef       *pmdGetter,
   [out] mdMethodDef       rmdOtherMethod[],  
   [in]  ULONG             cMax,
   [out] ULONG             *pcOtherMethod
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `prop`  
 no Um token que representa a propriedade para a qual retornar metadados.  
  
 `pClass`  
 fora Um ponteiro para o token de TypeDef que representa o tipo que implementa a propriedade.  
  
 `szProperty`  
 fora Um buffer para armazenar o nome da propriedade.  
  
 `cchProperty`  
 no O tamanho em caracteres largos de `szProperty` .  
  
 `pchProperty`  
 fora O número de caracteres largos retornados em `szProperty` .  
  
 `pdwPropFlags`  
 fora Um ponteiro para qualquer sinalizador de atributo aplicado à propriedade. Esse valor é um bitmask da enumeração [CorPropertyAttr](corpropertyattr-enumeration.md) .  
  
 `ppvSig`  
 fora Um ponteiro para a assinatura de metadados da propriedade.  
  
 `pbSig`  
 fora O número de bytes retornados em `ppvSig` .  
  
 `pdwCPlusTypeFlag`  
 fora Um sinalizador que especifica o tipo da constante que é o valor padrão da propriedade. Esse valor é da enumeração CorElementType.  
  
 `ppDefaultValue`  
 fora Um ponteiro para os bytes que armazenam o valor padrão para essa propriedade.  
  
 `pcchDefaultValue`  
 fora O tamanho em caracteres largos de `ppDefaultValue` , se `pdwCPlusTypeFlag` for ELEMENT_TYPE_STRING; caso contrário, esse valor não será relevante. Nesse caso, o comprimento de `ppDefaultValue` é inferido do tipo especificado por `pdwCPlusTypeFlag` .  
  
 `pmdSetter`  
 fora Um ponteiro para o token MethodDef que representa o método de acessador set para a propriedade.  
  
 `pmdGetter`  
 fora Um ponteiro para o token MethodDef que representa o método acessador get para a propriedade.  
  
 `rmdOtherMethod`  
 fora Uma matriz de tokens MethodDef que representa outros métodos associados à propriedade.  
  
 `cMax`  
 no O tamanho máximo da `rmdOtherMethod` matriz. Se você não fornecer uma matriz grande o suficiente para manter todos os métodos, eles serão ignorados sem aviso.  
  
 `pcOtherMethod`  
 fora O número de tokens MethodDef retornados em `rmdOtherMethod` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
