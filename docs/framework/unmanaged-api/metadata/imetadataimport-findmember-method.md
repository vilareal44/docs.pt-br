---
title: Método IMetaDataImport::FindMember
ms.date: 03/30/2017
api_name:
- IMetaDataImport.FindMember
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::FindMember
helpviewer_keywords:
- IMetaDataImport::FindMember method [.NET Framework metadata]
- FindMember method [.NET Framework metadata]
ms.assetid: ad32fb84-c2b6-41cd-888d-787ff3a90449
topic_type:
- apiref
ms.openlocfilehash: fce4f3875fbdb110134d6b7f684eff40821f6bdd
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503671"
---
# <a name="imetadataimportfindmember-method"></a>Método IMetaDataImport::FindMember
Obtém um ponteiro para o token MemberDef para o campo ou método que está incluído pelo especificado <xref:System.Type> e que tem o nome especificado e a assinatura de metadados.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT FindMember (  
   [in]  mdTypeDef         td,  
   [in]  LPCWSTR           szName,
   [in]  PCCOR_SIGNATURE   pvSigBlob,
   [in]  ULONG             cbSigBlob,
   [out] mdToken           *pmb  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `td`  
 no O token de TypeDef para a classe ou interface que inclui o membro a ser pesquisado. Se esse valor for `mdTokenNil` , a pesquisa será feita para uma global-Variable ou uma função global.  
  
 `szName`  
 no O nome do membro a ser pesquisado.  
  
 `pvSigBlob`  
 no Um ponteiro para a assinatura de metadados binários do membro.  
  
 `cbSigBlob`  
 no O tamanho em bytes de `pvSigBlob` .  
  
 `pmb`  
 fora Um ponteiro para o token MemberDef correspondente.  
  
## <a name="remarks"></a>Comentários  
 Você especifica o membro usando sua classe ou interface delimitadora ( `td` ), seu nome ( `szName` ) e, opcionalmente, sua assinatura ( `pvSigBlob` ). Pode haver vários membros com o mesmo nome em uma classe ou interface. Nesse caso, passe a assinatura do membro para localizar a correspondência exclusiva.  
  
 A assinatura passada para `FindMember` deve ter sido gerada no escopo atual, porque as assinaturas estão associadas a um escopo específico. Uma assinatura pode inserir um token que identifica a classe de circunscrição ou o tipo de valor. O token é um índice na tabela de TypeDef local. Você não pode criar uma assinatura de tempo de execução fora do contexto do escopo atual e usar essa assinatura como entrada para entrada no `FindMember` .  
  
 `FindMember`localiza somente os membros que foram definidos diretamente na classe ou interface; Ele não encontra membros herdados.  
  
> [!NOTE]
> `FindMember`é um método auxiliar. Ele chama [IMetaDataImport:: FindMethod](imetadataimport-findmethod-method.md); Se essa chamada não encontrar uma correspondência, o `FindMember` chamará [IMetaDataImport:: FindField](imetadataimport-findfield-method.md).  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
