---
title: Blazor para desenvolvedores de Web Forms ASP.NET
description: Saiba como criar aplicativos Web de pilha completa com o .NET usando Blazor o e o .NET Core de maneira simples e familiar.
author: danroth27
ms.author: daroth
no-loc:
- Blazor
- WebAssembly
ms.date: 09/11/2019
ms.openlocfilehash: 1c869cce6ab8a0ab7c4b83817fe1afc3d6a4a7fd
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88267458"
---
# <a name="no-locblazor-for-aspnet-web-forms-developers"></a>Blazor para desenvolvedores de Web Forms ASP.NET

![Captura de tela que mostra a capa do livro eletrônico de aplicativos sem servidor.](./media/index/blazor-for-aspnet-web-forms-developers.png)

> DOWNLOAD disponível em: <https://aka.ms/blazor-ebook>

PUBLICADO POR

Divisão de Desenvolvedores Microsoft, equipes dos produtos .NET e Visual Studio

Uma divisão da Microsoft Corporation

One Microsoft Way

Redmond, Washington 98052-6399

Copyright © 2020 da Microsoft Corporation

Todos os direitos reservados. Nenhuma parte do conteúdo deste guia pode ser reproduzida ou transmitida de nenhuma forma nem por nenhum meio sem a permissão por escrito do publicador.

Este livro é fornecido “no estado em que se encontra” e expressa os pontos de vista e as opiniões do autor. Os pontos de vista, as opiniões e as informações expressos neste guia, incluindo URLs e outras referências a sites da Internet, podem ser alteradas sem aviso prévio.

 Alguns exemplos aqui representados são fornecidos somente para fins de ilustração e são fictícios. Nenhuma associação ou conexão real é intencional ou deve ser inferida.

A Microsoft e as marcas listadas em <https://www.microsoft.com> na página da Web "Marcas" são marcas comerciais do grupo de empresas Microsoft.

Mac e macOS são marcas comerciais da Apple Inc.

Todas as outras marcas e logotipos são propriedade de seus respectivos proprietários.

Autores:

