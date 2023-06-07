---
title: Interface ICeeGen
ms.date: 03/30/2017
api_name:
- ICeeGen
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICeeGen
helpviewer_keywords:
- ICeeGen interface [.NET Framework metadata]
ms.assetid: 383d20b0-efe9-4e71-8fb8-1186b2e7d0c0
topic_type:
- apiref
ms.openlocfilehash: e6cf0aa6f731d0a417e1a2be0ca1d0f8c9299379
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008268"
---
# <a name="iceegen-interface"></a>Interface ICeeGen
Fornece métodos para a compilação dinâmica de código.  
  
 Esta interface está obsoleta e não deve ser usada.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método AddSectionReloc](iceegen-addsectionreloc-method.md)|Obsoleto. Adiciona uma instrução. realocação à base de código.|  
|[Método AllocateMethodBuffer](iceegen-allocatemethodbuffer-method.md)|Obsoleto. Cria um buffer do tamanho especificado para um método e Obtém o endereço virtual relativo do método.|  
|[Método ComputePointer](iceegen-computepointer-method.md)|Obsoleto. Determina o buffer para a seção de código especificada.|  
|[Método EmitString](iceegen-emitstring-method.md)|Obsoleto. Emite a cadeia de caracteres especificada na base de código.|  
|[Método GenerateCeeFile](iceegen-generateceefile-method.md)|Obsoleto. Gera um arquivo de base de código que contém a base de código atualmente carregada para isso `ICeeGen` .|  
|[Método GenerateCeeMemoryImage](iceegen-generateceememoryimage-method.md)|Obsoleto. Gera uma imagem na memória para a base de código.|  
|[Método GetIlSection](iceegen-getilsection-method.md)|Obsoleto. Obtém a seção da base de código de linguagem intermediária referenciada pelo identificador especificado.|  
|[Método GetIMapTokenIface](iceegen-getimaptokeniface-method.md)|Obsoleto. Obtém a interface referenciada pelo token especificado.|  
|[Método GetMethodBuffer](iceegen-getmethodbuffer-method.md)|Obsoleto. Obtém um buffer do tamanho apropriado para o método no endereço virtual relativo especificado.|  
|[Método GetSectionBlock](iceegen-getsectionblock-method.md)|Obsoleto. Obtém um bloco de seção da base de código.|  
|[Método GetSectionCreate](iceegen-getsectioncreate-method.md)|Obsoleto. Gera e obtém uma seção de código usando os valores de nome e sinalizador especificados.|  
|[Método GetSectionDataLen](iceegen-getsectiondatalen-method.md)|Obsoleto. Obtém o comprimento da seção especificada.|  
|[Método GetString](iceegen-getstring-method.md)|Obsoleto. Obtém a cadeia de caracteres armazenada no endereço virtual relativo especificado.|  
|[Método GetStringSection](iceegen-getstringsection-method.md)|Obsoleto. Obtém uma representação de cadeia de caracteres da seção de código referenciada pelo identificador especificado.|  
|[Método TruncateSection](iceegen-truncatesection-method.md)|Obsoleto. Trunca a seção de código especificada pelo comprimento especificado.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interfaces de metadados](metadata-interfaces.md)
