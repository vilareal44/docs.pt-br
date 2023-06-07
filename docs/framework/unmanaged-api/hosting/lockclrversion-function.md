---
title: Função LockClrVersion
ms.date: 03/30/2017
api_name:
- LockClrVersion
api_location:
- mscoree.dll
- mscoreei.dll
api_type:
- DLLExport
f1_keywords:
- LockClrVersion
helpviewer_keywords:
- LockClrVersion function [.NET Framework hosting]
ms.assetid: 1318ee37-c43b-40eb-bbe8-88fc46453d74
topic_type:
- apiref
ms.openlocfilehash: 09bcebfdcfea3d5728d404cdb6b5fb170a5432c3
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008489"
---
# <a name="lockclrversion-function"></a>Função LockClrVersion
Permite que o host determine qual versão do Common Language Runtime (CLR) será usada dentro do processo antes de inicializar explicitamente o CLR.  
  
 Essa função foi preterida no .NET Framework 4.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT LockClrVersion (  
    [in] FLockClrVersionCallback   hostCallback,  
    [in] FLockClrVersionCallback  *pBeginHostSetup,  
    [in] FLockClrVersionCallback  *pEndHostSetup  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `hostCallback`  
 no A função a ser chamada pelo CLR na inicialização.  
  
 `pBeginHostSetup`  
 no A função a ser chamada pelo host para informar ao CLR que a inicialização está sendo iniciada.  
  
 `pEndHostSetup`  
 no A função a ser chamada pelo host para informar ao CLR que a inicialização foi concluída.  
  
## <a name="return-value"></a>Valor Retornado  
 Esse método retorna códigos de erro COM padrão, conforme definido no WinError. h, além dos valores a seguir.  
  
|Código de retorno|Descrição|  
|-----------------|-----------------|  
|S_OK|O método foi concluído com êxito.|  
|E_INVALIDARG|Um ou mais argumentos são nulos.|  
  
## <a name="remarks"></a>Comentários  
 O host chama `LockClrVersion` antes de inicializar o CLR. `LockClrVersion`usa três parâmetros, todos os quais são retornos de chamada do tipo [FLockClrVersionCallback](flockclrversioncallback-function-pointer.md). Esse tipo é definido da seguinte maneira.  
  
```cpp  
typedef HRESULT ( __stdcall *FLockClrVersionCallback ) ();  
```  
  
 As etapas a seguir ocorrem na inicialização do tempo de execução:  
  
1. O host chama [CorBindToRuntimeEx](corbindtoruntimeex-function.md) ou uma das outras funções de inicialização de tempo de execução. Como alternativa, o host poderia inicializar o tempo de execução usando a ativação de objeto COM.  
  
2. O tempo de execução chama a função especificada pelo `hostCallback` parâmetro.  
  
3. A função especificada por `hostCallback` então faz a seguinte sequência de chamadas:  
  
    - A função especificada pelo `pBeginHostSetup` parâmetro.  
  
    - `CorBindToRuntimeEx`(ou outra função de inicialização de tempo de execução).  
  
    - [ICLRRuntimeHost:: SetHostControl](iclrruntimehost-sethostcontrol-method.md).  
  
    - [ICLRRuntimeHost:: iniciar](iclrruntimehost-start-method.md).  
  
    - A função especificada pelo `pEndHostSetup` parâmetro.  
  
 Todas as chamadas de `pBeginHostSetup` para `pEndHostSetup` devem ocorrer em um único thread ou fibra, com a mesma pilha lógica. Esse thread pode ser diferente do thread no qual `hostCallback` é chamado.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Funções de hospedagem CLR reprovadas](deprecated-clr-hosting-functions.md)