> **[Daniel Roth](https://github.com/danroth27)**, gerente principal de programa da Microsoft Corp.

> **[Jeff Fritz](https://github.com/csharpfritz)**, gerente de programas sênior, Microsoft Corp.

> **[Taylor Southwick](https://github.com/twsouthwick)**, engenheiro de software sênior, Microsoft Corp.

> **[Scott Addie](https://github.com/scottaddie)**, desenvolvedor de conteúdo sênior, Microsoft Corp.

> **[Steve "ardalis" Smith](https://ardalis.com)**, arquiteto de software e instrutor, ARDALIS Services LLC

## <a name="introduction"></a>Introdução

O .NET tem suporte de desenvolvimento de aplicativos Web por meio do ASP.NET, um conjunto abrangente de estruturas e ferramentas para a criação de qualquer tipo de aplicativo Web. O ASP.NET tem sua própria linhagem de estruturas e tecnologias da Web, começando desde o início até as páginas clássicas de Active Server (ASP). Estruturas como ASP.NET Web Forms, ASP.NET MVC, Páginas da Web do ASP.NET e mais recentes ASP.NET Core, fornecem uma maneira produtiva e poderosa de criar aplicativos Web *renderizados pelo servidor* , em que o conteúdo da interface do usuário é gerado dinamicamente no servidor em resposta a solicitações HTTP. Cada estrutura de ASP.NET atende a uma filosofia de criação de aplicativo e público diferente. O ASP.NET Web Forms fornecido com a versão original do .NET Framework e o desenvolvimento da Web habilitado usando muitos dos padrões familiares aos desenvolvedores de desktop, como controles de interface do usuário reutilizáveis com manipulação de eventos simples. No entanto, nenhuma das ofertas de ASP.NET fornece uma maneira de executar o código executado no navegador do usuário. Para fazer isso, é necessário escrever JavaScript e usar qualquer uma das muitas estruturas e ferramentas JavaScript que tenham sido divididas e em fase de popularidade ao longo dos anos: jQuery, Knockout, angular, reagir e assim por diante.

[Blazor](https://blazor.net) é uma nova estrutura da Web que altera o que é possível ao criar aplicativos Web com o .NET. Blazor é uma estrutura de interface do usuário da Web do lado do cliente baseada em C# em vez de JavaScript. Com Blazor você pode escrever a lógica do lado do cliente e os componentes da interface do usuário em C#, compilá-los em assemblies normais do .net e executá-los diretamente no navegador usando um novo padrão da Web aberto chamado WebAssembly . Ou, como alternativa, Blazor o pode executar os componentes da interface do usuário do .net no servidor e lidar com todas as interações da interface do usuário em uma conexão em tempo real com o navegador. Quando emparelhado com o .NET em execução no servidor, o Blazor permite o desenvolvimento para a Web de pilha completa com o .net. Embora Blazor Compartilhe muitas semelhanças com ASP.NET Web Forms, como ter um modelo de componente reutilizável e uma maneira simples de lidar com eventos de usuário, ele também se baseia nas bases do .NET Core para fornecer uma experiência de desenvolvimento Web moderna e de alto desempenho.

Este livro apresenta ASP.NET Web Forms desenvolvedores de Blazor forma que sejam familiares e convenientes. Ele apresenta Blazor conceitos em paralelo com conceitos análogos no ASP.NET Web Forms ao mesmo tempo em que explica novos conceitos que podem ser menos conhecidos. Ele aborda uma ampla variedade de tópicos e preocupações, incluindo criação de componentes, roteamento, layout, configuração e segurança. E, embora o conteúdo deste livro seja principalmente para habilitar o novo desenvolvimento, ele também aborda diretrizes e estratégias para migrar Web Forms ASP.NET existentes para Blazor quando você quiser modernizar um aplicativo existente.

## <a name="who-should-use-the-book"></a>Quem deve usar o livro

Este livro destina-se a ASP.NET Web Forms desenvolvedores que procuram uma introdução ao Blazor que se relaciona com seu conhecimento e suas habilidades existentes. Este livro pode ajudar na introdução rápida de um Blazor projeto baseado em novo ou no gráfico de um roteiro para modernizar um aplicativo ASP.NET Web Forms existente.

## <a name="how-to-use-the-book"></a>Como usar o livro

A primeira parte deste livro aborda o que Blazor é o e o compara com o desenvolvimento de aplicativos Web com o ASP.NET Web Forms. Em seguida, o livro aborda uma variedade de Blazor Tópicos, capítulo por capítulo e relaciona cada Blazor conceito ao conceito correspondente no ASP.NET Web Forms ou explica completamente todos os conceitos totalmente novos. O livro também se refere regularmente a um aplicativo de exemplo completo implementado em ASP.NET Web Forms e Blazor para demonstrar Blazor recursos e fornecer um estudo de caso para migrar do ASP.NET Web Forms para o Blazor . Você pode encontrar ambas as implementações do aplicativo de exemplo (ASP.NET Web Forms e Blazor versões) no [GitHub](https://github.com/dotnet-architecture/eshoponblazor).

## <a name="what-this-book-doesnt-cover"></a>O que este livro não abrange

Este livro é uma introdução ao Blazor , não um guia de migração abrangente. Embora inclua orientações sobre como abordar a migração de um projeto do ASP.NET Web Forms para o Blazor , ele não tenta cobrir todas as nuances e os detalhes. Para obter diretrizes mais gerais sobre como migrar do ASP.NET para o ASP.NET Core, consulte as [diretrizes de migração](https://docs.microsoft.com/aspnet/core/migration/proper-to-2x/) na documentação do ASP.NET Core.

### <a name="additional-resources"></a>Recursos adicionais

Você pode encontrar o Blazor Home page oficial e a documentação em <https://blazor.net> .

## <a name="send-your-feedback"></a>Envie seus comentários

Este livro e exemplos relacionados estão em constante evolução, para que seus comentários sejam bem-vindos! Se você tiver comentários sobre como esse livro pode ser melhorado, use a seção de comentários na parte inferior de qualquer página criada com base nos [problemas do GitHub](https://github.com/dotnet/docs/issues).

>[!div class="step-by-step"]
>[Próximo](introduction.md)
