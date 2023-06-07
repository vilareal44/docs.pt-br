---
title: Interface IMetaDataDispenser
ms.date: 03/30/2017
api_name:
- IMetaDataDispenser
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataDispenser
helpviewer_keywords:
- IMetaDataDispenser interface [.NET Framework metadata]
ms.assetid: 989840b3-9822-4ce5-a6c5-b375d3340a7a
topic_type:
- apiref
ms.openlocfilehash: 2bdfe65dbf923ec61d91a259b5257d892fef53da
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007332"
---
# <a name="imetadatadispenser-interface"></a>Interface IMetaDataDispenser
Fornece métodos para criar um novo escopo de metadados ou abrir um existente.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método DefineScope](imetadatadispenser-definescope-method.md)|Cria uma nova área na memória na qual você pode criar novos metadados.|  
|[Método OpenScope](imetadatadispenser-openscope-method.md)|Abre um arquivo existente em disco e mapeia seus metadados na memória.|  
|[Método OpenScopeOnMemory](imetadatadispenser-openscopeonmemory-method.md)|Abre uma área de memória que contém os metadados existentes. Ou seja, esse método abre uma área especificada de memória na qual os dados existentes são tratados como metadados.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataforma:** Consulte [requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataDispenserEx](imetadatadispenserex-interface.md)
- [Interfaces de metadados](metadata-interfaces.md)
