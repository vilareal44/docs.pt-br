---
title: Interface ICorRuntimeHost
ms.date: 03/30/2017
api_name:
- ICorRuntimeHost
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICorRuntimeHost
helpviewer_keywords:
- ICorRuntimeHost interface [.NET Framework hosting]
ms.assetid: 4369533d-7834-4497-bc37-bfea0ad737b1
topic_type:
- apiref
ms.openlocfilehash: 4b8018bb84dea08987d91f351b1ab0d9f3b48c56
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503894"
---
# <a name="icorruntimehost-interface"></a>Interface ICorRuntimeHost
Fornece métodos que permitem ao host iniciar e parar explicitamente o Common Language Runtime (CLR), criar e configurar domínios de aplicativo, acessar o domínio padrão e enumerar todos os domínios em execução no processo.  
  
 Na versão .NET Framework 2,0, essa interface é substituída por [ICLRRuntimeHost](iclrruntimehost-interface.md).  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método CloseEnum](icorruntimehost-closeenum-method.md)|Redefine um enumerador de domínio de volta para o início da lista de domínios.|  
|[Método CreateDomain](icorruntimehost-createdomain-method.md)|Cria um domínio de aplicativo. O chamador recebe um ponteiro de interface do tipo <xref:System._AppDomain> para uma instância do tipo <xref:System.AppDomain?displayProperty=nameWithType> .|  
|[Método CreateDomainEx](icorruntimehost-createdomainex-method.md)|Cria um domínio de aplicativo. Esse método permite que o chamador passe uma instância de IAppDomainSetup para configurar recursos adicionais da <xref:System._AppDomain> instância retornada.|  
|[Método CreateDomainSetup](icorruntimehost-createdomainsetup-method.md)|Obtém um ponteiro de interface do tipo `IAppDomainSetup` para uma <xref:System.AppDomainSetup> instância. `IAppDomainSetup`fornece métodos para configurar aspectos de um domínio de aplicativo antes de sua criação.|  
|[Método CreateEvidence](icorruntimehost-createevidence-method.md)|Obtém um ponteiro de interface do tipo <xref:System.Security.Principal.IIdentity> , que permite que o host crie evidências de segurança para passar para [CreateDomain](icorruntimehost-createdomain-method.md) ou [CreateDomainEx](icorruntimehost-createdomainex-method.md).|  
|[Método CreateLogicalThreadState](icorruntimehost-createlogicalthreadstate-method.md)|Não use.|  
|[Método CurrentDomain](icorruntimehost-currentdomain-method.md)|Obtém um ponteiro de interface do tipo <xref:System._AppDomain> que representa o domínio carregado no thread atual.|  
|[Método DeleteLogicalThreadState](icorruntimehost-deletelogicalthreadstate-method.md)|Não use.|  
|[Método EnumDomains](icorruntimehost-enumdomains-method.md)|Obtém um enumerador para os domínios no processo atual.|  
|[Método GetConfiguration](icorruntimehost-getconfiguration-method.md)|Obtém um objeto que permite ao host especificar a configuração de retorno de chamada do CLR.|  
|[Método GetDefaultDomain](icorruntimehost-getdefaultdomain-method.md)|Obtém um ponteiro de interface do tipo <xref:System._AppDomain> que representa o domínio padrão para o processo atual.|  
|[Método LocksHeldByLogicalThread](icorruntimehost-locksheldbylogicalthread-method.md)|Não use.|  
|[Método MapFile](icorruntimehost-mapfile-method.md)|Mapeia o arquivo especificado na memória. Esse método é obsoleto.|  
|[Método NextDomain](icorruntimehost-nextdomain-method.md)|Obtém um ponteiro de interface para o próximo domínio na enumeração.|  
|[Método de início](icorruntimehost-start-method.md)|Inicia o CLR.|  
|[Método Stop](icorruntimehost-stop-method.md)|Interrompe a execução do código no tempo de execução para o processo atual.|  
|[Método SwitchInLogicalThreadState](icorruntimehost-switchinlogicalthreadstate-method.md)|Não use.|  
|[Método SwitchOutLogicalThreadState](icorruntimehost-switchoutlogicalthreadstate-method.md)|Não use.|  
|[Método UnloadDomain](icorruntimehost-unloaddomain-method.md)|Descarrega o domínio de aplicativo especificado do processo atual.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **Versões do .NET Framework:** 1,0, 1,1  
  
## <a name="see-also"></a>Confira também

- <xref:System.AppDomain>
- [Hosting](index.md)
- [Interface ICLRRuntimeHost](iclrruntimehost-interface.md)
- [Hosts de runtime](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/a51xd4ze(v=vs.100))
- [Interfaces de hospedagem](hosting-interfaces.md)
- [Coclass CorRuntimeHost](corruntimehost-coclass.md)
