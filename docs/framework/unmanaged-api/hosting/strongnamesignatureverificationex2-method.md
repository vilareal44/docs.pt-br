---
title: Método StrongNameSignatureVerificationEx2
ms.date: 03/30/2017
api_name:
- ICLRStrongName2.StrongNameSignatureVerificationEx2
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- StrongNameSignatureVerificationEx2
helpviewer_keywords:
- StrongNameSignatureVerificationEx2 method, ICLRStrongName2 interface [.NET Framework hosting]
- ICLRStrongName2::StrongNameSignatureVerificationEx2 method [.NET Framework hosting]
ms.assetid: dfd4133f-a074-4db3-a7ee-4f250fe9ad3a
topic_type:
- apiref
ms.openlocfilehash: 5e6f77b9b5da061a75d23d7f3f7b673754b62afd
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84006357"
---
# <a name="strongnamesignatureverificationex2-method"></a>Método StrongNameSignatureVerificationEx2
Verifica a assinatura de um assembly com nome forte e fornece um mapeamento da chave ECMA para uma chave real.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT StrongNameSignatureVerificationEx (  
    [in]  LPCWSTR   wszFilePath,  
    [in]  BOOLEAN   fForceVerification,    [in]  BYTE      *pbEcmaPublicKey,  
    [in]  DWORD     cbEcmaPublicKey,  
    [out] BOOLEAN   *pfWasVerified  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `wszFilePath`  
 no O caminho para o arquivo executável portátil (. exe ou. dll) para o assembly a ser verificado.  
  
 `fForceVerification`  
 [in] `true` para executar a verificação, mesmo se for necessário substituir as configurações do registro; caso contrário, `false` .  
  
 `pbEcmaPublicKey`  
 no Um ponteiro para o mapeamento da chave pública ECMA para a chave real usada para verificação.  
  
 `cbEcmaPublicKey`  
 no O comprimento da chave pública ECMA real.  
  
 `pfWasVerified`  
 [fora] `true` se a assinatura de nome forte foi verificada; caso contrário, `false` . Esse parâmetro também será definido como `false` se a verificação tiver sido bem-sucedida devido a configurações do registro.  
  
## <a name="return-value"></a>Valor Retornado  
 `S_OK`se a verificação foi bem-sucedida; caso contrário, um valor HRESULT que indica falha (consulte [valores de HRESULT comuns](/windows/win32/seccrypto/common-hresult-values) para uma lista).  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MetaHost. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Método StrongNameSignatureVerification](iclrstrongname-strongnamesignatureverification-method.md)
- [Método StrongNameSignatureVerificationEx](iclrstrongname-strongnamesignatureverificationex-method.md)
- [Interface ICLRStrongName](iclrstrongname-interface.md)
