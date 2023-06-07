---
title: filtrar a saída de My.Application.Log
ms.date: 07/20/2015
helpviewer_keywords:
- My.Log object, filtering output
- My.Application.Log object, filtering output
- application event logs, output filtering
ms.assetid: 2c0a457a-38a4-49e1-934d-a51320b7b4ca
ms.openlocfilehash: aa63e7d23641ad71b135f15236e29399a535784f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84398247"
---
# <a name="walkthrough-filtering-myapplicationlog-output-visual-basic"></a>Instruções passo a passo: filtrando a saída de My.Application.Log (Visual Basic)

Este passo a passo demonstra como alterar a filtragem de log padrão do objeto `My.Application.Log` para controlar quais informações são passadas do objeto `Log` para os ouvintes e quais informações são gravadas pelos ouvintes. Você pode alterar o comportamento de registro em log mesmo após ter compilado o aplicativo, porque as informações de configuração são armazenadas no arquivo de configuração do aplicativo.

## <a name="getting-started"></a>Introdução

Cada mensagem que `My.Application.Log` grava tem um nível de gravidade associado, que os mecanismos de filtragem usam para controlar a saída de log. Este aplicativo de exemplo usa métodos `My.Application.Log` para gravar diversas mensagens de log com diferentes níveis de gravidade.

#### <a name="to-build-the-sample-application"></a>Para compilar o aplicativo de exemplo

1. Abra um novo projeto de aplicativo do Windows Visual Basic.

2. Adicione um botão denominado Button1 a Form1.

