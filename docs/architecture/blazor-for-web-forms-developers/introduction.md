---
title: Uma introdução ao Blazor para ASP.NET Web Forms desenvolvedores
description: Uma introdução ao Blazor e à gravação de aplicativos Web de pilha completa com o .net
author: danroth27
ms.author: daroth
no-loc:
- Blazor
- WebAssembly
ms.date: 09/11/2019
ms.openlocfilehash: a5aae6cf02ccec84ac8642b6ce8d9c919755e868
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88267562"
---
# <a name="an-introduction-to-no-locblazor-for-aspnet-web-forms-developers"></a>Uma introdução ao Blazor para ASP.NET Web Forms desenvolvedores

O ASP.NET Web Forms Framework foi um grampo do desenvolvimento para a Web .NET desde que o .NET Framework foi enviado pela primeira vez em 2002. Voltando quando a Web ainda estava em grande parte, ASP.NET Web Forms tornar a criação de aplicativos Web simples e produtiva, adotando muitos dos padrões que foram usados para o desenvolvimento de desktops. No ASP.NET Web Forms, as páginas da Web podem ser rapidamente compostas a partir de controles de interface do usuário reutilizáveis. As interações do usuário são tratadas naturalmente como eventos. Há um rico ecossistema de Web Forms controles de interface do usuário fornecidos pela Microsoft e pelos fornecedores de controle. Os controles facilitam os esforços de se conectar a fontes de dados e exibir visualizações de dados ricas. Para o visualmente inclinado, o designer de Web Forms fornece uma interface simples do tipo "arrastar e soltar" para gerenciar controles.

Ao longo dos anos, a Microsoft introduziu novas estruturas da Web baseadas em ASP.NET para abordar as tendências de desenvolvimento para a Web. Algumas dessas estruturas da Web incluem ASP.NET MVC, Páginas da Web do ASP.NET e mais recentes ASP.NET Core. Com cada nova estrutura, algumas têm previsto a recusa iminente de ASP.NET Web Forms e críticas-la como uma estrutura da Web desatualizada e de modo desatualizado. Apesar dessas previsões, muitos desenvolvedores da Web .NET continuam a encontrar ASP.NET Web Forms uma maneira simples, estável e produtiva de realizar seu trabalho.

No momento da escrita, quase meio milhão de desenvolvedores da Web usam ASP.NET Web Forms todos os meses. O ASP.NET Web Forms Framework é estável até o ponto em que documentos, amostras, livros e postagens de blog de uma década atrás permanecem úteis e relevantes. Para muitos desenvolvedores da Web .NET, "ASP.NET" ainda é sinônimo de "ASP.NET Web Forms" como era quando o .NET foi concebido pela primeira vez. Os argumentos nos prós e contras do ASP.NET Web Forms em comparação com as outras novas estruturas .NET da Web podem ser levadas em diante. ASP.NET Web Forms continua sendo uma estrutura popular para a criação de aplicativos Web.

Mesmo assim, as inovações no desenvolvimento de software não estão lentas. Todos os desenvolvedores de software precisam se manter atualizados de novas tecnologias e tendências. Duas tendências em particular valem a pena considerar:

1. A mudança para software livre e para várias plataformas
2. A mudança da lógica do aplicativo para o cliente

## <a name="an-open-source-and-cross-platform-net"></a>Um .NET de software livre e de plataforma cruzada

Quando o .NET e o ASP.NET Web Forms lançados pela primeira vez, o ecossistema da plataforma parecia muito diferente do que hoje. Os mercados de desktops e servidores foram dominados pelo Windows. Plataformas alternativas como macOS e Linux ainda estavam lutando para obter força. O ASP.NET Web Forms é fornecido com o .NET Framework como um componente somente do Windows, o que significa que ASP.NET Web Forms aplicativos só podem ser executados em computadores Windows Server. Muitos ambientes modernos agora usam diferentes tipos de plataformas para servidores e máquinas de desenvolvimento, de modo que o suporte entre plataformas para muitos usuários seja um requisito absoluto.

A maioria das estruturas da Web modernas agora também são de código-fonte aberto, o que tem vários benefícios. Os usuários não se comparam a um único proprietário de projeto para corrigir bugs e adicionar recursos. Os projetos de código-fonte aberto fornecem transparência aprimorada sobre o progresso do desenvolvimento e alterações futuras. Projetos de software livre aproveitam as contribuições de uma comunidade inteira e estimulam um ecossistema de código-fonte aberto de apoio. Apesar dos riscos de software livre, muitos consumidores e colaboradores encontraram atenuações adequadas que permitem aproveitar os benefícios de um ecossistema de software livre de maneira segura e razoável. Exemplos dessas atenuações incluem contratos de licença de colaborador, licenças amigáveis, exames de pedigree e bases de suporte.

