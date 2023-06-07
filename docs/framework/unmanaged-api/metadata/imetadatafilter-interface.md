---
title: Interface IMetaDataFilter
ms.date: 03/30/2017
api_name:
- IMetaDataFilter
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataFilter
helpviewer_keywords:
- IMetaDataFilter interface [.NET Framework metadata]
ms.assetid: ec0856ef-8c56-40ba-bf60-86e0ce8b337f
topic_type:
- apiref
ms.openlocfilehash: 821936d20a421739e8eb3d5df228888df7f022e3
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503777"
---
# <a name="imetadatafilter-interface"></a>Interface IMetaDataFilter
Fornece métodos para marcação e filtragem de tokens de metadados para evitar ações repetidas que já foram realizadas.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método IsTokenMarked](imetadatafilter-istokenmarked-method.md)|Obtém um valor que indica se o token de metadados especificado foi processado.|  
|[Método MarkToken](imetadatafilter-marktoken-method.md)|Define um valor que indica que o token de metadados especificado foi processado.|  
|[Método UnmarkAll](imetadatafilter-unmarkall-method.md)|Remove as marcas de processamento de todos os tokens no escopo de metadados atual.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interfaces de metadados](metadata-interfaces.md)
