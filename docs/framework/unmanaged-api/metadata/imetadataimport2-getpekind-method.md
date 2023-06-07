---
title: Método IMetaDataImport2::GetPEKind
ms.date: 03/30/2017
api_name:
- IMetaDataImport2.GetPEKind
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport2::GetPEKind
helpviewer_keywords:
- GetPEKind method [.NET Framework metadata]
- IMetaDataImport2::GetPEKind method [.NET Framework metadata]
ms.assetid: d91c3d89-8022-4a4c-a2a2-a8af2c387507
topic_type:
- apiref
ms.openlocfilehash: 3626998c456e23fb922ae45a68bedb0e45a7ccba
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84490426"
---
# <a name="imetadataimport2getpekind-method"></a>Método IMetaDataImport2::GetPEKind
Obtém um valor que identifica a natureza do código no arquivo executável portátil (PE), normalmente um arquivo DLL ou EXE, que é definido no escopo de metadados atual.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetPEKind (  
   [out] DWORD *pdwPEKind,  
   [out] DWORD *pdwMachine  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pdwPEKind`  
 fora Um ponteiro para um valor da enumeração [CorPEKind](corpekind-enumeration.md) que descreve o arquivo PE.  
  
 `pdwMachine`  
 fora Um ponteiro para um valor que identifica a arquitetura do computador. Consulte a próxima seção para obter os valores possíveis.  
  
## <a name="remarks"></a>Comentários  
 O valor referenciado pelo `pdwMachine` parâmetro pode ser um dos seguintes.  
  
|Valor|Arquitetura do computador|  
|-----------|--------------------------|  
|IMAGE_FILE_MACHINE_I386<br /><br /> 0x014C|x86|  
|IMAGE_FILE_MACHINE_IA64<br /><br /> 0x0200|IPF Intel|  
|IMAGE_FILE_MACHINE_AMD64<br /><br /> 0x8664|x64|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport2](imetadataimport2-interface.md)
- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Enumeração CorPEKind](corpekind-enumeration.md)
