---
title: Método IMetaDataImport::FindMemberRef
ms.date: 03/30/2017
api_name:
- IMetaDataImport.FindMemberRef
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::FindMemberRef
helpviewer_keywords:
- IMetaDataImport::FindMemberRef method [.NET Framework metadata]
- FindMemberRef method [.NET Framework metadata]
ms.assetid: 1ccda329-d752-4d89-abe8-511af3c3f4c9
topic_type:
- apiref
ms.openlocfilehash: 068014732cee91147edaec29fa0f954a741d8b5c
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84491622"
---
# <a name="imetadataimportfindmemberref-method"></a>Método IMetaDataImport::FindMemberRef
Obtém um ponteiro para o token de MemberRef para a referência de membro que está entre o especificado <xref:System.Type> e que tem o nome e a assinatura de metadados especificados.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT FindMemberRef (  
   [in]  mdTypeRef          td,  
   [in]  LPCWSTR            szName,
   [in]  PCCOR_SIGNATURE    pvSigBlob,
   [in]  ULONG              cbSigBlob,
   [out] mdMemberRef        *pmr  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `td`  
 no O token TypeRef para a classe ou interface que inclui a referência de membro a ser pesquisada. Se esse valor for `mdTokenNil` , a pesquisa será feita para uma variável global ou uma referência de função global.  
  
 `szName`  
 no O nome da referência de membro a ser pesquisada.  
  
 `pvSigBlob`  
 no Um ponteiro para a assinatura de metadados binários da referência de membro.  
  
 `cbSigBlob`  
 no O tamanho em bytes de `pvSigBlob` .  
  
 `pmr`  
 fora Um ponteiro para o token de MemberRef correspondente.  
  
## <a name="remarks"></a>Comentários  
 Você especifica o membro usando sua classe ou interface delimitadora ( `td` ), seu nome ( `szName` ) e, opcionalmente, sua assinatura ( `pvSigBlob` ).  
  
 A assinatura passada para `FindMemberRef` deve ter sido gerada no escopo atual, porque as assinaturas estão associadas a um escopo específico. Uma assinatura pode inserir um token que identifica a classe de circunscrição ou o tipo de valor. O token é um índice na tabela de TypeDef local. Você não pode criar uma assinatura de tempo de execução fora do contexto do escopo atual e usar essa assinatura como entrada para `FindMemberRef` .  
  
 `FindMemberRef`localiza somente referências de membros que foram definidas diretamente na classe ou na interface; Ele não localiza referências de membros herdadas.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