3. No manipulador de eventos <xref:System.Windows.Forms.Control.Click> de Button1, adicione o seguinte código:

     [!code-vb[VbVbcnMyApplicationLogFiltering#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyApplicationLogFiltering/VB/Form1.vb#1)]

4. Execute o aplicativo no depurador.

5. Pressione **Button1**.

     O aplicativo grava as informações a seguir no arquivo de log e na saída da depuração do aplicativo.

     `DefaultSource Information: 0 : In Button1_Click`

     `DefaultSource Error: 2 : Error in the application.`

6. Feche o aplicativo.

     Para obter informações sobre como exibir a janela de saída de depuração do aplicativo, consulte [Janela de Saída](/visualstudio/ide/reference/output-window). Para obter informações sobre o local do arquivo de log do aplicativo, consulte [Instruções passo a passo: determinando onde My.Application.Log grava informações](walkthrough-determining-where-my-application-log-writes-information.md).

    > [!NOTE]
    > Por padrão, o aplicativo libera a saída do arquivo de log quando é fechado.

     No exemplo acima, a segunda chamada para o método <xref:Microsoft.VisualBasic.Logging.Log.WriteEntry%2A> e a chamada para o método <xref:Microsoft.VisualBasic.Logging.Log.WriteException%2A> produzem a saída de log, enquanto as primeira e última chamadas para o método `WriteEntry` não. Isso ocorre porque os níveis de gravidade de `WriteEntry` e `WriteException` são "Information" e "Error", que são permitidos pela filtragem de log padrão do objeto `My.Application.Log`. No entanto, os eventos com os níveis de gravidade "Start" e "Stop" são impedidos de produzir a saída de log.

## <a name="filtering-for-all-myapplicationlog-listeners"></a>Filtragem de ouvintes de My.Application.Log

O objeto `My.Application.Log` usa um <xref:System.Diagnostics.SourceSwitch> chamado `DefaultSwitch` para controlar quais mensagens ele passa dos métodos `WriteEntry` e `WriteException` para os ouvintes de log. Você pode configurar `DefaultSwitch` no arquivo de configuração de aplicativo definindo seu valor como um dos valores de enumeração <xref:System.Diagnostics.SourceLevels>. Por padrão, seu valor é “Information".

Esta tabela mostra o nível de gravidade necessário para o log gravar uma mensagem nos ouvintes, dada uma determinada configuração `DefaultSwitch`.

|Valor DefaultSwitch|Gravidade da mensagem necessária para a saída|
|---|---|
|`Critical`|`Critical`|
|`Error`|`Critical` ou `Error`|
|`Warning`|`Critical`, `Error` ou `Warning`|
|`Information`|`Critical`, `Error`, `Warning` ou `Information`|
|`Verbose`|`Critical`, `Error`, `Warning`, `Information` ou `Verbose`|
|`ActivityTracing`|`Start`, `Stop`, `Suspend`, `Resume` ou `Transfer`|
|`All`|Todas as mensagens são permitidas.|
|`Off`|Todas as mensagens são bloqueadas.|

> [!NOTE]
> Os métodos `WriteEntry` e `WriteException` têm uma sobrecarga que não especifica um nível de gravidade. O nível de gravidade implícito para a sobrecarga `WriteEntry` é "Information" e o nível de gravidade implícito para a sobrecarga `WriteException` é "Error".

Esta tabela explica a saída de log mostrada no exemplo anterior: com a configuração de `DefaultSwitch` padrão de "Information", apenas a segunda chamada para o método `WriteEntry` e a chamada para o método `WriteException` produzem a saída de log.

#### <a name="to-log-only-activity-tracing-events"></a>Para registrar em log apenas eventos de rastreamento de atividade

1. Clique com o botão direito do mouse em app.config no **Gerenciador de Soluções** e selecione **Abrir**.

     -ou-

     Se não houver nenhum arquivo app.config:

    1. No menu **Projeto**, escolha **Adicionar Novo Item**.

    2. Na caixa de diálogo **Adicionar novo item**, escolha **Arquivo de configuração de aplicativo**.

    3. Clique em **Adicionar**.

2. Localize a seção `<switches>`, que está na seção `<system.diagnostics>`, que está na seção `<configuration>` superior.

3. Localize o elemento que adiciona `DefaultSwitch` à coleção de opções. Ele deve ser semelhante a este:

     `<add name="DefaultSwitch" value="Information" />`

4. Altere o valor do atributo `value` para "ActivityTracing".

5. O conteúdo do arquivo app.config deve ser semelhante ao XML a seguir:

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <system.diagnostics>
        <sources>
          <!-- This section configures My.Application.Log -->
          <source name="DefaultSource" switchName="DefaultSwitch">
            <listeners>
              <add name="FileLog"/>
            </listeners>
          </source>
        </sources>
        <switches>
          <add name="DefaultSwitch" value="ActivityTracing" />
        </switches>
        <sharedListeners>
          <add name="FileLog"
               type="Microsoft.VisualBasic.Logging.FileLogTraceListener,
                     Microsoft.VisualBasic, Version=8.0.0.0,
                     Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a,
                     processorArchitecture=MSIL"
               initializeData="FileLogWriter"/>
        </sharedListeners>
      </system.diagnostics>
    </configuration>
    ```

6. Execute o aplicativo no depurador.

7. Pressione **Button1**.

     O aplicativo grava as informações a seguir no arquivo de log e na saída da depuração do aplicativo:

     `DefaultSource Start: 4 : Entering Button1_Click`

     `DefaultSource Stop: 5 : Leaving Button1_Click`

8. Feche o aplicativo.

9. Altere o valor do atributo `value` de volta para "Information".

    > [!NOTE]
    > A configuração da opção `DefaultSwitch` controla apenas `My.Application.Log`. Ele não altera como as classes <xref:System.Diagnostics.Trace?displayProperty=nameWithType> e <xref:System.Diagnostics.Debug?displayProperty=nameWithType> do .NET Framework se comportam.

## <a name="individual-filtering-for-myapplicationlog-listeners"></a>Filtragem individual de ouvintes de My.Application.Log

O exemplo anterior mostra como alterar a filtragem para toda a saída `My.Application.Log`. Este exemplo demonstra como filtrar um ouvinte de log individual. Por padrão, um aplicativo tem dois ouvintes que gravam na saída de depuração do aplicativo e no arquivo de log.

O arquivo de configuração controla o comportamento dos ouvintes de log, permitindo que cada um deles tenha um filtro, que é semelhante a uma opção para `My.Application.Log`. Um ouvinte de log produzirá uma mensagem somente se a gravidade da mensagem for permitida pelo `DefaultSwitch` do log e pelo filtro do ouvinte de log.

Este exemplo demonstra como configurar a filtragem para um novo ouvinte de depuração e adicioná-lo ao objeto `Log`. O ouvinte de depuração padrão deve ser removido do objeto `Log`, de modo que seja claro que as mensagens de depuração são provenientes do novo ouvinte de depuração.

#### <a name="to-log-only-activity-tracing-events"></a>Para registrar em log apenas eventos de rastreamento de atividade

1. Clique com o botão direito do mouse em app.config no **Gerenciador de Soluções** e escolha **Abrir**.

     \-ou-

     Se não houver nenhum arquivo app.config:

    1. No menu **Projeto**, escolha **Adicionar Novo Item**.

    2. Na caixa de diálogo **Adicionar novo item**, escolha **Arquivo de configuração de aplicativo**.

    3. Clique em **Adicionar**.

2. Clique com o botão direito do mouse em app.config no **Gerenciador de Soluções**. Escolha **Abrir**.

3. Localize a seção `<listeners>`, na seção `<source>` com o atributo de `name` "DefaultSource", que está na seção `<sources>`. A seção `<sources>` está na seção `<system.diagnostics>`, na seção `<configuration>` superior.

4. Adicione esse elemento à seção `<listeners>`:

    ```xml
    <!-- Remove the default debug listener. -->
    <remove name="Default"/>
    <!-- Add a filterable debug listener. -->
    <add name="NewDefault"/>
    ```

5. Localize a seção `<sharedListeners>`, na seção `<system.diagnostics>`, na seção `<configuration>` superior.

6. Adicione esse elemento a essa seção `<sharedListeners>`:

    ```xml
    <add name="NewDefault"
         type="System.Diagnostics.DefaultTraceListener,
               System, Version=2.0.0.0, Culture=neutral,
               PublicKeyToken=b77a5c561934e089,
               processorArchitecture=MSIL">
        <filter type="System.Diagnostics.EventTypeFilter"
                initializeData="Error" />
    </add>
    ```

     O filtro <xref:System.Diagnostics.EventTypeFilter> escolhe um dos valores de enumeração <xref:System.Diagnostics.SourceLevels> como seu atributo `initializeData`.

7. O conteúdo do arquivo app.config deve ser semelhante ao XML a seguir:

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <system.diagnostics>
        <sources>
          <!-- This section configures My.Application.Log -->
          <source name="DefaultSource" switchName="DefaultSwitch">
            <listeners>
              <add name="FileLog"/>
              <!-- Remove the default debug listener. -->
              <remove name="Default"/>
              <!-- Add a filterable debug listener. -->
              <add name="NewDefault"/>
            </listeners>
          </source>
        </sources>
        <switches>
          <add name="DefaultSwitch" value="Information" />
        </switches>
        <sharedListeners>
          <add name="FileLog"
               type="Microsoft.VisualBasic.Logging.FileLogTraceListener,
                     Microsoft.VisualBasic, Version=8.0.0.0,
                     Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a,
                     processorArchitecture=MSIL"
               initializeData="FileLogWriter"/>
          <add name="NewDefault"
               type="System.Diagnostics.DefaultTraceListener,
                     System, Version=2.0.0.0, Culture=neutral,
                     PublicKeyToken=b77a5c561934e089,
                     processorArchitecture=MSIL">
            <filter type="System.Diagnostics.EventTypeFilter"
                    initializeData="Error" />
          </add>
        </sharedListeners>
      </system.diagnostics>
    </configuration>
    ```

8. Execute o aplicativo no depurador.

9. Pressione **Button1**.

     O aplicativo grava as informações a seguir no arquivo de log do aplicativo:

     `Default Information: 0 : In Button1_Click`

     `Default Error: 2 : Error in the application.`

     O aplicativo grava menos informações na saída da depuração do aplicativo devido à filtragem mais restritiva.

     `Default Error   2   Error`

10. Feche o aplicativo.

Para obter mais informações sobre como alterar as configurações de log após a implantação, consulte [Trabalhando com logs de aplicativo](working-with-application-logs.md).

## <a name="see-also"></a>Confira também

- [Passo a passo: determinar o local no qual My.Application.Log grava informações](walkthrough-determining-where-my-application-log-writes-information.md)
- [Passo a passo: alterar o local no qual My.Application.Log grava informações](walkthrough-changing-where-my-application-log-writes-information.md)
- [Passo a passo: criar ouvintes de log personalizados](walkthrough-creating-custom-log-listeners.md)
- [Como: gravar mensagens de log](how-to-write-log-messages.md)
- [Opções de rastreamento](../../../../framework/debug-trace-profile/trace-switches.md)
- [Registrando informações em log a partir do aplicativo](index.md)
