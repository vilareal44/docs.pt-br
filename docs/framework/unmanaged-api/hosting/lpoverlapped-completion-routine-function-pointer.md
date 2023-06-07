---
title: Ponteiro de função LPOVERLAPPED_COMPLETION_ROUTINE
ms.date: 03/30/2017
api_name:
- LPOVERLAPPED_COMPLETION_ROUTINE
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- LPOVERLAPPED_COMPLETION_ROUTINE
helpviewer_keywords:
- LPOVERLAPPED_COMPLETION_ROUTINE function pointer [.NET Framework hosting]
ms.assetid: 5fb645d9-b818-401c-8c2c-c30d86de58ba
topic_type:
- apiref
ms.openlocfilehash: c0bdd9e59f5794dbb0d447dc2cc6cb682bfdf09f
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008476"
---
# <a name="lpoverlapped_completion_routine-function-pointer"></a>Ponteiro de função LPOVERLAPPED_COMPLETION_ROUTINE
Aponta para uma função que notifica o host quando uma e/s sobreposta (ou seja, assíncrona) para um dispositivo for concluída.  
  
 Esse ponteiro de função foi preterido no .NET Framework 4.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef VOID (*LPOVERLAPPED_COMPLETION_ROUTINE) (  
    [in] DWORD  dwErrorCode,  
    [in] DWORD  dwNumberOfBytesTransfered,  
    [in] LPVOID lpOverlapped  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `dwErrorCode`  
 no Um valor que é um código de erro se o dispositivo tiver sido fechado; caso contrário, esse valor será zero.  
  
 Fechar um dispositivo faz com que todas as e/s pendentes para o dispositivo sejam concluídas imediatamente.  
  
 `dwNumberOfBytesTransfered`  
 no O número de bytes transferidos pela operação de e/s.  
  
 `lpOverlapped`  
 no Um ponteiro para uma estrutura que contém informações a serem usadas para concluir a solicitação de e/s.  
  
## <a name="remarks"></a>Comentários  
 A função para a qual os `LPOVERLAPPED_COMPLETION_ROUTINE` pontos é uma função de retorno de chamada e deve ser implementada pelo gravador do aplicativo de hospedagem. A função de retorno de chamada permite que o host processe a solicitação de e/s concluída.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** MSCorWks. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Funções de hospedagem CLR reprovadas](deprecated-clr-hosting-functions.md)
