---
title: Função LoadStringRC
ms.date: 03/30/2017
api_name:
- LoadStringRC
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- LoadStringRC
helpviewer_keywords:
- LoadStringRC function [.NET Framework hosting]
ms.assetid: 752e49b4-987c-4c28-a118-1a0c1ed510c5
topic_type:
- apiref
ms.openlocfilehash: 8bd0292ddf22453f8892ed8bddd10c2144877097
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008504"
---
# <a name="loadstringrc-function"></a>Função LoadStringRC
Traduz um valor HRESULT em uma mensagem de erro usando a cultura padrão do thread atual.  
  
 Essa função foi preterida no .NET Framework 4.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT LoadStringRC (  
    [in]  UINT    iResourceID,
    [out] LPWSTR  szBuffer,
    [in]  int     iMax,
    [in]  int     bQuiet  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `iResourceID`  
 no Um HRESULT.  
  
 `szBuffer`  
 fora Um buffer que contém a mensagem de erro após a conclusão bem-sucedida.  
  
 `iMax`  
 no O tamanho do buffer de mensagens de erro.  
  
 `bQuiet`  
 no Aceita.  
  
## <a name="return-value"></a>Valor Retornado  
 Esse método retorna códigos de erro padrão de Component Object Model (COM), conforme definido no WinError. h, além dos valores a seguir.  
  
|Código de retorno|Descrição|  
|-----------------|-----------------|  
|S_OK|O método foi concluído com êxito.|  
|E_INVALIDARG|`szBuffer`é nulo ou `iMax` é zero (0).|  
  
## <a name="remarks"></a>Comentários  
 Se o método não for concluído com êxito, `szBuffer` conterá uma cadeia de caracteres vazia.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** MSCorEE. dll e mscorwks. dll. Use o MSCorEE. dll em vez de mscorwks. dll para garantir que você direcione a versão correta do .NET Framework.  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Função LoadStringRCEx](loadstringrcex-function.md)
- [Funções de hospedagem CLR reprovadas](deprecated-clr-hosting-functions.md)
