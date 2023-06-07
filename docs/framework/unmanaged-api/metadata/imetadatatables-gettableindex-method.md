---
title: Método IMetaDataTables::GetTableIndex
ms.date: 03/30/2017
api_name:
- IMetaDataTables.GetTableIndex
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataTables::GetTableIndex
helpviewer_keywords:
- GetTableIndex method [.NET Framework metadata]
- IMetaDataTables::GetTableIndex method [.NET Framework metadata]
ms.assetid: c6ec3800-e0d9-4387-afb8-ddc0b818114c
topic_type:
- apiref
ms.openlocfilehash: 3638ab12fc311ece9f24608cbb36219e10f01f2d
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501153"
---
# <a name="imetadatatablesgettableindex-method"></a>Método IMetaDataTables::GetTableIndex
Obtém o índice da tabela referenciada pelo token especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetTableIndex (  
    [in]  ULONG   token,  
    [out] ULONG   *pixTbl  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `token`  
 no O token que faz referência à tabela.  
  
 `pixTbl`  
 fora Um ponteiro para o índice retornado para a tabela referenciada.  
  
## <a name="remarks"></a>Comentários  
 Não recomendamos o uso desse método, pois ele não retorna resultados consistentes. Para obter informações sobre a tabela de GUID, consulte a documentação de Common Language Infrastructure (CLI), especialmente "partição II: definição de metadados e semântica". A documentação está disponível online; consulte [padrões ECMA C# e Common Language Infrastructure](../../../standard/components.md#applicable-standards) e o [padrão ECMA-335-Common Language Infrastructure (CLI)](http://www.ecma-international.org/publications/standards/Ecma-335.htm).  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataTables](imetadatatables-interface.md)
- [Interface IMetaDataTables2](imetadatatables2-interface.md)
