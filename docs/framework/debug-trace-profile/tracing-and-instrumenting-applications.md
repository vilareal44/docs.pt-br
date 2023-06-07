---
title: Rastreamento e instrumentação de aplicativos
description: Rastrear e instrumentar aplicativos no .NET. O rastreamento permite que você monitore a execução do aplicativo enquanto ele está em execução. A instrumentação permite que você meça o nível de desempenho.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tracing [.NET Framework]
- debugging [.NET Framework], instrumentation
- performance monitoring, instrumentation
- instrumentation, about instrumentation
- tracing [.NET Framework], about tracing
- performance monitoring, tracing code
- Trace class, instrumentation for .NET applications
ms.assetid: 773b6fc4-9013-4322-b728-5dec7a72e743
ms.openlocfilehash: d5484129ac17ee20aafe305bea5599f85903dfa2
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803515"
---
# <a name="tracing-and-instrumenting-applications"></a>Rastreamento e instrumentação de aplicativos
O rastreamento é uma maneira de você monitorar a execução de seu aplicativo enquanto ele é executado. É possível adicionar a instrumentação de rastreamento e depuração ao aplicativo do .NET Framework durante seu desenvolvimento e usar essa instrumentação enquanto você estiver desenvolvendo o aplicativo e depois de implantá-lo. É possível usar as classes <xref:System.Diagnostics.Trace?displayProperty=nameWithType>, <xref:System.Diagnostics.Debug?displayProperty=nameWithType> e <xref:System.Diagnostics.TraceSource?displayProperty=nameWithType> para registrar informações sobre erros e a execução do aplicativo em logs, arquivos de texto ou outros dispositivos para análise posterior.  
  
 O termo *instrumentação* refere-se à capacidade de monitorar ou medir o nível de desempenho de um produto e de diagnosticar erros. Em programação, isso significa a capacidade de um aplicativo de incorporar:  
  
- **Rastreamento de código** –Recebimento de mensagens informativas sobre a execução de um aplicativo em tempo de execução.  
  
- **Depuração** – Rastreamento e correção de erros de programação em um aplicativo em desenvolvimento. Para obter mais informações, consulte [Depurando](/visualstudio/debugger/debugger-feature-tour).  
  
- **Contadores de desempenho** – Componentes que permitem acompanhar o desempenho do aplicativo. Para obter mais informações, consulte [contadores de desempenho](performance-counters.md).  
  
- **Logs de eventos** – Componentes que permitem receber e acompanhar os principais eventos na execução do aplicativo. Para obter mais informações, consulte a classe <xref:System.Diagnostics.EventLog>.  
  
 A instrumentação do aplicativo com a colocação de instruções de rastreamento em locais estratégicos no código é especialmente útil para aplicativos distribuídos. Usando as instruções de rastreamento, você pode instrumentar um aplicativo não apenas para exibir informações quando as coisas dão errado, mas também para monitorar o desempenho do aplicativo.  
  
 A classe <xref:System.Diagnostics.TraceSource> fornece recursos de rastreamento avançados e pode ser usada no lugar dos métodos estáticos das classes de rastreamento antigas <xref:System.Diagnostics.Trace> e <xref:System.Diagnostics.Debug>. As classes <xref:System.Diagnostics.Trace> e <xref:System.Diagnostics.Debug> conhecidas ainda são amplamente usadas, mas a classe <xref:System.Diagnostics.TraceSource> é recomendada para novos comandos de rastreamento, como <xref:System.Diagnostics.TraceSource.TraceEvent%2A> e <xref:System.Diagnostics.TraceSource.TraceData%2A>.  
  
 As classes <xref:System.Diagnostics.Trace> e <xref:System.Diagnostics.Debug> são idênticas, exceto pelo fato de que os procedimentos e as funções da classe <xref:System.Diagnostics.Trace> são compiladas por padrão em builds de versão, ao contrário daqueles da classe <xref:System.Diagnostics.Debug>.  
  
 As classes <xref:System.Diagnostics.Trace> e <xref:System.Diagnostics.Debug> fornecem os meios para monitorar e examinar o desempenho do aplicativo durante o desenvolvimento ou após a implantação. Por exemplo, é possível usar a classe <xref:System.Diagnostics.Trace> para acompanhar determinados tipos de ações em um aplicativo implantado, conforme eles ocorrem (por exemplo, criação de novas conexões de banco de dados) e, portanto, é possível monitorar a eficiência do aplicativo.  
  
