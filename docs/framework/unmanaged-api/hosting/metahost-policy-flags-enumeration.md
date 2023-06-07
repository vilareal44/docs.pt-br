---
title: Enumeração METAHOST_POLICY_FLAGS
ms.date: 03/30/2017
api_name:
- METAHOST_POLICY_FLAGS
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- METAHOST_POLICY_FLAGS
helpviewer_keywords:
- METAHOST_POLICY_FLAGS enumeration [.NET Framework hosting]
ms.assetid: 3bb4b526-0118-42e2-ba59-c95648528ce9
topic_type:
- apiref
ms.openlocfilehash: bb40ed65a2e34f1bf293e4c4c842d7db63d2eaa5
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008437"
---
# <a name="metahost_policy_flags-enumeration"></a>Enumeração METAHOST_POLICY_FLAGS
Fornece políticas de associação que são comuns à maioria dos hosts de tempo de execução. Essa enumeração é usada pelo método [ICLRMetaHostPolicy:: GetRequestedRuntime](iclrmetahostpolicy-getrequestedruntime-method.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum {  
    METAHOST_POLICY_HIGHCOMPAT              = 0x00,  
    METAHOST_POLICY_APPLY_UPGRADE_POLICY    = 0x08,  
    METAHOST_POLICY_EMULATE_EXE_LAUNCH      = 0x10,  
    METAHOST_POLICY_SHOW_ERROR_DIALOG       = 0x20,  
    METAHOST_POLICY_USE_PROCESS_IMAGE_PATH  = 0x40,  
    METAHOST_POLICY_ENSURE_SKU_SUPPORTED    = 0x80,  
    METAHOST_POLICY_IGNORE_ERROR_MODE       = 0x1000  
  
} METAHOST_POLICY_FLAGS;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`METAHOST_POLICY_HIGHCOMPAT`|Define a política de alta compatibilidade, que não considera Common Language Runtime (CLR) que é carregado no processo atual. Em vez disso, ele considera apenas o CLRs instalado e as preferências do componente, como derivado do próprio arquivo de assembly, a versão interna declarada ou o arquivo de configuração.|  
|`METAHOST_POLICY_APPLY_UPGRADE_POLICY`|Aplica a política de atualização ao resultado da Associação de versão quando uma correspondência exata não é encontrada, com base no conteúdo de HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft \\ . NETFramework\Policy\Upgrades. Isso tem o mesmo efeito que [RUNTIME_INFO_UPGRADE_VERSION](runtime-info-flags-enumeration.md).|  
|`METAHOST_POLICY_EMULATE_EXE_LAUNCH`|Os resultados da associação são retornados como se a imagem fornecida à chamada fosse iniciada em um novo processo. Atualmente, `GetRequestedRuntime` o ignora o conjunto de tempos de execução carregável e é associado ao conjunto de tempos de execução instalados. Esse sinalizador permite que um host determine a qual tempo de execução um EXE se associará quando for iniciado.|  
|`METAHOST_POLICY_SHOW_ERROR_DIALOG`|Uma caixa de diálogo de erro será exibida se `GetRequestedRuntime` o não conseguir encontrar um tempo de execução compatível com os parâmetros de entrada. A partir do .NET Framework 4,5, essa caixa de diálogo de erro pode assumir a forma de uma caixa de diálogo de recurso do Windows que pergunta se o usuário deseja habilitar o recurso apropriado.|  
|`METAHOST_POLICY_USE_PROCESS_IMAGE_PATH`|`GetRequestedRuntime`usa a imagem do processo (e qualquer arquivo de configuração correspondente) como entrada adicional para o processo de associação. Por padrão, `GetRequestedRuntime` o não retorna ao caminho da imagem do processo (normalmente, o exe que foi usado para iniciar o processo) ao determinar o tempo de execução ao qual associar.|  
|`METAHOST_POLICY_ENSURE_SKU_SUPPORTED`|`GetRequestedRuntime`deve verificar se o SKU apropriado está instalado quando não há informações disponíveis no arquivo de configuração. Isso permite que aplicativos que não têm arquivos de configuração falhem normalmente em SKUs menores do que a instalação padrão do .NET Framework. Por padrão, `GetRequestedRuntime` o não verifica se o SKU apropriado está instalado, a menos que o atributo SKU seja especificado no elemento arquivo de configuração `<supportedRuntime />` .|  
|`METAHOST_POLICY_IGNORE_ERROR_MODE`|`GetRequestedRuntime`deve ignorar SEM_FAILCRITICALERRORS (que é definido chamando a função [SetError](/windows/win32/api/errhandlingapi/nf-errhandlingapi-seterrormode) ) e mostrar a caixa de diálogo de erro. Por padrão, SEM_FAILCRITICALERRORS suprime a caixa de diálogo de erro. Ele pode ter sido herdado de outro processo e o erro silencioso pode ser indesejável em seu cenário.|  
  
## <a name="remarks"></a>Comentários  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MetaHost. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Hospedando enumerações](hosting-enumerations.md)
- [Método GetRequestedRuntime](iclrmetahostpolicy-getrequestedruntime-method.md)
