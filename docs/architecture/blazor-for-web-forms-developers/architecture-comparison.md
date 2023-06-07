---
title: Comparação de arquitetura do ASP.NET Web Forms e Blazor
description: Saiba como as arquiteturas do ASP.NET Web Forms e Blazor comparar.
author: danroth27
ms.author: daroth
no-loc:
- Blazor
ms.date: 09/11/2019
ms.openlocfilehash: 9a8e78338aff53002647a10ed9007296e4682b5a
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88267705"
---
# <a name="architecture-comparison-of-aspnet-web-forms-and-no-locblazor"></a>Comparação de arquitetura do ASP.NET Web Forms e Blazor

Embora o ASP.NET Web Forms e Blazor tenha muitos conceitos semelhantes, há diferenças em como eles funcionam. Este capítulo examina as arquiteturas e os trabalhos internos do ASP.NET Web Forms e Blazor .

## <a name="aspnet-web-forms"></a>Web Forms do ASP.NET

O ASP.NET Web Forms Framework se baseia em uma arquitetura centrada em página. Cada solicitação HTTP para um local no aplicativo é uma página separada com a qual o ASP.NET responde. À medida que as páginas são solicitadas, o conteúdo do navegador é substituído pelos resultados da página solicitada.

As páginas consistem nos seguintes componentes:

- Marcação HTML
- Código em C# ou Visual Basic
- Uma classe code-behind que contém recursos de lógica e manipulação de eventos
- Controles

Os controles são unidades reutilizáveis da interface do usuário da Web que podem ser colocadas e interagir de forma programática em uma página. As páginas são compostas por arquivos que terminam com *. aspx* contendo marcação, controles e algum código. As classes code-behind estão em arquivos com o mesmo nome base e uma extensão *. aspx.cs* ou *. aspx. vb* , dependendo da linguagem de programação usada. Curiosamente, o servidor Web interpreta o conteúdo dos arquivos *. aspx* e os compila sempre que eles forem alterados. Essa recompilação ocorre mesmo que o servidor Web já esteja em execução.

Os controles podem ser criados com marcação e entregues como controles de usuário. Um controle de usuário deriva da `UserControl` classe e tem uma estrutura semelhante à página. A marcação para controles de usuário é armazenada em um arquivo *. ascx* . Uma classe code-behind acompanhante reside em um arquivo *. ascx.cs* ou *. ascx. vb* . Os controles também podem ser compilados completamente com o código, herdando da `WebControl` `CompositeControl` classe base ou.

As páginas também têm um ciclo de vida de eventos extensivo. Cada página gera eventos para os eventos de inicialização, carregamento, PreRender e Unload que ocorrem quando o tempo de execução ASP.NET executa o código da página para cada solicitação.

Os controles em uma página normalmente são postbacks para a mesma página que apresentou o controle e transportam-se por uma carga de um campo de formulário oculto chamado `ViewState` . O `ViewState` campo contém informações sobre o estado dos controles no momento em que eles foram renderizados e apresentados na página, permitindo que o tempo de execução do ASP.net compare e identifique as alterações no conteúdo enviado ao servidor.

## Blazor

Blazor é uma estrutura de interface do usuário da Web do lado do cliente semelhante à natureza das estruturas de front-end do JavaScript como angular ou reagir. Blazor lida com as interações do usuário e renderiza as atualizações necessárias da interface do usuário. Blazor*não é* baseado em um modelo de solicitação-resposta. As interações do usuário são tratadas como eventos que não estão no contexto de nenhuma solicitação HTTP específica.

Blazor os aplicativos consistem em um ou mais componentes raiz que são processados em uma página HTML.

![::: no-Loc (mais alto)::: componentes em HTML](./media/architecture-comparison/blazor-components-in-html.png)

Como o usuário especifica onde os componentes devem renderizar e como os componentes são conectados para interações de usuário é o [modelo de hospedagem](hosting-models.md) específico.

Blazoros [componentes](components.md) são classes .NET que representam uma parte reutilizável da interface do usuário. Cada componente mantém seu próprio Estado e especifica sua própria lógica de renderização, que pode incluir a renderização de outros componentes. Os componentes especificam manipuladores de eventos para interações específicas do usuário para atualizar o estado do componente.

Depois que um componente manipula um evento, Blazor o renderiza o componente e controla o que foi alterado na saída renderizada. Os componentes não são renderizados diretamente para o Modelo de Objeto do Documento (DOM). Em vez disso, eles são renderizados em uma representação na memória do DOM chamado um `RenderTree` para que Blazor possa rastrear as alterações. Blazor compara a saída processada recentemente com a saída anterior para calcular uma comparação de interface do usuário que é aplicada com eficiência ao DOM.

![::: no-Loc (mais alto)::: interação DOM](./media/architecture-comparison/blazor-dom-interaction.png)

Os componentes também podem indicar manualmente que devem ser renderizados se seu estado for alterado fora de um evento de interface do usuário normal. Blazor usa um `SynchronizationContext` para impor um único thread lógico de execução. Os métodos de ciclo de vida de um componente e quaisquer retornos de chamada de evento que são gerados por Blazor são executados nesse `SynchronizationContext` .

>[!div class="step-by-step"]
>[Anterior](introduction.md) 
> [Avançar](hosting-models.md)
