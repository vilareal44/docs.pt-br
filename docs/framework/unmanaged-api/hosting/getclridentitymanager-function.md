---
title: Função GetCLRIdentityManager
ms.date: 03/30/2017
api_name:
- GetCLRIdentityManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- GetCLRIdentityManager
helpviewer_keywords:
- GetCLRIdentityManager function [.NET Framework hosting]
ms.assetid: 66eeca30-adb4-45f4-aff5-347564c95724
topic_type:
- apiref
ms.openlocfilehash: 8b1e918edf641d38dd6b91d790bcaff8020293a0
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84493260"
---
# <a name="getclridentitymanager-function"></a>Função GetCLRIdentityManager
Obtém um ponteiro para uma interface que permite que o Common Language Runtime (CLR) gerencie identidades.  
  
 Essa função foi preterida no .NET Framework 4.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
STDAPI GetCLRIdentityManager(  
    [in]  REFIID      riid,  
    [out] IUnknown  **ppManager  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `riid`  
 no Um `REFIID` (um identificador de interface) que especifica qual interface deve ser obtida. Esse valor deve ser IID_ICLRAssemblyIdentityManager ou IID_ICLRHostBindingPolicyManager.  
  
 `ppManager`  
 fora Um ponteiro para o endereço de um objeto [ICLRAssemblyIdentityManager](iclrassemblyidentitymanager-interface.md) ou [ICLRHostBindingPolicyManager](iclrhostbindingpolicymanager-interface.md) .  
  
## <a name="remarks"></a>Comentários  
 Chame a função [GetRealProcAddress](getrealprocaddress-function.md) para obter um ponteiro para a `GetCLRIdentityManager` função.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** MSCorWks. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Funções de hospedagem CLR reprovadas](deprecated-clr-hosting-functions.md)
