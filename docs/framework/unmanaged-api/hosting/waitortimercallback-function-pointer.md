---
title: Ponteiro de função WAITORTIMERCALLBACK
ms.date: 03/30/2017
api_name:
- WAITORTIMERCALLBACK
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- WAITORTIMERCALLBACK
helpviewer_keywords:
- WAITORTIMERCALLBACK function pointer [.NET Framework hosting]
ms.assetid: 1fec4aef-0a06-4df0-bae7-d31a9ef9603d
topic_type:
- apiref
ms.openlocfilehash: ee5dd611888ec52e360ef45fab4c01e9c5b2d6bb
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84009438"
---
# <a name="waitortimercallback-function-pointer"></a>Ponteiro de função WAITORTIMERCALLBACK
Aponta para uma função que notifica o host que um identificador de espera ( <xref:System.Threading.WaitHandle> ) foi sinalizado ou esgotou o tempo limite.  
  
 Esse ponteiro de função foi preterido no .NET Framework 4.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef VOID (__stdcall *WAITORTIMERCALLBACK) (  
    [in] PVOID lpParameter,  
    [in] BOOL  TimerOrWaitFired  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `lpParameter`  
 no Um ponteiro para um objeto que contém informações definidas pelo host.  
  
 `TimerOrWaitFired`  
 [in] `true` Se o identificador de espera expirou ou `false` se foi sinalizado.  
  
## <a name="remarks"></a>Comentários  
 A função para a qual os `WAITORTIMERCALLBACK` pontos é uma função de retorno de chamada e deve ser implementada pelo gravador do aplicativo de hospedagem.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** MSCorWks. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Funções de hospedagem CLR reprovadas](deprecated-clr-hosting-functions.md)
