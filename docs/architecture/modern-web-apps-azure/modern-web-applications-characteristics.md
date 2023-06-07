---
title: Características de aplicativos Web modernos
description: Arquitetar aplicativos Web modernos com o ASP.NET Core e o Azure | Características de aplicativos Web modernos
author: ardalis
ms.author: wiwagn
no-loc:
- Blazor
- WebAssembly
ms.date: 12/04/2019
ms.openlocfilehash: fd6658a6143e132d488660d1aa4a35e427ba2d84
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174790"
---
# <a name="characteristics-of-modern-web-applications"></a>Características de aplicativos Web modernos

> "… com o design correto, os recursos são obtidos sem esforços. Essa abordagem é árdua, mas continua a ter êxito."  
> _\-Dennis Ritchie_

Os aplicativos Web modernos têm maiores expectativas de usuário e maiores demandas do que nunca. Os aplicativos Web de hoje devem estar disponíveis 24/7 em qualquer lugar do mundo e ser utilizáveis praticamente em qualquer dispositivo ou tamanho de tela. Os aplicativos Web precisam ser seguros, flexíveis e escalonáveis para atender aos picos da demanda. Cada vez mais, os cenários complexos devem ser tratados por experiências avançadas do usuário criadas no cliente por meio de JavaScript e pela comunicação eficiente por meio de APIs Web.

O ASP.NET Core é otimizado para aplicativos Web modernos e cenários de hospedagem baseada em nuvem. Seu design modular permite que os aplicativos dependam somente dos recursos que realmente usarem, melhorando o desempenho e a segurança do aplicativo e, ao mesmo tempo, reduzindo os requisitos de recursos de hospedagem.

## <a name="reference-application-eshoponweb"></a>Referência de aplicativo: eShopOnWeb

Estas diretrizes incluem um aplicativo de referência, o _eShopOnWeb_, que demonstra alguns dos princípios e das recomendações. O aplicativo é uma loja online simples compatível com navegação em um catálogo de camisetas, canecas e outros itens de marketing. O aplicativo de referência é deliberadamente simples para facilitar o entendimento.

![eShopOnWeb](./media/image2-1.png)

**Figura 2-1.** eShopOnWeb

> ### <a name="reference-application"></a>Aplicativo de referência
>
> - **eShopOnWeb**  
>   <https://github.com/dotnet/eShopOnWeb>

## <a name="cloud-hosted-and-scalable"></a>Hospedado na nuvem e escalonável

O ASP.NET Core é otimizado para a nuvem (nuvem pública, nuvem privada, qualquer nuvem) porque tem baixa memória e alta taxa de transferência. Uma superfície menor dos aplicativos ASP.NET Core significa que você pode hospedar mais partes dele no mesmo hardware e pagar por menos recursos quando usar os serviços pré-pagos de hospedagem na nuvem. A taxa de transferência mais alta significa que você pode atender mais clientes por meio de um aplicativo considerando o mesmo hardware, reduzindo ainda mais a necessidade de investir em servidores e na infraestrutura de hospedagem.

## <a name="cross-platform"></a>Plataforma cruzada

O ASP.NET Core é multiplataforma e pode ser executado no Windows, no macOS e no Linux. Isso proporciona muitas novas opções para o desenvolvimento e a implantação de aplicativos criados com o ASP.NET Core. Os contêineres do Docker, tanto do Linux quanto do Windows, podem hospedar aplicativos ASP.NET Core, permitindo que eles aproveitem os benefícios de [contêineres e microsserviços](../microservices/index.md).

## <a name="modular-and-loosely-coupled"></a>Flexível e acoplado de forma flexível

Os pacotes NuGet são cidadãos de primeira classe no .NET Core, e os aplicativos ASP.NET Core são compostos por muitas bibliotecas por meio do NuGet. Essa granularidade da funcionalidade ajuda a garantir que os aplicativos dependam e implantem somente a funcionalidade de que realmente precisam, reduzindo sua área da superfície de vulnerabilidade de segurança.

