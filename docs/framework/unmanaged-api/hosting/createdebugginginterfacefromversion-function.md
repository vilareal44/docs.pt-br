---
title: Função CreateDebuggingInterfaceFromVersion
ms.date: 03/30/2017
api_name:
- CreateDebuggingInterfaceFromVersion
api_location:
- mscoree.dll
- mscoreei.dll
api_type:
- DLLExport
f1_keywords:
- CreateDebuggingInterfaceFromVersion
helpviewer_keywords:
- CreateDebuggingInterfaceFromVersion function [.NET Framework hosting]
ms.assetid: a746a849-463c-44f5-a2f0-9e812ed8bcc3
topic_type:
- apiref
ms.openlocfilehash: 6f5eec282aec6a2757664023ce8031410e316f10
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501840"
---
# <a name="createdebugginginterfacefromversion-function"></a>Função CreateDebuggingInterfaceFromVersion
Cria um objeto [ICorDebug](../debugging/icordebug-interface.md) com base nas informações de versão especificadas.  
  
 Essa função está obsoleta no .NET Framework 4. Em vez disso, para obter uma interface para o Common Language Runtime (CLR) 2,0, use o método [ICLRRuntimeInfo:: GetInterface](iclrruntimeinfo-getinterface-method.md) e especifique o identificador de classe CLSID_CLRDebuggingLegacy e o identificador de interface IID_ICorDebug. Para obter uma interface para o CLR 4 ou posterior, chame a função [CLRCreateInstance](clrcreateinstance-function.md) e especifique o identificador de classe CLSID_CLRDebugging e o identificador de interface IID_ICLRDebugging.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT CreateDebuggingInterfaceFromVersion (  
    [in]  int      iDebuggerVersion,
    [in]  LPCWSTR  szDebuggeeVersion,
    [out] IUnknown **ppCordb  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `iDebuggerVersion`  
 no A versão do `ICorDebug` que é esperada pelo depurador. Consulte a enumeração [CorDebugInterfaceVersion](../debugging/cordebuginterfaceversion-enumeration.md) para obter os valores válidos.  
  
 `szDebuggeeVersion`  
 no A versão de Common Language Runtime associada ao aplicativo ou processo a ser depurado. Consulte o método [GetVersionFromProcess](getversionfromprocess-function.md) ou [GetRequestedRuntimeVersion](getrequestedruntimeversion-function.md) para obter informações sobre como recuperar esse valor.  
  
 `ppCordb`  
 fora O local que recebe um ponteiro para o `ICorDebug` objeto.  
  
## <a name="return-value"></a>Valor Retornado  
 Esse método retorna códigos de erro COM padrão, conforme definido no arquivo WinError. h, além dos valores a seguir.  
  
|Código de retorno|Descrição|  
|-----------------|-----------------|  
|S_OK|O método foi concluído com êxito.|  
|E_INVALIDARG|`szDebuggeeVersion`ou `ppCordb` é nulo ou a cadeia de caracteres da versão está incorreta.|  
  
## <a name="remarks"></a>Comentários  
 O `szDebuggeeVersion` parâmetro é mapeado para a versão correspondente do MSCorDbi. dll.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Funções de hospedagem CLR reprovadas](deprecated-clr-hosting-functions.md)
