---
title: Interface ISymUnmanagedENCUpdate
ms.date: 03/30/2017
api_name:
- ISymUnmanagedENCUpdate
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedENCUpdate
helpviewer_keywords:
- ISymUnmanagedENCUpdate interface [.NET Framework debugging]
ms.assetid: 63a9ef45-01a6-46da-b958-5c6dc2dc232c
topic_type:
- apiref
ms.openlocfilehash: 1986d5f91a3dcfa31a43f729ee1f50129e083f5f
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501736"
---
# <a name="isymunmanagedencupdate-interface"></a>Interface ISymUnmanagedENCUpdate
Fornece funções para o recurso Editar e continuar.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método GetLocalVariableCount](isymunmanagedencupdate-getlocalvariablecount-method.md)|Obtém o número de variáveis locais.|  
|[Método GetLocalVariables](isymunmanagedencupdate-getlocalvariables-method.md)|Obtém as variáveis locais.|  
|[Método InitializeForEnc](isymunmanagedencupdate-initializeforenc-method.md)|Permite que os limites de método sejam computados antes da primeira chamada para o método [ISymUnmanagedENCUpdate:: UpdateSymbolStore2](isymunmanagedencupdate-updatesymbolstore2-method.md) .|  
|[Método UpdateMethodLines](isymunmanagedencupdate-updatemethodlines-method.md)|Permite atualizar as informações de linha de um método que não foi recompilado, mas cujas linhas foram movidas de forma independente. Um Delta para cada instrução é permitido.|  
|[Método UpdateSymbolStore2](isymunmanagedencupdate-updatesymbolstore2-method.md)|Permite que um compilador omita funções que não foram modificadas do fluxo do banco de dados do programa (PDB), desde que as informações da linha atendam aos requisitos. As informações de linha corretas podem ser determinadas com as informações da linha PDB antiga e um Delta para todas as linhas na função.|  
  
## <a name="requirements"></a>Requisitos  
 **Cabeçalho:** CorSym. idl, CorSym. h  
  
## <a name="see-also"></a>Confira também

- [Interfaces de armazenamento de símbolo de diagnóstico](diagnostics-symbol-store-interfaces.md)
