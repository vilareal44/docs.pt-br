---
title: Gerenciamento de estado
description: Aprenda abordagens diferentes para o gerenciamento de estado em ASP.NET Web Forms e mais incrivelmente.
author: csharpfritz
ms.author: jefritz
ms.date: 05/15/2020
ms.openlocfilehash: 2ca811f886c6a245ac16c2bd66a68ff16e5bfc44
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88267718"
---
# <a name="state-management"></a>Gerenciamento de estado

O gerenciamento de estado é um conceito-chave de Web Forms aplicativos, facilitado por meio do estado de exibição, estado de sessão, estado do aplicativo e recursos de postback. Esses recursos com estado da estrutura ajudaram a ocultar o gerenciamento de estado necessário para um aplicativo e permitir que os desenvolvedores de aplicativos se concentrem em fornecer sua funcionalidade. Com o ASP.NET Core e o mais bem, alguns desses recursos foram realocados e alguns foram completamente removidos. Este capítulo examina como manter o estado e fornecer a mesma funcionalidade com os novos recursos do mais novo.

## <a name="request-state-management-with-viewstate"></a>Solicitar gerenciamento de estado com ViewState

Ao discutir o gerenciamento de estado no aplicativo Web Forms, muitos desenvolvedores irão considerar imediatamente o ViewState. Em Web Forms, ViewState gerencia o estado do conteúdo entre solicitações HTTP enviando um bloco de texto codificado grande para o navegador. O campo ViewState pode ser sobrecarregado com conteúdo de uma página que contém muitos elementos, potencialmente expandindo para vários megabytes de tamanho.

Com o servidor mais incrivelmente, o aplicativo mantém uma conexão contínua com o servidor. O estado do aplicativo, chamado de *circuito*, é mantido na memória do servidor enquanto a conexão é considerada ativa. O estado só será Descartado quando o usuário sair do aplicativo ou de uma página específica no aplicativo. Todos os membros dos componentes ativos estão disponíveis entre as interações com o servidor.

Há várias vantagens desse recurso:

- O estado do componente está prontamente disponível e não reconstruído entre as interações.
- O estado não é transmitido para o navegador.

No entanto, há algumas desvantagens na persistência de estado de componente na memória a serem consideradas:

- Se o servidor for reiniciado entre a solicitação, o estado será perdido.
- Sua solução de balanceamento de carga do servidor Web do aplicativo deve incluir sessões adesivas para garantir que todas as solicitações do mesmo navegador retornem para o mesmo servidor. Se uma solicitação for para um servidor diferente, o estado será perdido.
- A persistência do estado do componente no servidor pode levar à pressão de memória no servidor Web.

Pelos motivos anteriores, não confie apenas no estado do componente para residir na memória no servidor. Seu aplicativo também deve incluir algum armazenamento de dados de backup para dados entre solicitações. Alguns exemplos simples dessa estratégia:

- Em um aplicativo de carrinho de compras, persista o conteúdo dos novos itens adicionados ao carrinho em um registro de banco de dados. Se o estado no servidor for perdido, você poderá reconstitui-lo dos registros do banco de dados.
- Em um formulário da Web de várias partes, seus usuários esperam que seu aplicativo se lembre de valores entre cada solicitação. Grave os dados entre cada um dos posts do usuário em um repositório de dados para que eles possam ser buscados e montados na estrutura de resposta do formulário final quando o formulário de várias partes for concluído.

Para obter detalhes adicionais sobre o gerenciamento de estado em aplicativos mais novos, consulte [ASP.NET Core o gerenciamento de estado mais incrivelmente](/aspnet/core/blazor/state-management).

## <a name="maintain-state-with-session"></a>Manter estado com sessão

Web Forms desenvolvedores podem manter informações sobre o usuário que está atuando no momento com o <xref:Microsoft.AspNetCore.Http.ISession?displayProperty=nameWithType> objeto Dictionary. É fácil adicionar um objeto com uma chave de cadeia de caracteres ao `Session` , e esse objeto estaria disponível posteriormente durante as interações do usuário com o aplicativo. Em uma tentativa de eliminar o gerenciamento de interação com HTTP, o `Session` objeto facilitou a manutenção do estado.