O ASP.NET Core também dá suporte total à [injeção de dependência](https://deviq.com/dependency-injection/), tanto internamente quanto no nível do aplicativo. As interfaces podem ter várias implementações que podem ser alternadas, conforme necessário. A injeção de dependência permite que os aplicativos sejam acoplados de modo flexível a essas interfaces, em vez de a implementações específicas, facilitando a extensão, a manutenção e o teste.

## <a name="easily-tested-with-automated-tests"></a>Testados facilmente com testes automatizados

Os aplicativos ASP.NET Core são compatíveis com teste de unidade, além disso, o acoplamento flexível e o suporte a injeções de dependência facilitam a alternância de interesses da infraestrutura com implementações fictícias para fins de teste. O ASP.NET Core também é fornecido com um TestServer que pode ser usado para hospedar aplicativos na memória. Os testes funcionais podem então fazer solicitações para esse servidor em memória, exercitando a pilha completa do aplicativo (incluindo middleware, roteamento, model binding, filtros, etc.) e recebendo uma resposta, tudo isso em uma fração do tempo que levaria para hospedar o aplicativo em um servidor real e fazer solicitações por meio da camada de rede. Esses testes são especialmente fáceis de serem gravados e significativos para APIs, que são cada vez mais importantes em aplicativos Web modernos.

## <a name="traditional-and-spa-behaviors-supported"></a>Comportamentos tradicionais e de SPA com suporte

Os aplicativos Web tradicionais envolvem pouco comportamento do lado do cliente e, em vez disso, dependem do servidor para todas as consultas, atualizações e navegação que o aplicativo pode precisar fazer. Cada nova operação feita pelo usuário é convertida em uma nova solicitação da Web, com o resultado sendo um recarregamento de página inteira no navegador do usuário final. As estruturas MVC (Modelo-Exibição-Controlador) clássicas normalmente seguem essa abordagem, com cada nova solicitação correspondente a uma ação de controlador diferente, que, por sua vez, trabalham com um modelo e retornam uma exibição. Algumas operações individuais em determinada página podem ser aprimoradas com a funcionalidade AJAX (Asynchronous JavaScript And XML), mas a arquitetura geral do aplicativo usava muitas diferentes exibições do MVC e pontos de extremidade de URL. Além disso, o ASP.NET Core MVC também é compatível com Razor Pages, uma maneira mais simples para organizar as páginas de estilo MVC.

Por outro lado, os SPAs (aplicativos de página única) envolvem muito poucos carregamentos de página do lado do servidor gerados dinamicamente (se houver). Muitos SPAs são inicializados em um arquivo HTML estático que carrega as bibliotecas JavaScript necessárias para iniciar e executar o aplicativo. Esses aplicativos fazem uso intenso de APIs Web para suas necessidades de dados e podem fornecer experiências do usuário muito mais avançadas.

Muitos aplicativos Web envolvem uma combinação do comportamento do aplicativo Web tradicional (normalmente para o conteúdo) e SPAs (para a interatividade). O ASP.NET Core é compatível com o MVC (baseado em Exibições ou em Páginas) e com as APIs Web no mesmo aplicativo, usando o mesmo conjunto de ferramentas e de bibliotecas da estrutura subjacentes.

## <a name="simple-development-and-deployment"></a>Desenvolvimento e implantação simples

ASP.NET Core aplicativos podem ser escritos usando editores de texto simples e interfaces de linha de comando, ou ambientes de desenvolvimento completos, como o Visual Studio. Normalmente, os aplicativos monolíticos são implantados em um único ponto de extremidade. As implantações podem ser automatizadas com facilidade para ocorrerem como parte de um pipeline de CI (integração contínua) e CD (entrega contínua). Além das ferramentas tradicionais de CI/CD, Microsoft Azure tem suporte integrado para repositórios git e pode implantar atualizações automaticamente conforme elas são feitas em uma ramificação ou marca git especificada. O Azure DevOps fornece uma compilação completa de CI/CD e um pipeline de implantação, e as ações do GitHub fornecem outra opção para projetos hospedados lá.

## <a name="traditional-aspnet-and-web-forms"></a>ASP.NET tradicional e Web Forms

Além do ASP.NET Core, o ASP.NET 4.x tradicional continua sendo uma plataforma robusta e confiável para a criação de aplicativos Web. O ASP.NET dá suporte a modelos de desenvolvimento de API Web e MVC, bem como Web Forms, que é adequado para o desenvolvimento de aplicativos baseado em página avançado e apresenta um ecossistema avançado de componentes de terceiros. A Microsoft Azure tem um excelente suporte de longa qualidade para aplicativos ASP.NET 4. x e muitos desenvolvedores estão familiarizados com essa plataforma.

## Blazor

Blazorestá incluído no ASP.NET Core 3,0 e posterior. Ele fornece um novo mecanismo para a criação de aplicativos avançados de cliente Web interativos usando Razor, C# e ASP.NET Core. Ele oferece outra solução a ser considerada ao desenvolver aplicativos Web modernos. Há duas versões do Blazor a serem consideradas: lado do servidor e do cliente.

O lado do servidor Blazor foi lançado em 2019 com ASP.NET Core 3,0. Como o nome indica, ele é executado no servidor, processando alterações no documento do cliente de volta para o navegador pela rede. O lado do servidor Blazor fornece uma experiência de cliente rica sem a necessidade de JavaScript do lado do cliente e sem a necessidade de carregamentos de página separados para cada interação de página de cliente. As alterações na página carregada são solicitadas e processadas pelo servidor e, em seguida, enviadas de volta para o cliente usando o Signalr.

O lado do cliente Blazor será lançado em 2020 e eliminará a necessidade de processar alterações no servidor. Em vez disso, ele será aproveitado WebAssembly para executar o código .net dentro do cliente. O cliente ainda pode fazer chamadas à API para o servidor, se necessário, para solicitar dados, mas todo o comportamento do lado do cliente é executado no cliente via WebAssembly , que já tem suporte em todos os principais navegadores e é apenas uma biblioteca JavaScript.

> ### <a name="references--modern-web-applications"></a>Referências – Aplicativos Web modernos
>
> - **Introdução ao ASP.NET Core**  
>   <https://docs.microsoft.com/aspnet/core/>
> - **Teste no ASP.NET Core**  
>   <https://docs.microsoft.com/aspnet/core/testing/>
> - **Blazor-Introdução**  
>   <https://blazor.net/docs/get-started.html>

>[!div class="step-by-step"]
>[Anterior](index.md) 
> [Avançar](choose-between-traditional-web-and-single-page-apps.md)
