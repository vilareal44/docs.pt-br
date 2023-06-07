---
title: alterar o local no qual My.Application.Log grava informações
ms.date: 07/20/2015
helpviewer_keywords:
- My.Application.Log object, walkthroughs
- event logs, changing output location
ms.assetid: ecc74f95-743c-450d-93f6-09a30db0fe4a
ms.openlocfilehash: f9e45cdf4507840f62e32678f4c0a7be2c0be054
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84398286"
---
# <a name="walkthrough-changing-where-myapplicationlog-writes-information-visual-basic"></a>Instruções passo a passo: alterando onde My.Application.Log grava informações (Visual Basic)

É possível usar os objetos `My.Application.Log` e `My.Log` para registrar em log as informações sobre eventos que ocorrem em seu aplicativo. Este passo a passo mostra como substituir as configurações padrão e fazer com que o objeto `Log` grave em outros ouvintes de log.

## <a name="prerequisites"></a>Pré-requisitos

O objeto `Log` pode gravar informações em vários ouvintes de log. Você precisa determinar a configuração atual dos ouvintes de log antes de alterar as configurações. Para obter mais informações, consulte [Instruções passo a passo: Determinando onde My.Application.Log grava informações](walkthrough-determining-where-my-application-log-writes-information.md).

Talvez você queira examinar [Como gravar informações de evento em um arquivo de texto](how-to-write-event-information-to-a-text-file.md) ou [Como gravar em um log de eventos do aplicativo](how-to-write-to-an-application-event-log.md).

### <a name="to-add-listeners"></a>Para adicionar ouvintes

1. Clique com o botão direito do mouse em app.config no **Gerenciador de Soluções** e escolha **Abrir**.

     \- ou –

     Se não houver nenhum arquivo app.config:

    1. No menu **Projeto**, escolha **Adicionar Novo Item**.

    2. Na caixa de diálogo **Adicionar Novo Item**, selecione **Arquivo de Configuração de Aplicativo**.

    3. Clique em **Adicionar**.

2. Localize a seção `<listeners>`, na seção `<source>` com o `name` atributo "DefaultSource", na seção `<sources>`. A seção `<sources>` está na seção `<system.diagnostics>`, na seção `<configuration>` superior.

3. Adicione esses elementos a essa seção `<listeners>`.

    ```xml
    <!-- Uncomment to connect the application file log. -->
    <!-- <add name="FileLog" /> -->
    <!-- Uncomment to connect the event log. -->
    <!-- <add name="EventLog" /> -->
    <!-- Uncomment to connect the event log. -->
    <!-- <add name="Delimited" /> -->
    <!-- Uncomment to connect the XML log. -->
    <!-- <add name="XmlWriter" /> -->
    <!-- Uncomment to connect the console log. -->
    <!-- <add name="Console" /> -->
    ```

4. Remova a marca de comentário dos ouvintes de log que você deseja receber mensagens de `Log`.

5. Localize a seção `<sharedListeners>`, na seção `<system.diagnostics>`, na seção `<configuration>` superior.

