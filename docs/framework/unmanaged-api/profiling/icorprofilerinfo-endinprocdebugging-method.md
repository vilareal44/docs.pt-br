---
title: Método ICorProfilerInfo::EndInprocDebugging
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.EndInprocDebugging
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::EndInprocDebugging
helpviewer_keywords:
- ICorProfilerInfo::EndInprocDebugging method [.NET Framework profiling]
- EndInprocDebugging method [.NET Framework profiling]
ms.assetid: 35bc1188-9767-4141-8038-60ea015b99ac
topic_type:
- apiref
ms.openlocfilehash: afbb007b6293e6e9cff92281a6f5e93b1e7924ec
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502971"
---
# <a name="icorprofilerinfoendinprocdebugging-method"></a>Método ICorProfilerInfo::EndInprocDebugging
Desliga uma sessão de depuração em processo. Esse método é obsoleto no .NET Framework versão 2,0.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT EndInprocDebugging(  
    [in]  DWORD dwProfilerContext);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `dwProfilerContext`  
 no Um valor que identifica a sessão de depuração. Esse valor deve ser o mesmo que o recebido no método [ICorProfilerInfo:: BeginInprocDebugging](icorprofilerinfo-begininprocdebugging-method.md) .  
  
## <a name="remarks"></a>Comentários  
 Você deve chamar [ICorProfilerInfo:: BeginInprocDebugging](icorprofilerinfo-begininprocdebugging-method.md) e `EndInprocDebugging` dentro do mesmo método de retorno de chamada.  
  
 Os serviços de depuração CLR oferecem suporte à depuração em processo limitada no .NET Framework versões 1,0 e 1,1. A depuração em processo habilitou um criador de perfil para usar as partes de inspeção da API de depuração. No entanto, devido aos comentários do cliente, a depuração em processo foi removida da .NET Framework na versão 2,0 e substituída por um conjunto de funcionalidades que está mais alinhado com a API de criação de perfil.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **Versão do .NET Framework:** 1,0  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
