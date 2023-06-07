---
title: Método ICLRMetaHostPolicy::GetRequestedRuntime
ms.date: 03/30/2017
api_name:
- ICLRMetaHostPolicy.GetRequestedRuntime
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRMetaHostPolicy::GetRequestedRuntime
helpviewer_keywords:
- GetRequestedRuntime method [.NET Framework hosting]
- ICLRMetaHostPolicy::GetRequestedRuntime method [.NET Framework hosting]
ms.assetid: 59ec1832-9cc1-4b5c-983d-03407e51de56
topic_type:
- apiref
ms.openlocfilehash: 37167b7a9aefa6cd9d5e4df043e8bbc1b0514261
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504115"
---
# <a name="iclrmetahostpolicygetrequestedruntime-method"></a>Método ICLRMetaHostPolicy::GetRequestedRuntime

Fornece uma interface para uma versão preferencial do Common Language Runtime (CLR) com base em uma política de hospedagem, assembly gerenciado, Cadeia de caracteres de versão e fluxo de configuração. Esse método, na verdade, não carrega nem ativa o CLR, mas simplesmente retorna a interface [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) que representa o resultado da política. Esse método substitui os métodos [GetRequestedRuntimeInfo](getrequestedruntimeinfo-function.md), [GetRequestedRuntimeVersion](getrequestedruntimeversion-function.md), [CorBindToRuntimeHost](corbindtoruntimehost-function.md), [CorBindToRuntimeByCfg](corbindtoruntimebycfg-function.md)e [GetCORRequiredVersion](getcorrequiredversion-function.md) .

## <a name="syntax"></a>Sintaxe

```cpp
HRESULT GetRequestedRuntime(
    [in]  METAHOST_POLICY_FLAGS dwPolicyFlags,
    [in]  LPCWSTR pwzBinary,
    [in]  IStream *pCfgStream,
    [in, out, size_is(*pcchVersion)] LPWSTR pwzVersion,
    [in, out]  DWORD *pcchVersion,
    [out, size_is(*pcchImageVersion)] LPWSTR pwzImageVersion,
    [in, out]  DWORD *pcchImageVersion,
    [out] DWORD *pdwConfigFlags,
    [in]  REFIID  riid
    [out, iid_is(riid), retval] LPVOID *ppRuntime);
```

## <a name="parameters"></a>Parâmetros

|Nome|Descrição|
|----------|-----------------|
|`dwPolicyFlags`|no Necessário. Especifica um membro da enumeração de [METAHOST_POLICY_FLAGS](metahost-policy-flags-enumeration.md) , que representa uma política de associação e qualquer número de modificadores. A única política disponível no momento é [METAHOST_POLICY_HIGHCOMPAT](metahost-policy-flags-enumeration.md).<br /><br /> Os modificadores incluem [METAHOST_POLICY_EMULATE_EXE_LAUNCH](metahost-policy-flags-enumeration.md), [METAHOST_POLICY_APPLY_UPGRADE_POLICY](metahost-policy-flags-enumeration.md), [METAHOST_POLICY_SHOW_ERROR_DIALOG](metahost-policy-flags-enumeration.md), [METAHOST_POLICY_USE_PROCESS_IMAGE_PATH](metahost-policy-flags-enumeration.md)e [METAHOST_POLICY_ENSURE_SKU_SUPPORTED](metahost-policy-flags-enumeration.md).|
|`pwzBinary`|[in] Opcional. Especifica o caminho do arquivo do assembly.|
|`pCfgStream`|[in] Opcional. Especifica o arquivo de configuração como um <xref:System.Runtime.InteropServices.ComTypes.IStream?displayProperty=nameWithType> .|
|`pwzVersion`|[entrada, saída] Adicional. Especifica ou retorna a versão de CLR preferida a ser carregada.|
|`pcchVersion`|[entrada, saída] Necessário. Especifica o tamanho esperado de `pwzVersion` como entrada, para evitar estouros de buffer. Se `pwzVersion` for NULL, `pcchVersion` conterá o tamanho esperado de `pwzVersion` quando `GetRequestedRuntime` retorna, para permitir pré-alocação; caso contrário, `pcchVersion` conterá o número de caracteres gravados `pwzVersion` .|
|`pwzImageVersion`|fora Adicional. Quando `GetRequestedRuntime` retorna, contém a versão do CLR correspondente à interface [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) que é retornada.|
|`pcchImageVersion`|[entrada, saída] Adicional. Especifica o tamanho de `pwzImageVersion` como entrada para evitar estouros de buffer. Se `pwzImageVersion` for NULL, `pcchImageVersion` conterá o tamanho necessário de `pwzImageVersion` quando `GetRequestedRuntime` retorna, para permitir a pré-autenticação.|
|`pdwConfigFlags`|fora Adicional. Se `GetRequestedRuntime` o usar um arquivo de configuração durante o processo de associação, quando ele retornar, `pdwConfigFlags` conterá um valor [METAHOST_CONFIG_FLAGS](metahost-config-flags-enumeration.md) que indica se o [\<startup>](../../configure-apps/file-schema/startup/startup-element.md) elemento tem o `useLegacyV2RuntimeActivationPolicy` atributo definido e o valor do atributo. Aplique a máscara de [METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_MASK](metahost-config-flags-enumeration.md) para `pdwConfigFlags` para obter os valores relevantes para `useLegacyV2RuntimeActivationPolicy` .|
|`riid`|no Especifica o identificador de interface IID_ICLRRuntimeInfo para a interface [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) solicitada.|
|`ppRuntime`|fora Quando `GetRequestedRuntime` retorna, contém um ponteiro para a interface [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) correspondente.|

