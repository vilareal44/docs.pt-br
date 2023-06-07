---
title: Interface IMetaDataTables
ms.date: 03/30/2017
api_name:
- IMetaDataTables
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataTables
helpviewer_keywords:
- IMetaDataTables interface [.NET Framework metadata]
ms.assetid: 31272cce-506a-4f18-bcbf-01ee45e36356
topic_type:
- apiref
ms.openlocfilehash: 2105033e684ec172e24adfb14bcab7668b388af3
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501115"
---
# <a name="imetadatatables-interface"></a>Interface IMetaDataTables
Fornece métodos para o armazenamento e a recuperação de informações de metadados em tabelas.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método GetBlob](imetadatatables-getblob-method.md)|Obtém um ponteiro para o objeto binário grande (BLOB) no índice de coluna especificado.|  
|[Método GetBlobHeapSize](imetadatatables-getblobheapsize-method.md)|Obtém o tamanho, em bytes, do heap de BLOB.|  
|[Método GetCodedTokenInfo](imetadatatables-getcodedtokeninfo-method.md)|Obtém um ponteiro para uma matriz de tokens associados ao índice de linha especificado.|  
|[Método GetColumn](imetadatatables-getcolumn-method.md)|Obtém um ponteiro para os valores contidos na coluna no índice de coluna especificado, na tabela no índice de tabela especificado.|  
|[Método GetColumnInfo](imetadatatables-getcolumninfo-method.md)|Obtém dados sobre a coluna especificada na tabela especificada.|  
|[Método GetGuid](imetadatatables-getguid-method.md)|Obtém um GUID da linha no índice especificado.|  
|[Método GetGuidHeapSize](imetadatatables-getguidheapsize-method.md)|Obtém o tamanho, em bytes, do heap GUID.|  
|[Método GetNextBlob](imetadatatables-getnextblob-method.md)|Obtém o índice do próximo BLOB na tabela.|  
|[Método GetNextGuid](imetadatatables-getnextguid-method.md)|Obtém o índice do próximo valor de GUID na coluna da tabela atual.|  
|[Método GetNextString](imetadatatables-getnextstring-method.md)|Obtém o índice da próxima cadeia de caracteres na coluna da tabela atual.|  
|[Método GetNextUserString](imetadatatables-getnextuserstring-method.md)|Obtém o índice da linha que contém a próxima cadeia de caracteres embutida em código na coluna da tabela atual.|  
|[Método GetNumTables](imetadatatables-getnumtables-method.md)|Obtém o número de tabelas no escopo da `IMetaDataTables` instância atual.|  
|[Método GetRow](imetadatatables-getrow-method.md)|Obtém a linha no índice de linha especificado, na tabela no índice de tabela especificado.|  
|[Método GetString](imetadatatables-getstring-method.md)|Obtém a cadeia de caracteres no índice especificado da coluna de tabela no escopo de referência atual.|  
|[Método GetStringHeapSize](imetadatatables-getstringheapsize-method.md)|Obtém o tamanho, em bytes, do heap de cadeia de caracteres.|  
|[Método GetTableIndex](imetadatatables-gettableindex-method.md)|Obtém o índice da tabela referenciada pelo token especificado.|  
|[Método GetTableInfo](imetadatatables-gettableinfo-method.md)|Obtém o nome, o tamanho da linha, o número de linhas, o número de colunas e o índice da coluna de chave da tabela no índice de tabela especificado.|  
|[Método GetUserString](imetadatatables-getuserstring-method.md)|Obtém a cadeia de caracteres embutida em código no índice especificado na coluna de cadeia de caracteres no escopo atual.|  
|[Método GetUserStringHeapSize](imetadatatables-getuserstringheapsize-method.md)|Obtém o tamanho, em bytes, do heap de cadeia de caracteres do usuário.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interfaces de metadados](metadata-interfaces.md)
- [Interface IMetaDataTables2](imetadatatables2-interface.md)
