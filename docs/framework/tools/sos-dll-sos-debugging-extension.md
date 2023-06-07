---
title: SOS.dll (Extensão de Depuração SOS)
description: Use SOS.dll, a extensão de depuração SOS. Depurar programas gerenciados no Visual Studio e no WinDbg.exe obtendo informações sobre o ambiente CLR interno.
ms.date: 03/30/2017
helpviewer_keywords:
- debugging extensions
- SOS debugging extensions
- SOS.dll
ms.assetid: 9ac1b522-77ab-4cdc-852a-20fcdc9ae498
ms.openlocfilehash: cc9fed8432b5b24c20c3c470a842895a901d9efb
ms.sourcegitcommit: b4f8849c47c1a7145eb26ce68bc9f9976e0dbec3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87517172"
---
# <a name="sosdll-sos-debugging-extension"></a>SOS.dll (extensão de depuração SOS)

A Extensão de Depuração SOS (SOS.dll) ajuda a depurar programas gerenciados no Visual Studio e no depurador do Windows (WinDbg.exe) fornecendo informações sobre o ambiente interno do CLR (Common Language Runtime). Essa ferramenta exige que o projeto tenha depuração não gerenciada habilitada. SOS.dll é instalado automaticamente com o .NET Framework. Para usar SOS.dll no Visual Studio, instale o [WDK (Kit de Driver do Windows)](/windows-hardware/drivers/download-the-wdk).

## <a name="syntax"></a>Sintaxe

```console
![command] [options]
```

## <a name="commands"></a>Comandos

