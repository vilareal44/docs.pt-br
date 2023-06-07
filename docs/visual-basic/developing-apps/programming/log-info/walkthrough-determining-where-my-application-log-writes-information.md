---
title: determinar o local no qual My.Application.Log grava informações
ms.date: 07/20/2015
helpviewer_keywords:
- My.Log object, output location
- output, application log location
- My.Application.Log object, output location
- event logs, determining output location
- application event logs, output location
- applications [Visual Basic], output location
ms.assetid: 5b70143a-7741-45f2-ae1d-03324a3a4189
ms.openlocfilehash: 00b543dbe96ca99446f6797a13b66ee62c422b93
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84398273"
---
# <a name="walkthrough-determining-where-myapplicationlog-writes-information-visual-basic"></a>Instruções passo a passo: determinando onde My.Application.Log grava informações (Visual Basic)

O objeto `My.Application.Log` pode gravar informações em vários ouvintes de log. Os ouvintes de log são configurados pelo arquivo de configuração do computador e podem ser substituídos pelo arquivo de configuração do aplicativo. Este tópico descreve as configurações padrão e como determinar as configurações para seu aplicativo.

Para obter mais informações sobre os locais de saída padrão, consulte [Trabalhando com logs de aplicativo](working-with-application-logs.md).

### <a name="to-determine-the-listeners-for-myapplicationlog"></a>Para determinar os ouvintes de My.Application.Log

1. Localize o arquivo de configuração do assembly. Se você estiver desenvolvendo o assembly, poderá acessar o app.config em Visual Studio no **Gerenciador de Soluções**. Caso contrário, o nome do arquivo de configuração é o nome do assembly acrescentado com ".config" e ele está localizado no mesmo diretório do assembly.

    > [!NOTE]
    > Nem todo assembly tem um arquivo de configuração.

    O arquivo de configuração é um arquivo XML.

2. Localize a seção `<listeners>`, na seção `<source>` com o `name` atributo "DefaultSource", localizado na seção `<sources>`. A seção `<sources>` está localizada na seção `<system.diagnostics>`, na seção `<configuration>` superior.

    Se estas seções não existirem, o arquivo de configuração do computador poderá configurar os ouvintes de log `My.Application.Log`. As etapas a seguir descrevem como determinar o que define o arquivo de configuração do computador:

    1. Localize o arquivo machine.config do computador. Normalmente, ele está localizado no diretório *SystemRoot\Microsoft.NET\Framework\frameworkVersion\CONFIG*, em que `SystemRoot` é o diretório do sistema operacional e `frameworkVersion` é a versão do .NET Framework.

        As configurações em machine.config podem ser substituídas por um arquivo de configuração de aplicativo.

        Se os elementos opcionais listados abaixo não existirem, você poderá criá-los.

    2. Localize a seção `<listeners>`, na seção `<source>` com o atributo `name` "DefaultSource", na seção `<sources>`, na seção `<system.diagnostics>`, na seção superior `<configuration>`.

        Se estas seções não existirem, o `My.Application.Log` terá apenas os ouvintes de log padrão.

3. Localize os elementos <`add>` na seção <`listeners>`.

     Esses elementos adicionam os ouvintes de log nomeados à origem `My.Application.Log`.

4. Localize os elementos `<add>` com os nomes dos ouvintes de log na seção `<sharedListeners>`, na seção `<system.diagnostics>`, na seção superior `<configuration>`.

5. Para muitos tipos de ouvintes compartilhados, os dados de inicialização do ouvinte incluem uma descrição de onde o ouvinte direciona os dados:

    - Um ouvinte <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener?displayProperty=nameWithType> grava em um arquivo de log, conforme descrito na introdução.

    - O ouvinte <xref:System.Diagnostics.EventLogTraceListener?displayProperty=nameWithType> grava informações no log de eventos do computador especificado pelo parâmetro `initializeData`. Para exibir um log de eventos, é possível usar o **Gerenciador de Servidores** ou o **Visualizador de Eventos do Windows**. Para obter mais informações, consulte [Eventos ETW no .NET Framework](../../../../framework/performance/etw-events.md).

    - Os ouvintes <xref:System.Diagnostics.DelimitedListTraceListener?displayProperty=nameWithType> e <xref:System.Diagnostics.XmlWriterTraceListener?displayProperty=nameWithType> gravam no arquivo especificado no parâmetro `initializeData`.

    - Um ouvinte <xref:System.Diagnostics.ConsoleTraceListener?displayProperty=nameWithType> grava no console da linha de comando.

    - Para obter informações sobre onde outros tipos de ouvintes de log gravam informações, consulte a documentação daquele tipo.

## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Logging.Log?displayProperty=nameWithType>
- <xref:System.Diagnostics.DefaultTraceListener>
- <xref:System.Diagnostics.EventLogTraceListener>
- <xref:System.Diagnostics.DelimitedListTraceListener>
- <xref:System.Diagnostics.XmlWriterTraceListener>
- <xref:System.Diagnostics.ConsoleTraceListener>
- <xref:System.Diagnostics>
- [Trabalhar com logs do aplicativo](working-with-application-logs.md)
- [Como: registrar exceções em log](how-to-log-exceptions.md)
- [Como: gravar mensagens de log](how-to-write-log-messages.md)
- [Passo a passo: alterar o local no qual My.Application.Log grava informações](walkthrough-changing-where-my-application-log-writes-information.md)
- [Eventos ETW no .NET Framework](../../../../framework/performance/etw-events.md)
- [Solução de problemas: ouvintes de log](troubleshooting-log-listeners.md)
