---
title: Medindo o aprimoramento da inicialização com o .NET Nativo
ms.date: 03/30/2017
ms.assetid: c4d25b24-9c1a-4b3e-9705-97ba0d6c0289
ms.openlocfilehash: 41a693f18ffea0e5ce0ca742bc251d147e8e3784
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79180994"
---
# <a name="measuring-startup-improvement-with-net-native"></a>Medindo o aprimoramento da inicialização com o .NET Nativo
.NET Native melhora significativamente o tempo de inicialização dos aplicativos. Esse aprimoramento é particularmente perceptível em dispositivos portáteis, de baixa energia e com aplicativos complexos. Este tópico ajuda você a começar a trabalhar com a instrumentação básica necessária para medir essa melhoria de inicialização.  
  
 Para facilitar a investigações de desempenho, o .NET Framework e o Windows usam uma estrutura de evento chamada ETW (Rastreamento de Eventos para Windows) que permite que seu aplicativo notifique as ferramentas quando eventos ocorrerem. Em seguida, você pode usar uma ferramenta chamada PerfView para exibir e analisar eventos de ETW facilmente. Este tópico explica como:  
  
- Usar a classe <xref:System.Diagnostics.Tracing.EventSource> para emitir eventos.  
  
- Usar o PerfView para coletar tais eventos.  
  
- Usar o PerfView para coletar tais eventos.  
  
## <a name="using-eventsource-to-emit-events"></a>Usando o EventSource para emitir eventos  
 O <xref:System.Diagnostics.Tracing.EventSource> fornece uma classe base da qual criar um provedor de eventos personalizado. Em geral, você pode criar uma subclasse de <xref:System.Diagnostics.Tracing.EventSource> e os métodos `Write*` com seus próprios métodos de evento. Um padrão singleton geralmente é usado para cada <xref:System.Diagnostics.Tracing.EventSource>.  
  
 Por exemplo, a classe no exemplo a seguir pode ser usada para medir duas características de desempenho:  
  
- O tempo até que o construtor de classe `App` foi chamado.  
  
