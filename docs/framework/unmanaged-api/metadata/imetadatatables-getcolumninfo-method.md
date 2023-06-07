---
title: Método IMetaDataTables::GetColumnInfo
ms.date: 10/10/2019
api_name:
- IMetaDataTables.GetColumnInfo
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataTables::GetColumnInfo
helpviewer_keywords:
- IMetaDataTables::GetColumnInfo method [.NET Framework metadata]
- GetColumnInfo method [.NET Framework metadata]
ms.assetid: 68c160ea-ae7d-4750-985d-a038b2c8e7d9
topic_type:
- apiref
ms.openlocfilehash: a044924810016eea60682b8765aeee448b552f0d
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501190"
---
# <a name="imetadatatablesgetcolumninfo-method"></a>Método IMetaDataTables::GetColumnInfo
Obtém dados sobre a coluna especificada na tabela especificada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetColumnInfo (
    [in]  ULONG        ixTbl,  
    [in]  ULONG        ixCol,  
    [out] ULONG        *poCol,  
    [out] ULONG        *pcbCol,  
    [out] ULONG        *pType,  
    [out] const char   **ppName  
);  
```  
  
## <a name="parameters"></a>Parâmetros
=======

 `ixTbl`  
 no O índice da tabela desejada.  
  
 `ixCol`  
 no O índice da coluna desejada.  
  
 `poCol`  
 fora Um ponteiro para o deslocamento da coluna na linha.  
  
 `pcbCol`  
 fora Um ponteiro para o tamanho, em bytes, da coluna.  
  
 `pType`  
 fora Um ponteiro para o tipo dos valores na coluna.  
  
 `ppName`  
 fora Um ponteiro para um ponteiro para o nome da coluna.  

## <a name="remarks"></a>Comentários

O tipo de coluna retornado cai dentro de um intervalo de valores:

| pType                    | Descrição   | Função auxiliar                   |
|--------------------------|---------------|-----------------------------------|
| `0`..`iRidMax`<br>(0.. 63)   | Eliminá           | **IsRidType**<br>**IsRidOrToken** |
| `iCodedToken`..`iCodedTokenMax`<br>(64.. 95) | Token codificado | **IsCodedTokenType** <br>**IsRidOrToken** |
| `iSHORT`(96)            | Int16         | **Isfixatype**                   |
| `iUSHORT`(97)           | UInt16        | **Isfixatype**                   |
| `iLONG`(98)             | Int32         | **Isfixatype**                   |
| `iULONG`(99)            | UInt32        | **Isfixatype**                   |
| `iBYTE`(100)            | Byte          | **Isfixatype**                   |
| `iSTRING`(101)          | String        | **Isheaptype**                    |
| `iGUID`(102)            | Guid          | **Isheaptype**                    |
| `iBLOB`(103)            | Blob          | **Isheaptype**                    |

Os valores que são armazenados no *heap* (ou seja, `IsHeapType == true` ) podem ser lidos usando:

- `iSTRING`: **IMetadataTables. GetString**
- `iGUID`: **IMetadataTables. GETguid**
- `iBLOB`: **IMetadataTables. getBlob**

> [!IMPORTANT]
> Para usar as constantes definidas na tabela acima, inclua a diretiva `#define _DEFINE_META_DATA_META_CONSTANTS` fornecida pelo arquivo de cabeçalho *cor. h* .

## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataTables](imetadatatables-interface.md)
- [Interface IMetaDataTables2](imetadatatables2-interface.md)
