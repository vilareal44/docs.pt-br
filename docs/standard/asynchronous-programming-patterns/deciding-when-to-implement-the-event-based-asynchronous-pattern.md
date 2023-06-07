---
title: Decidindo quando implementar o padrão assíncrono baseado em evento
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- Event-based Asynchronous Pattern
- ProgressChangedEventArgs class
- BackgroundWorker component
- events [.NET Framework], asynchronous
- AsyncOperationManager class
- threading [.NET Framework], asynchronous features
- AsyncOperation class
- AsyncCompletedEventArgs class
ms.assetid: a00046aa-785d-4f7f-a8e5-d06475ea50da
ms.openlocfilehash: c235a838504889a105ef98df47f7373a145503da
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289441"
---
# <a name="deciding-when-to-implement-the-event-based-asynchronous-pattern"></a>Decidindo quando implementar o padrão assíncrono baseado em evento

O Padrão assíncrono baseado em evento oferece um padrão para expor o comportamento assíncrono de uma classe. Com a introdução deste padrão, o .NET Framework define dois padrões para expor o comportamento assíncrono: o Padrão assíncrono baseado na interface do <xref:System.IAsyncResult?displayProperty=nameWithType> e o padrão baseado no evento. Este tópico descreve quando é apropriado implementar os dois padrões.

Para obter mais informações sobre a programação assíncrona com a interface <xref:System.IAsyncResult>, confira [APM (modelo de programação assíncrona)](asynchronous-programming-model-apm.md).

## <a name="general-principles"></a>Noções básicas gerais

Em geral, você deve expor recursos assíncronos usando o Padrão assíncrono baseado em evento sempre que possível. No entanto, há alguns requisitos que o padrão baseado em evento não pode atender. Nesses casos, talvez seja necessário implementar o padrão <xref:System.IAsyncResult> além do padrão baseado em evento.

> [!NOTE]
> É raro o padrão <xref:System.IAsyncResult> ser implementado sem que o padrão baseado em evento também seja implementado.

## <a name="guidelines"></a>Diretrizes

A lista a seguir descreve as diretrizes para quando você deve implementar o Padrão Assíncrono baseado em Evento:

- Use o padrão baseado em evento como a API padrão para expor o comportamento assíncrono para sua classe.

- Não exponha o padrão <xref:System.IAsyncResult>, principalmente quando a classe for usada em um aplicativo cliente, por exemplo, o Windows Forms.

- Basta expor o padrão <xref:System.IAsyncResult> quando ele for necessário para atender seus requisitos. Por exemplo, a compatibilidade com uma API existente pode exigir que você exponha o padrão <xref:System.IAsyncResult>.

- Não exponha o padrão <xref:System.IAsyncResult> sem também expor o padrão baseado em evento.

- Se você precisar expor o padrão <xref:System.IAsyncResult>, exponha-o como uma opção avançada. Por exemplo, se você gerar um objeto de proxy, gere o padrão baseado em evento por padrão, com uma opção para gerar o padrão <xref:System.IAsyncResult>.

- Crie sua implementação do padrão baseado em evento na implementação do padrão <xref:System.IAsyncResult>.

