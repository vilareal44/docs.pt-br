---
title: Método ICorDebugManagedCallback2::CreateConnection
ms.date: 03/30/2017
api_name:
- ICorDebugManagedCallback2.CreateConnection
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugManagedCallback2::CreateConnection
helpviewer_keywords:
- CreateConnection method [.NET Framework debugging]
- ICorDebugManagedCallback2::CreateConnection method [.NET Framework debugging]
ms.assetid: 49e647be-9d63-4250-9d11-704e2a400d1b
topic_type:
- apiref
ms.openlocfilehash: 8e31f0a649fd1ca80d6557a0a7176549c67bf203
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501918"
---
# <a name="icordebugmanagedcallback2createconnection-method"></a>Método ICorDebugManagedCallback2::CreateConnection
Notifica o depurador de que uma nova conexão foi criada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT CreateConnection (  
    [in] ICorDebugProcess     *pProcess,  
    [in] CONNID               dwConnectionId,  
    [in] WCHAR                *pConnName  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pProcess`  
 no Um ponteiro para um objeto "ICorDebugProcess" que representa o processo no qual a conexão foi criada  
  
 `dwConnectionId`  
 no A ID da nova conexão.  
  
 `pConnName`  
 no Um ponteiro para o nome da nova conexão.  
  
## <a name="remarks"></a>Comentários  
 Um `CreateConnection` retorno de chamada será acionado em qualquer um dos seguintes casos:  
  
- Quando um depurador é anexado a um processo que contém conexões. Nesse caso, o tempo de execução irá gerar e distribuir um evento `CreateConnection` e um evento [ICorDebugManagedCallback2:: ChangeConnection](icordebugmanagedcallback2-changeconnection-method.md) para cada conexão no processo.  
  
- Quando um host chama [ICLRDebugManager:: BeginConnect](../hosting/iclrdebugmanager-beginconnection-method.md) na API de [hospedagem](../hosting/index.md).  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorDebug.idl, CorDebug.h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorDebugManagedCallback2](icordebugmanagedcallback2-interface.md)
- [Interface ICorDebugManagedCallback](icordebugmanagedcallback-interface.md)
