---
title: Método ICLRStrongName::StrongNameSignatureVerificationEx
ms.date: 03/30/2017
api_name:
- ICLRStrongName.StrongNameSignatureVerificationEx
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRStrongName::StrongNameSignatureVerificationEx
helpviewer_keywords:
- StrongNameSignatureVerificationEx method, ICLRStrongName interface [.NET Framework hosting]
- ICLRStrongName::StrongNameSignatureVerificationEx method [.NET Framework hosting]
ms.assetid: dbd2f662-208b-4174-b301-5c99af91040f
topic_type:
- apiref
ms.openlocfilehash: 5bd985a6870ae6f4cc2302b6a11e8e139180d839
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503985"
---
# <a name="iclrstrongnamestrongnamesignatureverificationex-method"></a>Método ICLRStrongName::StrongNameSignatureVerificationEx
Obtém um valor que indica se o manifesto do assembly no caminho fornecido contém uma assinatura de nome forte.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT StrongNameSignatureVerificationEx (  
    [in]  LPCWSTR   wszFilePath,  
    [in]  BOOLEAN   fForceVerification,  
    [out] BOOLEAN   *pfWasVerified  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `wszFilePath`  
 no O caminho para o arquivo executável portátil (. exe ou. dll) para o assembly a ser verificado.  
  
 `fForceVerification`  
 [in] `true` para executar a verificação, mesmo se for necessário substituir as configurações do registro; caso contrário, `false` .  
  
 `pfWasVerified`  
 [fora] `true` se a assinatura de nome forte foi verificada; caso contrário, `false` . `pfWasVerified`também será definido como `false` se a verificação tiver sido bem-sucedida devido a configurações do registro.  
  
## <a name="return-value"></a>Valor Retornado  
 `S_OK`se a verificação foi bem-sucedida; caso contrário, um valor HRESULT que indica falha (consulte [valores de HRESULT comuns](/windows/win32/seccrypto/common-hresult-values) para uma lista).  
  
## <a name="remarks"></a>Comentários  
 O método [ICLRStrongName:: StrongNameSignatureVerificationEx](iclrstrongname-strongnamesignatureverificationex-method.md) fornece um recurso semelhante ao método [ICLRStrongName:: StrongNameSignatureVerification](iclrstrongname-strongnamesignatureverification-method.md) . No entanto, o segundo parâmetro de entrada e o parâmetro de saída para [ICLRStrongName:: StrongNameSignatureVerificationEx](iclrstrongname-strongnamesignatureverificationex-method.md) são do tipo `BOOLEAN` em vez de `DWORD` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MetaHost. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Método StrongNameSignatureVerification](iclrstrongname-strongnamesignatureverification-method.md)
- [Interface ICLRStrongName](iclrstrongname-interface.md)
