---
title: Enumeração CorThreadSafetyOptions
ms.date: 03/30/2017
api_name:
- CorThreadSafetyOptions
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorThreadSafetyOptions
helpviewer_keywords:
- CorThreadSafetyOptions enumeration [.NET Framework metadata]
ms.assetid: dae07d9b-df51-488c-b17e-52d6e48217bd
topic_type:
- apiref
ms.openlocfilehash: 8c0527a7bc3cde7344bf809dc8e6f5a3fac04852
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007501"
---
# <a name="corthreadsafetyoptions-enumeration"></a>Enumeração CorThreadSafetyOptions

Especifica sinalizadores para selecionar opções para a segurança do thread.

## <a name="syntax"></a>Sintaxe

```cpp
typedef enum CorThreadSafetyOptions {
    MDThreadSafetyDefault       = 0x00000000,
    MDThreadSafetyOff           = 0x00000000,
    MDThreadSafetyOn            = 0x00000001,
} CorThreadSafetyOptions;
```

## <a name="members"></a>Membros

|Membro|Descrição|
|------------|-----------------|
|`MDThreadSafetyDefault`|Valor padrão. Igual a `MDThreadSafetyOff`.|
|`MDThreadSafetyOff`|Indica que um bloqueio de leitor/gravador não pode ser definido.|
|`MDThreadSafetyOn`|Indica que um bloqueio de leitor/gravador pode ser definido.|

## <a name="requirements"></a>Requisitos

**Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).

**Cabeçalho:** CorHdr. h

**.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]

## <a name="see-also"></a>Confira também

- [Enumerações de metadados](metadata-enumerations.md)
