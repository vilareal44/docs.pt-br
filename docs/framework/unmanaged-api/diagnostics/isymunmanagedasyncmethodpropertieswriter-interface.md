---
title: Interface ISymUnmanagedAsyncMethodPropertiesWriter
ms.date: 03/30/2017
ms.assetid: caa71820-8058-4b6a-93a2-25ee757d92d3
ms.openlocfilehash: 04876483fd42e3f6e55222416fd0747891734a52
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501853"
---
# <a name="isymunmanagedasyncmethodpropertieswriter-interface"></a>Interface ISymUnmanagedAsyncMethodPropertiesWriter
Permite que você defina informações de método assíncrono opcional para cada símbolo de método. Sempre usar com um método aberto; ou seja, entre as chamadas para o [método OpenMethod](isymunmanagedwriter-openmethod-method.md) e o [método CloseMethod](isymunmanagedwriter-closemethod-method.md).  
  
## <a name="syntax"></a>Sintaxe  
  
```idl  
[object,uuid(FC073774-1739-4232-BD56-A027294BEC15),pointer_default(unique)]interface ISymUnmanagedAsyncMethodPropertiesWriter : IUnknown  
```  
  
## <a name="methods"></a>Métodos  
 Essa interface contém os seguintes métodos:  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método DefineAsyncStepInfo](isymunmanagedasyncmethodpropertieswriter-defineasyncstepinfo-method.md)|Defina um grupo de operações assíncronas Await no método atual.<br /><br /> Cada deslocamento de rendimento corresponde a uma instrução de retorno de Await, identificando um possível rendimento. Cada `breakpointMethod` / `breakpointOffset` par identifica onde a operação assíncrona será retomada; ela pode estar em um método diferente.|  
|[Método DefineCatchHandlerILOffset](isymunmanagedasyncmethodpropertieswriter-definecatchhandleriloffset-method.md)|Define o deslocamento de IL para o manipulador catch gerado pelo compilador que encapsula um método assíncrono.<br /><br /> O deslocamento de IL do catch gerado é usado pelo depurador para lidar com o Catch como se fosse um código de não usuário, mesmo que possa ocorrer em um método de código de usuário. Em particular, ele é usado em resposta a um evento de exceção **CatchHandlerFound** .|  
|[Método DefineKickoffMethod](isymunmanagedasyncmethodpropertieswriter-definekickoffmethod-method.md)|Define o método inicial que inicia a operação assíncrona.|  
  
## <a name="requirements"></a>Requisitos  
 **Cabeçalho:** CorSym. idl, CorSym. h  
  
## <a name="see-also"></a>Confira também

- [Interfaces de armazenamento de símbolo de diagnóstico](diagnostics-symbol-store-interfaces.md)
