---
title: Escolha entre aplicativos Web tradicionais e aplicativos de página única
description: Saiba como escolher entre aplicativos Web tradicionais e SPAs (aplicativos de única página) ao criar aplicativos Web.
author: ardalis
ms.author: wiwagn
no-loc:
- Blazor
- WebAssembly
ms.date: 12/04/2019
ms.openlocfilehash: 4fe889fe86d96a5b2ffa5bd879d2ec1801a3cf20
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174361"
---
# <a name="choose-between-traditional-web-apps-and-single-page-apps-spas"></a>Escolher entre aplicativos Web tradicionais e SPAs (aplicativos de página única)

> "Lei de Atwood: qualquer aplicativo que pode ser escrito em JavaScript será, em última análise, escrito em JavaScript."  
> _\-Jeff Atwood_

Há duas abordagens gerais para a criação de aplicativos Web hoje: os aplicativos Web tradicionais que executam a maior parte da lógica do aplicativo no servidor e os SPAs (aplicativos de página única) que executam a maior parte da lógica da interface do usuário em um navegador da Web, comunicando-se com o servidor Web principalmente por meio de APIs Web. Uma abordagem híbrida também é possível, o mais simples é hospedar um ou mais aplicativos avançados semelhantes ao SPA em um aplicativo Web tradicional mais amplo.

Use aplicativos Web tradicionais quando:

- Os requisitos do lado do cliente do aplicativo são simples ou até mesmo somente leitura.

- Seu aplicativo precisa funcionar em navegadores sem suporte a JavaScript.

- Sua equipe não está familiarizada com as técnicas de desenvolvimento do JavaScript ou do TypeScript.

Use um SPA quando:

- Seu aplicativo precisa expor uma interface do usuário avançada com muitos recursos.

- Sua equipe está familiarizada com o desenvolvimento do JavaScript e/ou do TypeScript.

- Seu aplicativo já deve expor uma API para outros clientes (internos ou públicos).

Além disso, as estruturas de SPA exigem um maior conhecimento em arquitetura e segurança. Elas passam por uma maior variação devido a atualizações frequentes e novas estruturas comparado aos aplicativos Web tradicionais. A configuração de processos automatizados de compilação e implantação e a utilização de opções de implantação como contêineres pode ser mais difícil com aplicativos SPA do que aplicativos Web tradicionais.

Melhorias na experiência do usuário possibilitadas pela abordagem SPA devem ser avaliadas em relação a essas considerações.

## Blazor

ASP.NET Core 3,0 apresenta um novo modelo para a criação de uma interface do usuário rica, interativa e combinável chamada Blazor . Blazoro lado do servidor permite que os desenvolvedores criem interface do usuário com C# e Razor no servidor e que a interface do usuário seja conectada interativamente ao navegador em tempo real usando uma conexão de sinalização persistente.

BlazorWebAssemblyapresenta outra opção para Blazor aplicativos, permitindo que eles sejam executados no navegador usando o WebAssembly . Como o .NET real está em execução WebAssembly , você pode usar novamente o código e as bibliotecas de partes do lado do servidor do seu aplicativo.

Blazorfornece uma nova opção a ser considerada ao avaliar se deve-se criar um aplicativo Web puramente renderizado no servidor ou um SPA. Você pode criar comportamentos avançados do lado do cliente, semelhantes ao SPA Blazor , usando, sem a necessidade de um desenvolvimento de JavaScript significativo. Blazoros aplicativos podem chamar APIs para solicitar dados ou executar operações do lado do servidor.

Considere criar seu aplicativo Web com Blazor quando:

- Seu aplicativo deve expor uma interface do usuário rica

- Sua equipe é mais confortável com o desenvolvimento do .NET do que o desenvolvimento de JavaScript ou TypeScript

