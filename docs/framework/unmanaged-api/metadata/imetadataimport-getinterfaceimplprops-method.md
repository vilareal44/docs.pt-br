---
title: Método IMetaDataImport::GetInterfaceImplProps
ms.date: 02/25/2019
api_name:
- IMetaDataImport.GetInterfaceImplProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetInterfaceImplProps
helpviewer_keywords:
- IMetaDataImport::GetInterfaceImplProps method [.NET Framework metadata]
- GetInterfaceImpProps method [.NET Framework metadata]
ms.assetid: be3f5985-b1e4-4036-8602-c16e8508d4af
topic_type:
- apiref
ms.openlocfilehash: 1c9d9647084aa729817eeeb17ee3f5cd320c0d29
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84491206"
---
# <a name="imetadataimportgetinterfaceimplprops-method"></a>Método IMetaDataImport::GetInterfaceImplProps
Obtém um ponteiro para os tokens de metadados para o <xref:System.Type> que implementa o método especificado e para a interface que declara esse método.
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetInterfaceImplProps (  
   [in]  mdInterfaceImpl        iiImpl,  
   [out] mdTypeDef              *pClass,  
   [out] mdToken                *ptkIface  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `iiImpl`  
 no O token de metadados que representa o método para o qual retornar os tokens de classe e de interface.  
  
 `pClass`  
 fora O token de metadados que representa a classe que implementa o método.  
  
 `ptkIface`  
 fora O token de metadados que representa a interface que define o método implementado.  

## <a name="remarks"></a>Comentários

 Você Obtém o valor para `iImpl` chamando o método [EnumInterfaceImpls](imetadataimport-enuminterfaceimpls-method.md) .

 Por exemplo, suponha que uma classe tenha um `mdTypeDef` valor de token de 0x02000007 e que ele implemente três interfaces cujos tipos têm tokens:

- 0x02000003 (TypeDef)
- 0x0100000A (TypeRef)
- 0x0200001C (TypeDef)

Conceitualmente, essas informações são armazenadas em uma tabela de implementação de interface como:

| Número da linha | Token de classe | Token de interface |
|------------|-------------|-----------------|
| 4          |             |                 |
| 5          | 02000007    | 02000003        |
| 6          | 02000007    | 0100000A        |
| 7          |             |                 |
| 8          | 02000007    | 0200001C        |

Lembre-se de que o token é um valor de 4 bytes:

- Os 3 bytes inferiores contêm o número da linha, ou RID.
- O byte superior mantém o tipo de token – 0x09 para `mdtInterfaceImpl` .

`GetInterfaceImplProps`Retorna as informações mantidas na linha cujo token você fornece no `iImpl` argumento.
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
