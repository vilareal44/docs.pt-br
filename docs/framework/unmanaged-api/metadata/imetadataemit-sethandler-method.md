---
title: Método IMetaDataEmit::SetHandler
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.SetHandler
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::SetHandler
helpviewer_keywords:
- IMetaDataEmit::SetHandler method [.NET Framework metadata]
- SetHandler method [.NET Framework metadata]
ms.assetid: c6c1aaaf-e2cd-407c-b73e-fbe6ffd83bb3
topic_type:
- apiref
ms.openlocfilehash: 4fa227d18b8cb10936d93fda9bcaf413ce63ca3b
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84003926"
---
# <a name="imetadataemitsethandler-method"></a>Método IMetaDataEmit::SetHandler
Define o método referenciado pelo `IUnknown` ponteiro especificado como um retorno de chamada de notificação para remapeamentos de token.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetHandler (
    [in]  IUnknown    *pUnk  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pUnk`  
 no O manipulador a ser registrado.  
  
## <a name="remarks"></a>Comentários  
 O mecanismo de metadados envia a notificação usando o método fornecido pelo `SetHandler` , para compiladores que não geram registros de forma otimizada e que gostaria de otimizar os registros salvos.  
  
 Se o método de retorno de chamada não for fornecido por meio `SetHandler` do, nenhuma otimização será executada no salvamento, exceto onde vários escopos de importação tiverem sido mesclados usando `IMapToken` na mesclagem para cada escopo.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataEmit](imetadataemit-interface.md)
- [Interface IMetaDataEmit2](imetadataemit2-interface.md)
