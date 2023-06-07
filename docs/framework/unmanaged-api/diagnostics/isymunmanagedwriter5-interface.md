---
title: Interface ISymUnmanagedWriter5
ms.date: 03/30/2017
ms.assetid: 15b8526e-4f5d-475c-a1e3-d8b2d145c879
ms.openlocfilehash: d9204457b71b670e1c96ed228ad11116bdf41fe6
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84493572"
---
# <a name="isymunmanagedwriter5-interface"></a>Interface ISymUnmanagedWriter5
Interface ISymUnmanagedWriter5.  
  
## <a name="syntax"></a>Sintaxe  
  
```idl  
[object,uuid(DCF7780D-BDE9-45DF-ACFE-21731A32000C),pointer_default(unique)]interface ISymUnmanagedWriter5 : ISymUnmanagedWriter4  
```  
  
## <a name="methods"></a>Métodos  
 Essa interface contém os seguintes métodos:  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método CloseMapTokensToSourceSpans](isymunmanagedwriter5-closemaptokenstosourcespans-method.md)|Feche a seção de dados personalizados especiais para obter informações de mapeamento de token para origem. Depois de fechada, não é possível adicionar mais informações de mapeamento.|  
|[Método MapTokenToSourceSpan](isymunmanagedwriter5-maptokentosourcespan-method.md)|Mapeia o token de metadados fornecido para o span de linha de origem fornecido no arquivo de origem especificado.<br /><br /> Deve ser chamado entre as chamadas para o [método OpenMapTokensToSourceSpans](isymunmanagedwriter5-openmaptokenstosourcespans-method.md) e o [método CloseMapTokensToSourceSpans](isymunmanagedwriter5-closemaptokenstosourcespans-method.md).|  
|[Método OpenMapTokensToSourceSpans](isymunmanagedwriter5-openmaptokenstosourcespans-method.md)|Abra uma seção especial de dados personalizados para emitir informações de mapeamento de extensão de token para origem no. Abrir esta seção quando um método já estiver aberto, ou vice-versa, é um erro.|  
  
## <a name="requirements"></a>Requisitos  
 **Cabeçalho:** CorSym. idl, CorSym. h  
  
## <a name="see-also"></a>Confira também

- [Interfaces de armazenamento de símbolo de diagnóstico](diagnostics-symbol-store-interfaces.md)
- [Interface ISymUnmanagedWriter4](isymunmanagedwriter4-interface.md)
