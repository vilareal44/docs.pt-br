---
title: Método ICeeGen::AddSectionReloc
ms.date: 03/30/2017
api_name:
- ICeeGen.AddSectionReloc
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICeeGen::AddSectionReloc
helpviewer_keywords:
- AddSectionReloc method [.NET Framework metadata]
- ICeeGen::AddSectionReloc method [.NET Framework metadata]
ms.assetid: b500a260-1d57-4953-95e1-c27063f7c8da
topic_type:
- apiref
ms.openlocfilehash: 2f66d34fcfdd8c61dcc92817ec1a928ac5b603fc
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008892"
---
# <a name="iceegenaddsectionreloc-method"></a>Método ICeeGen::AddSectionReloc
Adiciona uma instrução. realocação à base de código.  
  
 Este método é obsoleto e não deve ser usado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT AddSectionReloc (  
   [in] HCEESECTION            section,  
   [in] ULONG                  offset,  
   [in] HCEESECTION            relativeTo,
   [in] CeeSectionRelocType    relocType  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `section`  
 no A seção de código na memória à qual adicionar uma instrução. realocação.  
  
 `offset`  
 no O deslocamento da seção.  
  
 `relativeTo`  
 no A seção à qual `offset` se refere.  
  
 `relocType`  
 no Um dos valores de [CeeSectionRelocType](ceesectionreloctype-enumeration.md) , indicando o tipo de instrução. realocação a ser adicionado.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICeeGen](iceegen-interface.md)