A Comunidade do .NET adotou o suporte de plataforma cruzada e o código-fonte aberto. O .NET Core é uma implementação de software livre e de plataforma cruzada do .NET que é executada em uma infinidade de plataformas, incluindo Windows, macOS e várias distribuições do Linux. O Xamarin fornece mono, uma versão de código aberto do .NET. O mono é executado no Android, no iOS e em uma variedade de outros fatores forma, incluindo inspeções e TVs inteligentes. A Microsoft anunciou que o [.NET 5](https://devblogs.microsoft.com/dotnet/introducing-net-5/) reconciliará o .NET Core e o mono em "um único tempo de execução e estrutura do .NET que pode ser usado em qualquer lugar e que tenha comportamentos de tempo de execução uniformes e experiências de desenvolvedor."

O ASP.NET Web Forms se beneficiar da mudança para o suporte de software livre e entre plataformas? A resposta, infelizmente, é não, ou pelo menos não na mesma extensão do restante da plataforma. Recentemente, a equipe do .NET [tornou claro](https://devblogs.microsoft.com/dotnet/net-core-is-the-future-of-net/) que ASP.NET Web Forms não serão portados para .NET Core ou .NET 5. Por quê?

Houve esforços nos primórdios do .NET Core para a porta ASP.NET Web Forms. O número de alterações significativas necessárias foi considerado muito drástico. Há também uma admissão aqui que, mesmo para a Microsoft, há um limite para o número de estruturas da Web que ele pode dar suporte simultaneamente. Talvez alguém na Comunidade demore a causa da criação de uma versão de software livre e de várias plataformas do ASP.NET Web Forms. O [código-fonte para ASP.NET Web Forms](https://github.com/microsoft/referencesource) foi disponibilizado publicamente no formulário de referência. Mas, por enquanto, parece que ASP.NET Web Forms permanecerão somente Windows e sem um modelo de contribuição de código aberto. Se o suporte entre plataformas ou o código-fonte aberto se tornar importante para seus cenários, você precisará procurar algo novo.

Isso significa que o ASP.NET Web Forms está *inoperante* e não deve mais ser usado? Claro que não! Desde que o .NET Framework seja fornecido como parte do Windows, o ASP.NET Web Forms será uma estrutura com suporte. Para muitos desenvolvedores de Web Forms, a falta de suporte de plataforma cruzada e de software livre é um problema não. Se você não tiver um requisito para suporte de plataforma cruzada, software livre ou qualquer um dos outros novos recursos no .NET Core ou no .NET 5, então, acompanhando o ASP.NET Web Forms no Windows está bem. O ASP.NET Web Forms continuará a ser uma maneira produtiva de escrever aplicativos Web por muitos anos.

Mas há outra tendência que vale a pena considerar, e essa é a mudança para o cliente.

## <a name="client-side-web-development"></a>Desenvolvimento para a Web do lado do cliente

Todos os. Estruturas da Web baseadas em rede, incluindo ASP.NET Web Forms, historicamente tinham uma coisa em comum: são *renderizadas pelo servidor*. Em aplicativos Web renderizados pelo servidor, o navegador faz uma solicitação ao servidor, que executa algum código (código .NET em aplicativos ASP.NET) para produzir uma resposta. Essa resposta é enviada de volta ao navegador para manipular. Nesse modelo, o navegador é usado como um mecanismo de processamento fino. O trabalho pesado de produzir a interface do usuário, executar a lógica de negócios e o estado de gerenciamento ocorre no servidor.

No entanto, os navegadores se tornaram plataformas versáteis. Eles implementam um número cada vez maior de padrões abertos da Web que concedem acesso aos recursos da máquina do usuário. Por que não aproveitar a capacidade de computação, o armazenamento, a memória e outros recursos do dispositivo cliente? As interações de interface do usuário em particular podem se beneficiar de uma sensação mais rica e interativa quando lidas pelo menos parcialmente ou completamente no lado do cliente. A lógica e os dados que devem ser manipulados no servidor ainda podem ser tratados no lado do servidor. Chamadas de API Web ou até mesmo protocolos em tempo real, como WebSockets, podem ser usadas. Esses benefícios estarão disponíveis para desenvolvedores da Web gratuitamente se estiverem dispostos a escrever JavaScript. As estruturas de interface do usuário do lado do cliente, como angular, reagir e Vue, simplificam o desenvolvimento para a Web no lado do cliente e crescem em popularidade. ASP.NET Web Forms os desenvolvedores também podem aproveitar o aproveitamento do cliente e até mesmo ter um suporte pronto para uso com estruturas JavaScript integradas, como o ASP.NET AJAX.

Mas a ponte de duas plataformas e ecossistemas diferentes (.NET e JavaScript) vem com um custo. A experiência é necessária em dois mundos paralelos com linguagens, estruturas e ferramentas diferentes. O código e a lógica não podem ser facilmente compartilhados entre o cliente e o servidor, resultando em uma sobrecarga de duplicação e de engenharia. Também pode ser difícil acompanhar o ecossistema do JavaScript, que tem um histórico de evolução na velocidade de batalhas. A estrutura de front-end e as preferências de ferramenta de compilação mudam rapidamente. O setor observou a progressão de Grunt para Gulp para webpack e assim por diante. A mesma rotatividade de Restless ocorreu com estruturas de front-end como jQuery, Knockout, angular, reagir e Vue. Mas, considerando a monopolização do navegador do JavaScript, houve pouca escolha na questão. Ou seja, até que a Comunidade da Web tenha se reunido e tenha causado a ocorrência de um *Miracle* !

## <a name="no-locwebassembly-fulfills-a-need"></a>WebAssembly atende a uma necessidade

Em 2015, os principais fornecedores de navegadores ingressaram em um grupo de comunidades W3C para criar um novo Web Standard aberto chamado WebAssembly . WebAssembly é um código de byte para a Web. Se você puder compilar seu código para WebAssembly , ele poderá ser executado em qualquer navegador em qualquer plataforma em uma velocidade quase nativa. Os esforços iniciais se concentram no C/C++. O resultado foi uma demonstração drástica da execução direta de mecanismos gráficos 3D nativos no navegador, sem plug-ins. WebAssembly desde que tenha sido padronizado e implementado por todos os principais navegadores.

O trabalho na execução do .NET no WebAssembly foi anunciado no final de 2017 e deve ser fornecido em 2020, incluindo suporte do .NET 5. A capacidade de executar o código .NET diretamente no navegador permite o desenvolvimento para a Web de pilha completa com o .NET.

## <a name="no-locblazor-full-stack-web-development-with-net"></a>Blazor: desenvolvimento para a Web de pilha completa com .NET

Por si só, a capacidade de executar código .NET em um navegador não fornece uma experiência de ponta a ponta para criar aplicativos Web do lado do cliente. É aí que Blazor entra em. Blazor é uma estrutura de interface do usuário da Web do lado do cliente baseada em C# em vez de JavaScript. Blazor pode ser executado diretamente no navegador via WebAssembly . Nenhum plug-in de navegador é necessário. Como alternativa, Blazor os aplicativos podem executar o lado do servidor no .NET Core e lidar com todas as interações do usuário em uma conexão em tempo real com o navegador.

Blazor tem excelente suporte a ferramentas no Visual Studio e Visual Studio Code. A estrutura também inclui um modelo de componente completo da interface do usuário e tem instalações internas para:

- Formulários e validação
- Injeção de dependência
- Roteamento do lado do cliente
- Layouts
- Depuração no navegador
- Interoperabilidade do JavaScript

Blazor tem muito em comum com ASP.NET Web Forms. Ambas as estruturas oferecem modelos de programação de interface do usuário com estado baseado em componente e orientados por eventos. A principal diferença arquitetônica é que ASP.NET Web Forms é executado somente no servidor. Blazor pode ser executado no cliente no navegador. Mas se você estiver vindo de uma ASP.NET Web Forms plano de fundo, há muita coisa Blazor que se sentirá familiar. Blazor é uma solução natural para ASP.NET Web Forms desenvolvedores que buscam uma maneira de aproveitar o desenvolvimento do lado do cliente e o futuro da plataforma cruzada de software livre do .NET.

Este livro fornece uma introdução ao Blazor que é fornecido especificamente para desenvolvedores de ASP.NET Web Forms. Cada Blazor conceito é apresentado no contexto de Web Forms ASP.net análogos e recursos e práticas. No final deste livro, você terá uma compreensão de:

- Como criar Blazor aplicativos.
- Como o Blazor funciona.
- Como o Blazor se relaciona com o .NET Core.
- Estratégias razoáveis para migrar os aplicativos ASP.NET Web Forms existentes para Blazor onde for apropriado.

## <a name="get-started-with-no-locblazor"></a>Introdução ao Blazor

A introdução ao Blazor é fácil. Vá para <https://blazor.net> e siga os links para instalar os modelos de projeto e SDK do .NET Core apropriados Blazor . Você também encontrará instruções para configurar as Blazor ferramentas no Visual Studio ou Visual Studio Code.

>[!div class="step-by-step"]
>[Anterior](index.md) 
> [Avançar](architecture-comparison.md)
