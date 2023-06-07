---
title: Método ISymUnmanagedWriter::SetScopeRange
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter.SetScopeRange
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter::SetScopeRange
helpviewer_keywords:
- SetScopeRange method [.NET Framework debugging]
- ISymUnmanagedWriter::SetScopeRange method [.NET Framework debugging]
ms.assetid: d4d98676-444b-46ca-bfe6-0d827385cd22
topic_type:
- apiref
ms.openlocfilehash: a1da070f261f224d212d1fba81c287285a54d0d0
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501686"
---
# <a name="isymunmanagedwritersetscoperange-method"></a>Método ISymUnmanagedWriter::SetScopeRange
Define o intervalo de deslocamento do escopo léxico especificado. O escopo se torna o novo escopo atual e é enviado por push para uma pilha de escopos. Os escopos devem formar uma hierarquia. Irmãos não têm permissão para se sobrepor.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT OpenScope(  
    [in] ULONG32  scopeID,  
    [in] ULONG32  startOffset,  
    [in] ULONG32  endOffset);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `scopeId`  
 no O identificador de escopo para o escopo.  
  
 `startOffset`  
 no O deslocamento, em bytes, da primeira instrução no escopo léxico a partir do início do método.  
  
 `endOffset`  
 no O deslocamento, em bytes, da última instrução no escopo léxico desde o início do método.  
  
## <a name="return-value"></a>Valor Retornado  
 S_OK se o método tiver sucesso; caso contrário, E_FAIL ou algum outro código de erro.  
  
## <a name="remarks"></a>Comentários  
 [ISymUnmanagedWriter:: OpenScope](isymunmanagedwriter-openscope-method.md) retorna um identificador de escopo opaco que pode ser usado com `ISymUnmanagedWriter::SetScopeRange` o para definir o deslocamento de início e de término de um escopo em um momento posterior. Nesse caso, os deslocamentos passados para `ISymUnmanagedWriter::OpenScope` e [ISymUnmanagedWriter:: CloseScope](isymunmanagedwriter-closescope-method.md) são ignorados. Os identificadores de escopo só são válidos no método atual.  
  
## <a name="requirements"></a>Requisitos  
 **Cabeçalho:** CorSym. idl, CorSym. h  
  
## <a name="see-also"></a>Confira também

- [Interface ISymUnmanagedWriter](isymunmanagedwriter-interface.md)
