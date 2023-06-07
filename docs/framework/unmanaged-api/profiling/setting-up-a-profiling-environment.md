---
title: Configurando um ambiente de criação de perfil
ms.date: 03/30/2017
helpviewer_keywords:
- environment variables, profiling API
- profiling API [.NET Framework], setting up environment
- COR_PROFILER environment variable
- Windows Service applications, profiling
- profiling API [.NET Framework], Windows Service applications
- COR_ENABLE_PROFILING environment variable
- profiling API [.NET Framework], enabling
ms.assetid: fefca07f-7555-4e77-be86-3c542e928312
ms.openlocfilehash: adf790e0b2d2b72b5a1f0b2a41b80db6d5026869
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84494014"
---
# <a name="setting-up-a-profiling-environment"></a>Configurando um ambiente de criação de perfil
> [!NOTE]
> Houve alterações substanciais na criação de perfil no .NET Framework 4.  
  
 Quando um processo gerenciado (aplicativo ou serviço) é iniciado, ele carrega o Common Language Runtime (CLR). Quando o CLR é inicializado, ele avalia as duas variáveis ambientais a seguir para decidir se o processo deve se conectar a um criador de perfil:  
  
- COR_ENABLE_PROFILING: o CLR se conectará a um criador de perfil somente se essa variável de ambiente existir e estiver definida como 1.  
  
- COR_PROFILER: se a verificação de COR_ENABLE_PROFILING for aprovada, o CLR se conectará ao criador de perfil que tem esse CLSID ou ProgID, que deve ter sido armazenado anteriormente no registro. A variável de ambiente COR_PROFILER é definida como uma cadeia de caracteres, conforme mostrado nos dois exemplos a seguir.  
  
    ```cpp  
    set COR_PROFILER={32E2F4DA-1BEA-47ea-88F9-C5DAF691C94A}  
    set COR_PROFILER="MyProfiler"  
    ```  
  
 Para criar o perfil de um aplicativo CLR, você deve definir o COR_ENABLE_PROFILING e COR_PROFILER variáveis de ambiente antes de executar o aplicativo. Você também deve verificar se a DLL do criador de perfil está registrada.  
  
> [!NOTE]
> A partir do .NET Framework 4, os profileres não precisam ser registrados.  
  
> [!NOTE]
> Para usar .NET Framework versões 2,0, 3,0 e 3,5 de filers no .NET Framework 4 e versões posteriores, você deve definir a variável de ambiente COMPLUS_ProfAPI_ProfilerCompatibilitySetting.  
  
## <a name="environment-variable-scope"></a>Escopo da variável de ambiente  
 A maneira como você define as variáveis de ambiente COR_ENABLE_PROFILING e COR_PROFILER determinará seu escopo de influência. Você pode definir essas variáveis de uma das seguintes maneiras:  
  
- Se você definir as variáveis em uma chamada [ICorDebug:: CreateProcess](../debugging/icordebug-createprocess-method.md) , elas serão aplicadas somente ao aplicativo que você está executando no momento. (Eles também serão aplicados a outros aplicativos iniciados pelo aplicativo que herdam o ambiente.)  
  
- Se você definir as variáveis em uma janela de prompt de comando, elas serão aplicadas a todos os aplicativos iniciados nessa janela.  
  
- Se você definir as variáveis no nível de usuário, elas serão aplicadas a todos os aplicativos que você iniciar com o explorador de arquivos. Uma janela de prompt de comando que você abrir depois de definir as variáveis terá essas configurações de ambiente e, portanto, qualquer aplicativo que você iniciar nessa janela. Para definir variáveis de ambiente no nível do usuário, clique com o botão direito do mouse em **meu computador**, clique em **Propriedades**, clique na guia **avançado** , clique em **variáveis de ambiente**e adicione as variáveis à lista variáveis de **usuário** .  
  
