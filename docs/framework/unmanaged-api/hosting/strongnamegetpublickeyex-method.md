---
title: Método StrongNameGetPublicKeyEx
ms.date: 03/30/2017
api_name:
- ICLRStrongName2.StrongNameGetPublicKeyEx
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- StrongNameGetPublicKeyEx
helpviewer_keywords:
- StrongNameGetPublicKeyEx method, ICLRStrongName2 interface [.NET Framework hosting]
- ICLRStrongName2::StrongNameGetPublicKeyEx method [.NET Framework hosting]
ms.assetid: 63d8260c-fb32-4f8f-a357-768afd570f68
topic_type:
- apiref
ms.openlocfilehash: 1904a98f254a988ce035847a4cdeede182aa07bf
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84006363"
---
# <a name="strongnamegetpublickeyex-method"></a>Método StrongNameGetPublicKeyEx
Obtém a chave pública de um par de chaves pública/privada e especifica um algoritmo de hash e um algoritmo de assinatura.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT StrongNameGetPublicKey (
    [in]  LPCWSTR   pwzKeyContainer,  
    [in]  BYTE      *pbKeyBlob,  
    [in]  ULONG     cbKeyBlob,  
    [out] BYTE      **ppbPublicKeyBlob,  
    [out] ULONG     *pcbPublicKeyBlob  
    [in]  ULONG     uHashAlgId,  
    [in]  ULONG     uReserved,  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pwzKeyContainer`  
 no O nome do contêiner de chave que contém o par de chaves pública/privada. Se `pbKeyBlob` for NULL, `szKeyContainer` deve especificar um contêiner válido no CSP (provedor de serviços de criptografia). Nesse caso, o `StrongNameGetPublicKeyEx` método extrai a chave pública do par de chaves armazenado no contêiner.  
  
 Se `pbKeyBlob` não for NULL, o par de chaves será considerado contido no BLOB (objeto binário grande) de chave.  
  
 As chaves devem ter chaves de assinatura de 1024 bits Rivest-Shamir-Adleman (RSA). Não há suporte para nenhum outro tipo de chave no momento.  
  
 `pbKeyBlob`  
 no Um ponteiro para o par de chaves pública/privada. Esse par está no formato criado pela função do Win32 `CryptExportKey` . Se `pbKeyBlob` for NULL, o contêiner de chave especificado por `szKeyContainer` será considerado para conter o par de chaves.  
  
 `cbKeyBlob`  
 no O tamanho, em bytes, de `pbKeyBlob` .  
  
 `ppbPublicKeyBlob`  
 fora O BLOB de chave pública retornado. O `ppbPublicKeyBlob` parâmetro é alocado pelo Common Language Runtime e retornado ao chamador. O chamador deve liberar a memória usando o método [ICLRStrongName:: StrongNameFreeBuffer](iclrstrongname-strongnamefreebuffer-method.md) .  
  
 `pcbPublicKeyBlob`  
 fora O tamanho do BLOB de chave pública retornado.  
  
 `uHashAlgId`  
 no O algoritmo de hash de assembly. Consulte a seção comentários para obter uma lista de valores aceitos.  
  
 `uReserved`  
 no Reservado para uso futuro; o padrão é NULL.  
  
## <a name="return-value"></a>Valor Retornado  
 `S_OK`Se o método foi concluído com êxito; caso contrário, um valor HRESULT que indica falha (consulte [valores de HRESULT comuns](/windows/win32/seccrypto/common-hresult-values) para uma lista).  
  
## <a name="remarks"></a>Comentários  
 A chave pública está contida em uma estrutura [PublicKeyBlob](../strong-naming/publickeyblob-structure.md) .  
  
## <a name="remarks"></a>Comentários  
 A tabela a seguir mostra o conjunto de valores aceitos para o `uHashAlgId` parâmetro.  
  
|Name|Valor|  
|----------|-----------|  
|Nenhum|0|  
|SHA-1|0x8004|  
|SHA-256|0x800c|  
|SHA-384|0x800d|  
|SHA-512|0x800e|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MetaHost. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Método StrongNameTokenFromPublicKey](iclrstrongname-strongnametokenfrompublickey-method.md)
- [Estrutura PublicKeyBlob](../strong-naming/publickeyblob-structure.md)
- [Interface ICLRStrongName](iclrstrongname-interface.md)
- [Método StrongNameGetPublicKey](iclrstrongname-strongnamegetpublickey-method.md)
