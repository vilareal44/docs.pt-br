---
title: Enumeração CorAssemblyFlags
ms.date: 03/30/2017
api_name:
- CorAssemblyFlags
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorAssemblyFlags
helpviewer_keywords:
- CorAssemblyFlags enumeration [.NET Framework metadata]
ms.assetid: bb8db3b6-d81d-49fc-b74c-dbc908a9eab9
topic_type:
- apiref
ms.openlocfilehash: b1a83f07f03ddb17d5c306453cf838101a77ed65
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007930"
---
# <a name="corassemblyflags-enumeration"></a>Enumeração CorAssemblyFlags
Contém valores que descrevem os metadados aplicados a uma compilação de assembly.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum CorAssemblyFlags {  
  
    afPublicKey             =   0x0001,  
    afPA_None               =   0x0000,  
    afPA_MSIL               =   0x0010,  
    afPA_x86                =   0x0020,  
    afPA_IA64               =   0x0030,  
    afPA_AMD64              =   0x0040,  
    afPA_ARM                =   0x0050,  
    afPA_NoPlatform         =   0x0070,  
    afPA_Specified          =   0x0080,  
    afPA_Mask               =   0x0070,  
    afPA_FullMask           =   0x00F0,  
    afPA_Shift              =   0x0004,  
  
    afEnableJITcompileTracking  =   0x8000,  
    afDisableJITcompileOptimizer=   0x4000,  
  
    afRetargetable          =   0x0100,  
    afContentType_Default        =   0x0000,  
    afContentType_WindowsRuntime =   0x0200,  
    afContentType_Mask           =   0x0E00,  
  
} CorAssemblyFlags;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`afPublicKey`|Indica que a referência de assembly contém a chave pública completa sem hash.|  
|`afPA_None`|Indica que a arquitetura do processador não está especificada.|  
|`afPA_MSIL`|Indica que a arquitetura do processador é neutra (PE32).|  
|`afPA_x86`|Indica que a arquitetura do processador é x86 (PE32).|  
|`afPA_IA64`|Indica que a arquitetura do processador é Itanium (PE32 +).|  
|`afPA_AMD64`|Indica que a arquitetura do processador é AMD x64 (PE32 +).|  
|`afPA_ARM`|Indica que a arquitetura do processador é ARM (PE32).|  
|`afPA_NoPlatform`|Indica que o assembly é um assembly de referência; ou seja, ele se aplica a qualquer arquitetura, mas não pode ser executado em nenhuma arquitetura. Portanto, o sinalizador é o mesmo que `afPA_Mask` .|  
|`afPA_Specified`|Indica que os sinalizadores de arquitetura do processador devem ser propagados para o `AssemblyRef` registro.|  
|`afPA_Mask`|Uma máscara que descreve a arquitetura do processador.|  
|`afPA_FullMask`|Especifica que a descrição da arquitetura do processador está incluída.|  
|`afPA_Shift`|Indica uma contagem de turnos nos sinalizadores de arquitetura do processador de e para o índice.|  
|`afEnableJITcompileTracking`|Indica o valor correspondente do <xref:System.Diagnostics.DebuggableAttribute.DebuggingModes> do <xref:System.Diagnostics.DebuggableAttribute> .|  
|`afDisableJITcompileOptimizer`|Indica o valor correspondente do <xref:System.Diagnostics.DebuggableAttribute.DebuggingModes> do <xref:System.Diagnostics.DebuggableAttribute> .|  
|`afRetargetable`|Indica que o assembly pode ser redirecionado em tempo de execução para um assembly de um Publicador diferente.|  
|`afContentType_Mask`|Uma máscara que descreve o tipo de conteúdo.|  
|`afContentType_Default`|Indica o tipo de conteúdo padrão.|  
|`afContentType_WindowsRuntime`|Indica o tipo de conteúdo de Windows Runtime.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorHdr. h  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Enumerações de metadados](metadata-enumerations.md)
