---
title: Método IMetaDataTables::GetUserString
ms.date: 03/30/2017
api_name:
- IMetaDataTables.GetUserString
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataTables::GetUserString
helpviewer_keywords:
- IMetaDataTables::GetUserString method [.NET Framework metadata]
- GetUserString method, IMetaDataTables interface [.NET Framework metadata]
ms.assetid: 35b8f0d6-9aba-4714-adb2-62020a38fb7e
topic_type:
- apiref
ms.openlocfilehash: 21ce66722e069573b651ada950b64ef6d97220fb
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501138"
---
# <a name="imetadatatablesgetuserstring-method"></a>Método IMetaDataTables::GetUserString

Obtém a cadeia de caracteres embutida em código no índice especificado na coluna de cadeia de caracteres no escopo atual.

## <a name="syntax"></a>Sintaxe

```cpp
HRESULT GetUserString (
    [in]  ULONG       ixUserString,
    [out] ULONG       *pcbData,
    [out] const void  **ppData
);
```

## <a name="parameters"></a>Parâmetros

`ixUserString`\
no O valor de índice do qual a cadeia de caracteres embutida em código será recuperada.

`pcbData`\
fora Um ponteiro para o tamanho de `ppData` .

`ppData`\
fora Um ponteiro para um ponteiro para a cadeia de caracteres retornada.

## <a name="requirements"></a>Requisitos

**Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).

**Cabeçalho:** Cor. h

**Biblioteca:** Usado como um recurso em MsCorEE. dll

**.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]

## <a name="see-also"></a>Confira também

- [Interface IMetaDataTables](imetadatatables-interface.md)
- [Interface IMetaDataTables2](imetadatatables2-interface.md)
