---
title: Método IValidator::FormatEventInfo
ms.date: 03/30/2017
api_name:
- IValidator.FormatEventInfo
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- FormatEventInfo
helpviewer_keywords:
- IValidator::FormatEventInfo method [.NET Framework hosting]
- FormatEventInfo method, IValidator interface [.NET Framework hosting]
ms.assetid: 4c0c7477-05ba-461b-b21b-cbfba95f1db1
topic_type:
- apiref
ms.openlocfilehash: 0c60631b5e034bc46d74412440d35d526359d043
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008567"
---
# <a name="ivalidatorformateventinfo-method"></a>Método IValidator::FormatEventInfo
Obtém a mensagem de erro correspondente ao erro de validação especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT FormatEventInfo(  
    [in] HRESULT            hVECode,  
    [in] VEContext          Context,  
    [in, out] LPWSTR        msg,  
    [in] unsigned long      ulMaxLength,  
    [in] SAFEARRAY(VARIANT) psa  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `hVECode`  
 no O valor HRESULT que foi passado para o manipulador de erro de validação.  
  
 `Context`  
 no Uma `VEContext` instância que contém informações de contexto sobre o erro de validação.  
  
 `msg`  
 [entrada, saída] Uma cadeia de caracteres que contém a mensagem de erro retornada.  
  
 `ulMaxLength`  
 no O comprimento máximo da mensagem de erro.  
  
 `psa`  
 no Uma matriz segura que contém parâmetros adicionais que descrevem o erro.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** IValidator. idl, IValidator. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
