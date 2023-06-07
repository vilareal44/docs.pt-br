---
title: Manipulando e acionando eventos
description: Saiba como manipular e disparar eventos .NET, que se baseiam no modelo delegado. Esse modelo permite que os assinantes se registrem ou recebam notificações de provedores.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- delegate model for events
- application development [.NET], events
- application development [.NET Framework], events
- application development [.NET Core], events
- events [.NET]
- events [.NET Core]
- events [.NET Framework]
ms.assetid: b6f65241-e0ad-4590-a99f-200ce741bb1f
ms.openlocfilehash: 8cf0ff323e9bf7305e3d9cbb6dabd8f685059e97
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84447102"
---
# <a name="handling-and-raising-events"></a>Manipulando e acionando eventos

Os eventos no .NET são baseados no modelo de representante. O modelo de representante segue o [padrão de design do observador](observer-design-pattern.md), que permite a um assinante se registrar em um provedor e receber notificações dele. Um remetente de eventos envia uma notificação por push de que um evento ocorreu e um receptor de eventos recebe essa notificação e define uma resposta. Este artigo descreve os principais componentes do modelo de representante, como consumir eventos em aplicativos e como implementar eventos no código.  
  
 Para saber mais sobre como manipular eventos em aplicativos da Windows 8.x Store, confira [Visão geral de eventos e eventos roteados](https://docs.microsoft.com/previous-versions/windows/apps/hh758286(v=win.10)).  
  
## <a name="events"></a>Eventos

Um evento é uma mensagem enviada por um objeto para sinalizar a ocorrência de uma ação. A ação pode ser causada pela interação do usuário, como o clique em um botão, ou ser resultado de alguma outra lógica de programa, como a alteração do valor de uma propriedade. O objeto que aciona o evento é chamado de *remetente do evento*. O remetente do evento não sabe qual objeto ou método receberá (identificador) os eventos que ele aciona. O evento normalmente é membro do remetente do evento. Por exemplo, o evento <xref:System.Web.UI.WebControls.Button.Click> é membro da classe <xref:System.Web.UI.WebControls.Button> e o evento <xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged> é membro da classe que implementa a interface <xref:System.ComponentModel.INotifyPropertyChanged>.  
  
Para definir um evento, use o C# [`event`](../../csharp/language-reference/keywords/event.md) ou a [`Event`](../../visual-basic/language-reference/statements/event-statement.md) palavra-chave Visual Basic na assinatura de sua classe de evento e especifique o tipo de delegado para o evento. Os representantes são descritos na próxima seção.  
  
Normalmente, para acionar um evento, você adiciona um método que é marcado como `protected` e `virtual` (em C#) ou `Protected` e `Overridable` (no Visual Basic). Dê a esse método o nome `On`*EventName*; por exemplo, `OnDataReceived`. O método deve usar um parâmetro que especifica um objeto de dados de evento, que é um objeto do tipo <xref:System.EventArgs> ou um tipo derivado. Você fornece esse método para permitir que as classes derivadas substituam a lógica para acionamento do evento. Uma classe derivada sempre deve chamar o método `On`*EventName* da classe base a fim de garantir que os representantes registrados recebam o evento.  

O exemplo de código a seguir mostra como declarar um evento denominado `ThresholdReached`. O evento está associado ao representante <xref:System.EventHandler> e é gerado em um método chamado `OnThresholdReached`.  
  
 [!code-csharp[EventsOverview#1](~/samples/snippets/csharp/VS_Snippets_CLR/eventsoverview/cs/programtruncated.cs#1)]
 [!code-vb[EventsOverview#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/eventsoverview/vb/module1truncated.vb#1)]  
  
## <a name="delegates"></a>Delegados

Um representante é um tipo que contém uma referência a um método. Um representante é declarado com uma assinatura que mostra o tipo de retorno e os parâmetros para os métodos aos quais faz referência, e pode conter referências apenas aos métodos que correspondem à sua assinatura. Portanto, um representante é equivalente a um ponteiro de função fortemente tipado ou um retorno de chamada. Uma declaração de representante é suficiente para definir uma classe de representante.  
  
Representantes têm muitos usos no .NET. No contexto de eventos, um representante é um intermediário (ou mecanismo do tipo ponteiro) entre a origem do evento e o código que manipula o evento. Associe um representante a um evento incluindo o tipo de representante na declaração do evento, como mostrado no exemplo da seção anterior. Para obter mais informações sobre representantes, consulte a classe <xref:System.Delegate>.  
  
O .NET fornece os representantes <xref:System.EventHandler> e <xref:System.EventHandler%601> para dar suporte à maioria dos cenários de evento. Use o representante <xref:System.EventHandler> para todos os eventos que não incluem dados de evento. Use o representante <xref:System.EventHandler%601> para eventos que incluem dados sobre o evento. Esses representantes não têm valor de tipo de retorno e usam dois parâmetros (um objeto para a origem do evento e um objeto para dados do evento).  
  
Os representantes são [multicast](xref:System.MulticastDelegate), o que significa que eles podem manter referências a mais de um método de manipulação de eventos. Para obter detalhes, consulte a página de referência <xref:System.Delegate>. Os representantes proporcionam flexibilidade e controle refinado na manipulação de eventos. Um representante atua como um dispatcher de evento para a classe que aciona o evento ao manter uma lista de manipuladores de eventos registrados para o evento.  
  
Para cenários em que os representantes <xref:System.EventHandler> e <xref:System.EventHandler%601> não funcionam, você pode definir um representante. Os cenários que exigem que você defina um representante são muito raros; por exemplo, quando você deve trabalhar com código que não reconhece genéricos. Você marca um delegado com a [`delegate`](../../csharp/language-reference/builtin-types/reference-types.md#the-delegate-type) [`Delegate`](../../visual-basic/language-reference/statements/delegate-statement.md) palavra-chave C# e Visual Basic na declaração. O exemplo a seguir mostra como declarar um representante chamado `ThresholdReachedEventHandler`.  
  
[!code-csharp[EventsOverview#4](~/samples/snippets/csharp/VS_Snippets_CLR/eventsoverview/cs/programtruncated.cs#4)]
[!code-vb[EventsOverview#4](~/samples/snippets/visualbasic/VS_Snippets_CLR/eventsoverview/vb/module1truncated.vb#4)]  
  
## <a name="event-data"></a>Dados de evento

Os dados associados a um evento podem ser fornecidos por meio de uma classe de dados do evento. O .NET fornece muitas classes de dados de evento que podem ser usadas em seus aplicativos. Por exemplo, a classe <xref:System.IO.Ports.SerialDataReceivedEventArgs> é a classe de dados de evento do evento <xref:System.IO.Ports.SerialPort.DataReceived?displayProperty=nameWithType>. O .NET segue um padrão de nomenclatura de terminação para todas as classes de dados de evento com `EventArgs`. Determine qual classe de dados de evento está associada a um evento observando o representante do evento. Por exemplo, o representante <xref:System.IO.Ports.SerialDataReceivedEventHandler> inclui a classe <xref:System.IO.Ports.SerialDataReceivedEventArgs> como um de seus parâmetros.  
  
A classe <xref:System.EventArgs> é o tipo base para todas as classes de dados de evento. <xref:System.EventArgs> também é a classe usada quando um evento não tem nenhum dado associado. Quando você criar um evento cuja finalidade seja apenas notificar outras classes de que algo aconteceu e que não precise passar nenhum dado, inclua a classe <xref:System.EventArgs> como o segundo parâmetro no representante. Você poderá passar o valor <xref:System.EventArgs.Empty?displayProperty=nameWithType> quando nenhum dado for fornecido. O representante <xref:System.EventHandler> inclui a classe <xref:System.EventArgs> como um parâmetro.  
  
Quando quiser criar uma classe de dados de evento personalizada, crie uma classe derivada de <xref:System.EventArgs> e forneça todos os membros necessários para passar dados que estejam relacionados ao evento. Normalmente, você deve usar o mesmo padrão de nomenclatura do .NET e terminar o nome da classe de dados de evento com `EventArgs`.  
  
O exemplo a seguir mostra uma classe de dados de evento chamada `ThresholdReachedEventArgs`. Ele contém propriedades que são específicas ao evento que está sendo acionado.  
  
[!code-csharp[EventsOverview#3](~/samples/snippets/csharp/VS_Snippets_CLR/eventsoverview/cs/programtruncated.cs#3)]
[!code-vb[EventsOverview#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/eventsoverview/vb/module1truncated.vb#3)]  
  
## <a name="event-handlers"></a>Manipuladores de eventos

Para responder a um evento, você pode definir um método de manipulador de eventos no receptor do evento. Esse método deve corresponder à assinatura do representante para o evento que está sendo manipulado. No manipulador de eventos, execute as ações que são necessárias quando o evento é acionado, como coletar a entrada do usuário depois que ele clica em um botão. Para receber notificações de ocorrência de eventos, o método de manipulador de eventos deve estar inscrito no evento.  
  
O exemplo a seguir mostra um método de manipulador de eventos chamado `c_ThresholdReached` que corresponde à assinatura para o representante <xref:System.EventHandler>. O método está inscrito no evento `ThresholdReached`.  
  
[!code-csharp[EventsOverview#2](~/samples/snippets/csharp/VS_Snippets_CLR/eventsoverview/cs/programtruncated.cs#2)]
[!code-vb[EventsOverview#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/eventsoverview/vb/module1truncated.vb#2)]  
  
## <a name="static-and-dynamic-event-handlers"></a>Manipuladores de eventos estáticos e dinâmicos  

O .NET permite que os assinantes se registrem para receber notificações de eventos de modo estático ou dinâmico. Os manipuladores de eventos estáticos permanecem em vigor por toda a vida da classe cujos eventos eles manipulam. Os manipuladores de eventos dinâmicos são ativados e desativados explicitamente durante a execução do programa, geralmente em resposta a alguma lógica de programa condicional. Por exemplo, eles podem ser usados se as notificações de eventos forem necessárias apenas sob determinadas condições, ou se um aplicativo fornecer vários manipuladores de eventos e as condições de tempo de execução definirem o apropriado para uso. O exemplo na seção anterior mostra como adicionar dinamicamente um manipulador de eventos. Para obter mais informações, veja [Eventos](../../visual-basic/programming-guide/language-features/events/index.md) (no Visual Basic) e [Eventos](../../csharp/programming-guide/events/index.md) (em C#).  
  
## <a name="raising-multiple-events"></a>Acionando vários eventos  
 Se sua classe acionar vários eventos, o compilador vai gerar um campo por instância de representante de evento. Se o número de eventos for grande, o custo de armazenamento de um campo por representante pode não ser aceitável. Para esses casos, o .NET fornece propriedades de evento que você pode usar com outra estrutura de dados de sua escolha para armazenar representantes de eventos.  
  
 As propriedades de evento consistem em declarações de evento acompanhadas por acessadores de evento. Os acessadores de evento são métodos que você define para adicionar ou remover instâncias de representante de evento da estrutura de dados de armazenamento. Observe que as propriedades de evento são mais lentas que os campos de evento, pois cada representante de evento deve ser recuperado para que possa ser invocado. A compensação está entre a memória e a velocidade. Se sua classe define muitos eventos que raramente são acionados, você desejará implementar as propriedades de evento. Para saber mais, confira [Como manipular vários eventos usando propriedades de evento](how-to-handle-multiple-events-using-event-properties.md).  
  
## <a name="related-topics"></a>Tópicos relacionados  
  
|Title|Descrição|  
|-----------|-----------------|  
|[Como acionar e consumir eventos](how-to-raise-and-consume-events.md)|Contém exemplos de como acionar e consumir eventos.|  
|[Como manipular vários eventos usando propriedades de evento](how-to-handle-multiple-events-using-event-properties.md)|Mostrar como usar propriedades de evento para manipular vários eventos.|  
|[Padrão de design do observador](observer-design-pattern.md)|Descreve o padrão de design que permite a um assinante se registrar em um provedor e receber notificações dele.|  
|[Como consumir eventos em um aplicativo Web Forms](how-to-consume-events-in-a-web-forms-application.md)|Mostra como manipular um evento acionado por um controle do Web Forms.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.EventHandler>
- <xref:System.EventHandler%601>
- <xref:System.EventArgs>
- <xref:System.Delegate>
- [Eventos (Visual Basic)](../../visual-basic/programming-guide/language-features/events/index.md)
- [Eventos (guia de programação em C#)](../../csharp/programming-guide/events/index.md)
- [Visão geral de eventos e eventos roteados (aplicativos UWP)](/windows/uwp/xaml-platform/events-and-routed-events-overview)