- Evitar expor os dois padrões, o padrão baseado em evento e o padrão <xref:System.IAsyncResult>, na mesma classe. Exponha o padrão baseado em evento em classes de "nível superior" e o padrão <xref:System.IAsyncResult> em classes de "nível inferior". Por exemplo, compare o padrão baseado em evento no componente <xref:System.Net.WebClient> com o padrão <xref:System.IAsyncResult> na classe <xref:System.Web.HttpRequest>.

  - Exponha o padrão baseado em evento e o padrão <xref:System.IAsyncResult> na mesma classe quando a compatibilidade exigir isso. Por exemplo, se já tiver lançado uma API que usa o padrão <xref:System.IAsyncResult>, precisará manter o padrão <xref:System.IAsyncResult> por questões de compatibilidade com versões anteriores.

  - Exponha o padrão baseado em evento e o padrão <xref:System.IAsyncResult> na mesma classe se a complexidade do modelo de objeto resultante for maior que o benefício de separar as implementações. É melhor expor os dois padrões em uma única classe para evitar expor o padrão baseado em evento.

  - Se quiser expor o padrão baseado em evento e o padrão <xref:System.IAsyncResult> em uma única classe, use o <xref:System.ComponentModel.EditorBrowsableAttribute> definido como <xref:System.ComponentModel.EditorBrowsableState.Advanced> para marcar a implementação do padrão <xref:System.IAsyncResult> como um recurso avançado. Isso dá a indicação aos ambientes de design, como o Visual Studio IntelliSense, para não exibir as propriedades e os métodos de <xref:System.IAsyncResult>. Essas propriedades e métodos ainda são totalmente utilizáveis, mas, trabalhando com o IntelliSense, o desenvolvedor terá uma visão clara da API.

## <a name="criteria-for-exposing-the-iasyncresult-pattern-in-addition-to-the-event-based-pattern"></a>Critérios para expor o Padrão de IAsyncResult além do Padrão baseado em evento

O Padrão Assíncrono Baseado em Evento é muito vantajoso quando aplicado aos cenários mencionados anteriormente. No entanto, ele apresenta algumas desvantagens e você precisa estar atento caso o desempenho for seu requisito mais importante.

Há três cenários que o padrão baseado em evento não trata e o padrão <xref:System.IAsyncResult> também não:

- Bloqueio da espera em um <xref:System.IAsyncResult>

- Bloqueio da espera em vários objetos <xref:System.IAsyncResult>

- Sondagem de conclusão no <xref:System.IAsyncResult>

Você pode lidar com esses cenários usando o padrão baseado em evento, mas isso é mais complicado do que usar o padrão <xref:System.IAsyncResult>.

Os desenvolvedores geralmente usam o padrão <xref:System.IAsyncResult> em serviços que normalmente têm requisitos de desempenho muito elevados. Por exemplo, a sondagem do cenário de conclusão é uma técnica de servidor de alto desempenho.

Além disso, o padrão baseado em evento é menos eficiente que o padrão <xref:System.IAsyncResult> porque cria mais objetos, especialmente o <xref:System.EventArgs>, e por sincronizar entre threads.

A lista a seguir mostra algumas recomendações a serem seguidas se você decidir usar o padrão <xref:System.IAsyncResult>:

- Só exponha o padrão <xref:System.IAsyncResult> quando você precisar de suporte para os objetos <xref:System.Threading.WaitHandle> ou <xref:System.IAsyncResult>.

- Só exponha o padrão <xref:System.IAsyncResult> quando você tiver uma API existente que usa o padrão <xref:System.IAsyncResult>.

- Se você já tiver uma API baseada no padrão <xref:System.IAsyncResult>, considere também expor o padrão baseado em evento em sua próxima versão.

- Só exponha o padrão <xref:System.IAsyncResult> caso tiver requisitos de alto desempenho que você verificou não poderem ser atendidos pelo padrão baseado em evento, mas que podem ser atendidos pelo padrão <xref:System.IAsyncResult>.

## <a name="see-also"></a>Veja também

- [Como implementar um componente compatível com o padrão assíncrono baseado em evento](component-that-supports-the-event-based-asynchronous-pattern.md)
- [EAP (Padrão Assíncrono baseado em Evento)](event-based-asynchronous-pattern-eap.md)
- [Implementando o Padrão Assíncrono baseado em Evento](implementing-the-event-based-asynchronous-pattern.md)
- [Práticas recomendadas para a implementação do padrão assíncrono baseado em evento](best-practices-for-implementing-the-event-based-asynchronous-pattern.md)
- [Visão geral do padrão assíncrono baseado em evento](event-based-asynchronous-pattern-overview.md)
