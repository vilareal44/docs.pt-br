---
title: Método IMetaDataImport::FindField
ms.date: 03/30/2017
api_name:
- IMetaDataImport.FindField
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::FindField
helpviewer_keywords:
- IMetaDataImport::FindField method [.NET Framework metadata]
- FindField method [.NET Framework metadata]
ms.assetid: 38cd4e16-fbb2-471c-aa73-ac51a1931ad2
topic_type:
- apiref
ms.openlocfilehash: 11ea6e468909ea42e38bdc7b76c60c460c98025e
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503660"
---
# <a name="imetadataimportfindfield-method"></a>Método IMetaDataImport::FindField
Obtém um ponteiro para o token FieldDef do campo que está incluído pelo especificado <xref:System.Type> e que tem o nome e a assinatura de metadados especificados.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT FindField (  
   [in]  mdTypeDef         td,  
   [in]  LPCWSTR           szName,  
   [in]  PCCOR_SIGNATURE   pvSigBlob,  
   [in]  ULONG             cbSigBlob,  
   [out] mdFieldDef        *pmb  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `td`  
 no O token de TypeDef para a classe ou interface que inclui o campo a ser pesquisado. Se esse valor for `mdTokenNil` , a pesquisa será feita para uma variável global.  
  
 `szName`  
 no O nome do campo a ser pesquisado.  
  
 `pvSigBlob`  
 no Um ponteiro para a assinatura de metadados binários do campo.  
  
 `cbSigBlob`  
 no O tamanho em bytes de `pvSigBlob` .  
  
 `pmb`  
 fora Um ponteiro para o token FieldDef correspondente.  
  
## <a name="remarks"></a>Comentários  
 Você especifica o campo usando sua classe ou interface delimitadora ( `td` ), seu nome ( `szName` ) e, opcionalmente, sua assinatura ( `pvSigBlob` ).  
  
 A assinatura passada para `FindField` deve ter sido gerada no escopo atual, porque as assinaturas estão associadas a um escopo específico. Uma assinatura pode inserir um token que identifica a classe de circunscrição ou o tipo de valor. (O token é um índice na tabela de TypeDef local). Você não pode criar uma assinatura de tempo de execução fora do contexto do escopo atual e usar essa assinatura como entrada para `FindField` .  
  
 `FindField`localiza somente os campos que foram definidos diretamente na classe ou interface; Ele não localiza campos herdados.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
