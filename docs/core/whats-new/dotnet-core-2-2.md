---
title: Novidades do .NET Core 2.2
description: Conheça os novos recursos do .NET Core 2.2.
dev_langs:
- csharp
- vb
ms.date: 12/04/2018
ms.openlocfilehash: 656ef9aa2745c935c37b69ae5a54b8d126700e55
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608306"
---
# <a name="whats-new-in-net-core-22"></a>Novidades do .NET Core 2.2

O .NET Core 2.2 inclui aprimoramentos na implantação do aplicativo, na manipulação de eventos para serviços de runtime, na autenticação de Bancos de Dados SQL do Azure, no desempenho do compilador JIT e na injeção de código antes da execução do método `Main`.

## <a name="new-deployment-mode"></a>Novo modo de implantação

A partir do .NET Core 2.2, você pode implantar [executáveis dependentes da estrutura](../deploying/index.md#publish-framework-dependent), que são arquivos **.exe** em vez de arquivos **.dll**. Com uma funcionalidade semelhante às implantações dependentes de estrutura, os FDEs (executáveis dependentes de estrutura) ainda contam com a presença de uma versão compartilhada de todo o sistema do .NET Core para executar. Seu aplicativo contém apenas seu código e quaisquer dependências de terceiros. Ao contrário de implantações dependentes de estrutura, os FDEs são específicos da plataforma.

Esse novo modo de implantação tem a vantagem distinta da criar um executável em vez de uma biblioteca, o que significa que você pode executar seu aplicativo diretamente, sem invocar `dotnet` primeiro.

## <a name="core"></a>Núcleo

**Manipulação de eventos nos serviços de runtime**

Muitas vezes, convém monitorar o uso dos serviços de runtime do seu aplicativo, como o GC, o JIT e o ThreadPool, para entender como eles afetam seu aplicativo.Em sistemas Windows, isso normalmente é feito pelo monitorando de eventos ETW do processo atual.Embora isso continue funcionando bem, nem sempre é possível usar ETW se você estiver executando em um ambiente com poucos privilégios, ou no Linux ou macOS.

A partir do .NET Core 2.2, os eventos de CoreCLR podem ser consumidos usando a classe <xref:System.Diagnostics.Tracing.EventListener?displayProperty=nameWithType>. Esses eventos descrevem o comportamento desses serviços de runtime como GC, JIT, ThreadPool e interoperabilidade. Esses são os mesmos eventos são expostos como parte do provedor ETW CoreCLR.Isso permite que os aplicativos consumam esses eventos ou usem um mecanismo de transporte para enviá-los para um serviço de agregação de telemetria. Veja como assinar eventos no exemplo de código a seguir:

```csharp
internal sealed class SimpleEventListener : EventListener
{
    // Called whenever an EventSource is created.
    protected override void OnEventSourceCreated(EventSource eventSource)
    {
        // Watch for the .NET runtime EventSource and enable all of its events.
        if (eventSource.Name.Equals("Microsoft-Windows-DotNETRuntime"))
        {
            EnableEvents(eventSource, EventLevel.Verbose, (EventKeywords)(-1));
        }
    }

    // Called whenever an event is written.
    protected override void OnEventWritten(EventWrittenEventArgs eventData)
    {
        // Write the contents of the event to the console.
        Console.WriteLine($"ThreadID = {eventData.OSThreadId} ID = {eventData.EventId} Name = {eventData.EventName}");
        for (int i = 0; i < eventData.Payload.Count; i++)
        {
            string payloadString = eventData.Payload[i]?.ToString() ?? string.Empty;
            Console.WriteLine($"\tName = \"{eventData.PayloadNames[i]}\" Value = \"{payloadString}\"");
        }
        Console.WriteLine("\n");
    }
}
```

Além disso, o .NET Core 2.2 adiciona as duas propriedades a seguir à classe <xref:System.Diagnostics.Tracing.EventWrittenEventArgs> para fornecer informações adicionais sobre eventos ETW:

- <xref:System.Diagnostics.Tracing.EventWrittenEventArgs.OSThreadId?displayProperty=nameWithType>

- <xref:System.Diagnostics.Tracing.EventWrittenEventArgs.TimeStamp?displayProperty=nameWithType>

## <a name="data"></a>Dados

**Autenticação do AAD para Bancos de Dados SQL do Azure com a propriedade SqlConnection.AccessToken**

A partir do .NET Core 2.2, um token de acesso emitido pelo Azure Active Directory pode ser usado para autenticar em um Banco de Dados SQL do Azure. Para dar suporte a tokens de acesso, a propriedade <xref:System.Data.SqlClient.SqlConnection.AccessToken> foi adicionada à classe <xref:System.Data.SqlClient.SqlConnection>. Para aproveitar a autenticação do AAD, baixe a versão 4.6 do pacote NuGet System.Data.SqlClient. Para usar o recurso, obtenha o valor do token de acesso usando a [Biblioteca de Autenticação do Active Directory para .NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) contida no pacote NuGet [`Microsoft.IdentityModel.Clients.ActiveDirectory`](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/).

## <a name="jit-compiler-improvements"></a>Melhorias do compilador JIT

**A compilação em camadas permanece um recurso ativado mediante consentimento**

No .NET Core 2.1, o compilador JIT implementou uma nova tecnologia compiladora, a *compilação hierárquica*, como um recurso ativado mediante consentimento. O objetivo da compilação em camadas é melhorar o desempenho. Uma das tarefas importantes executadas pelo compilador JIT é otimizar a execução de código. No entanto, para caminhos de código pouco usados, o compilador pode gastar mais tempo otimizando o código do que o runtime gasta executando código não otimizado. A compilação em camadas introduz dois estágios na compilação JIT:

- Uma **primeira camada**, que gera código o mais rápido possível.

- Uma **segunda camada**, que gera código otimizado para os métodos executados com frequência. A segunda camada de compilação é executada em paralelo para melhorar o desempenho.

Para saber mais sobre melhoria de desempenho que pode resultar da compilação em camadas, consulte [Anúncio do .NET Core 2.2 Versão prévia 2](https://devblogs.microsoft.com/dotnet/announcing-net-core-2-2-preview-2/).

No .NET Core 2.2 Versão prévia 2, a compilação em camadas está habilitada por padrão. No entanto, decidimos que ainda não estamos prontos para habilitar a compilação em camadas por padrão. Portanto, no .NET Core 2.2, a compilação em camadas continua a ser um recurso ativado mediante consentimento. Para saber mais sobre o consentimento com a compilação em camadas, confira [Aprimoramentos do compilador Jit](dotnet-core-2-1.md#jit-compiler-improvements) em [O que há de novo no .NET Core 2.1](dotnet-core-2-1.md).

## <a name="runtime"></a>Runtime

**Injeção de código antes de executar o método Main**

A partir do .NET Core 2.2, você pode usar um gancho de inicialização para injetar código antes da execução do método Main de um aplicativo. Ganchos de inicialização tornam possível para um host personalizar o comportamento de aplicativos após eles terem sido implantados, sem a necessidade de recompilar ou alterar o aplicativo.

Esperamos que os provedores de hospedagem definam a configuração e a política personalizadas, incluindo configurações que possivelmente influenciam o comportamento de carregamento do ponto de entrada principal, como o comportamento <xref:System.Runtime.Loader.AssemblyLoadContext?displayProperty=nameWithType> . O gancho pode ser usado para configurar a injeção de rastreamento ou de telemetria, para configurar os retornos de chamada para tratamento ou para definir outro comportamento dependente do ambiente. O gancho é separado do ponto de entrada, para que o código do usuário não precise ser modificado.

Confira [Gancho de inicialização do host](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/host-startup-hook.md) para saber mais.

## <a name="see-also"></a>Confira também

- [Novidades do .NET Core 3.1](dotnet-core-3-1.md)
- [Novidades do ASP.NET Core 2.2](/aspnet/core/release-notes/aspnetcore-2.2)
- [Novos recursos no EF Core 2.2](/ef/core/what-is-new/ef-core-2.2)