## <a name="remarks"></a>Comentários

Quando esse método for bem sucedido, ele terá o efeito colateral de combinar sinalizadores adicionais com os sinalizadores de inicialização padrão atuais da interface de tempo de execução retornada, se e somente se um ou mais dos seguintes elementos existirem no fluxo de configuração dentro da `<configuration><runtime>` seção:

- `<gcServer enabled="true"/>`faz com que `STARTUP_SERVER_GC` seja definido.

- `<etwEnable enabled="true"/>`faz com que `STARTUP_ETW` seja definido.

- `<appDomainResourceMonitoring enabled="true"/>`faz com que `STARTUP_ARM` seja definido.

O valor padrão resultante `STARTUP_FLAGS` é a combinação de bits ou de valores definidos na lista anterior com os sinalizadores de inicialização padrão.

## <a name="return-value"></a>Valor Retornado

Esse método retorna os HRESULTs específicos a seguir, bem como os erros de HRESULT que indicam falha de método.

|HRESULT|Descrição|
|-------------|-----------------|
|S_OK|O método foi concluído com êxito.|
|E_POINTER|`pwzVersion`Não é nulo e `pcchVersion` é nulo.<br /><br /> -ou-<br /><br /> `pwzImageVersion`Não é nulo e `pcchImageVersion` é nulo.|
|E_INVALIDARG|`dwPolicyFlags`não especifica `METAHOST_POLICY_HIGHCOMPAT` .|
|ERROR_INSUFFICIENT_BUFFER|A memória alocada para `pwzVersion` é inadequada.<br /><br /> -ou-<br /><br /> A memória alocada para `pwzImageVersion` é inadequada.|
|CLR_E_SHIM_RUNTIMELOAD|`dwPolicyFlags`inclui METAHOST_POLICY_APPLY_UPGRADE_POLICY e ambos `pwzVersion` e `pcchVersion` são nulos.|

## <a name="requirements"></a>Requisitos

**Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).

**Cabeçalho:** MetaHost. h

**Biblioteca:** Incluído como um recurso em MSCorEE. dll

**.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]

## <a name="see-also"></a>Confira também

- [Interface ICLRMetaHostPolicy](iclrmetahostpolicy-interface.md)
- [Interfaces de hospedagem CLR adicionadas ao .NET Framework 4 e 4.5](clr-hosting-interfaces-added-in-the-net-framework-4-and-4-5.md)
- [Interfaces de hospedagem](hosting-interfaces.md)
- [Hosting](index.md)
