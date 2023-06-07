---
title: Hospedando enumerações
ms.date: 03/30/2017
helpviewer_keywords:
- unmanaged enumerations [.NET Framework], hosting
- enumerations [.NET Framework hosting]
- hosting enumerations [.NET Framework]
ms.assetid: e09131eb-1f7d-4f52-ae42-7393e9b62ef6
ms.openlocfilehash: 8edace3191ee4477b19f199d5db6c891c993dcd5
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504297"
---
# <a name="hosting-enumerations"></a>Hospedando enumerações
Esta seção descreve as enumerações não gerenciadas que a API de hospedagem usa.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Enumeração CLSID_RESOLUTION_FLAGS](clsid-resolution-flags-enumeration.md)  
 Contém valores que indicam como o Common Language Runtime (CLR) deve resolver um `CLSID` .  
  
 [Enumeração COR_GC_STAT_TYPES](cor-gc-stat-types-enumeration.md)  
 Especifica as estatísticas a serem registradas para uma coleta de lixo.  
  
 [Enumeração COR_GC_THREAD_STATS_TYPES](cor-gc-thread-stats-types-enumeration.md)  
 Indica as estatísticas de coleta de lixo para um thread.  
  
 [Enumeração EApiCategories](eapicategories-enumeration.md)  
 Descreve as categorias de recursos que o host pode bloquear de ser executado em código parcialmente confiável.  
  
 [Enumeração EBindPolicyLevels](ebindpolicylevels-enumeration.md)  
 Fornece sinalizadores que especificam o nível no qual aplicar ou modificar a política de assembly.  
  
 [Enumeração ECLRAssemblyIdentityFlags](eclrassemblyidentityflags-enumeration.md)  
 Indica o tipo de identidade de um assembly.  
  
 [Enumeração EClrEvent](eclrevent-enumeration.md)  
 Descreve os eventos CLR para os quais o host pode registrar retornos de chamada.  
  
 [Enumeração EClrFailure](eclrfailure-enumeration.md)  
 Descreve o conjunto de falhas para as quais um host pode definir ações de política.  
  
 [Enumeração EClrOperation](eclroperation-enumeration.md)  
 Descreve o conjunto de operações para as quais um host pode aplicar ações de política.  
  
 [Enumeração EClrUnhandledException](eclrunhandledexception-enumeration.md)  
 Descreve as opções disponíveis para gerenciar exceções que não são manipuladas no código do usuário.  
  
 [Enumeração EContextType](econtexttype-enumeration.md)  
 Descreve o contexto de segurança do thread em execução no momento.  
  
 [Enumeração ECustomDumpFlavor](ecustomdumpflavor-enumeration.md)  
 Contém valores que indicam quais itens incluir em um subconjunto personalizado de um despejo de heap ao relatar erros.  
  
 [Enumeração ECustomDumpItemKind](ecustomdumpitemkind-enumeration.md)  
 Reservado para a extensão futura da estrutura da [estrutura CustomDumpItem](customdumpitem-structure.md) .  
  
 [Enumeração EHostApplicationPolicy](ehostapplicationpolicy-enumeration.md)  
 Indica como modificar um objeto de interface de [interface IHostAssemblyManager](ihostassemblymanager-interface.md) . Esta enumeração foi preterida.  
  
 [Enumeração EHostBindingPolicyModifyFlags](ehostbindingpolicymodifyflags-enumeration.md)  
 Permite que o host especifique o tipo de redirecionamento que o CLR deve executar ao aplicar modificações de política de um assembly de origem a um assembly de destino.  
  
 [Enumeração EInitializeNewDomainFlags](einitializenewdomainflags-enumeration.md)  
 Permite que o host forneça o tempo de execução com informações sobre a inicialização de um domínio de aplicativo.  
  
 [Enumeração EMemoryAvailable](ememoryavailable-enumeration.md)  
 Contém valores que indicam a quantidade de memória física livre no computador.  
  
 [Enumeração EMemoryCriticalLevel](ememorycriticallevel-enumeration.md)  
 Contém valores que indicam o impacto de uma falha quando uma alocação de memória específica é solicitada, mas não pode ser satisfeita.  
  
 [Enumeração EPolicyAction](epolicyaction-enumeration.md)  
 Descreve as ações de política que o host pode definir para operações descritas pela [Enumeração EClrOperation](eclroperation-enumeration.md) e falhas descritas pela [Enumeração EClrFailure](eclrfailure-enumeration.md).  
  
 [Enumeração ESymbolReadingPolicy](esymbolreadingpolicy-enumeration.md)  
 Contém valores que definem a política para ler arquivos de banco de dados do programa (PDB).  
  
 [Enumeração ETaskType](etasktype-enumeration.md)  
 Contém valores que indicam o tipo de tarefa representado por uma [interface ICLRTask](iclrtask-interface.md) ou uma interface de [interface IHostTask](ihosttask-interface.md) .  
  
 [Enumeração HOST_TYPE](host-type-enumeration.md)  
 Contém valores que especificam o tipo de host que está iniciando um aplicativo.  
  
 [Enumeração MALLOC_TYPE](malloc-type-enumeration.md)  
 Contém valores que especificam as características da memória que está sendo alocada.  
  
 [Enumeração METAHOST_CONFIG_FLAGS](metahost-config-flags-enumeration.md)  
 Descreve os possíveis sinalizadores retornados no `pdwConfigFlags` parâmetro do método [ICLRMetaHostPolicy:: GetRequestedRuntime](iclrmetahostpolicy-getrequestedruntime-method.md) .  
  
 [Enumeração METAHOST_POLICY_FLAGS](metahost-policy-flags-enumeration.md)  
 Fornece políticas de associação que são comuns à maioria dos hosts de tempo de execução.  
  
 [Enumeração RUNTIME_INFO_FLAGS](runtime-info-flags-enumeration.md)  
 Contém valores que indicam quais informações sobre o CLR devem ser retornadas.  
  
 [Enumeração StackOverflowType](stackoverflowtype-enumeration.md)  
 Contém valores que indicam a causa subjacente de um evento de estouro de pilha.  
  
 [Enumeração STARTUP_FLAGS](startup-flags-enumeration.md)  
 Contém valores que indicam o comportamento de inicialização do CLR.  
  
 [Enumeração ValidatorFlags](validatorflags-enumeration.md)  
 Contém valores que indicam o tipo de validação que deve ser executado em uma chamada para o [método Validate](iclrvalidator-validate-method.md).  
  
 [Enumeração WAIT_OPTION](wait-option-enumeration.md)  
 Indica a ação que um host deve executar se uma operação solicitada pelos blocos CLR.  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Hospedando coclasses](hosting-coclasses.md)  
  
 [Interfaces de hospedagem](hosting-interfaces.md)  
  
 [Funções de hospedagem CLR reprovadas](deprecated-clr-hosting-functions.md)  
  
 [Estruturas de hospedagem](hosting-structures.md)
