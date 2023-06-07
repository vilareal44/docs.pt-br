---
title: Enumeração CorPinvokeMap
ms.date: 03/30/2017
api_name:
- CorPinvokeMap
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorPinvokeMap
helpviewer_keywords:
- CorPinvokeMap enumeration [.NET Framework metadata]
ms.assetid: f14f986e-f6ce-42bc-aa23-18150c46d28c
topic_type:
- apiref
ms.openlocfilehash: 199a649b0481c2a740926636345eefbda6831ef2
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007540"
---
# <a name="corpinvokemap-enumeration"></a>Enumeração CorPinvokeMap
Especifica as opções para uma chamada PInvoke.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum  CorPinvokeMap {  
  
    pmNoMangle          = 0x0001,  
  
    pmCharSetMask       = 0x0006,  
    pmCharSetNotSpec    = 0x0000,  
    pmCharSetAnsi       = 0x0002,  
    pmCharSetUnicode    = 0x0004,  
    pmCharSetAuto       = 0x0006,  
  
    pmBestFitUseAssem   = 0x0000,  
    pmBestFitEnabled    = 0x0010,  
    pmBestFitDisabled   = 0x0020,  
    pmBestFitMask       = 0x0030,  
  
    pmThrowOnUnmappableCharUseAssem   = 0x0000,  
    pmThrowOnUnmappableCharEnabled    = 0x1000,  
    pmThrowOnUnmappableCharDisabled   = 0x2000,  
    pmThrowOnUnmappableCharMask       = 0x3000,  
  
    pmSupportsLastError = 0x0040,
  
    pmCallConvMask      = 0x0700,  
    pmCallConvWinapi    = 0x0100,  
    pmCallConvCdecl     = 0x0200,  
    pmCallConvStdcall   = 0x0300,  
    pmCallConvThiscall  = 0x0400,  
    pmCallConvFastcall  = 0x0500,  
  
    pmMaxValue          = 0xFFFF  
  
} CorPinvokeMap;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`pmNoMangle`|Use cada nome de membro conforme especificado.|  
|`pmCharSetMask`|Reservado.|  
|`pmCharSetNotSpec`|Reservado.|  
|`pmCharSetAnsi`|Realizar marshaling de cadeias de caracteres como cadeias de caracteres de vários bytes.|  
|`pmCharSetUnicode`|Realizar marshaling de cadeias de caracteres como cadeias de caracteres Unicode de 2 bytes.|  
|`pmCharSetAuto`|Realizar marshaling automático de cadeias de caracteres apropriado para o sistema operacional de destino. O padrão é Unicode no Windows NT, Windows 2000, Windows XP e na família Windows Server 2003; o padrão é ANSI no Windows 98 e Windows me.|  
|`pmBestFitUseAssem`|Reservado.|  
|`pmBestFitEnabled`|Execute o mapeamento de melhor ajuste de caracteres Unicode que não têm uma correspondência exata no conjunto de caracteres ANSI.|  
|`pmBestFitDisabled`|Não execute o mapeamento de melhor ajuste de caracteres Unicode. Nesse caso, todos os caracteres mapeável serão substituídos por um '? '.|  
|`pmBestFitMask`|Reservado.|  
|`pmThrowOnUnmappableCharUseAssem`|Reservado.|  
|`pmThrowOnUnmappableCharEnabled`|Gerar uma exceção quando o marshaling interop encontrar um caractere mapeável.|  
|`pmThrowOnUnmappableCharDisabled`|Não lance uma exceção quando o marshaling interop encontra um caractere mapeável.|  
|`pmThrowOnUnmappableCharMask`|Reservado|  
|`pmSupportsLastError`|Permitir que o receptor chame a função do Win32 `SetLastError` antes de retornar do método atribuído.|  
|`pmCallConvMask`|Reservado|  
|`pmCallConvWinapi`|Use a Convenção de chamada de plataforma padrão. Por exemplo, no Windows, o padrão é `StdCall` e em Windows CE .net é `Cdecl` .|  
|`pmCallConvCdecl`|Use a `Cdecl` Convenção de chamada. Nesse caso, o chamador limpa a pilha. Isso permite chamar funções com `varargs` (ou seja, funções que aceitam um número variável de parâmetros).|  
|`pmCallConvStdcall`|Use a `StdCall` Convenção de chamada. Nesse caso, o receptor limpa a pilha. Essa é a convenção padrão para chamar funções não gerenciadas com a invocação da plataforma.|  
|`pmCallConvThiscall`|Use a `ThisCall` Convenção de chamada. Nesse caso, o primeiro parâmetro é o `this` ponteiro e é armazenado no registro ecx. Outros parâmetros são enviados por push na pilha. A `ThisCall` Convenção de chamada é usada para chamar métodos em classes exportadas de uma dll não gerenciada.|  
|`pmCallConvFastcall`|Reservado.|  
|`pmMaxValue`|Reservado.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorHdr. h  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Enumerações de metadados](metadata-enumerations.md)
