---
title: Enumeração STARTUP_FLAGS
ms.date: 03/30/2017
api_name:
- STARTUP_FLAGS
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- STARTUP_FLAGS
helpviewer_keywords:
- STARTUP_FLAGS enumeration [.NET Framework hosting]
ms.assetid: 4f043594-0c45-4bc6-988e-a6793f0d8d06
topic_type:
- apiref
ms.openlocfilehash: b4694efffa0a3dd6fed1f97fc2359c5eb335d440
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84006409"
---
# <a name="startup_flags-enumeration"></a>Enumeração STARTUP_FLAGS
Contém valores que indicam o comportamento de inicialização do Common Language Runtime (CLR). Por padrão, a coleta de lixo não é simultânea e somente a biblioteca de classes base é carregada na área de domínio neutro.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum {  
    STARTUP_CONCURRENT_GC                         = 0x1,  
    STARTUP_LOADER_OPTIMIZATION_MASK              = 0x3<<1,  
    STARTUP_LOADER_OPTIMIZATION_SINGLE_DOMAIN     = 0x1<<1,  
    STARTUP_LOADER_OPTIMIZATION_MULTI_DOMAIN      = 0x2<<1,  
    STARTUP_LOADER_OPTIMIZATION_MULTI_DOMAIN_HOST = 0x3<<1,  
  
    STARTUP_LOADER_SAFEMODE                       = 0x10,  
    STARTUP_LOADER_SETPREFERENCE                  = 0x100,  
  
    STARTUP_SERVER_GC                             = 0x1000,  
    STARTUP_HOARD_GC_VM                           = 0x2000,  
  
    STARTUP_SINGLE_VERSION_HOSTING_INTERFACE      = 0x4000,  
    STARTUP_LEGACY_IMPERSONATION                  = 0x10000,  
    STARTUP_DISABLE_COMMITTHREADSTACK             = 0x20000,  
    STARTUP_ALWAYSFLOW_IMPERSONATION              = 0x40000,  
    STARTUP_TRIM_GC_COMMIT                        = 0x80000,  
  
    STARTUP_ETW                                   = 0x100000,  
    STARTUP_ARM                                   = 0x400000  
} STARTUP_FLAGS;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`STARTUP_CONCURRENT_GC`|Especifica que a coleta de lixo simultânea deve ser usada. Se o chamador solicitar a compilação do servidor e a coleta de lixo simultânea em um computador com um único processador, a compilação da estação de trabalho e a coleta de lixo não simultânea serão executadas em vez disso. **Observação:**  Não há suporte para a coleta de lixo simultânea em aplicativos que executam o emulador x86 WOW64 em sistemas de 64 bits que implementam a arquitetura Intel Itanium (anteriormente chamada IA-64). Para obter mais informações sobre como usar o WOW64 em sistemas Windows de 64 bits, consulte [executando aplicativos de 32 bits](/windows/desktop/WinProg64/running-32-bit-applications).|  
|`STARTUP_LOADER_OPTIMIZATION_MASK`|Especifica que a otimização do carregador deve ocorrer.|  
|`STARTUP_LOADER_OPTIMIZATION_SINGLE_DOMAIN`|Especifica que nenhum assembly é carregado como domínio neutro.|  
|`STARTUP_LOADER_OPTIMIZATION_MULTI_DOMAIN`|Especifica que todos os assemblies são carregados como domínio neutro.|  
|`STARTUP_LOADER_OPTIMIZATION_MULTI_DOMAIN_HOST`|Especifica que todos os assemblies de nome forte são carregados como domínio neutro.|  
|`STARTUP_LOADER_SAFEMODE`|Especifica que a política de versão do CLR não será aplicada à versão passada. A versão exata especificada do CLR será carregada. O Shim não avalia a política para determinar a versão mais recente compatível.|  
|`STARTUP_LOADER_SETPREFERENCE`|Especifica que o tempo de execução preferencial será definido, mas não iniciado de fato.|  
|`STARTUP_SERVER_GC`|Especifica que a coleta de lixo do servidor será usada.|  
|`STARTUP_HOARD_GC_VM`|Especifica que a coleta de lixo manterá o endereço virtual usado.|  
|`STARTUP_SINGLE_VERSION_HOSTING_INTERFACE`|Especifica que a combinação de uma interface de hospedagem não será permitida.|  
|`STARTUP_LEGACY_IMPERSONATION`|Especifica que a representação não deve fluir entre pontos assíncronos por padrão.|  
|`STARTUP_DISABLE_COMMITTHREADSTACK`|Especifica que a pilha de threads completa não deve ser confirmada quando o thread inicia a execução.|  
|`STARTUP_ALWAYSFLOW_IMPERSONATION`|Especifica que as impessoas gerenciadas e as impessoas obtidas por meio da invocação de plataforma fluirão entre pontos assíncronos. Por padrão, somente as impessoas gerenciadas serão fluidas entre pontos assíncronos.|  
|`STARTUP_TRIM_GC_COMMIT`|Especifica que a coleta de lixo usará menos espaço confirmado quando a memória do sistema estiver baixa. Consulte `gcTrimCommitOnLowMemory` em [otimização para hospedagem na Web compartilhada](../../../standard/garbage-collection/optimization-for-shared-web-hosting.md).|  
|`STARTUP_ETW`|Especifica que o ETW (rastreamento de eventos para Windows) está habilitado para eventos de Common Language Runtime. A partir do Windows Vista, o rastreamento de eventos está sempre habilitado, portanto, esse sinalizador não tem nenhum efeito. Consulte [controlando o log de .NET Framework](../../performance/controlling-logging.md).|  
|`STARTUP_ARM`|Especifica que o monitoramento de recursos de domínio de aplicativo está habilitado. Consulte a <xref:System.AppDomain.MonitoringIsEnabled%2A?displayProperty=nameWithType> propriedade e o [ \<appDomainResourceMonitoring> elemento](../../configure-apps/file-schema/runtime/appdomainresourcemonitoring-element.md).|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Hospedando enumerações](hosting-enumerations.md)
