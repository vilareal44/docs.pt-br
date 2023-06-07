---
title: Função LoadStringRCEx
ms.date: 03/30/2017
api_name:
- LoadStringRCEx
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- LoadStringRCEx
helpviewer_keywords:
- LoadStringRCEx function [.NET Framework hosting]
ms.assetid: bc789636-ca14-4f07-8f77-9305874d7495
topic_type:
- apiref
ms.openlocfilehash: a05cbe985c2cfebb67756fdfb54398b36e87f441
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008502"
---
# <a name="loadstringrcex-function"></a>Função LoadStringRCEx
Traduz um valor HRESULT para uma mensagem de erro apropriada para a cultura especificada.  
  
 Essa função foi preterida no .NET Framework 4.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT LoadStringRCEx (  
    [in]  LCID    lcid,
    [in]  UINT    iResouceID,
    [out] LPWSTR  szBuffer,
    [in]  int     iMax,
    [in]  int     bQuiet,
    [out] int    *pcwchUsed  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `lcid`  
 no Um identificador de cultura. Pass-1 para `lcid` para usar a cultura padrão.  
  
 `iResourceID`  
 no Um HRESULT.  
  
 `szBuffer`  
 fora Um buffer que contém a mensagem de erro após a conclusão bem-sucedida.  
  
 `iMax`  
 no O tamanho do buffer de mensagens de erro.  
  
 `bQuiet`  
 no Aceita.  
  
 `pcwchUsed`  
 fora Um ponteiro para o comprimento da mensagem de erro.  
  
## <a name="return-value"></a>Valor Retornado  
 Esse método retorna códigos de erro COM padrão, conforme definido no WinError. h, além dos valores a seguir.  
  
|Código de retorno|Descrição|  
|-----------------|-----------------|  
|S_OK|O método foi concluído com êxito.|  
|E_INVALIDARG|`szBuffer`é NULL ou `iMax` é zero (0).|  
  
## <a name="remarks"></a>Comentários  
 Se o método não for concluído com êxito, `szBuffer` conterá uma cadeia de caracteres vazia.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Globalization.CultureInfo.LCID%2A?displayProperty=nameWithType>
- [Função LoadStringRC](loadstringrc-function.md)
- [Funções de hospedagem CLR reprovadas](deprecated-clr-hosting-functions.md)
