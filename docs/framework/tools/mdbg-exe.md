---
title: MDbg.exe (Depurador de Linha de Comando do .NET Framework)
description: Entenda MDbg.exe, o depurador de linha de comando para .NET, que ajuda os fornecedores de ferramentas e os desenvolvedores de aplicativos a localizar e corrigir bugs em programas direcionados ao CLR.
ms.date: 03/30/2017
helpviewer_keywords:
- command-line debugger [.NET Framework]
- MDbg.exe
ms.assetid: 28a3f509-07e2-4dbe-81df-874c5e969cc4
ms.openlocfilehash: 1c663474e5084afa1824f0f6b0740ae03a344e92
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904215"
---
# <a name="mdbgexe-net-framework-command-line-debugger"></a>MDbg.exe (Depurador de Linha de Comando do .NET Framework)
O Depurador de Linha de Comando do .NET Framework ajuda fornecedores de ferramentas e desenvolvedores de aplicativos na localização e na correção de bugs em programas com o Common Language Runtime do .NET Framework como destino. Essa ferramenta usa a API de depuração do runtime para fornecer serviços de depuração. É possível usar MDbg.exe para depurar apenas o código gerenciado; não há suporte para depurar o código não gerenciado.  
  
Essa ferramenta está disponível por meio do NuGet. Para obter informações sobre a instalação, consulte [MDbg 0.1.0](https://www.nuget.org/packages/MDbg/0.1.0). Para executar a ferramenta, use o Console do Gerenciador de Pacotes. Para saber mais sobre como usar o Console do Gerenciador de Pacotes, confira o artigo [Console do Gerenciador de Pacotes](/nuget/tools/package-manager-console).
  
No prompt de comando do Gerenciador de Pacotes, digite o seguinte:  
  
## <a name="syntax"></a>Syntax  
  
```console  
MDbg [ProgramName[arguments]] [options]  
```  
  
## <a name="commands"></a>Comandos  
 Quando você estiver no depurador (como indicado pelo prompt **mdbg>**), digite um dos comandos descritos na próxima seção:  
  
 **comando** [*arguments*]  
  
 Os comandos de MDbg.exe diferenciam maiúsculas de minúsculas.  
  
|Comando|Descrição|  
|-------------|-----------------|  
|**ap**[**rocess**] [*number*]|Alterna para outro processo depurado ou imprime processos disponíveis. Os números não são PIDs (IDs de processo) reais, e sim uma lista indexada em 0.|  
|**a**[**ttach**] [*pid*]|Anexa a um processo ou imprime processos disponíveis.|  
|**b**[**reak**] [*ClassName.Method* &#124; *FileName:LineNo*]|Define um ponto de interrupção no método especificado. Os módulos são verificados sequencialmente.<br /><br /> -   **break** *FileName:LineNo* define um ponto de interrupção em um local na origem.<br />-   **break** *~number* define um ponto de interrupção em um símbolo exibido recentemente com o comando **x**.<br />-   **break** *module!ClassName.Method+IlOffset* define um ponto de interrupção no local totalmente qualificado.|  
|**block**[**ingObjects**]|Exibe bloqueios de monitor, que estão bloqueando threads.|  
|**ca**[**tch**] [*exceptionType*]|Faz o depurador parar em todas as exceções, e não apenas em exceções sem tratamento.|  
|**cl**[**earException**]|Marca a exceção atual como tratada de forma que a execução possa continuar. Se a causa da exceção não tiver sido tratada, a exceção poderá ser rapidamente emitida novamente.|  
|**conf**[**ig**] [*option value*]|Exibe todas as opções configuráveis e mostra como as opções são invocadas sem valores opcionais. Se a opção for especificada, define `value` como a opção atual. As seguintes opções estão disponíveis:<br /><br /> -   `extpath` define o caminho para pesquisar extensões quando o comando `load` é usado.<br />-   `extpath+` adiciona um caminho para carregar extensões.|  
|**del**[**ETE**]|Exclui um ponto de interrupção.|  
|**de**[**xar**]|É desanexado de um processo depurado.|  
|**d**[**own**] [*frames*]|Move o registro de ativação ativo para baixo.|  
|**echo**|Repete uma mensagem para o console.|  
|**enableNotif**[**ication**] *typeName* 0&#124;1|Habilita (1) ou desabilita (0) notificações personalizadas para o tipo especificado.|  
|**ex**[**it**] [*exitcode*]|Sai do shell MDbg.exe e especifica como opção o código de saída do processo.|  
|**fo**[**reach**] [*OtherCommand*]|Executa um comando em todos os threads. *OtherCommand* é um comando válido que opera no thread, **foreach** *OtherCommand* executa o mesmo comando em todos os threads.|  
|**f**[**unceval**] [ `-ad` *num*] *FunctionName* [*args...* ]|Executa uma avaliação da função no thread ativo atual em que a função a ser avaliada é *functionName*. O nome da função deve ser totalmente qualificado, incluindo namespaces.<br /><br /> A opção `-ad` especifica o domínio do aplicativo a ser usado para resolver a função. Se a opção `-ad` não for especificada, o domínio do aplicativo para resolução assumirá como padrão o domínio do aplicativo em que o thread usado na avaliação da função está localizado.<br /><br /> Se a função que está sendo avaliada não for estática, o primeiro parâmetro passado deverá ser um ponteiro `this`. Todos os domínios de aplicativo são pesquisados em busca de argumentos para a avaliação da função.<br /><br /> Para solicitar um valor de um domínio do aplicativo, anteceda a variável com o módulo e o nome de domínio do módulo; por exemplo, `funceval -ad 0 System.Object.ToString hello.exe#0!MyClass.g_rootRef`. Esse comando avalia a função `System.Object.ToString` no domínio do aplicativo `0`. Como o método `ToString` é uma função de instância, o primeiro parâmetro deve ser um ponteiro `this`.|  
|**g**[**o**]|Faz o programa continuar até encontrar um ponto de interrupção, o programa sai ou um evento (por exemplo, uma exceção sem tratamento) faz o programa parar.|  
|**h**[**elp**] [*comando*]<br /><br /> -ou-<br /><br /> **?** [*comando*]|Exibe uma descrição de todos os comandos ou uma descrição detalhada de um comando especificado.|  
|**ig**[**nore**] [*event*]|Faz o depurador parar somente em exceções sem tratamento.|  
|**int**[**ercept**] *FrameNumber*|Reverte o depurador para um número de quadro especificado.<br /><br /> Se o depurador encontrar uma exceção, use esse comando para reverter o depurador para o número de quadro especificado. É possível alterar o estado do programa usando o comando **set** e continuar usando o comando **go**.|  
|**k**[**mal**)|Para o processo ativo.|  
|**l**[**ist**] [*modules* &#124; *appdomains* &#124; *assemblies*]|Exibe os módulos carregados, os domínios de aplicativo ou os assemblies.|  
|**lo**[**ad**] *assemblyName*|Carrega uma extensão da seguinte maneira: o assembly especificado é carregado e uma tentativa é feita para, em seguida, executar o método estático `LoadExtension` com base no tipo `Microsoft.Tools.Mdbg.Extension.Extension`.|  
|**log** [*eventType*]|Defina ou exiba os eventos que serão registrados em log.|  
|**mo**[**de**] [*option on/off*]|Define opções de depurador diferentes. Use `mode` sem opções para obter uma lista dos modos de depuração e suas configurações atuais.|  
|**mon**[**itorInfo**] *monitorReference*|Exibe informações de bloqueio do monitor do objeto.|  
|**newo**[**bj**] *typeName* [*arguments...*]|Cria um novo objeto do tipo *typeName*.|  
|**n**[**ext**]|Executa o código e avança para a próxima linha (mesmo que a próxima linha inclua muitas chamadas de função).|  
|**Opendump** *pathToDumpFile*|Abre o arquivo de despejo especificado para depuração.|  
|**o**[**UT**]|Move para o final da função atual.|  
|**pa**[**th**] [*pathName*]|Procura os arquivos de origem no caminho especificado se o local nos binários não estiver disponível.|  
|**p**[**rint**] [*var*] &#124; [`-d`]|Imprime todas as variáveis no escopo (**print**), imprime a variável especificada (**print** *var*) ou imprime as variáveis do depurador (**print**`-d`).|  
|**printe**[**xception**] [*-r*]|Imprime a última exceção no thread atual. Use a opção `–r` (recursiva) para percorrer a propriedade `InnerException` no objeto de exceção e obter informações sobre a cadeia inteira de exceções.|  
|**pro**[**cessenum**]|Exibe os processos ativos.|  
|**q**[**uit**] [*exitcode*]|Sai do shell MDbg.exe, especificando como opção o código de saída do processo.|  
|**re**[**sume**] [`*` &#124; [`~`]*threadNumber*]|Retoma o thread atual ou o thread especificado pelo parâmetro *threadNumber*.<br /><br /> Se o parâmetro *threadNumber* for especificado como `*` ou se o número de threads começar com `~`, o comando se aplicará a todos os threads, exceto o especificado por *threadNumber*.<br /><br /> A retomada de um thread não suspenso não tem nenhum efeito.|  
|**r**[**un**] [ `-d` ( `ebug` ) &#124;- `o` () `ptimize` &#124;`-enc` ] [[*path_to_exe*] [*args_to_exe*]]|Para o processo atual (se houver algum) e inicia um novo. Se nenhum argumento executável for passado, esse comando executará o programa que foi executado anteriormente com o comando `run`. Se o argumento executável for fornecido, o programa especificado será executado usando os argumentos fornecidos como opção.<br /><br /> Se os eventos de carga da classe, de carga do módulo e de início do thread forem ignorados (como são por padrão), o programa parará na primeira instrução executável do thread principal.<br /><br /> É possível forçar o depurador a compilar o código com JIT (just-in-time) usando um destes três sinalizadores:<br /><br /> -   `-d`*(* `ebug` *)* desabilita otimizações. Este é o padrão para MDbg.exe.<br />-   `-o`*(* `ptimize` *)* força o código a ser executado mais como faz fora do depurador, mas também torna a experiência de depuração mais difícil. Esse é o padrão de uso fora do depurador.<br />-   `-enc` habilita o recurso Editar e Continuar, mas incorre em um impacto no desempenho.|  
|**Set** *variable*=*value*|Altera o valor de qualquer variável no escopo.<br /><br /> Também é possível criar variáveis próprias do depurador e atribuir valores de referência a elas dentro do aplicativo. Esses valores funcionam como identificadores para o valor original, e mesmo o valor original está fora do escopo. Todas as variáveis do depurador devem começar com `$` (por exemplo, `$var`). Desmarque estes identificadores definindo-os como nada usando o seguinte comando:<br /><br /> `set $var=`|  
|Número de **SetIP** [ `-il` ] *number*|Define o IP (ponteiro da instrução) atual no arquivo como a posição especificada. Se você especificar a opção `-il`, o número representará um deslocamento MSIL (Microsoft Intermediate Language) no método. Do contrário, o número representa um número de linha de origem.|  
|**sh**[**ow**] [*lines*]|Especifica o número de linhas que serão mostradas.|  
|**s**[**tapa**]|Move a execução para a próxima função na linha atual ou move para a próxima linha se não houver função a ser realizada.|  
|**su**[**spend**] [\* &#124; [~]*threadNumber*]|Suspende o thread atual ou o thread especificado pelo parâmetro *threadNumber*.  Se *threadNumber* for especificado como `*`, o comando se aplicará a todos os threads. Se o número do thread começar com `~`, o comando se aplicará a todos os threads, exceto o especificado por *threadNumber*. Os threads suspensos são excluídos da execução quando o processo é executado pelo comando **go** ou **step**. Se não houver threads não suspensos no processo e você emitir o comando **go**, o processo não continuará. Nesse caso, pressione CTRL-C para interromper o processo.|  
|**sy**[**mbol**] *commandName* [*commandValue*]|Especifica um dos comandos a seguir:<br /><br /> -   `symbol path` [`"value"`] – Exibe ou define o caminho de símbolo atual.<br />-   `symbol addpath``"value"`-Adiciona ao seu caminho de símbolo atual.<br />-   `symbol reload` [`"module"`] – Recarrega todos os símbolos ou os símbolos do módulo especificado.<br />-   `symbol list` [`module`] – Mostra os símbolos carregados atualmente para todos os módulos ou o módulo especificado.|  
|**t**[**hread**] [*newThread*] [-*apelido de Nick*`]`|O comando de thread sem parâmetros exibe todos os threads gerenciados no processo atual. Os threads costumam ser identificados pelos números de thread; se o thread tiver um apelido atribuído, o apelido será exibido no lugar. É possível usar o parâmetro `-nick` para atribuir um apelido a um thread.<br /><br /> -   **thread** `-nick` *threadName* atribui um apelido ao thread em execução no momento.<br /><br /> Os apelidos não podem ser números. Se o thread atual já tiver um apelido atribuído, o apelido anterior será substituído pelo novo. Se o novo apelido for uma cadeia de caracteres vazia (""), o apelido do thread atual será excluído e nenhum apelido novo será atribuído ao thread.|  
|**u**[**p**]|Move o registro de ativação ativo para cima.|  
|**uwgc**[**handle**] [*var*] &#124; [*address*]|Imprime a variável acompanhada por um identificador. O identificador pode ser especificado por nome ou endereço.|  
|**ao**|Exibe as instruções `when` ativas no momento.<br /><br /> **quando** **excluir todos os** &#124; `num` [ `num` [ `num` ...]]-exclui a `when` instrução especificada pelo número, ou todas as `when` instruções, se `all` for especificado.<br /><br /> **quando** `stopReason` [ `specific_condition` ] **fazer** `cmd` [ `cmd` [ `cmd` ...]]-O parâmetro *stopReason* pode ser um dos seguintes:<br /><br /> `StepComplete`, `ProcessExited`, `ThreadCreated`, `BreakpointHit`, `ModuleLoaded`, `ClassLoaded`, `AssemblyLoaded`, `AssemblyUnloaded`, `ControlCTrapped`, `ExceptionThrown`, `UnhandledExceptionThrown`, `AsyncStop`, `AttachComplete`, `UserBreak`, `EvalComplete`, `EvalException`, `RemapOpportunityReached`, `NativeStop`.<br /><br /> *specific_condition* pode ser um dos seguintes:<br /><br /> -   *number* – Para `ThreadCreated` e `BreakpointHit`, dispara a ação somente quando parado por uma ID de thread/número do ponto de interrupção com o mesmo valor.<br />-[ `!` ]*Name* -for `ModuleLoaded` , `ClassLoaded` , `AssemblyLoaded` , `AssemblyUnloaded` , `ExceptionThrown` e e `UnhandledExceptionThrown` disparará a ação somente quando o nome corresponder ao nome do *stopReason*.<br /><br /> *specific_condition* deve estar vazio para outros valores de *stopReason*.|  
|**w**[**aqui**] [ `-v` ] [ `-c` *profundidade*] [*ThreadID*]|Exibe informações de depuração sobre quadros de pilha.<br /><br /> – A opção `-v` fornece informações detalhadas sobre cada registro de ativação exibido.<br />– A especificação de um número para `depth` limita o número de quadros exibidos. Use o comando **all** para exibir todos os quadros. O padrão é 100.<br />– Se especificar o parâmetro *threadID*, você poderá controlar qual thread está associado à pilha. O padrão é apenas o thread atual. Use o comando **all** para obter todos os threads.|  
|**x** [ `-c` *numSymbols*] [*módulo*[ `!` *padrão*]]|Exibe funções correspondentes ao `pattern` para um módulo.<br /><br /> Se *numSymbols* for especificado, a saída será limitada ao número especificado. Se `!` (indicando uma expressão regular) não for especificado para *pattern*, todas as funções serão exibidas. Se *module* não for fornecido, todos os módulos carregados serão exibidos. Os símbolos ( *~#* ) podem ser usados para definir pontos de interrupção usando o comando **Break** .|  
  
## <a name="remarks"></a>Comentários  
 Compile o aplicativo a ser depurado usando sinalizadores específicos do compilador que fazem o compilador gerenciar símbolos de depuração. Consulte a documentação do compilador para obter mais informações sobre esses sinalizadores. É possível depurar aplicativos otimizados, mas algumas informações de depuração estarão ausentes. Por exemplo, muitas variáveis locais não serão visíveis e as linhas de origem serão imprecisas.  
  
 Depois de compilar o aplicativo, digite **mdbg** no prompt de comando para iniciar uma sessão de depuração, como mostrado no exemplo a seguir.  
  
```console  
C:\Program Files\Microsoft Visual Studio 8\VC>mdbg  
MDbg (Managed debugger) v2.0.50727.42 (RTM.050727-4200) started.  
Copyright (C) Microsoft Corporation. All rights reserved.  
  
For information about commands type "help";  
to exit program type "quit".  
mdbg>  
```  
  
 O prompt `mdbg>` indica que você está no depurador.  
  
 Depois que você estiver no depurador, use os comandos e os argumentos descritos na seção anterior.  
  
## <a name="see-also"></a>Veja também

- [Ferramentas](index.md)
- [Prompts de comando](developer-command-prompt-for-vs.md)