Para obter mais informações sobre o Blazor , consulte Introdução [ Blazor ao ](https://blazor.net/docs/get-started.html).

## <a name="when-to-choose-traditional-web-apps"></a>Quando escolher aplicativos Web tradicionais

A seguir há uma explicação mais detalhada dos motivos já mencionados para escolher os aplicativos Web tradicionais.

**Seu aplicativo tem requisitos simples do lado do cliente, possivelmente somente leitura**

Muitos aplicativos Web são consumidos principalmente em um modo somente leitura pela grande maioria de seus usuários. Os aplicativos somente leitura (ou predominantemente de leitura) tendem a ser muito mais simples do que aqueles que mantêm e manipulam uma grande quantidade de estado. Por exemplo, um mecanismo de pesquisa pode consistir em um único ponto de entrada com uma caixa de texto e uma segunda página para exibição dos resultados da pesquisa. Os usuários anônimos podem fazer solicitações com facilidade, e há pouca necessidade de uma lógica do lado do cliente. Da mesma forma, um aplicativo voltado para o público de um sistema de gerenciamento de conteúdo ou blog normalmente consiste principalmente em conteúdo com pouco comportamento do lado do cliente. Esses aplicativos são facilmente criados como aplicativos Web tradicionais baseados em servidor, que executam lógica no servidor Web e renderizam HTML para serem exibidos no navegador. O fato de que cada página exclusiva do site tem sua própria URL que pode ser marcada e indexada por mecanismos de pesquisa (por padrão, sem a necessidade de adicioná-la como um recurso separado do aplicativo) também é um benefício claro nesses cenários.

**Seu aplicativo precisa funcionar em navegadores sem suporte a JavaScript**

Os aplicativos Web que precisam funcionar em navegadores com suporte limitado ou nenhum suporte a JavaScript precisam ser escritos com fluxos de trabalho de aplicativo Web tradicional (ou, pelo menos, poder fazer fallback para esse comportamento). Os SPAs exigem um JavaScript do lado do cliente para funcionar; se ele não estiver disponível, os SPAs não serão uma boa opção.

**Sua equipe não está familiarizada com as técnicas de desenvolvimento de JavaScript ou TypeScript**

Caso sua equipe não esteja familiarizada com o JavaScript ou o TypeScript, mas esteja familiarizada com o desenvolvimento de aplicativos Web do lado do servidor, provavelmente, ela poderá fornecer um aplicativo Web tradicional mais rapidamente do que um SPA. A menos que o aprendizado de programação de SPAs seja uma meta ou a experiência do usuário proporcionada por um SPA seja necessária, os aplicativos Web tradicionais serão uma opção mais produtiva para equipes que já estão familiarizadas com sua criação.

## <a name="when-to-choose-spas"></a>Quando escolher SPAs

Veja a seguir uma explicação mais detalhada de quando escolher um estilo de desenvolvimento de Aplicativos de Página Única para seu aplicativo Web.

**Seu aplicativo precisa expor uma interface do usuário avançada com muitos recursos**

Os SPAs podem dar suporte à funcionalidade avançada do lado do cliente que não exige o recarregamento da página conforme os usuários executam ações ou navegam entre as áreas do aplicativo. OS SPAs podem ser carregados com mais agilidade, buscando dados em segundo plano, e as ações de usuário individuais são mais ágeis na resposta pois os recarregamentos de página inteira são raros. Os SPAs podem dar suporte a atualizações incrementais, salvando formulários ou documentos parcialmente preenchidos sem que o usuário precise clicar em um botão para enviar um formulário. Os SPAs podem ser compatíveis com comportamentos avançados do lado do cliente, como operações do tipo "arrastar e soltar", com muito mais agilidade do que os aplicativos tradicionais. Os SPAs podem ser designados para serem executados em um modo desconectado, fazendo atualizações em um modelo do lado do cliente que são, por fim, sincronizadas com o servidor depois que uma conexão é restabelecida. Escolha um aplicativo estilo SPA se os requisitos do seu aplicativo incluírem funcionalidades avançadas que vão além do que os formulários HTML típicos oferecem.

Frequentemente, o SPAs precisa implementar recursos que são internos a aplicativos Web tradicionais, como exibir uma URL significativa na barra de endereços que reflete a operação atual (e permitir que os usuários marquem ou vinculem o link profundo para essa URL para retornar a ela). Os SPAs também devem permitir que os usuários usem os botões Voltar e Avançar do navegador com resultados que não os surpreenderão.

**Sua equipe está familiarizada com o desenvolvimento de JavaScript e/ou TypeScript**

A configuração de SPAs exige familiaridade com o JavaScript e/ou o TypeScript e bibliotecas e técnicas de programação do lado do cliente. Sua equipe deve ser competente na escrita de JavaScript moderno usando uma estrutura de SPA como o Angular.

> ### <a name="references--spa-frameworks"></a>Referências – Estruturas de SPA
>
> - **Angular**  
>   <https://angular.io>
> - **Reagir**
>   <https://reactjs.org/>
> - **Comparação de Estruturas em JavaScript**  
>   <https://jsreport.io/the-ultimate-guide-to-javascript-frameworks/>

**Seu aplicativo já deve expor uma API para outros clientes (internos ou públicos)**

Caso você já esteja dando suporte a uma API Web para uso por outros clientes, poderá ser necessário menos esforço para criar uma implementação de SPA que utiliza essas APIs em vez de reproduzir a lógica no formulário do lado do servidor. Os SPAs fazem uso extensivo de APIs Web para consultar e atualizar os dados conforme os usuários interagem com o aplicativo.

## <a name="when-to-choose-blazor"></a>Quando escolherBlazor

Veja a seguir uma explicação mais detalhada de quando escolher Blazor para seu aplicativo Web.

**Seu aplicativo deve expor uma interface do usuário rica**

Como SPAs com base em JavaScript, Blazor os aplicativos podem dar suporte ao comportamento de cliente avançado sem recargas de página. Esses aplicativos são mais responsivos aos usuários, buscando apenas os dados (ou HTML) necessários para responder a uma determinada interação do usuário. Projetado corretamente, os aplicativos do lado do servidor Blazor podem ser configurados para serem executados como aplicativos do lado do cliente Blazor com alterações mínimas, uma vez que esse recurso tem suporte.

**Sua equipe é mais confortável com o desenvolvimento do .NET do que o desenvolvimento de JavaScript ou TypeScript**

Muitos desenvolvedores são mais produtivos com o .NET e o Razor do que com linguagens do lado do cliente, como JavaScript ou TypeScript. Como o lado do servidor do aplicativo já está sendo desenvolvido com o .NET, o uso de Blazor garante que todos os desenvolvedores do .NET da equipe possam entender e potencialmente criar o comportamento do front-end do aplicativo.

## <a name="decision-table"></a>Tabela de decisão

A tabela de decisão a seguir resume alguns dos fatores básicos a serem considerados ao escolher entre um aplicativo Web tradicional, um SPA ou um Blazor aplicativo.

| **Fator**                                           | **Aplicativo Web tradicional** | **Aplicativo de página única** | **BlazorAplicação**  |
| ---------------------------------------------------- | ----------------------- | --------------------------- | --------------- |
| É necessária a familiaridade da equipe com JavaScript/TypeScript | **Mínimo**             | **Obrigatório**                | **Mínimo**     |
| Suporte a navegadores sem scripts                   | **Com suporte**           | **Sem suporte**           | **Com suporte**   |
| Comportamento mínimo do aplicativo do lado do cliente             | **Apropriado**         | **Exagero**                | **Viáveis**      |
| Requisitos avançados e complexos de interface do usuário            | **Certo**             | **Apropriado**             | **Apropriado** |

>[!div class="step-by-step"]
>[Anterior](modern-web-applications-characteristics.md) 
> [Avançar](architectural-principles.md)
