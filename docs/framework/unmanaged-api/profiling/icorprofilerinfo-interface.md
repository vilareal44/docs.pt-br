---
title: Interface ICorProfilerInfo
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo
helpviewer_keywords:
- ICorProfilerInfo interface [.NET Framework profiling]
ms.assetid: eb4e4ce0-06e7-4469-bbc4-edc2eb5da4b1
topic_type:
- apiref
ms.openlocfilehash: cc8ab6f0c8115da4d74280023dc692b66846ed94
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84497745"
---
# <a name="icorprofilerinfo-interface"></a>Interface ICorProfilerInfo
Fornece métodos para uso por infilers de código para se comunicar com o Common Language Runtime (CLR) para controlar o monitoramento de eventos e informações de solicitação.  
  
> [!NOTE]
> Cada método na `ICorProfilerInfo` interface retorna um HRESULT para indicar êxito ou falha. Consulte CorError. h para obter uma lista de possíveis códigos de retorno.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método BeginInprocDebugging](icorprofilerinfo-begininprocdebugging-method.md)|Inicializa o suporte à depuração em processo. Esse método é obsoleto no .NET Framework versão 2,0.|  
|[Método EndInprocDebugging](icorprofilerinfo-endinprocdebugging-method.md)|Desliga uma sessão de depuração em processo. Esse método é obsoleto no .NET Framework versão 2,0.|  
|[Método ForceGC](icorprofilerinfo-forcegc-method.md)|Força a coleta de lixo a ocorrer dentro do tempo de execução.|  
|[Método GetAppDomainInfo](icorprofilerinfo-getappdomaininfo-method.md)|Obtém informações sobre o domínio de aplicativo especificado.|  
|[Método GetAssemblyInfo](icorprofilerinfo-getassemblyinfo-method.md)|Obtém informações sobre o assembly especificado.|  
|[Método GetClassFromObject](icorprofilerinfo-getclassfromobject-method.md)|Obtém o `ClassID` de um<br /><br /> objeto, dado seu `ObjectID` .|  
|[Método GetClassFromToken](icorprofilerinfo-getclassfromtoken-method.md)|Obtém a ID da classe, dado o token de metadados. Esse método é obsoleto no .NET Framework versão 2,0. Em vez disso, use o método [ICorProfilerInfo2:: GetClassFromTokenAndTypeArgs](icorprofilerinfo2-getclassfromtokenandtypeargs-method.md) .|  
|[Método GetClassIDInfo](icorprofilerinfo-getclassidinfo-method.md)|Obtém o módulo pai e o token de metadados para a classe especificada.|  
|[Método GetCodeInfo](icorprofilerinfo-getcodeinfo-method.md)|Obtém a extensão do código nativo associado à ID da função especificada. Esse método é obsoleto. Em vez disso, use o método [ICorProfilerInfo2:: GetCodeInfo2](icorprofilerinfo2-getcodeinfo2-method.md) .|  
|[Método GetCurrentThreadID](icorprofilerinfo-getcurrentthreadid-method.md)|Obtém a ID do thread atual, se for um thread gerenciado.|  
|[Método GetEventMask](icorprofilerinfo-geteventmask-method.md)|Obtém as categorias de evento atuais para as quais o criador de perfil deseja receber notificações de eventos do CLR.|  
|[Método GetFunctionFromIP](icorprofilerinfo-getfunctionfromip-method.md)|Mapeia um ponteiro de instrução de código gerenciado para um `FunctionID` .|  
|[Método GetFunctionFromToken](icorprofilerinfo-getfunctionfromtoken-method.md)|Obtém a ID de uma função. Esse método é obsoleto no .NET Framework versão 2,0. Em vez disso, use o método [ICorProfilerInfo2:: GetFunctionFromTokenAndTypeArgs](icorprofilerinfo2-getfunctionfromtokenandtypeargs-method.md) .|  
|[Método GetFunctionInfo](icorprofilerinfo-getfunctioninfo-method.md)|Obtém a classe pai e o token de metadados para a função especificada.|  
|[Método GetHandleFromThread](icorprofilerinfo-gethandlefromthread-method.md)|Mapeia a ID de um thread para um identificador de thread do Win32.|  
|[Método GetILFunctionBody](icorprofilerinfo-getilfunctionbody-method.md)|Obtém um ponteiro para o corpo de um método no código da MSIL (Microsoft Intermediate Language), começando em seu cabeçalho.|  
|[Método GetILFunctionBodyAllocator](icorprofilerinfo-getilfunctionbodyallocator-method.md)|Obtém uma interface que fornece um método para alocar memória a ser usada para alternar o corpo de um método no código MSIL.|  
|[Método GetILToNativeMapping](icorprofilerinfo-getiltonativemapping-method.md)|Obtém um mapa de deslocamentos MSIL para deslocamentos nativos para o código contido na função especificada.|  
|[Método GetInprocInspectionInterface](icorprofilerinfo-getinprocinspectioninterface-method.md)|Obtém um objeto que pode ser consultado para uma interface ICorDebugProcess. Esse método é obsoleto no .NET Framework versão 2,0.|  
|[Método GetInprocInspectionIThisThread](icorprofilerinfo-getinprocinspectionithisthread-method.md)|Obtém um objeto que pode ser consultado para a interface ICorDebugThread. Esse método é obsoleto no .NET Framework versão 2,0.|  
|[Método GetModuleInfo](icorprofilerinfo-getmoduleinfo-method.md)|Dada uma ID de módulo, retorna o nome do arquivo do módulo e a ID do assembly pai do módulo.|  
|[Método GetModuleMetaData](icorprofilerinfo-getmodulemetadata-method.md)|Obtém uma instância de interface de metadados que mapeia para o módulo especificado.|  
|[Método GetObjectSize](icorprofilerinfo-getobjectsize-method.md)|Obtém o tamanho de um objeto especificado.|  
|[Método GetThreadContext](icorprofilerinfo-getthreadcontext-method.md)|Obtém a identidade de contexto atualmente associada ao thread especificado.|  
|[Método GetThreadInfo](icorprofilerinfo-getthreadinfo-method.md)|Obtém a identidade do Thread Win32 atual para o thread especificado.|  
|[Método GetTokenAndMetadataFromFunction](icorprofilerinfo-gettokenandmetadatafromfunction-method.md)|Obtém o token de metadados e uma instância da interface de metadados que pode ser usada em relação ao token para a função especificada.|  
|[Método IsArrayClass](icorprofilerinfo-isarrayclass-method.md)|Determina se a classe especificada é uma classe de matriz.|  
|[Método SetEnterLeaveFunctionHooks](icorprofilerinfo-setenterleavefunctionhooks-method.md)|Especifica funções implementadas pelo Profiler a serem chamadas em "Inserir", "deixar" e "tailcall" ganchos de funções gerenciadas.|  
|[Método SetEventMask](icorprofilerinfo-seteventmask-method.md)|Define um valor que especifica os tipos de eventos para os quais o criador de perfil deseja receber a notificação do CLR.|  
|[Método SetFunctionIDMapper](icorprofilerinfo-setfunctionidmapper-method.md)|Especifica a função implementada pelo criador de perfil que será chamada para mapear `FunctionID` valores para valores alternativos, que são passados para os ganchos de entrada/saída da função do criador de perfil.|  
|[Método SetFunctionReJIT](icorprofilerinfo-setfunctionrejit-method.md)|Não implementado. Não use.|  
|[Método SetILFunctionBody](icorprofilerinfo-setilfunctionbody-method.md)|Substitui o corpo da função especificada no módulo especificado.|  
|[Método SetILInstrumentedCodeMap](icorprofilerinfo-setilinstrumentedcodemap-method.md)|Especifica como os deslocamentos de um MSIL original da função especificada são mapeados para os novos deslocamentos do MSIL do criador de perfil da função.|  
  
## <a name="remarks"></a>Comentários  
 Um criador de perfil chama um método na `ICorProfilerInfo` interface para se comunicar com o CLR para controlar as informações de solicitação e monitoramento de eventos.  
  
 Os métodos da `ICorProfilerInfo` interface são implementados pelo CLR usando o modelo de thread livre. Cada método retorna um HRESULT para indicar êxito ou falha. Consulte CorError. h para obter uma lista de possíveis códigos de retorno.  
  
 O CLR passa, por meio da implementação do criador de perfil de [ICorProfilerCallback:: Initialize](icorprofilercallback-initialize-method.md), uma `ICorProfilerInfo` interface para cada criador de perfil de código durante a inicialização. Um criador de perfil de código pode então chamar métodos da `ICorProfilerInfo` interface para obter informações sobre o código gerenciado que está sendo executado sob o controle do CLR.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Interface ICorProfilerInfo2](icorprofilerinfo2-interface.md)
