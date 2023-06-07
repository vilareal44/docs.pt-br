---
title: Registrando em log com o SDK do Azure para .NET
description: Saiba como habilitar o registro em log com o SDK do Azure para bibliotecas de cliente .NET
ms.date: 03/20/2020
ms.custom: azure-sdk-dotnet
ms.author: casoper
author: camsoper
ms.openlocfilehash: 0b255713bc9c13e0cbdaeb25a3d0fe46e91e815d
ms.sourcegitcommit: 3492dafceb5d4183b6b0d2f3bdf4a1abc4d5ed8c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/16/2020
ms.locfileid: "86416026"
---
# <a name="logging-with-the-azure-sdk-for-net"></a>Registrando em log com o SDK do Azure para .NET

As bibliotecas de cliente do [SDK do Azure](https://azure.microsoft.com/downloads/) para .net incluem a capacidade de registrar as operações da biblioteca de cliente. Isso permite que você monitore as solicitações e respostas de e/s que as bibliotecas de cliente estão fazendo para os serviços do Azure. Normalmente, os logs são usados para depurar ou diagnosticar problemas de comunicação. Este artigo descreve três abordagens para habilitar o registro em log com o SDK do Azure para .NET:

- Registrar na janela do console
- Log para rastreamentos de diagnóstico do .NET
- Configurar log personalizado

> [!IMPORTANT]
> Este artigo se aplica a bibliotecas de cliente que usam as versões mais recentes do SDK do Azure para .NET. Para ver se há suporte para uma biblioteca, consulte a lista de [versões mais recentes do SDK do Azure](https://azure.github.io/azure-sdk/releases/latest/index.html). Se seu aplicativo estiver usando uma versão mais antiga das bibliotecas de cliente do SDK do Azure, consulte as instruções específicas na documentação do serviço aplicável.

## <a name="log-information"></a>Informações do log

O SDK registra as informações a seguir, corrigindo a consulta de parâmetro e os valores de cabeçalho para remover dados pessoais.

Entrada do log de solicitação HTTP:

- ID Exclusiva
- Método HTTP
- URI
- Cabeçalhos de solicitação de saída

Entrada do log de resposta HTTP:

- Duração da operação de e/s (tempo decorrido)
- ID da Solicitação
- Código de status HTTP
- Frase de motivo HTTP
- Cabeçalhos de resposta
- Informações de erro, quando aplicável

Para conteúdo de solicitação e resposta:

- Fluxo de conteúdo como texto ou bytes, dependendo do cabeçalho Content-Type.
     > [! Observação} o log de conteúdo está desabilitado por padrão. Para habilitá-lo, defina `Diagnostics.IsLoggingContentEnabled` como `true` em `ClientOptions` .

Os logs de eventos são gerados normalmente em um desses três níveis:

- Informações para eventos de solicitação e resposta
- Aviso de erros
- Detalhado para mensagens detalhadas e log de conteúdo

## <a name="enable-logging-with-built-in-methods"></a>Habilitar o registro em log com métodos internos

O SDK do Azure para bibliotecas de cliente .NET registra eventos no rastreamento de eventos para Windows (ETW) por meio da [ `EventSource` classe](/dotnet/api/system.diagnostics.tracing.eventsource), que é típica para .net. As fontes de eventos permitem que você use o registro em log estruturado em seu código de aplicativo com uma sobrecarga mínima de desempenho. Para obter acesso a esses logs de eventos, você precisa registrar ouvintes de eventos.

O SDK inclui a `Azure.Core.Diagnostics.AzureEventSourceListener` classe (definida no pacote NuGet do Azure. Core), que contém dois métodos estáticos que simplificam o registro em log abrangente para seu aplicativo .net: `CreateConsoleLogger` e `CreateTraceLogger` . Esses métodos usam um parâmetro opcional que especifica um nível de log.

### <a name="log-to-the-console-window"></a>Registrar na janela do console

Uma filosofia principal do SDK do Azure para bibliotecas de cliente .NET é simplificar a capacidade de exibir logs abrangentes em tempo real. O `CreateConsoleLogger` método permite que você envie logs para a janela do console com uma única linha de código:

```csharp
using AzureEventSourceListener listener = AzureEventSourceListener.CreateConsoleLogger();
```

### <a name="log-to-diagnostic-traces"></a>Log para rastreamentos de diagnóstico

Se você implementar os ouvintes de rastreamento, poderá usar o `CreateTraceLogger` método para fazer logon no mecanismo de rastreamento de eventos do .NET padrão ( [`System.Diagnostics.Tracing`](/dotnet/api/system.diagnostics.tracing) ). Para obter mais informações sobre o rastreamento de eventos no .NET, consulte [ouvintes de rastreamento](../framework/debug-trace-profile/trace-listeners.md). Este exemplo especifica um nível de log detalhado:

```csharp
using AzureEventSourceListener listener = AzureEventSourceListener.CreateTraceLogger(EventLevel.Verbose);
```

## <a name="configure-custom-logging"></a>Configurar log personalizado

Conforme mencionado acima, você precisa registrar ouvintes de eventos para receber mensagens de log do SDK do Azure para .NET. Se você não quiser implementar o log abrangente usando um dos métodos simplificados acima, poderá construir uma instância da `AzureEventSourceListener` classe e passá-la para uma função de retorno de chamada que você escreve. Esse método receberá mensagens de log que você pode processar, mas é necessário. Além disso, ao construir a instância, você pode especificar os níveis de log a serem incluídos.

O exemplo a seguir cria um ouvinte de eventos que faz logon no console com uma mensagem personalizada e é filtrado para os eventos principais do Azure do nível detalhado.

```csharp
using AzureEventSourceListener listener = new AzureEventSourceListener((e, message) =>
    {
        // Only log messages from Azure-Core event source
        if (e.EventSource.Name == "Azure-Core")
        {
            Console.WriteLine($"{DateTime.Now} {message}");
        }
    },
    level: EventLevel.Verbose);
```

## <a name="next-steps"></a>Próximas etapas

- [Habilitar o log de diagnóstico para aplicativos no serviço Azure App](/azure/app-service/troubleshoot-diagnostic-logs)
- Examinar o [log de segurança do Azure e as opções de auditoria](/azure/security/fundamentals/log-audit)
- Saiba como trabalhar com [os logs da plataforma Azure](/azure/azure-monitor/platform/platform-logs-overview)
- Leia mais sobre [rastreamento e log do .NET Core](../core/diagnostics/logging-tracing.md)