|Comando|Descrição|
|-------------|-----------------|
|**AnalyzeOOM** (**ao**)|Exibe as informações do último erro de OOM (memória insuficiente) ocorrido em uma solicitação de alocação para o heap da coleta de lixo. (Na coleta de lixo do servidor, ele exibirá OOM, se houver, em cada heap da coleta de lixo.)|
|**BPMD** [**-nofuturemodule**] [ \<*module name*> \<*method name*> ] [**-MD**  < `MethodDesc`>] **-list** **-Clear** \<*pending breakpoint number*> **-ClearAll**|Cria um ponto de interrupção no método especificado do módulo especificado.<br /><br /> Se o módulo e o método especificados não forem carregados, esse comando aguardará uma notificação de que o módulo foi carregado e compilado JIT (just-in-time) antes de criar um ponto de interrupção.<br /><br /> É possível gerenciar a lista dos pontos de interrupção pendentes usando as opções **-list**, **-clear** e **-clearall**:<br /><br /> A opção **-list** gerencia uma lista de todos os pontos de interrupção pendentes. Se um ponto de interrupção pendente tiver uma ID de módulo diferente de zero, esse ponto de interrupção será específico de uma função nesse módulo carregado em especial. Se o ponto de interrupção pendente tiver uma ID de módulo zero, esse ponto de interrupção se aplicará aos módulos que ainda não foram carregados.<br /><br /> Use a opção **-clear** ou **-clearall** para remover os pontos de interrupção pendentes da lista.|
|**CLRStack** [**-a**] [**-l**] [**-p**] [**-n**]|Fornece um rastreamento de pilha apenas do código gerenciado.<br /><br /> A opção **-p** mostra argumentos para a função gerenciada.<br /><br /> A opção **-l** mostra informações sobre as variáveis locais em um quadro. A extensão de depuração SOS não pode recuperar nomes locais, portanto, a saída para nomes locais está no formato \<*local address*> **=** \<*value*> .<br /><br /> A opção **-a**(all) é um atalho para **-l** e **-p** combinados.<br /><br /> A opção **-n** desabilita a exibição dos nomes de arquivo de origem e dos números de linha. Se o depurador tiver a opção SYMOPT_LOAD_LINES especificada, o SOS pesquisará os símbolos de cada quadro gerenciado e, se bem-sucedido, exibirá o nome do arquivo de origem e o número da linha correspondentes. O parâmetro **-n** (sem números de linha) pode ser especificado para desabilitar esse comportamento.<br /><br /> A Extensão de Depuração do SOS não exibe quadros de transição em plataformas com base em x64 e IA-64.|
|**COMState**|Lista o modelo de apartment COM para cada thread e um ponteiro `Context`, se disponível.|
|**DumpArray** [**-início** \<*startIndex*> ] [**-comprimento** \<*length*> ] [**-detalhes**] [**-nofields**]\<*array object address*><br /><br /> -ou-<br /><br /> **Da** [**-Start** \<*startIndex*> ] [**comprimento** \<*length*> ] [**-detalhe**] [**-nofields**] *endereço do objeto de matriz*>|Examina elementos de um objeto da matriz.<br /><br /> A opção **-start** especifica o índice inicial no qual os elementos são exibidos.<br /><br /> A opção **-length** especifica quantos elementos devem ser mostrados.<br /><br /> A opção **-details** exibe detalhes do elemento usando os formatos **DumpObj** e **DumpVC**.<br /><br /> A opção **-nofields** impede que as matrizes sejam exibidas. Essa opção só está disponível quando a opção **-detail** é especificada.|
|**DumpAssembly**\<*assembly address*>|Exibe informações sobre um assembly.<br /><br /> O comando **DumpAssembly** lista vários módulos, se existirem.<br /><br /> É possível obter um endereço de assembly usando o comando **DumpDomain**.|
|**DumpClass**\<*EEClass address*>|Exibe informações sobre a estrutura `EEClass` associada a um tipo.<br /><br /> O comando **DumpClass** exibe valores de campo estáticos, mas não exibe valores de campo não estáticos.<br /><br /> Use o comando **DumpMT**, **DumpObj**, **Name2EE** ou **Token2EE** para obter um endereço da estrutura do `EEClass`.|
|**DumpDomain** [ \<*domain address*> ]|Enumera cada objeto <xref:System.Reflection.Assembly> carregado no endereço do objeto <xref:System.AppDomain> especificado.  Quando chamado sem parâmetros, o comando **DumpDomain** lista todos os objetos <xref:System.AppDomain> em um processo.|
|**Dumpheap** [**-stat**] [**-Strings**] [**-Short**] [**-min** \<*size*> ] [-**Max** \<*size*> ] [**-thinlock**] [**-startAtLowerBound**] [**-MT** \<*MethodTable address*> ] [**-Type** \<*partial type name*> ] [*Start* [*end*]]|Exibe informações sobre o heap com coleta de lixo e as estatísticas de coleção sobre objetos.<br /><br /> O comando **DumpHeap** exibirá um aviso se detectar fragmentação excessiva no heap do coletor de lixo.<br /><br /> A opção **-stat** restringe a saída ao resumo de tipo estatístico.<br /><br /> A opção **-strings** restringe a saída a um resumo de valor da cadeia de caracteres estatístico.<br /><br /> A opção **-short** limita a saída apenas ao endereço de cada objeto. Isso permite redirecionar facilmente do comando para outro comando do depurador para automação.<br /><br /> A opção **-min** ignora objetos menores que o parâmetro `size`, especificado em bytes.<br /><br /> A opção **-max** ignora objetos maiores que o parâmetro `size`, especificado em bytes.<br /><br /> A opção **-thinlock** relata ThinLocks.  Para obter mais informações, consulte o comando **SyncBlk**.<br /><br /> A opção `-startAtLowerBound` força o exame do heap para iniciar no limite inferior de um intervalo de endereços fornecido. Durante a fase de planejamento, o heap não costuma ser examinável porque os objetos estão sendo movidos. Essa opção força **DumpHeap** para iniciar seu exame no limite inferior especificado. Você deve fornecer o endereço de um objeto válido como o limite inferior para que essa opção funcione. É possível exibir a memória no endereço de um objeto inválido para encontrar manualmente a próxima tabela do método. Se a coleta de lixo estiver atualmente em uma chamada para `memcopy`, você também poderá encontrar o endereço do próximo objeto adicionando o tamanho ao endereço inicial, fornecido como um parâmetro.<br /><br /> A opção **-mt** lista apenas os objetos que correspondem à estrutura especificada `MethodTable`.<br /><br /> A opção **-type** lista apenas esses objetos cujo nome do tipo é uma correspondência de subcadeia de caracteres da cadeia de caracteres especificada.<br /><br /> O parâmetro `start` inicia a listagem com base no endereço especificado.<br /><br /> O parâmetro `end` para a listagem no endereço especificado.|
|**DumpIL** \<*Managed DynamicMethod object*> &#124; \<*DynamicMethodDesc pointer*> &#124;\<*MethodDesc pointer*>|Exibe o MSIL (Microsoft Intermediate Language) associado a um método gerenciado.<br /><br /> Observe que o MSIL dinâmico é emitido de forma diferente do que o MSIL carregado com base em um assembly. O MSIL dinâmico faz referência a objetos em uma matriz de objetos gerenciados, em vez dos tokens de metadados.|
|**DumpLog** [**-addr** \<*addressOfStressLog*> ] [<*Filenam* `e`>]|Grava o conteúdo de um log de estresse na memória no arquivo especificado. Se você não especificar um nome, esse comando criará um arquivo chamado StressLog.txt no diretório atual.<br /><br /> O log de estresse na memória ajuda a diagnosticar falhas de estresse sem usar bloqueios ou E/S. Para habilitar o log de estresse, defina as seguintes chaves do Registro em HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\.NETFramework:<br /><br /> (DWORD) StressLog = 1<br /><br /> (DWORD) LogFacility = 0xffffffff<br /><br /> (DWORD) StressLogSize = 65536<br /><br /> A opção `-addr` permite especificar um log de estresse diferente do log padrão.|
|**DumpMD**\<*MethodDesc address*>|Exibe informações sobre uma estrutura `MethodDesc` no endereço especificado.<br /><br /> É possível usar o comando **IP2MD** para obter o endereço da estrutura `MethodDesc` com base em uma função gerenciada.|
|**DumpMT** [**-MD**]\<*MethodTable address*>|Exibe informações sobre uma tabela de métodos no endereço especificado. A especificação da opção **-MD** exibe uma lista de todos os métodos definidos com o objeto.<br /><br /> Cada objeto gerenciado contém um ponteiro da tabela do método.|
|**DumpMethodSig** \<*sigaddr*> DumpMethodSig  < *moduleadd*`r`>|Exibe informações sobre uma estrutura `MethodSig` no endereço especificado.|
|**DumpModule** [**-MT**]\<*Module address*>|Exibe informações sobre um módulo no endereço especificado. A opção **-mt** exibe os tipos definidos em um módulo e os tipos referenciados pelo módulo<br /><br /> É possível usar o comando **DumpDomain** ou **DumpAssembly** para recuperar o endereço de um módulo.|
|**DumpObj** [**-nofields**]\<*object address*><br /><br /> -ou-<br /><br /> **Fazer**\<*object address*>|Exibe informações sobre um objeto no endereço especificado. O comando **DumpObj** exibe os campos, as informações da estrutura `EEClass`, a tabela do método e o tamanho do objeto.<br /><br /> É possível usar o comando **DumpStackObjects** para recuperar o endereço de um objeto.<br /><br /> Observe que é possível executar o comando **DumpObj** em campos do tipo `CLASS` porque eles também são objetos.<br /><br /> A opção `-`**nofields** evita a exibição de campos do objeto e é útil para objetos como String.|
|**DumpRuntimeTypes**|Exibe os objetos de tipo do runtime no heap do coletor de lixo e lista seus nomes de tipo associados e tabelas de método.|
|**DumpStack** [**-EE**] [**-n**] [ `top` *pilha* [ `bottom` *STAC* `k` ]]|Exibe um rastreamento de pilha.<br /><br /> A opção **-EE** faz com que o comando **DumpStack** exiba apenas funções gerenciadas. Use os parâmetros `top` e `bottom` para limitar os quadros de pilha exibidos em plataformas x86.<br /><br /> A opção **-n** desabilita a exibição dos nomes de arquivo de origem e dos números de linha. Se o depurador tiver a opção SYMOPT_LOAD_LINES especificada, o SOS pesquisará os símbolos de cada quadro gerenciado e, se bem-sucedido, exibirá o nome do arquivo de origem e o número da linha correspondentes. O parâmetro **-n** (sem números de linha) pode ser especificado para desabilitar esse comportamento.<br /><br /> Em plataformas x86 e x64, o comando **DumpStack** cria um rastreamento de pilha detalhado.<br /><br /> Em plataformas com base em IA-64, o comando **DumpStack** imita o comando **K** do depurador. Os parâmetros `top` e `bottom` são ignorados em plataformas com base em IA-64.|
|**DumpSig** \<*sigaddr*>\<*moduleaddr*>|Exibe informações sobre uma estrutura `Sig` no endereço especificado.|
|**DumpSigElem** \<*sigaddr*>\<*moduleaddr*>|Exibe um único elemento de um objeto de assinatura. Na maioria dos casos, você deve usar **DumpSig** para observar objetos de assinatura individuais. No entanto, se uma assinatura tiver sido corrompida de alguma forma, será possível usar **DumpSigElem** para ler partes válidas dela.|
|**DumpStackObjects** [**-verificar**] [ `top` *pilha* [ `bottom` *pilha*]]<br /><br /> -ou-<br /><br /> **DSO** [**-Verify**] [ `top` *pilha* [ `bottom` *pilha*]]|Exibe todos os objetos gerenciados encontradas dentro dos limites da pilha atual.<br /><br /> A opção **-verify** valida cada campo `CLASS` não estático de um campo do objeto.<br /><br /> Use o comando **DumpStackObject** com comandos de rastreamento de pilha como o comando **K** e o comando **CLRStack** para determinar os valores de variáveis locais e parâmetros.|
|**DumpVC** \<*MethodTable address*>\<*Address*>|Exibe informações sobre os campos de uma classe de valor no endereço especificado.<br /><br /> O parâmetro **MethodTable** permite que o comando **DumpVC** interprete os campos corretamente. As classes de valor não têm uma tabela de método como seu primeiro campo.|
|**EEHeap** [**-gc**] [**-loader**]|Exibe informações sobre a memória de processo consumida por estruturas de dados CLR internas.<br /><br /> As opções **-gc** e **-loader** limitam a saída desse comando ao coletor de lixo ou a estruturas de dados do carregador.<br /><br /> As informações do coletor de lixo lista os intervalos de cada segmento no heap gerenciado.  Se o ponteiro estiver dentro de um intervalo de segmentos indicado por **-gc**, o ponteiro será um ponteiro de objeto.|
|**EEStack** [**-short**] [**-EE**]|Executa o comando **DumpStack** em todos os threads no processo.<br /><br /> A opção **-EE** é passada diretamente para o comando **DumpStack**. O parâmetro **-short** limita a saída aos seguintes tipos de threads:<br /><br /> Threads que tenham utilizado um bloqueio.<br /><br /> Threads que foram interrompidos para permitir uma coleta de lixo.<br /><br /> Threads que estão atualmente em código gerenciado.|
|**EEVersion**|Exibe a versão do CLR.|
|**Ehinfo** [ \<*MethodDesc address*> ] [ \<*Code address*> ]|Exibe os blocos de tratamento de exceções em um método especificado.  Este comando exibe os endereços de código e os deslocamentos do bloco de cláusula (o bloco `try`) e o bloco do manipulador (o bloco `catch`).|
|**perguntas frequentes**|Exibe perguntas frequentes|
|**FinalizeQueue** [**-detail**] &#124; [**-allReady**] [**-short**]|Exibe todos os objetos registrados para a finalização.<br /><br /> A opção **-detail** exibe informações extras sobre todos os `SyncBlocks` que precisam ser limpos e todos os `RuntimeCallableWrappers` (RCWs) que aguardam limpeza. Ambas as estruturas de dados são armazenadas em cache e limpas pelo thread de finalizador quando é executado.<br /><br /> A opção `-allReady` exibe todos os objetos prontos para finalização, independentemente de também já estarem marcados assim pela coleta de lixo, ou serão marcado pela próxima coleta de lixo. Os objetos prontos na lista "pronta para finalização" são objetos finalizáveis que não têm mais raízes. Essa opção pode ser muito cara, porque verifica se todos os objetos nas filas finalizáveis ainda têm raízes.<br /><br /> A opção `-short` limita a saída ao endereço de cada objeto. Se for usado com **-allReady**, ele enumerará todos os objetos que têm um finalizador que não tem mais raiz. Se for usado independentemente, ele listará todos os objetos nas filas finalizáveis e "prontas para finalização".|
|**FindAppDomain**\<*Object address*>|Determina o domínio do aplicativo de um objeto no endereço especificado.|
|**FindRoots** **-Gen** \<*N*> &#124; **-Gen qualquer** &#124;\<*object address*>|Faz o depurador parar no elemento a ser depurado na próxima coleção da geração especificada. O efeito será redefinido assim que ocorrer a parada. Para parar na próxima coleção, você precisa reemitir o comando. A *\<object address>* forma desse comando é usada após a interrupção causada pelo **-Gen** ou **-Gen** que ocorreu. Nesse momento, o depurador está no estado correto de **FindRoots** para identificar raízes para objetos nas gerações condenadas atuais.|
|**GCHandles** [**-perdomain**]|Exibe estatísticas sobre identificadores do coletor de lixo no processo.<br /><br /> A opção **-perdomain** organiza as estatísticas por domínio do aplicativo.<br /><br /> Use o comando **GCHandles** para encontrar perdas de memória causadas por perdas de identificador do coletor de lixo. Por exemplo, uma perda de memória ocorre quando o código mantém uma grande matriz porque um identificador do coletor de lixo forte continua apontando para ela e o identificador é descartado sem liberá-lo.|
|**GCHandleLeaks**|Procura na memória referências para identificadores do coletor de lixo fortes e fixos no processo e exibe os resultados. Se um identificador for encontrado, o comando **GCHandleLeaks** exibirá o endereço da referência. Se um identificador não for encontrado na memória, esse comando exibirá uma notificação.|
|**GCInfo**\<*MethodDesc address*>\<*Code address*>|Exibe dados que indicam quando registros ou locais da pilha contêm objetos gerenciados. Se ocorrer uma coleta de lixo, o coletor deverá saber os locais de referências para objetos de forma que possa atualizá-los com novos valores de ponteiro do objeto.|
|**Gcroot** [**-nostacks**]\<*Object address*>|Exibe informações sobre referências (ou nós) para um objeto no endereço especificado.<br /><br /> O comando **GCRoot** examina todo o heap gerenciado e a tabela do identificador para identificadores dentro de outros objetos e identificadores na pilha. Em seguida, cada pilha é pesquisada em busca de ponteiros para objetos, e a fila de finalizadores também é pesquisada.<br /><br /> Este comando não determina se uma raiz da pilha é válida ou se é descartada. Use os comandos **CLRStack** e **U** para desmontar o quadro a que valor de local ou argumento pertence para determinar se a raiz da pilha ainda está em uso.<br /><br /> A opção **-nostacks** restringe a pesquisa a identificadores do coletor de lixo e objetos alcançáveis.|
|**GCWhere**  *\<object address>*|Exibe o local e o tamanho no heap da coleta de lixo do argumento passado. Quando o argumento está no heap gerenciado, mas não é um endereço de objeto válido, o tamanho é exibido como 0 (zero).|
|**ajuda** [ \<*command*> ] [ `faq` ]|Exibe todos os comandos disponíveis quando nenhum parâmetro é especificado ou exibe informações detalhadas sobre o comando especificado.<br /><br /> O parâmetro `faq` exibe respostas para perguntas frequentes.|
|**HeapStat** [**-inclUnrooted** &#124; **-iu**]|Exibe os tamanhos de geração para cada heap e o espaço livre total em cada geração em cada heap. Se a opção -**inclUnrooted** for especificada, o relatório incluirá informações sobre os objetos gerenciados do heap da coleta de lixo que não tem mais raiz.|
|**HistClear**|Libera todos os recursos usados pela família de comandos `Hist`.<br /><br /> Em geral, você não precisa chamar explicitamente `HistClear`, porque cada `HistInit` limpa os recursos anteriores.|
|**HistInit**|Inicializa as estruturas de SOS com base no log de estresse salvo no elemento a ser depurado.|
|**HistObj***\<obj_address>*|Examina todos os registros de realocação do log de estresse e exibe a cadeia de realocações da coleta de lixo que podem ter levado o endereço a ser passado como um argumento.|
|**HistObjFind**  *\<obj_address>*|Exibe todas as entradas de log que fazem referência a um objeto no endereço especificado.|
|**HistRoot***\<root>*|Exibe informações relacionadas a ambas as promoções e realocações da raiz especificada.<br /><br /> O valor raiz pode ser usado para rastrear o movimento de um objeto por meio das coletas de lixo.|
|**IP2MD**\<*Code address*>|Exibe a estrutura `MethodDesc` no endereço especificado no código compilado com JIT.|
|`ListNearObj` (`lno`) *\<obj_address>*|Exibe os objetos anteriores e posteriores ao endereço especificado. O comando procura o endereço no heap da coleta de lixo semelhante a um início válido de um objeto gerenciado (com base em uma tabela de método válida) e o objeto posterior ao endereço do argumento.|
|**MinidumpMode** [**0**] [**1**]|Impede a execução de comandos não seguros durante o uso de um minidespejo.<br /><br /> Passe **0** para desabilitar esse recurso ou **1** para habilitar esse recurso. Por padrão, o valor **MinidumpMode** é definido como **0**.<br /><br /> Minidespejos criados com os comandos **.dump /m** ou **.dump** têm dados específicos limitados a CLR e permitem que você execute apenas um subconjunto de comandos SOS corretamente. Alguns comandos podem falhar com erros inesperados porque as áreas necessárias de memória não são mapeadas ou estão mapeadas apenas parcialmente. Essa opção evita a execução de comandos não seguros em minidespejos.|
|**Name2EE** \<*module name*>\<*type or method name*><br /><br /> -ou-<br /><br /> **Name2EE** \<*module name*> **!**\<*type or method name*>|Exibe a estrutura `MethodTable` e a estrutura `EEClass` do tipo ou do método especificado no módulo especificado.<br /><br /> O módulo especificado deve ser carregado no processo.<br /><br /> Para obter o nome do tipo apropriado, procure o módulo usando o [Ildasm.exe (IL Disassembler)](ildasm-exe-il-disassembler.md). Também é possível passar `*` como o parâmetro de nome do módulo para pesquisar todos os módulos gerenciados carregados. O parâmetro *module name* também pode ser o nome do depurador para um módulo, como `mscorlib` ou `image00400000`.<br /><br /> Este comando dá suporte à sintaxe do depurador do Windows de <`module`>`!`<`type`>. O tipo deve estar totalmente qualificado.|
|**ObjSize** [ \<*Object address*> ] &#124; [**-Aggregate**] [**-stat**]|Exibe o tamanho do objeto especificado. Se você não especificar parâmetros, o comando **ObjSize** exibirá o tamanho de todos os objetos encontrados em threads gerenciados, exibirá todos os identificadores do coletor de lixo do processo e totalizará o tamanho de todos os objetos apontados por esses identificadores. O comando **ObjSize** inclui o tamanho de todos os objetos filho além do pai.<br /><br /> A opção **-aggregate** pode ser usada com o argumento **-stat** para obter uma exibição detalhada dos tipos que ainda têm raízes. Usando **!dumpheap -stat** e **!objsize -aggregate -stat**, é possível determinar quais objetos não têm mais raízes e diagnosticar diversos problemas de memória.|
|**Reexception** [**-Nested**] [**-Lines**] [ \<*Exception object address*> ]<br /><br /> -ou-<br /><br /> **PE** [**-aninhado**] [ \<*Exception object address*> ]|Exibe e formata campos de qualquer objeto derivado da classe <xref:System.Exception> no endereço especificado. Se você não especificar um endereço, o comando **PrintException** exibirá a última exceção gerada no thread atual.<br /><br /> A opção **-nested** exibe detalhes sobre objetos de exceção aninhados.<br /><br /> A opção **-lines** exibe informações da origem, se disponível.<br /><br /> É possível usar esse comando para formatar e exibir o campo `_stackTrace`, que é uma matriz binária.|
|**ProcInfo** [**-env**] [**-time**] [**-mem**]|Exibe variáveis de ambiente do processo, o tempo de CPU do kernel e as estatísticas de uso da memória.|
|**RCWCleanupList**\<*RCWCleanupList address*>|Exibe a lista de runtime callable wrappers no endereço especificado que aguardam limpeza.|
|**SaveModule** \<*Base address*>\<*Filename*>|Grava uma imagem, carregada na memória do endereço especificado, no arquivo especificado.|
|**SOSFlush**|Libera um cache SOS interno.|
|**StopOnException** [**-Derived**] [**-Create** &#124; **-create2**] \<*Exception*>\<*Pseudo-register number*>|Faz com que o depurador pare quando a exceção especificada é lançada, mas continua executando enquanto outras exceções são lançadas.<br /><br /> A opção **-derived** captura a exceção especificada e cada exceção derivada da exceção especificada.|
|**SyncBlk** [**-todos os** &#124; \<*syncblk number*> ]|Exibe a estrutura `SyncBlock` especificada ou todas as estruturas `SyncBlock`.  Se você não passar argumentos, o comando **SyncBlk** exibirá a estrutura `SyncBlock` correspondente aos objetos pertencentes a um thread.<br /><br /> Uma estrutura `SyncBlock` é um contêiner de informações extras que não precisa ser criada para cada objeto. Ele pode armazenar dados de interoperabilidade COM, códigos de hash e informações de bloqueio para operações thread-safe.|
|**ThreadPool**|Exibe informações sobre o pool de threads gerenciados, inclusive o número de solicitações de trabalho na fila, o número de threads de porta de conclusão e o número de timers.|
|**Token2EE** \<*module name*>\<*token*>|Transforma o token de metadados especificado no módulo especificado em uma estrutura `MethodTable` ou na estrutura `MethodDesc`.<br /><br /> É possível passar `*` para o parâmetro de nome do módulo para saber para o que o token é mapeado em cada módulo gerenciado carregado. Também é possível passar o nome do depurador para um módulo como, por exemplo, `mscorlib` ou `image00400000`.|
|**Threads** [**-live**] [**-special**]|Exibe todos os threads gerenciados no processo.<br /><br /> O comando **Threads** exibe a ID abreviada do depurador, a ID do thread do CLR e a ID do thread do sistema operacional.  Além disso, o comando **Threads** exibe uma coluna Domain que indica o domínio do aplicativo no qual um thread está em execução, uma coluna APT que exibe o modo apartment COM e uma coluna Exception que exibe a última exceção gerada no thread.<br /><br /> A opção **-live** exibe threads associados a um thread dinâmico.<br /><br /> A opção **-special** exibe todos os threads especiais criados pelo CLR. Entre os threads especiais estão threads de coleta de lixo (em coleta de lixo simultânea e de servidor), threads auxiliares de depurador, threads finalizadores, threads de descarregamento <xref:System.AppDomain> e threads de timer do pool de threads.|
|**ThreadState\<** *State value field* **>**|Exibe o estado do thread. O parâmetro `value` é o valor do campo `State` na saída de relatório **Threads**.<br /><br /> Exemplo:<br /><br /> `0:003> !Threads     ThreadCount:      2     UnstartedThread:  0     BackgroundThread: 1     PendingThread:    0     DeadThread:       0     Hosted Runtime:   no                                           PreEmptive   GC Alloc           Lock            ID OSID ThreadOBJ    State     GC       Context       Domain   Count APT Exception        0    1  250 0019b068      a020 Disabled 02349668:02349fe8 0015def0     0 MTA        2    2  944 001a6020      b220 Enabled  00000000:00000000 0015def0     0 MTA (Finalizer)     0:003> !ThreadState b220         Legal to Join         Background         CLR Owns         CoInitialized         In Multi Threaded Apartment`|
|**TraverseHeap** [**-XML**]\<*filename*>|Grava informações do heap no arquivo especificado em um formato compreendido pelo do criador de perfil do CLR. A opção **-xml** faz o comando **TraverseHeap** formatar o arquivo como XML.<br /><br /> Baixe o Criador de Perfil do CLR no [Centro de Download da Microsoft](https://go.microsoft.com/fwlink/?LinkID=67325).|
|**U** [**-GCInfo**] [**-ehinfo**] [**-n**] \<*MethodDesc address*> &#124;\<*Code address*>|Exibe uma desmontagem anotada de um método gerenciado especificado por um ponteiro de estrutura `MethodDesc` para o método ou por um endereço de código dentro do corpo do método. O comando **U** exibe todo o método do começo ao fim, com anotações que convertem tokens de metadados em nomes.<br /><br /> A opção **-gcinfo** faz o comando **U** exibir a estrutura `GCInfo` do método.<br /><br /> A opção **-ehinfo** exibe informações sobre exceção do método. Também é possível pode obter essas informações com o comando **EHInfo**.<br /><br /> A opção **-n** desabilita a exibição dos nomes de arquivo de origem e dos números de linha. Se o depurador tiver a opção SYMOPT_LOAD_LINES especificada, o SOS pesquisará os símbolos de cada quadro gerenciado e, se bem-sucedido, exibirá o nome do arquivo de origem e o número da linha correspondentes. Especifique a opção **-n** para desabilitar esse comportamento.|
|**VerifyHeap**|Verifica o heap do coletor de lixo em busca de sinais de corrupção e exibe todos os erros encontrados.<br /><br /> As corrupções de heap podem ser causadas por chamadas de invocação da plataforma construídas incorretamente.|
|**VerifyObj**\<*object address*>|Verifica o objeto passado como um argumento em busca de sinais de corrupção.|
|**VMMap**|Atravessa o espaço do endereço virtual e exibe o tipo de proteção aplicado a cada região.|
|**VMStat**|Fornece uma exibição resumida do espaço de endereço virtual, ordenado pelo tipo de proteção aplicado a essa memória (livre, reservada, confirmada, particular, mapeada, imagem). A coluna TOTAL exibe o resultado da coluna AVERAGE multiplicada pela coluna BLK COUNT.|

## <a name="remarks"></a>Comentários

A Extensão de Depuração SOS permite exibir informações sobre o código executado dentro do CLR. Por exemplo, é possível usar a Extensão de Depuração SOS para exibir informações sobre o heap gerenciado, procurar corrupções de heap, exibir tipos de dados internos usados pelo runtime e exibir informações sobre todos os códigos gerenciados em execução dentro do runtime.

Para usar a Extensão de Depuração SOS no Visual Studio, instale o [WDK (Kit de Driver do Windows)](/windows-hardware/drivers/download-the-wdk). Para obter informações sobre o ambiente de depuração integrado no Visual Studio, confira [Ambientes de Depuração](/windows-hardware/drivers/debugger/debuggers-in-the-debugging-tools-for-windows-package).

Também é possível usar a Extensão de Depuração SOS carregando-a no [depurador WinDbg.exe](/windows-hardware/drivers/debugger/debugger-download-tools) e executando comandos dentro de WinDbg.exe.

Para carregar a Extensão de Depuração SOS no depurador WinDbg.exe, execute o seguinte comando na ferramenta:

```console
.loadby sos clr
```

WinDbg.exe e o Visual Studio usam uma versão de SOS.dll que corresponde à versão de Mscorwks.dll atualmente em uso. Por padrão, você deve usar a versão de SOS.dll correspondente à versão atual de Mscorwks.dll.

Para usar um arquivo de despejo criado em outro computador, verifique se o arquivo Mscorwks.dll que acompanha essa instalação está no caminho do símbolo e carregue a versão correspondente de SOS.dll.

Para carregar uma versão específica de SOS.dll, digite o seguinte comando no Depurador do Windows:

```console
.load <full path to sos.dll>
```

## <a name="examples"></a>Exemplos

O comando a seguir exibe o conteúdo de uma matriz no endereço `00ad28d0`.  A exibição começa pelo segundo elemento e continua por mais cinco elementos.

```console
!dumparray -start 2 -length 5 -detail 00ad28d0
```

O comando a seguir exibe o conteúdo de um assembly no endereço `1ca248`.

```console
!dumpassembly 1ca248
```

O comando a seguir exibe informações sobre o heap do coletor de lixo.

```console
!dumpheap
```

O comando a seguir grava o conteúdo do log de estresse na memória em um arquivo (padrão) chamado StressLog.txt no diretório atual.

```console
!DumpLog
```

O comando a seguir exibe a estrutura `MethodDesc` no endereço `902f40`.

```console
!dumpmd 902f40
```

O comando a seguir exibe informações sobre um módulo no endereço `1caa50`.

```console
!dumpmodule 1caa50
```

O comando a seguir exibe informações sobre um objeto no endereço `a79d40`.

```console
!DumpObj a79d40
```

O comando a seguir exibe os campos de uma classe de valor no endereço `00a79d9c` usando a tabela de método no endereço `0090320c`.

```console
!DumpVC 0090320c 00a79d9c
```

O comando a seguir exibe a memória do processo usada pelo coletor de lixo.

```console
!eeheap -gc
```

O comando a seguir exibe todos os objetos programados para a finalização.

```console
!finalizequeue
```

O comando a seguir determina o domínio do aplicativo de um objeto no endereço `00a79d98`.

```console
!findappdomain 00a79d98
```

O comando a seguir exibe todos os identificadores do coletor de lixo no processo atual.

```console
!gcinfo 5b68dbb8
```

O comando a seguir exibe as estruturas `MethodTable` e `EEClass` do método `Main` na classe `MainClass` do módulo `unittest.exe`.

```console
!name2ee unittest.exe MainClass.Main
```

O comando a seguir exibe informações sobre o token de metadados no endereço `02000003` do módulo `unittest.exe`.

```console
!token2ee unittest.exe 02000003
```

## <a name="see-also"></a>Consulte também

- [Ferramentas](index.md)
- [Prompts de comando](developer-command-prompt-for-vs.md)