A assinatura do objeto .NET Framework `Session` não é igual ao `Session` objeto ASP.NET Core. Considere [a documentação da nova sessão de ASP.NET Core](/dotnet/api/microsoft.aspnetcore.http.isession) antes de decidir migrar e usar o novo recurso de estado de sessão.

A sessão está disponível em um servidor ASP.NET Core e mais incrivelmente, mas não é recomendável de usar para armazenar dados em um repositório de dados adequadamente. O estado da sessão também não funcionará se os visitantes recusarem o uso de cookies HTTP em seu aplicativo devido a questões de privacidade.

A configuração para ASP.NET Core e o estado de sessão está disponível no [artigo gerenciamento de sessão e estado no ASP.NET Core](/aspnet/core/fundamentals/app-state#session-state).

## <a name="application-state"></a>Estado do aplicativo

O `Application` objeto no Web Forms Framework fornece um repositório maciço e de solicitação cruzada para interagir com a configuração e o estado do escopo do aplicativo. O estado do aplicativo era um lugar ideal para armazenar várias propriedades de configuração de aplicativo que seriam referenciadas por todas as solicitações, independentemente do usuário que faz a solicitação. O problema com o `Application` objeto era que os dados não persistiram em vários servidores. O estado do objeto de aplicativo foi perdido entre as reinicializações.

Assim como acontece com `Session` o, é recomendável que os dados sejam movidos para um repositório de backup persistente que possa ser acessado por várias instâncias de servidor. Se houver dados voláteis que você gostaria de poder acessar entre solicitações e usuários, você poderia armazená-los facilmente em um serviço singleton que pode ser injetado em componentes que exigem essas informações ou interação.

A construção de um objeto para manter o estado do aplicativo e seu consumo pode se parecer com a seguinte implementação:

```csharp
public class MyApplicationState
{
    public int VisitorCounter { get; private set; } = 0;

    public void IncrementCounter() => VisitorCounter += 1;
}
```

```csharp
app.AddSingleton<MyApplicationState>();
```

```razor
@inject MyApplicationState AppState

<label>Total Visitors: @AppState.VisitorCounter</label>
```

O `MyApplicationState` objeto é criado apenas uma vez no servidor, e o valor `VisitorCounter` é buscado e a saída é obtida no rótulo do componente. O `VisitorCounter` valor deve ser persistido e recuperado de um armazenamento de dados de backup para durabilidade e escalabilidade.

## <a name="in-the-browser"></a>No navegador

Os dados do aplicativo também podem ser armazenados no lado do cliente no dispositivo do usuário para que ele esteja disponível posteriormente. Há dois recursos de navegador que permitem a persistência de dados em escopos diferentes do navegador do usuário:

- `localStorage` -escopo do navegador inteiro do usuário. Se a página for recarregada, o navegador será fechado e reaberto, ou outra guia será aberta com a mesma URL, então o mesmo `localStorage` será fornecido pelo navegador
- `sessionStorage` -escopo da guia atual do navegador do usuário. Se a guia for recarregada, o estado persiste. No entanto, se o usuário abrir outra guia para seu aplicativo ou fechar e reabrir o navegador, o estado será perdido.

Você pode escrever um código JavaScript personalizado para interagir com esses recursos, ou há vários pacotes NuGet que você pode usar que fornecem essa funcionalidade. Um desses pacotes é [Microsoft. AspNetCore. ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).

Para obter instruções sobre como utilizar esse pacote para interagir `localStorage` com `sessionStorage` o e o, consulte o artigo de [Gerenciamento de estado mais incrivelmente](/aspnet/core/blazor/state-management#protected-browser-storage-experimental-package) .

>[!div class="step-by-step"]
>[Anterior](pages-routing-layouts.md) 
> [Avançar](forms-validation.md)