## <a name="code-tracing-and-debugging"></a>Rastreamento e depuração de código  
 Durante o desenvolvimento, use os métodos de saída da classe <xref:System.Diagnostics.Debug> para exibir mensagens na janela de Saída do IDE (ambiente de desenvolvimento integrado) do Visual Studio. Por exemplo:  
  
```vb  
Trace.WriteLine("Hello World!")  
Debug.WriteLine("Hello World!")  
```  
  
```csharp  
System.Diagnostics.Trace.WriteLine("Hello World!");  
System.Diagnostics.Debug.WriteLine("Hello World!");  
```  
  
 Cada um desses exemplos exibirá “Olá, Mundo!” na janela de Saída quando o aplicativo for executado no depurador.  
  
 Isso permite depurar os aplicativos e otimizar seu desempenho com base em seu comportamento no ambiente de teste. É possível depurar o aplicativo no build de depuração com o atributo condicional <xref:System.Diagnostics.Debug> ativado, de modo que toda a saída de depuração seja recebida. Quando o aplicativo está pronto para liberação, é possível compilar o build de versão sem ativar o atributo condicional <xref:System.Diagnostics.Debug>, de modo que o compilador não inclua o código de depuração no executável final. Para obter mais informações, consulte [Como compilar condicionalmente com Trace e Debug](how-to-compile-conditionally-with-trace-and-debug.md). Para obter mais informações sobre diferentes configurações de build para o aplicativo, consulte [Compilando e criando](/visualstudio/ide/compiling-and-building-in-visual-studio).  
  
 Você também pode rastrear a execução de código em um aplicativo instalado, usando métodos da classe <xref:System.Diagnostics.Trace>. Incluindo [Opções de Rastreamento](trace-switches.md) em seu código, você pode controlar se o rastreamento ocorre e qual a sua extensão. Isso permite monitorar o status do aplicativo em um ambiente de produção. Isso é particularmente importante em um aplicativo de negócios que usa vários componentes executados em vários computadores. É possível controlar como as opções são usadas após a implantação por meio do arquivo de configuração. Para obter mais informações, consulte [Como criar, inicializar e configurar opções de rastreamento](how-to-create-initialize-and-configure-trace-switches.md).  
  
 Quando você estiver desenvolvendo um aplicativo para o qual pretende usar o rastreamento, você geralmente incluirá mensagens de rastreamento e depuração no código do aplicativo. Quando estiver pronto para implantar o aplicativo, compile o build de versão sem ativar o atributo condicional **Debug**. No entanto, você pode ativar o atributo condicional **Trace**, de modo que o compilador inclua o código de rastreamento no executável. Para obter mais informações, consulte [Como compilar condicionalmente com Trace e Debug](how-to-compile-conditionally-with-trace-and-debug.md).  
  
### <a name="phases-of-code-tracing"></a>Fases de rastreamento de código  
 Há três fases de rastreamento de código:  
  
1. **Instrumentação** – você adiciona o código de rastreamento ao aplicativo.  
  
2. **Rastreamento** – o código de rastreamento grava as informações no destino especificado.  
  
3. **Análise** – você avalia as informações de rastreamento para identificar e entender problemas no aplicativo.  
  
 Durante o desenvolvimento, todos os métodos de saída de depuração e rastreamento gravam informações na janela de Saída do Visual Studio por padrão. Em um aplicativo implantado, os métodos gravam informações de rastreamento nos destinos especificados. Para obter mais informações sobre como especificar um destino de saída de rastreamento ou de depuração, consulte [Ouvintes de rastreamento](trace-listeners.md).  
  
 Veja a seguir uma visão geral das principais etapas geralmente envolvidas no uso do rastreamento para analisar e corrigir problemas potenciais em aplicativos implantados. Para obter mais informações sobre como executar essas etapas, consulte o link apropriado.  
  
##### <a name="to-use-tracing-in-an-application"></a>Para usar o rastreamento em um aplicativo  
  
1. Considere qual saída de rastreamento você desejará receber no local depois de implantar o aplicativo.  
  
2. Crie um conjunto de opções. Para obter mais informações, consulte [Como: configurar opções de rastreamento](how-to-create-initialize-and-configure-trace-switches.md).  
  
3. Adicione as instruções de rastreamento ao código do aplicativo.  
  
4. Determine o local em que você deseja que a saída de rastreamento seja exibida e adicione os ouvintes apropriados. Para obter mais informações, consulte [Criando e inicializando ouvintes de rastreamento](how-to-create-and-initialize-trace-listeners.md).  
  
