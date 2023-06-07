---
title: Método ICorDebugManagedCallback::UpdateModuleSymbols
ms.date: 03/30/2017
api_name:
- ICorDebugManagedCallback.UpdateModuleSymbols
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugManagedCallback::UpdateModuleSymbols
helpviewer_keywords:
- UpdateModuleSymbols method [.NET Framework debugging]
- ICorDebugManagedCallback::UpdateModuleSymbols method [.NET Framework debugging]
ms.assetid: 0863f644-58e8-45a0-b0c3-a28e99b20938
topic_type:
- apiref
ms.openlocfilehash: c0381cf924e44e581c8b275c9750cacba045cf1b
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501775"
---
# <a name="icordebugmanagedcallbackupdatemodulesymbols-method"></a>Método ICorDebugManagedCallback::UpdateModuleSymbols
Notifica o depurador de que os símbolos de um módulo Common Language Runtime foram alterados.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT UpdateModuleSymbols (  
    [in] ICorDebugAppDomain *pAppDomain,  
    [in] ICorDebugModule    *pModule,  
    [in] IStream            *pSymbolStream  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pAppDomain`  
 no Um ponteiro para um objeto ICorDebugAppDomain que representa o domínio do aplicativo que contém o módulo no qual os símbolos foram alterados.  
  
 `pModule`  
 no Um ponteiro para um objeto ICorDebugModule que representa o módulo no qual os símbolos foram alterados.  
  
 `pSymbolStream`  
 no Um ponteiro para um objeto COM `IStream` do Win32 que contém os símbolos modificados.  
  
## <a name="remarks"></a>Comentários  
 Esse método fornece uma oportunidade de atualizar a exibição do depurador dos símbolos de um módulo chamando [ISymUnmanagedReader:: UpdateSymbolStore](../diagnostics/isymunmanagedreader-updatesymbolstore-method.md) ou [ISymUnmanagedReader:: ReplaceSymbolStore](../diagnostics/isymunmanagedreader-replacesymbolstore-method.md).  
  
 Esse retorno de chamada pode ocorrer várias vezes para o mesmo módulo.  
  
 Um depurador deve tentar associar pontos de interrupção de nível de origem não associados.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorDebug.idl, CorDebug.h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorDebugManagedCallback](icordebugmanagedcallback-interface.md)
