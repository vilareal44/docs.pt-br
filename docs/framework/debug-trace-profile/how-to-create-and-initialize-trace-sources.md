---
title: Como criar e inicializar fontes de rastreamento
description: Crie e inicialize fontes de rastreamento usando a classe de rastreamento no .NET. Essa classe fornece métodos para rastreamento de eventos e dados e emissão de rastreamentos informativos.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- trace sources
- initializing trace sources
- configuration files [.NET Framework], trace sources
ms.assetid: f88dda6f-5fda-45be-9b3c-745a9b708c4d
ms.openlocfilehash: 55d7854bff991ba185d3f5d6e4c6e7222c9e3039
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051266"
---
# <a name="how-to-create-and-initialize-trace-sources"></a>Como criar e inicializar fontes de rastreamento
A classe <xref:System.Diagnostics.TraceSource> é usada por aplicativos para produzir rastreamentos que podem ser associados ao aplicativo. <xref:System.Diagnostics.TraceSource> fornece métodos de rastreamento que permitem rastrear eventos com facilidade, rastrear dados e emitir rastreamentos informativos. A saída de rastreamento de <xref:System.Diagnostics.TraceSource> pode ser criada e inicializada com ou sem o uso de arquivos de configuração. Este tópico fornece instruções para ambas as opções. No entanto, recomendamos o uso de arquivos de configuração para facilitar a reconfiguração dos rastreamentos produzidos por origens de rastreamento em tempo de execução.  
  
### <a name="to-create-and-initialize-a-trace-source-using-a-configuration-file"></a>Para criar e inicializar uma origem de rastreamento usando um arquivo de configuração  
  
1. Crie um projeto de aplicativo de console do Visual Studio (.NET Framework) e substitua o código fornecido pelo código a seguir. Esse código registra erros e avisos e gera alguns deles no console e outros no arquivo myListener que é criado pelas entradas no arquivo de configuração.  
  
     [!code-csharp[TraceSourceExample1#1](../../../samples/snippets/csharp/VS_Snippets_CLR/tracesourceexample1/cs/program.cs#1)]
     [!code-vb[TraceSourceExample1#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/tracesourceexample1/vb/program.vb#1)]  
  
2. Adicione um arquivo de configuração de aplicativo, caso um não esteja presente, ao projeto para inicializar a origem de rastreamento chamada `TraceSourceApp` no exemplo de código da etapa 1.  
  
3. Substitua o conteúdo do arquivo de configuração padrão pelas configurações a seguir para inicializar um ouvinte de rastreamento do console e um ouvinte de rastreamento do text writer na origem de rastreamento que foi criada na etapa 1.  
  
    ```xml  
    <configuration>  
      <system.diagnostics>  
        <sources>  
          <source name="TraceSourceApp"
            switchName="sourceSwitch"
            switchType="System.Diagnostics.SourceSwitch">  
            <listeners>  
              <add name="console"
                type="System.Diagnostics.ConsoleTraceListener">  
                <filter type="System.Diagnostics.EventTypeFilter"
                  initializeData="Error"/>  
              </add>  
              <add name="myListener"/>  
              <remove name="Default"/>  
            </listeners>  
          </source>  
        </sources>  
        <switches>  
          <add name="sourceSwitch" value="Error"/>  
        </switches>  
        <sharedListeners>  
          <add name="myListener"
            type="System.Diagnostics.TextWriterTraceListener"
            initializeData="myListener.log">  
            <filter type="System.Diagnostics.EventTypeFilter"
              initializeData="Error"/>  
          </add>  
        </sharedListeners>  
      </system.diagnostics>  
    </configuration>  
    ```  
  
     Além de configurar os ouvintes de rastreamento, o arquivo de configuração cria filtros para ambos os ouvintes e cria uma opção de origem para a origem de rastreamento. Duas técnicas são mostradas para a adição de ouvintes de rastreamento: adicionar o ouvinte diretamente à origem de rastreamento e adicionar um ouvinte à coleção de ouvintes compartilhados e, em seguida, adicioná-lo pelo nome à origem de rastreamento. Os filtros identificados para os dois ouvintes são inicializados com níveis de origem diferentes. Isso resulta na gravação de algumas mensagens por apenas um dos dois ouvintes.  
  
     O arquivo de configuração inicializa as configurações para a origem de rastreamento no momento em que o aplicativo é inicializado. O aplicativo pode alterar dinamicamente as propriedades definidas pelo arquivo de configuração para substituir as configurações especificadas pelo usuário. Por exemplo, talvez você deseje garantir que as mensagens críticas sejam sempre enviadas para um arquivo de texto, independentemente das definições de configuração atuais. O exemplo de código demonstra como substituir as configurações do arquivo de configuração para garantir que as mensagens críticas sejam geradas para os ouvintes de rastreamento.  
  
     A alteração das configurações do arquivo de configuração durante a execução do aplicativo não altera as configurações iniciais. Para alterar as configurações, reinicie o aplicativo ou atualize o aplicativo de forma programática usando o método <xref:System.Diagnostics.Trace.Refresh%2A?displayProperty=nameWithType>.  
  
### <a name="to-initialize-trace-sources-listeners-and-filters-without-a-configuration-file"></a>Para inicializar origens de rastreamento, ouvintes e filtros sem um arquivo de configuração  
  
- Use o exemplo de código a seguir para habilitar o rastreamento por meio de uma origem de rastreamento sem usar um arquivo de configuração. Essa não é uma prática recomendada, mas pode haver circunstâncias em que você não deseja depender dos arquivos de configuração para garantir o rastreamento.  
  
     [!code-csharp[TraceSourceExample2#1](../../../samples/snippets/csharp/VS_Snippets_CLR/tracesourceexample2/cs/program.cs#1)]
     [!code-vb[TraceSourceExample2#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/tracesourceexample2/vb/program.vb#1)]  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Diagnostics.TraceSource>
- <xref:System.Diagnostics.TextWriterTraceListener>
- <xref:System.Diagnostics.ConsoleTraceListener>
- <xref:System.Diagnostics.EventTypeFilter>
- [Rastreamento e instrumentação de aplicativos](tracing-and-instrumenting-applications.md)
