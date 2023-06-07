---
title: Método ISymUnmanagedWriter::CloseScope
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter.CloseScope
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter::CloseScope
helpviewer_keywords:
- CloseScope method [.NET Framework debugging]
- ISymUnmanagedWriter::CloseScope method [.NET Framework debugging]
ms.assetid: 6dade525-7770-4cb4-bafd-4bb995ad0d87
topic_type:
- apiref
ms.openlocfilehash: e2e911fb1d737ebb6b2106c89ac11335788ace4f
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501723"
---
# <a name="isymunmanagedwriterclosescope-method"></a>Método ISymUnmanagedWriter::CloseScope
Fecha o escopo léxico atual.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT CloseScope(  
    [in] ULONG32 endOffset);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `endOffset`  
 no O deslocamento do início do método do ponto no final da última instrução no escopo léxico, em bytes.  
  
## <a name="return-value"></a>Valor Retornado  
 S_OK se o método tiver sucesso; caso contrário, E_FAIL ou algum outro código de erro.  
  
## <a name="remarks"></a>Comentários  
 Depois que um escopo é fechado, nenhuma outra variável pode ser definida dentro dele.  
  
 [ISymUnmanagedWriter:: OpenScope](isymunmanagedwriter-openscope-method.md) retorna um identificador de escopo opaco que pode ser usado com [ISymUnmanagedWriter:: SetScopeRange](isymunmanagedwriter-setscoperange-method.md) para definir posteriormente o deslocamento de início e de término do escopo. Nesse caso, os deslocamentos passados para `ISymUnmanagedWriter::OpenScope` e `ISymUnmanagedWriter::CloseScope` são ignorados. Os identificadores de escopo são válidos somente no método atual.  
  
## <a name="requirements"></a>Requisitos  
 **Cabeçalho:** CorSym. idl, CorSym. h  
  
## <a name="see-also"></a>Confira também

- [Interface ISymUnmanagedWriter](isymunmanagedwriter-interface.md)
