---
title: Método IMetaDataImport::FindMethod
ms.date: 03/30/2017
api_name:
- IMetaDataImport.FindMethod
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::FindMethod
helpviewer_keywords:
- FindMethod method [.NET Framework metadata]
- IMetaDataImport::FindMethod method [.NET Framework metadata]
ms.assetid: 0f9bde1d-e306-438d-941b-d0925b322304
topic_type:
- apiref
ms.openlocfilehash: c2ec907759a25048444ebcc81bf5bb0fd23ced58
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503647"
---
# <a name="imetadataimportfindmethod-method"></a>Método IMetaDataImport::FindMethod
Obtém um ponteiro para o token MethodDef do método que está incluído pelo especificado <xref:System.Type> e que tem o nome e a assinatura de metadados especificados.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT FindMethod (  
   [in]  mdTypeDef          td,  
   [in]  LPCWSTR            szName,
   [in]  PCCOR_SIGNATURE    pvSigBlob,
   [in]  ULONG              cbSigBlob,
   [out] mdMethodDef        *pmb  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `td`  
 no O `mdTypeDef` token do tipo (uma classe ou interface) que inclui o membro a ser pesquisado. Se esse valor for `mdTokenNil` , a pesquisa será feita para uma função global.  
  
 `szName`  
 no O nome do método a ser pesquisado.  
  
 `pvSigBlob`  
 no Um ponteiro para a assinatura de metadados binários do método.  
  
 `cbSigBlob`  
 no O tamanho em bytes de `pvSigBlob` .  
  
 `pmb`  
 fora Um ponteiro para o token MethodDef correspondente.  
  
## <a name="remarks"></a>Comentários  
 Você especifica o método usando sua classe ou interface de delimitadora ( `td` ), seu nome ( `szName` ) e, opcionalmente, sua assinatura ( `pvSigBlob` ). Pode haver vários métodos com o mesmo nome em uma classe ou interface. Nesse caso, passe a assinatura do método para localizar a correspondência exclusiva.  
  
 A assinatura passada para `FindMethod` deve ter sido gerada no escopo atual, porque as assinaturas estão associadas a um escopo específico. Uma assinatura pode inserir um token que identifica a classe de circunscrição ou o tipo de valor. O token é um índice na tabela de TypeDef local. Você não pode criar uma assinatura de tempo de execução fora do contexto do escopo atual e usar essa assinatura como entrada para entrada no `FindMethod` .  
  
 `FindMethod`localiza somente os métodos que foram definidos diretamente na classe ou interface; Ele não encontra os métodos herdados.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Reflection.MethodInfo>
- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
