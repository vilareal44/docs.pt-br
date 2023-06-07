---
title: Método IMetaDataTables::GetTableInfo
ms.date: 03/30/2017
api_name:
- IMetaDataTables.GetTableInfo
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataTables::GetTableInfo
helpviewer_keywords:
- IMetaDataTables::GetTableInfo method [.NET Framework metadata]
- GetTableInfo method [.NET Framework metadata]
ms.assetid: 50cbe557-2322-41aa-8e0d-f967602eaa0f
topic_type:
- apiref
ms.openlocfilehash: 7e60dd9535809ca13f3bbe6ac76f5ea1209df734
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501155"
---
# <a name="imetadatatablesgettableinfo-method"></a>Método IMetaDataTables::GetTableInfo
Obtém o nome, o tamanho da linha, o número de linhas, o número de colunas e o índice da coluna de chave da tabela especificada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetTableInfo (  
    [in]  ULONG       ixTbl,  
    [out] ULONG       *pcbRow,  
    [out] ULONG       *pcRows,  
    [out] ULONG       *pcCols,  
    [out] ULONG       *piKey,  
    [out] const char  **ppName  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `ixTbl`  
 no O identificador da tabela cujas propriedades retornar.  
  
 `pcbRow`  
 fora Um ponteiro para o tamanho, em bytes, de uma linha de tabela.  
  
 `pcRows`  
 fora Um ponteiro para o número de linhas na tabela.  
  
 `pcCols`  
 fora Um ponteiro para o número de colunas na tabela.  
  
 `piKey`  
 fora Um ponteiro para o índice da coluna de chave ou-1 se a tabela não tiver nenhuma coluna de chave.  
  
 `ppName`  
 fora Um ponteiro para um ponteiro para o nome da tabela.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataTables](imetadatatables-interface.md)
- [Interface IMetaDataTables2](imetadatatables2-interface.md)