- Se você definir as variáveis no nível do computador, elas serão aplicadas a todos os aplicativos iniciados nesse computador. Uma janela de prompt de comando aberta nesse computador terá essas configurações de ambiente e, portanto, qualquer aplicativo que você iniciar nessa janela. Isso significa que todos os processos gerenciados nesse computador serão iniciados com seu criador de perfil. Para definir variáveis de ambiente no nível do computador, clique com o botão direito do mouse em **meu computador**, clique em **Propriedades**, clique na guia **avançado** , clique em **variáveis de ambiente**, adicione as variáveis à lista variáveis do **sistema** e reinicie o computador. Após a reinicialização, as variáveis estarão disponíveis em todo o sistema.  
  
 Se você estiver criando o perfil de um serviço do Windows, deverá reiniciar o computador depois de definir as variáveis de ambiente e registrar a DLL do criador de perfil. Para obter mais informações sobre essas considerações, consulte a seção [criando o perfil de um serviço do Windows](#windows_service).  
  
## <a name="additional-considerations"></a>Considerações adicionais  
  
- A classe Profiler implementa as interfaces [ICorProfilerCallback](icorprofilercallback-interface.md) e [ICorProfilerCallback2](icorprofilercallback2-interface.md) . No .NET Framework versão 2,0, um criador de perfil deve implementar `ICorProfilerCallback2` . Caso contrário, não `ICorProfilerCallback2` será carregado.  
  
- Somente um criador de perfil pode criar um profile de um processo de uma vez em um determinado ambiente. Você pode registrar dois profileres diferentes em ambientes diferentes, mas cada um deve criar um perfil de processos separados. O criador de perfil deve ser implementado como uma DLL de servidor COM em processo, que é mapeada no mesmo espaço de endereço que o processo cujo perfil está sendo criado. Isso significa que o criador de perfil é executado em processo. O .NET Framework não oferece suporte a nenhum outro tipo de servidor COM. Por exemplo, se um criador de perfil quiser monitorar aplicativos de um computador remoto, ele deverá implementar agentes coletores em cada computador. Esses agentes resultarão em lote e os comunicarão com o computador de coleta de dados central.  
  
- Como o criador de perfil é um objeto COM que é instanciado em processo, cada aplicativo de criação de perfil terá sua própria cópia do criador de perfis. Portanto, uma única instância do criador de perfil não precisa lidar com dados de vários aplicativos. No entanto, você precisará adicionar lógica ao código de log do criador de perfil para evitar que o arquivo de log seja sobregravado de outros aplicativos com criação de perfil.  
  
## <a name="initializing-the-profiler"></a>Inicializando o criador de perfil  
 Quando as duas verificações de variável de ambiente são aprovadas, o CLR cria uma instância do criador de perfil de maneira semelhante à `CoCreateInstance` função com. O criador de perfil não é carregado por meio de uma chamada direta para `CoCreateInstance` . Portanto, uma chamada para `CoInitialize` , que requer a definição do modelo de Threading, é evitada. Em seguida, o CLR chama o método [ICorProfilerCallback:: Initialize](icorprofilercallback-initialize-method.md) no criador de perfil. A assinatura desse método é a seguinte.  
  
```cpp  
HRESULT Initialize(IUnknown *pICorProfilerInfoUnk)  
```  
  
 O criador de perfil deve consultar `pICorProfilerInfoUnk` um ponteiro de interface [ICorProfilerInfo](icorprofilerinfo-interface.md) ou [ICorProfilerInfo2](icorprofilerinfo2-interface.md) e salvá-lo para que possa solicitar mais informações posteriormente durante a criação de perfil.  
  
## <a name="setting-event-notifications"></a>Definindo notificações de eventos  
 Em seguida, o criador de perfil chama o método [ICorProfilerInfo:: SetEventMask](icorprofilerinfo-seteventmask-method.md) para especificar em quais categorias de notificações ele está interessado. Por exemplo, se o criador de perfil estiver interessado apenas em funções Enter e sair de notificações e notificações de coleta de lixo, ele especificará o seguinte.  
  
```cpp  
ICorProfilerInfo* pInfo;  
pICorProfilerInfoUnk->QueryInterface(IID_ICorProfilerInfo, (void**)&pInfo);  
pInfo->SetEventMask(COR_PRF_MONITOR_ENTERLEAVE | COR_PRF_MONITOR_GC)  
```  
  
 Ao definir a máscara de notificações dessa maneira, o criador de perfil pode limitar quais notificações ele recebe. Essa abordagem ajuda o usuário a criar um criador de perfil simples ou de finalidade especial. Ele também reduz o tempo de CPU que seria desperdiçado enviando notificações que o criador de perfil simplesmente ignoraria.  
  
 Determinados eventos do criador de perfil são imutáveis. Isso significa que assim que esses eventos são definidos no retorno de `ICorProfilerCallback::Initialize` chamada, eles não podem ser desativados e novos eventos não podem ser ativados. As tentativas de alterar um evento imutável resultarão no `ICorProfilerInfo::SetEventMask` retorno de um HRESULT com falha.  
  
<a name="windows_service"></a>
## <a name="profiling-a-windows-service"></a>Criação de perfil de um serviço do Windows  
 A criação de perfil de um serviço do Windows é como a criação de perfil de um aplicativo Common Language Runtime. Ambas as operações de criação de perfil são habilitadas por meio de variáveis de ambiente. Como um serviço do Windows é iniciado quando o sistema operacional é iniciado, as variáveis de ambiente discutidas anteriormente neste tópico já devem estar presentes e definidas para os valores necessários antes do início do sistema. Além disso, a DLL de criação de perfil já deve estar registrada no sistema.  
  
 Depois de definir as variáveis de ambiente COR_ENABLE_PROFILING e COR_PROFILER e registrar a DLL do criador de perfil, você deve reiniciar o computador de destino para que o serviço do Windows possa detectar essas alterações.  
  
 Observe que essas alterações habilitarão a criação de perfil em todo o sistema. Para impedir a execução de perfil de todos os aplicativos gerenciados que são executados posteriormente, você deve excluir as variáveis de ambiente do sistema depois de reiniciar o computador de destino.  
  
 Essa técnica também leva a todos os processos CLR que estão sendo Profiles. O criador de perfil deve adicionar lógica ao seu retorno de chamada [ICorProfilerCallback:: Initialize](icorprofilercallback-initialize-method.md) para detectar se o processo atual é de interesse. Se não for, o criador de perfil poderá falhar o retorno de chamada sem executar a inicialização.  
  
## <a name="see-also"></a>Confira também

- [Visão geral da criação de perfil](profiling-overview.md)
