---
title: Método ICLRStrongName::StrongNameKeyGen
ms.date: 03/30/2017
api_name:
- ICLRStrongName.StrongNameKeyGen
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRStrongName::StrongNameKeyGen
helpviewer_keywords:
- StrongNameKeyGen method, ICLRStrongName interface [.NET Framework hosting]
- ICLRStrongName::StrongNameKeyGen method [.NET Framework hosting]
ms.assetid: ac5c1245-9acf-4271-9c08-3d9b7c670df3
topic_type:
- apiref
ms.openlocfilehash: 69ba58cc8c5235a15749281b3107481be9528f84
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503972"
---
# <a name="iclrstrongnamestrongnamekeygen-method"></a>Método ICLRStrongName::StrongNameKeyGen
Cria um novo par de chaves públicas/privadas para uso de nome forte.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT StrongNameKeyGen (  
    [in]  LPCWSTR   wszKeyContainer,  
    [in]  DWORD     dwFlags,  
    [out] BYTE      **ppbKeyBlob,  
    [out] ULONG     *pcbKeyBlob  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `wszKeyContainer`  
 no O nome do contêiner de chave solicitado. `wszKeyContainer`deve ser uma cadeia de caracteres não vazia ou NULL para gerar um nome temporário.  
  
 `dwFlags`  
 no Um valor que especifica se a chave deve ser desregistrada. Os seguintes valores têm suporte:  
  
- 0x00000000-usado quando `wszKeyContainer` é nulo para gerar um nome de contêiner de chave temporário.  
  
- 0x00000001 ( `SN_LEAVE_KEY` ) – especifica que a chave deve ser registrada.  
  
 `ppbKeyBlob`  
 fora O par de chaves pública/privada retornado.  
  
 `pcbKeyBlob`  
 fora O tamanho, em bytes, de `ppbKeyBlob` .  
  
## <a name="return-value"></a>Valor Retornado  
 `S_OK`Se o método foi concluído com êxito; caso contrário, um valor HRESULT que indica falha (consulte [valores de HRESULT comuns](/windows/win32/seccrypto/common-hresult-values) para uma lista).  
  
## <a name="remarks"></a>Comentários  
 O método [ICLRStrongName:: StrongNameKeyGen](iclrstrongname-strongnamekeygen-method.md) cria uma chave de 1024 bits. Depois que a chave for recuperada, você deverá chamar o método [ICLRStrongName:: StrongNameFreeBuffer](iclrstrongname-strongnamefreebuffer-method.md) para liberar a memória alocada.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MetaHost. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Método StrongNameKeyGenEx](iclrstrongname-strongnamekeygenex-method.md)
- [Interface ICLRStrongName](iclrstrongname-interface.md)
