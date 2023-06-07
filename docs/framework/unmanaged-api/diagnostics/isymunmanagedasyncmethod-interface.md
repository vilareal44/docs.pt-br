---
title: Interface ISymUnmanagedAsyncMethod
ms.date: 03/30/2017
ms.assetid: f2de5224-fd91-45de-9e58-bc600c6d22f1
ms.openlocfilehash: 448ed719331469dce8f15500f14d5c1b0707ecf7
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504446"
---
# <a name="isymunmanagedasyncmethod-interface"></a>Interface ISymUnmanagedAsyncMethod
Essa interface é o complemento de leitura para a [interface ISymUnmanagedAsyncMethodPropertiesWriter](isymunmanagedasyncmethodpropertieswriter-interface.md).  
  
## <a name="syntax"></a>Sintaxe  
  
```idl  
[object,uuid(B20D55B3-532E-4906-87E7-25BD5734ABD2),pointer_default(unique)]interface ISymUnmanagedAsyncMethod : IUnknown  
```  
  
## <a name="methods"></a>Métodos  
 Essa interface contém os seguintes métodos:  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método GetAsyncStepInfo](isymunmanagedasyncmethod-getasyncstepinfo-method.md)|Consulte o [método DefineAsyncStepInfo](isymunmanagedasyncmethodpropertieswriter-defineasyncstepinfo-method.md).|  
|[Método GetAsyncStepInfoCount](isymunmanagedasyncmethod-getasyncstepinfocount-method.md)|Consulte o [método DefineAsyncStepInfo](isymunmanagedasyncmethodpropertieswriter-defineasyncstepinfo-method.md).|  
|[Método GetCatchHandlerILOffset](isymunmanagedasyncmethod-getcatchhandleriloffset-method.md)|Consulte o [método DefineCatchHandlerILOffset](isymunmanagedasyncmethodpropertieswriter-definecatchhandleriloffset-method.md).|  
|[Método GetKickoffMethod](isymunmanagedasyncmethod-getkickoffmethod-method.md)|Consulte o [método DefineKickoffMethod](isymunmanagedasyncmethodpropertieswriter-definekickoffmethod-method.md).|  
|[Método HasCatchHandlerILOffset](isymunmanagedasyncmethod-hascatchhandleriloffset-method.md)|Consulte o [método DefineCatchHandlerILOffset](isymunmanagedasyncmethodpropertieswriter-definecatchhandleriloffset-method.md).|  
|[Método IsAsyncMethod](isymunmanagedasyncmethod-isasyncmethod-method.md)|Verifica se o método tem informações assíncronas ou não.<br /><br /> Se esse método retornar `FALSE` , ele será inválido para chamar outros métodos nesta interface. Eles serão retornados `E_UNEXPECTED` nesse caso.|  
  
## <a name="requirements"></a>Requisitos  
 **Cabeçalho:** CorSym. idl, CorSym. h  
  
## <a name="see-also"></a>Confira também

- [Interfaces de armazenamento de símbolo de diagnóstico](diagnostics-symbol-store-interfaces.md)
