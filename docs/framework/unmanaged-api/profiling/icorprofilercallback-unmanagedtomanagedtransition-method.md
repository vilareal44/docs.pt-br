---
title: Método ICorProfilerCallback::UnmanagedToManagedTransition
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.UnmanagedToManagedTransition
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::UnmanagedToManagedTransition
helpviewer_keywords:
- ICorProfilerCallback::UnmanagedToManagedTransition method [.NET Framework profiling]
- UnmanagedToManagedTransition method [.NET Framework profiling]
ms.assetid: ade2cc01-9b81-4e09-a5f9-b3b9dda27e96
topic_type:
- apiref
ms.openlocfilehash: 8734fa9c9418b818cbe14ebe87ce2af6fa59c078
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499838"
---
# <a name="icorprofilercallbackunmanagedtomanagedtransition-method"></a>Método ICorProfilerCallback::UnmanagedToManagedTransition
Notifica o criador de perfil de que uma transição de código não gerenciado para código gerenciado ocorreu.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT UnmanagedToManagedTransition(  
    [in] FunctionID functionId,  
    [in] COR_PRF_TRANSITION_REASON reason);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `functionId`  
 no A ID da função que está sendo chamada.  
  
 `reason`  
 no Um valor da enumeração [COR_PRF_TRANSITION_REASON](cor-prf-transition-reason-enumeration.md) que indica se a transição ocorreu devido a uma chamada em código gerenciado a partir de código não gerenciado ou por causa de um retorno de uma função não gerenciada chamada por um gerenciado.  
  
## <a name="remarks"></a>Comentários  
 Se o valor de `reason` for COR_PRF_TRANSITION_RETURN e `functionId` não for NULL, a ID da função será a da função não gerenciada e nunca terá sido compilada usando o compilador JIT (just-in-time). As funções não gerenciadas têm algumas informações básicas associadas a elas, como um nome e alguns metadados.  
  
 Se o valor de `reason` for COR_PRF_TRANSITION_CALL, talvez seja possível que a função chamada (ou seja, a função gerenciada) ainda não tenha sido compilada em JIT.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
- [Método ManagedToUnmanagedTransition](icorprofilercallback-managedtounmanagedtransition-method.md)
- [Usando o PInvoke explícito em C++ (atributo DllImport)](/cpp/dotnet/using-explicit-pinvoke-in-cpp-dllimport-attribute)
- [Usando a interoperabilidade C++ (PInvoke implícito)](/cpp/dotnet/using-cpp-interop-implicit-pinvoke)
