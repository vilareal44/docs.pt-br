---
title: Método IMetaDataImport::GetEventProps
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetEventProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetEventProps
helpviewer_keywords:
- IMetaDataImport::GetEventProps method [.NET Framework metadata]
- GetEventProps method [.NET Framework metadata]
ms.assetid: 5eaf3b4a-92b7-4d5b-97e0-1e83721e0052
topic_type:
- apiref
ms.openlocfilehash: 3b47d1559300a462ccda42bc88da43f66c1043ec
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84491297"
---
# <a name="imetadataimportgeteventprops-method"></a>Método IMetaDataImport::GetEventProps
Obtém informações de metadados para o evento representado pelo token de evento especificado, incluindo o tipo declarativo, os métodos Add e remove para delegados e quaisquer sinalizadores e outros dados associados.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetEventProps (  
   [in]  mdEvent       ev,  
   [out] mdTypeDef     *pClass,
   [out] LPCWSTR       szEvent,
   [in]  ULONG         cchEvent,
   [out] ULONG         *pchEvent,
   [out] DWORD         *pdwEventFlags,  
   [out] mdToken       *ptkEventType,  
   [out] mdMethodDef   *pmdAddOn,
   [out] mdMethodDef   *pmdRemoveOn,
   [out] mdMethodDef   *pmdFire,
   [out] mdMethodDef   rmdOtherMethod[],
   [in]  ULONG         cMax,  
   [out] ULONG         *pcOtherMethod  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `ev`  
 no O token de metadados do evento que representa o evento para o qual obter metadados.  
  
 `pClass`  
 fora Um ponteiro para o token de TypeDef que representa a classe que declara o evento.  
  
 `szEvent`  
 fora O nome do evento referenciado por `ev` .  
  
 `pchEvent`  
 no O comprimento solicitado em caracteres largos de `szEvent` .  
  
 `pdwEventFlags`  
 fora O comprimento retornado em caracteres largos de `szEvent` .  
  
 `ptkEventType`  
 fora Um ponteiro para um token de metadados TypeRef ou TypeDef que representa o <xref:System.Delegate> tipo do evento.  
  
 `pmdAddOn`  
 fora Um ponteiro para o token de metadados que representa o método que Adiciona manipuladores para o evento.  
  
 `pmdRemoveOn`  
 fora Um ponteiro para o token de metadados que representa o método que remove manipuladores para o evento.  
  
 `pmdFire`  
 fora Um ponteiro para o token de metadados que representa o método que gera o evento.  
  
 `rmdOtherMethod`  
 fora Uma matriz de ponteiros de token para outros métodos associados ao evento.  
  
 `cMax`  
 no O tamanho máximo da `rmdOtherMethod` matriz.  
  
 `pcOtherMethod`  
 fora O número de tokens retornados em `rmdOtherMethod` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
