---
title: Método ICLRProfiling::AttachProfiler
ms.date: 03/30/2017
api_name:
- IClrProfiling.AttachProfiler Method
api_location:
- Mscorwks.dll
api_type:
- COM
f1_keywords:
- IClrProfiling::AttachProfiler
helpviewer_keywords:
- AttachProfiler method [.NET Framework profiling]
- IClrProfiling::AttachProfiler method [.NET Framework profiling]
ms.assetid: 535a6839-c443-405b-a6f4-e2af90725d5b
topic_type:
- apiref
ms.openlocfilehash: 48ac09e1862ae58e79707235e891f72920de1251
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500553"
---
# <a name="iclrprofilingattachprofiler-method"></a>Método ICLRProfiling::AttachProfiler
Anexa o criador de perfil especificado ao processo especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT AttachProfiler(  
  [in] DWORD dwProfileeProcessID,  
  [in] DWORD dwMillisecondsMax,                     // optional  
  [in] const CLSID * pClsidProfiler,  
  [in] LPCWSTR wszProfilerPath,                     // optional  
  [in] size_is(cbClientData)] void * pvClientData,  // optional  
  [in] UINT cbClientData);                          // optional  
```  
  
## <a name="parameters"></a>Parâmetros

- `dwProfileeProcessID`

  \[no] a ID do processo do processo ao qual o criador de perfil deve ser anexado. Em um computador de 64 bits, o bit de execução do processo de perfil deve corresponder ao bit de execução do processo de gatilho que está chamando `AttachProfiler` . Se a conta de usuário sob a qual o `AttachProfiler` é chamado tiver privilégios administrativos, o processo de destino poderá ser qualquer processo no sistema. Caso contrário, o processo de destino deve ser de propriedade da mesma conta de usuário.

- `dwMillisecondsMax`

  \[em] o tempo de duração, em milissegundos, para `AttachProfiler` concluir. O processo de disparo deve passar um tempo limite que é conhecido como suficiente para que o criador de perfil específico conclua sua inicialização.
  
- `pClsidProfiler`

  \[in] um ponteiro para o CLSID do criador de perfil a ser carregado. O processo de disparo pode reutilizar essa memória após o `AttachProfiler` retorno.

- `wszProfilerPath`

  \[in] o caminho completo para o arquivo DLL do criador de perfil a ser carregado. Essa cadeia de caracteres deve conter no máximo 260 caracteres, incluindo o terminador nulo. Se `wszProfilerPath` for nulo ou uma cadeia de caracteres vazia, o Common Language Runtime (CLR) tentará localizar o local do arquivo DLL do criador de perfil procurando no registro para o CLSID que `pClsidProfiler` aponta para.

- `pvClientData`

  \[in] um ponteiro para os dados a serem passados para o criador de perfil pelo método [ICorProfilerCallback3:: InitializeForAttach](icorprofilercallback3-initializeforattach-method.md) . O processo de disparo pode reutilizar essa memória após o `AttachProfiler` retorno. Se `pvClientData` for NULL, `cbClientData` deverá ser 0 (zero).

- `cbClientData`

  \[em] o tamanho, em bytes, dos dados que `pvClientData` aponta para.

## <a name="return-value"></a>Valor Retornado  
 Esse método retorna os HRESULTs a seguir.  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|O criador de perfil especificado foi anexado com êxito ao processo de destino.|  
|CORPROF_E_PROFILER_ALREADY_ACTIVE|Já existe um criador de perfil ativo ou anexando ao processo de destino.|  
|CORPROF_E_PROFILER_NOT_ATTACHABLE|O criador de perfil especificado não oferece suporte a anexos. O processo de disparo pode tentar anexar um criador de perfil diferente.|  
|CORPROF_E_PROFILEE_INCOMPATIBLE_WITH_TRIGGER|Não é possível solicitar um anexo do criador de perfil porque a versão do processo de destino é incompatível com o processo atual que está chamando `AttachProfiler` .|  
|HRESULT_FROM_WIN32 (ERROR_ACCESS_DENIED)|O usuário do processo de gatilho não tem acesso ao processo de destino.|  
|HRESULT_FROM_WIN32 (ERROR_PRIVILEGE_NOT_HELD)|O usuário do processo de gatilho não tem os privilégios necessários para anexar um criador de perfil ao processo de destino fornecido. O log de eventos do aplicativo pode conter mais informações.|  
|CORPROF_E_IPC_FAILED|Ocorreu uma falha ao se comunicar com o processo de destino. Isso geralmente acontece se o processo de destino estava sendo desligado.|  
|HRESULT_FROM_WIN32 (ERROR_FILE_NOT_FOUND)|O processo de destino não existe ou não está executando um CLR que ofereça suporte a anexos. Isso pode indicar que o CLR foi descarregado desde a chamada para o método de enumeração de tempo de execução.|  
|HRESULT_FROM_WIN32 (ERROR_TIMEOUT)|O tempo limite expirou sem começar a carregar o criador de perfil. Você pode repetir a operação de anexação. Os tempos limite ocorrem quando um finalizador no processo de destino é executado por um tempo maior do que o valor de tempo limite.|  
|E_INVALIDARG|Um ou mais parâmetros têm valores inválidos.|  
|E_FAIL|Ocorreu alguma outra falha não especificada.|  
|Outros códigos de erro|Se o método [ICorProfilerCallback3:: InitializeForAttach](icorprofilercallback3-initializeforattach-method.md) do criador de perfil retornar um HRESULT que indica falha, `AttachProfiler` retornará esse mesmo HRESULT. Nesse caso, E_NOTIMPL é convertida em CORPROF_E_PROFILER_NOT_ATTACHABLE.|  
  
## <a name="remarks"></a>Comentários  
  
## <a name="memory-management"></a>Gerenciamento de memória  
 Ao manter as convenções com, o chamador de `AttachProfiler` (por exemplo, o código de gatilho criado pelo desenvolvedor do criador de perfil) é responsável por alocar e desalocar a memória para os dados aos quais o `pvClientData` parâmetro aponta. Quando o CLR executa a `AttachProfiler` chamada, ele faz uma cópia da memória que `pvClientData` aponta para e a transmite para o processo de destino. Quando o CLR dentro do processo de destino recebe sua própria cópia do `pvClientData` bloco, ele passa o bloco para o criador de perfil por meio do `InitializeForAttach` método e, em seguida, desaloca sua cópia do `pvClientData` bloco do processo de destino.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
- [Interface ICorProfilerInfo3](icorprofilerinfo3-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Criação de perfil](index.md)