5. Teste e depure o aplicativo e o código de rastreamento que ele contém.  
  
6. Compile o aplicativo no código executável usando um dos seguintes procedimentos:  
  
    - Use o menu **Compilar** juntamente com a página **Depurar** da caixa de diálogo **Páginas de Propriedades** no **Gerenciador de Soluções**. Use essa opção quando estiver compilando no Visual Studio.  
  
         \- ou –  
  
    - Use as diretivas do compilador **Trace** e **Debug** para o método de compilação de linha de comando. Para obter mais informações, consulte [Compilando condicionalmente com Trace e Debug](how-to-compile-conditionally-with-trace-and-debug.md). Use essa opção quando estiver compilando na linha de comando.  
  
7. Se ocorrer um problema durante o tempo de execução, ative a opção de rastreamento apropriada. Para obter mais informações, consulte [Configurando Opções de Rastreamento](how-to-create-initialize-and-configure-trace-switches.md).  
  
     O código de rastreamento grava mensagens de rastreamento em um destino especificado, por exemplo, uma tela, um arquivo de texto ou um log de eventos. O tipo de ouvinte que você incluiu na <xref:System.Diagnostics.Trace.Listeners%2A?displayProperty=nameWithType> coleção determina o destino.  
  
8. Analise as mensagens de rastreamento para identificar e entender o problema no aplicativo.  
  
## <a name="trace-instrumentation-and-distributed-applications"></a>Instrumentação de rastreamento e aplicativos distribuídos  
 Ao criar um aplicativo distribuído, talvez seja difícil testá-lo da maneira em que ele será usado. Poucas equipes de desenvolvimento têm a capacidade de testar todas as combinações possíveis de sistemas operacionais ou navegadores da Web (incluindo todas as opções de idiomas localizados) ou de simular o grande número de usuários que acessarão o aplicativo ao mesmo tempo. Nessas circunstâncias, não é possível testar como um aplicativo distribuído responderá a grandes volumes, diferentes configurações e comportamentos exclusivos do usuário final. Além disso, muitas partes de um aplicativo distribuído não tem nenhuma interface do usuário com a qual é possível interagir diretamente ou exibir a atividade dessas partes.  
  
 No entanto, é possível compensar isso permitindo que os aplicativos distribuídos descrevam determinados eventos de interesse para os administradores do sistema, especialmente, as coisas que dão errado, com a *instrumentação* do aplicativo – ou seja, colocando instruções de rastreamento em locais estratégicos no código. Portanto, se algo inesperado ocorrer em tempo de execução (por exemplo, um tempo de resposta excessivamente lento), será possível determinar a provável causa.  
  
 Com instruções de rastreamento, você pode evitar a difícil tarefa de examinar o código-fonte original, modificá-lo, recompilá-lo e tentar produzir o erro em tempo de execução no ambiente de depuração. Lembre-se de que é possível instrumentar um aplicativo não apenas para exibir erros, mas também para monitorar o desempenho.  
  
## <a name="strategic-placement-of-trace-statements"></a>Posicionamento estratégico de instruções de rastreamento  
 Tenha cuidado especial ao colocar as instruções de rastreamento para uso durante o tempo de execução. Considere quais informações de rastreamento são provavelmente necessárias em um aplicativo implantado, para que todos os prováveis cenários de rastreamento sejam abordados de forma adequada. No entanto, como os aplicativos que usam o rastreamento variam muito, não há diretrizes gerais para o posicionamento estratégico de rastreamento. Para obter mais informações sobre como inserir instruções de rastreamento, consulte [Como adicionar instruções de rastreamento ao código do aplicativo](how-to-add-trace-statements-to-application-code.md).  
  
## <a name="output-from-tracing"></a>Saída de rastreamento  
 A saída de rastreamento é coletada por objetos chamados *ouvintes*. Um ouvinte é um objeto que recebe a saída de rastreamento e grava-a em um dispositivo de saída (geralmente, uma janela, um log ou um arquivo de texto). Quando um ouvinte de rastreamento é criado, normalmente, ele é adicionado à coleção <xref:System.Diagnostics.Trace.Listeners%2A?displayProperty=nameWithType>, permitindo que o ouvinte receba toda a saída de rastreamento.  
  
 As informações de rastreamento são sempre gravadas, pelo menos, no destino de saída <xref:System.Diagnostics.Trace> padrão, o <xref:System.Diagnostics.DefaultTraceListener>. Se, por algum motivo, você excluiu o <xref:System.Diagnostics.DefaultTraceListener> sem adicionar outros ouvintes à coleção <xref:System.Diagnostics.Trace.Listeners%2A>, não receberá nenhuma mensagem de rastreamento. Para obter mais informações, consulte [ouvintes de rastreamento](trace-listeners.md).  
  
 Os seis membros <xref:System.Diagnostics.Debug> e métodos <xref:System.Diagnostics.Trace> que gravam informações de rastreamento são listados na tabela a seguir.  
  