- O tempo até que o construtor `MainPage` foi chamado.  
  
 [!code-csharp[ProjectN_ETW#1](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_etw/cs/etw1.cs#1)]  
  
 Há algumas coisas a serem observadas aqui. Primeiro, um singleton é criado em `AppEventSource.Log`. Essa instância será usada para todos os logs. Em segundo lugar, cada método de evento possui um <xref:System.Diagnostics.Tracing.EventAttribute>. Isso ajuda as ferramentas associarem o índice do método <xref:System.Diagnostics.Tracing.EventSource.WriteEvent%2A> com o método que foi chamado em `AppEventSource`.  
  
 Observe que esses eventos são puramente ilustrativos. A maioria dos códigos de aplicativo serão executado após esses eventos. Você deve compreender quais eventos no código correspondem às interações do usuário, os medem melhoram os parâmetros de comparação. Além disso, os próprios eventos registram somente uma única instância no tempo. Muitas vezes é útil ter eventos de início e interrupção emparelhados para cada operação. Quando examinar a inicialização do aplicativo, o evento de inicialização é geralmente o evento de "Processo/Iniciar" que o sistema operacional emite.  
  
 Por exemplo, suponha que você está criando um leitor de RSS. Alguns locais interessantes para registrar um evento são:  
  
- Quando a página principal é processada pela primeira vez.  
  
- Quando os textos de RSS antigos são desserializados do armazenamento local.  
  
- Quando seu aplicativo começa a sincronização de novos textos.  
  
- Quando seu aplicativo concluir a sincronização dos textos novos.  
  
 A instrumentação de um aplicativo é simples: basta chamar o método apropriado na classe derivada. Usando o `AppEventSource` do exemplo anterior, você pode instrumentar um aplicativo da seguinte maneira:  
  
 [!code-csharp[ProjectN_ETW#2](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_etw/cs/etw2.cs#2)]  
  
 Quando o aplicativo for instrumentado, você estará pronto para coletar os eventos.  
  
## <a name="gathering-events-with-perfview"></a>Eventos de coleta com PerfView  
 O PerfView usa eventos ETW para ajudar você a executar todos os tipos de investigações de desempenho no seu aplicativo. Ele também inclui uma configuração de GUI que permite ativar ou desativar os diferentes tipos de registro. O PerfView é uma ferramenta gratuita e pode ser baixado do [Centro de Download da Microsoft](https://www.microsoft.com/download/details.aspx?id=28567). Para obter mais informações, assista aos [vídeos de tutorial do PerfView](https://channel9.msdn.com/Series/PerfView-Tutorial).  
  
> [!NOTE]
> O PerfView não pode ser usado para coletar os eventos nos sistemas ARM. Para coletar os eventos nos sistemas ARM, use o Windows Performance Recorder (WPR). Para obter mais informações, consulte a [postagem no blog de Vance Morrison](https://docs.microsoft.com/archive/blogs/vancem/collecting-etwperfview-data-on-an-windows-rt-winrt-arm-surface-device).  
  
 Você também pode invocar o PerfView da linha de comando. Para registrar somente os eventos do provedor, abra a janela do Prompt de comandos e digite o comando:  
  
```console
perfview -KernelEvents:Process -OnlyProviders:*MyCompany-MyApp collect outputFile
```  
  
 onde:  
  
 `-KernelEvents:Process`  
 Indica que você deseja saber quando o processo é iniciado e é interrompido. É necessário que o evento de início/processo para seu aplicativo para pode ser subtraído de outros tempos de eventos.  
  
 `-OnlyProviders:*MyCompany-MyApp`  
 Desativa todos os outros provedores que o PerfView ativa por padrão e ativa o provedor MyCompany-MyApp.  (O asterisco indica que é um <xref:System.Diagnostics.Tracing.EventSource>.)  
  
 `collect outputFile`  
 Indica que você deseja iniciar a coleta e salvar os dados para outputFile.etl.zip.  
  
 Execute o aplicativo após iniciar o PerfView. Há algumas coisas a lembrar quando ao executar seu aplicativo:  
  
- Use uma compilação de versão, não uma compilação de depuração. Compilações de depuração normalmente contêm códigos de verificação e manuseio de erros extras que pode fazer com que o aplicativo seja executado mais lentamente do que o esperado.  
  
- Executar seu aplicativo com o depurador anexado afetará o desempenho do aplicativo.  
  
- O Windows usa várias estratégias de caching em cache para agilizar os tempos de inicialização do aplicativo. Se seu aplicativo estiver armazenado em cache na memória e não precisar ser carregado do disco, ele será iniciado mais rapidamente. Para garantir a consistência, inicie e feche seu aplicativo algumas vezes antes de medi-lo.  
  
 Ao executar o aplicativo para que o PerfView possa coletar eventos emitidos, escolha o botão **Parar Coleta**. Em geral, você deve interromper a coleta antes de fechar o aplicativo para que não ocorram eventos estranhos. No entanto, se você medir o desempenho de desligamento ou suspensão, é recomendável continuar a coleta.  
  
## <a name="displaying-the-events"></a>Exibir os eventos  
 Para exibir os eventos que já foram coletados, use o PerfView para abrir o arquivo .etl ou .etl.zip criado e escolha **Eventos**. O ETW irá coletar informações sobre um grande número de eventos, incluindo eventos de outros processos. Para restringir sua investigação, preencha as seguintes caixas de texto no modo de exibição de eventos:  
  
- Na caixa **Filtro do Processo**, especifique o nome do aplicativo (sem ".exe").  
  
- Na caixa **Filtro de Tipos de Evento**, especifique `Process/Start | MyCompany-MyApp`. Isso define um filtro para eventos de MyApp-MyCompany e o evento Windows Kernel/Processo/Início.  
  
 Selecione todos os eventos listados no painel à esquerda (Ctrl-A) e pressione a tecla **Enter**. Agora, você poderá ver as marcações de tempo de cada evento. Essas marcações de tempo são relativas ao início do rastreamento, portanto você precisa subtrair o horário de cada evento da hora de início do processo para identificar o tempo decorrido desde a inicialização. Se você usar Ctrl+Clique para selecionar duas marcações de tempo, verá que a diferença entre eles é exibida na barra de status na parte inferior da página. Isso facilita ver o tempo decorrido entre dois eventos em exibição (incluindo o início do processo). Você pode abrir o menu de atalho para o modo de exibição e selecionar diversas opções úteis, como exportar para arquivos CSV ou abrir o Microsoft Excel para salvar ou processar os dados.  
  
 Ao repetir o procedimento para seu aplicativo original e a versão que você criou usando a cadeia de ferramentas .NET Native, você pode comparar a diferença no desempenho.   .NET Native aplicativos geralmente são iniciados mais rapidamente do que os aplicativos nativos do non-.NET. Se você estiver interessado em se aprofundar-se, o PerfView também pode identificar as partes do código que estão levando mais tempo. Para obter mais informações, assista aos [tutoriais do PerfView](https://channel9.msdn.com/Series/PerfView-Tutorial) ou leia a [entrada no blog de Vance Morrison](https://docs.microsoft.com/archive/blogs/vancem/publication-of-the-perfview-performance-analysis-tool).  
  
## <a name="see-also"></a>Confira também

- <xref:System.Diagnostics.Tracing.EventSource>
