---
title: Interface IMetaDataAssemblyEmit
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyEmit
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyEmit
helpviewer_keywords:
- IMetaDataAssemblyEmit interface [.NET Framework metadata]
ms.assetid: 34fb03cc-2285-4a45-ac48-ad993b7a921a
topic_type:
- apiref
ms.openlocfilehash: 807e1ee831a43a4ef1e7b0a269ee38131f24081e
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008112"
---
# <a name="imetadataassemblyemit-interface"></a>Interface IMetaDataAssemblyEmit
Fornece métodos que dão suporte ao modelo de autodescrição usado pelo Common Language Runtime para resolver e consumir recursos.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método DefineAssembly](imetadataassemblyemit-defineassembly-method.md)|Cria uma estrutura de dados de assembly contendo metadados para o assembly especificado e retorna o token de metadados associado.|  
|[Método DefineAssemblyRef](imetadataassemblyemit-defineassemblyref-method.md)|Cria uma `AssemblyRef` estrutura que contém metadados para o assembly ao qual este assembly faz referência e retorna o token de metadados associado.|  
|[Método DefineExportedType](imetadataassemblyemit-defineexportedtype-method.md)|Cria uma `ExportedType` estrutura que contém metadados para o tipo exportado especificado e retorna o token de metadados associado.|  
|[Método DefineFile](imetadataassemblyemit-definefile-method.md)|Cria uma `File` estrutura de metadados que contém metadados para o assembly referenciado por esse assembly e retorna o token de metadados associado.|  
|[Método DefineManifestResource](imetadataassemblyemit-definemanifestresource-method.md)|Cria uma `ManifestResource` estrutura que contém metadados para o recurso de manifesto especificado e retorna o token de metadados associado.|  
|[Método SetAssemblyProps](imetadataassemblyemit-setassemblyprops-method.md)|Modifica a estrutura de `Assembly` metadados especificada.|  
|[Método SetAssemblyRefProps](imetadataassemblyemit-setassemblyrefprops-method.md)|Modifica a estrutura de `AssemblyRef` metadados especificada.|  
|[Método SetExportedTypeProps](imetadataassemblyemit-setexportedtypeprops-method.md)|Modifica a estrutura de `ExportedType` metadados especificada.|  
|[Método SetFileProps](imetadataassemblyemit-setfileprops-method.md)|Modifica a estrutura de `File` metadados especificada.|  
|[Método SetManifestResourceProps](imetadataassemblyemit-setmanifestresourceprops-method.md)|Modifica a estrutura de `ManifestResource` metadados especificada.|  
  
## <a name="remarks"></a>Comentários  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interfaces de metadados](metadata-interfaces.md)
- [Interface IMetaDataAssemblyImport](imetadataassemblyimport-interface.md)