|Método|Saída|  
|------------|------------|  
|`Assert`|O texto especificado; ou, se nenhum for especificado, a Pilha de Chamadas. A saída será gravada somente se a condição especificada como um argumento na `Assert` instrução for **false**.|  
|`Fail`|O texto especificado; ou, se nenhum for especificado, a Pilha de Chamadas.|  
|`Write`|O texto especificado.|  
|`WriteIf`|O texto especificado, se a condição especificada como um argumento na `WriteIf` instrução for satisfeita.|  
|`WriteLine`|O texto especificado e um retorno de carro.|  
|`WriteLineIf`|O texto especificado e um retorno de carro, se a condição especificada como um argumento na `WriteLineIf` instrução for satisfeita.|  
  
 Todos os ouvintes na coleção <xref:System.Diagnostics.Trace.Listeners%2A> recebem as mensagens descritas na tabela acima, mas as ações executadas podem variar, dependendo de qual tipo de ouvinte recebe a mensagem. Por exemplo, o <xref:System.Diagnostics.DefaultTraceListener> exibe uma caixa de diálogo de asserção quando recebe uma `Fail` notificação ou com falha `Assert` , mas um <xref:System.Diagnostics.TextWriterTraceListener> simplesmente grava a saída em seu fluxo.  
  
 Produza resultados personalizados implementando seu próprio ouvinte. Um ouvinte de rastreamento personalizado pode, por exemplo, exibir as mensagens em uma caixa de mensagem ou se conectar a um banco de dados para adicionar mensagens a uma tabela. Todos os ouvintes personalizados devem dar suporte aos seis métodos mencionados acima. Para obter mais informações sobre como criar ouvintes definidos pelo desenvolvedor, consulte <xref:System.Diagnostics.TraceListener> na referência do .NET Framework.  
  
 Os `Write` `WriteLine` métodos e sempre gravam o texto que você especificar. `Assert`, `WriteIf` e `WriteLineIf` exigem um argumento booliano que controla se eles gravam o texto especificado; eles gravam o texto especificado somente se a expressão for **true** (for `WriteIf` e `WriteLineIf` ) ou **false** (for `Assert` ). O `Fail` método sempre grava o texto especificado. Para obter mais informações, consulte [Como adicionar instruções de rastreamento ao código do aplicativo](how-to-add-trace-statements-to-application-code.md) e a referência do .NET Framework.  
  
## <a name="security-concerns"></a>Problemas de segurança  
 Se você não desabilitar o rastreamento e a depuração antes de implantar um aplicativo ASP.NET, o aplicativo poderá revelar informações sobre si mesmo que podem ser exploradas por um programa mal-intencionado. Para obter mais informações, consulte [Como compilar condicionalmente com Trace e Debug](how-to-compile-conditionally-with-trace-and-debug.md), [Compilando e criando](/visualstudio/ide/compiling-and-building-in-visual-studio) e [Como criar, inicializar e configurar opções de rastreamento](how-to-create-initialize-and-configure-trace-switches.md). A depuração também é configurável por meio do IIS (Serviços de Informações da Internet).  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Diagnostics.Trace>
- <xref:System.Diagnostics.TraceSource>
- [Contratos de código](code-contracts.md)
- [Tipos de projeto C#, F# e Visual Basic](/visualstudio/debugger/debugging-preparation-csharp-f-hash-and-visual-basic-project-types)
- [Como adicionar instruções de rastreamento ao código de um aplicativo](how-to-add-trace-statements-to-application-code.md)
- [Como compilar condicionalmente com Trace e Debug](how-to-compile-conditionally-with-trace-and-debug.md)
- [Como criar, inicializar e configurar opções de rastreamento](how-to-create-initialize-and-configure-trace-switches.md)
- [Como criar e inicializar fontes de rastreamento](how-to-create-and-initialize-trace-sources.md)
- [Como usar TraceSource e filtros com ouvintes de rastreamento](how-to-use-tracesource-and-filters-with-trace-listeners.md)
- [Ouvintes de rastreamento](trace-listeners.md)
- [Opções de rastreamento](trace-switches.md)