6. Adicione esses elementos a essa seção `<sharedListeners>`.

    ```xml
    <add name="FileLog"
         type="Microsoft.VisualBasic.Logging.FileLogTraceListener,
               Microsoft.VisualBasic, Version=8.0.0.0,
               Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a"
         initializeData="FileLogWriter" />
    <add name="EventLog"
         type="System.Diagnostics.EventLogTraceListener,
               System, Version=2.0.0.0,
               Culture=neutral, PublicKeyToken=b77a5c561934e089"
         initializeData="sample application"/>
    <add name="Delimited"
         type="System.Diagnostics.DelimitedListTraceListener,
               System, Version=2.0.0.0,
               Culture=neutral, PublicKeyToken=b77a5c561934e089"
         initializeData="c:\temp\sampleDelimitedFile.txt"
         traceOutputOptions="DateTime" />
    <add name="XmlWriter"
         type="System.Diagnostics.XmlWriterTraceListener,
               System, Version=2.0.0.0,
               Culture=neutral, PublicKeyToken=b77a5c561934e089"
         initializeData="c:\temp\sampleLogFile.xml" />
    <add name="Console"
         type="System.Diagnostics.ConsoleTraceListener,
               System, Version=2.0.0.0,
               Culture=neutral, PublicKeyToken=b77a5c561934e089"
         initializeData="true" />
    ```

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
              <!-- Uncomment to connect the application file log. -->
              <!-- <add name="FileLog" /> -->
              <!-- Uncomment to connect the event log. -->
              <!-- <add name="EventLog" /> -->
              <!-- Uncomment to connect the event log. -->
              <!-- <add name="Delimited" /> -->
              <!-- Uncomment to connect the XML log. -->
              <!-- <add name="XmlWriter" /> -->
              <!-- Uncomment to connect the console log. -->
              <!-- <add name="Console" /> -->
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
                     Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a"
               initializeData="FileLogWriter" />
          <add name="EventLog"
               type="System.Diagnostics.EventLogTraceListener,
                     System, Version=2.0.0.0,
                     Culture=neutral, PublicKeyToken=b77a5c561934e089"
               initializeData="sample application"/>
          <add name="Delimited"
               type="System.Diagnostics.DelimitedListTraceListener,
                     System, Version=2.0.0.0,
                     Culture=neutral, PublicKeyToken=b77a5c561934e089"
               initializeData="c:\temp\sampleDelimitedFile.txt"
               traceOutputOptions="DateTime" />
          <add name="XmlWriter"
               type="System.Diagnostics.XmlWriterTraceListener,
                     System, Version=2.0.0.0,
                     Culture=neutral, PublicKeyToken=b77a5c561934e089"
               initializeData="c:\temp\sampleLogFile.xml" />
          <add name="Console"
               type="System.Diagnostics.ConsoleTraceListener,
                     System, Version=2.0.0.0,
                     Culture=neutral, PublicKeyToken=b77a5c561934e089"
               initializeData="true" />
        </sharedListeners>
      </system.diagnostics>
    </configuration>
    ```

### <a name="to-reconfigure-a-listener"></a>Para reconfigurar um ouvinte

1. Localize o elemento `<add>` do ouvinte na seção `<sharedListeners>`.

2. O atributo `type` fornece o nome do tipo de ouvinte. Esse tipo deve herdar da classe <xref:System.Diagnostics.TraceListener>. Use o nome de tipo de nome forte para garantir que o tipo correto seja usado. Para obter mais informações, consulte a seção “Para fazer referência a um tipo de nome forte”.

     Alguns tipos que você pode usar são:

    - Um ouvinte <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener?displayProperty=nameWithType>, que grava em um arquivo de log.

    - O ouvinte <xref:System.Diagnostics.EventLogTraceListener?displayProperty=nameWithType> que grava informações no log de eventos do computador especificado pelo parâmetro `initializeData`.

    - Os ouvintes <xref:System.Diagnostics.DelimitedListTraceListener?displayProperty=nameWithType> e <xref:System.Diagnostics.XmlWriterTraceListener?displayProperty=nameWithType>, que gravam no arquivo especificado no parâmetro `initializeData`.

    - Um ouvinte <xref:System.Diagnostics.ConsoleTraceListener?displayProperty=nameWithType>, que grava no console da linha de comando.

     Para obter informações sobre onde outros tipos de ouvintes de log gravam informações, consulte a documentação daquele tipo.

3. Quando o aplicativo cria o objeto de ouvinte de log, ele passa o atributo `initializeData` como o parâmetro de construtor. O significado do atributo `initializeData` depende o ouvinte de rastreamento.

4. Depois de criar o ouvinte de log, o aplicativo define as propriedades do ouvinte. Essas propriedades são definidas pelos outros atributos no elemento `<add>`. Para obter mais informações sobre as propriedades de um ouvinte específico, consulte a documentação daquele tipo de ouvinte.

### <a name="to-reference-a-strongly-named-type"></a>Para fazer referência a um tipo de nome forte

1. Para garantir que o tipo correto seja usado para seu ouvinte de log, certifique-se de usar o nome de tipo totalmente qualificado e o nome do assembly de nome forte. A sintaxe de um tipo de nome forte é a seguinte:

     \<*type name*>, \<*assembly name*>, \<*version number*>, \<*culture*>, \<*strong name*>

2. Este exemplo de código mostra como determinar o nome do tipo do nome forte para um tipo totalmente qualificado —"System.Diagnostics.FileLogTraceListener" nesse caso.

     [!code-vb[VbVbalrMyApplicationLog#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyApplicationLog/VB/Form1.vb#15)]

     Esta é a saída e ela pode ser usada para fazer referência de forma única a um tipo de nome forte, como no procedimento "Para adicionar ouvintes" acima.

     `Microsoft.VisualBasic.Logging.FileLogTraceListener, Microsoft.VisualBasic, Version=8.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a`

## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Logging.Log?displayProperty=nameWithType>
- <xref:System.Diagnostics.TraceListener>
- <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener?displayProperty=nameWithType>
- <xref:System.Diagnostics.EventLogTraceListener?displayProperty=nameWithType>
- [Como: gravar informações de evento em um arquivo de texto](how-to-write-event-information-to-a-text-file.md)
- [Como: gravar em um Log de Eventos do Aplicativo](how-to-write-to-an-application-event-log.md)
