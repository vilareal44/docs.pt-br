---
title: Método ICorRuntimeHost::CreateEvidence
ms.date: 03/30/2017
api_name:
- ICorRuntimeHost.CreateEvidence
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICorRuntimeHost::CreateEvidence
helpviewer_keywords:
- CreateEvidence method [.NET Framework hosting]
- ICorRuntimeHost::CreateEvidence method [.NET Framework hosting]
ms.assetid: e235ea80-b84c-4442-a4c3-fc96c25a8eb9
topic_type:
- apiref
ms.openlocfilehash: 264f16fc9e767584229376e67f5aee6db1069025
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501606"
---
# <a name="icorruntimehostcreateevidence-method"></a>Método ICorRuntimeHost::CreateEvidence
Obtém um ponteiro de interface do tipo <xref:System.Security.Principal.IIdentity?displayProperty=nameWithType> , que permite que o host crie evidências de segurança para passar para o método [CreateDomain](icorruntimehost-createdomain-method.md) ou [CreateDomainEx](icorruntimehost-createdomainex-method.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT CreateEvidence (  
    [out] IUnknown** pEvidence  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pEvidence`  
 fora Um ponteiro de interface para uma <xref:System.Security.Principal.IIdentity?displayProperty=nameWithType> instância usada para criar evidências de segurança. Esse ponteiro é digitado `IUnknown` , de modo que os chamadores normalmente devem chamar `QueryInterface` essa interface para obter um ponteiro para um <xref:System.Security.Principal.IIdentity?displayProperty=nameWithType> .  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|A operação foi bem-sucedida.|  
|S_FALSE|Falha ao concluir a operação.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Se um método retornar E_FAIL, o Common Language Runtime (CLR) não poderá mais ser usado no processo. As chamadas subsequentes para qualquer API de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
|HOST_E_CLRNOTAVAILABLE|O CLR não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
  
## <a name="remarks"></a>Comentários  
 Esse método retorna uma coleção vazia que não pode ser populada a partir de código nativo. <xref:System.Security.Policy.Evidence>Em vez disso, você deve usar o método.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **Versão do .NET Framework:** 1,0, 1,1  
  
## <a name="see-also"></a>Confira também

- <xref:System._AppDomain>
- <xref:System.AppDomain>
- [Interface ICorRuntimeHost](icorruntimehost-interface.md)
