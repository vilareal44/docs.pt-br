---
title: Eventos – Guia de Programação em C#
description: Saiba mais sobre os eventos. Eventos permitem que uma classe ou objeto notifique outras classes ou objetos quando algo interessante ocorrer.
ms.date: 07/20/2015
helpviewer_keywords:
- classes [C#], events
- C# language, events
- events [C#]
ms.assetid: a8e51b22-d294-44fb-9539-0072f06c4cb3
ms.openlocfilehash: 3160d1e381c6cb3af0f017538f9555b3fded9910
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302068"
---
# <a name="events-c-programming-guide"></a>Eventos (Guia de Programação em C#)
Eventos permitem que uma [classe](../../language-reference/keywords/class.md) ou objeto notifique outras classes ou objetos quando algo interessante ocorre. A classe que envia (ou *aciona*) o evento é chamada de *editor* e as classes que recebem (ou *manipulam*) os eventos são chamadas *assinantes*.  
  
Em um aplicativo Windows Forms em C# ou Web típico, você assina eventos acionados pelos controles, como botões e caixas de listagem. Você pode usar o IDE (ambiente de desenvolvimento integrado) do Visual C# para procurar os eventos que um controle publica e selecionar aqueles que você deseja manipular. O IDE oferece uma maneira fácil de adicionar automaticamente um método de manipulador de eventos vazio e o código para assinar o evento. Para obter mais informações, consulte [como assinar e cancelar a assinatura de eventos](./how-to-subscribe-to-and-unsubscribe-from-events.md).
  
## <a name="events-overview"></a>Visão geral sobre eventos  
 Os eventos têm as seguintes propriedades:  
  
- O editor determina quando um evento é acionado. Os assinantes determinam a ação que é executada em resposta ao evento.  
  
- Um evento pode ter vários assinantes. Um assinante pode manipular vários eventos de vários publicadores.  
  
- Eventos que não têm assinantes nunca são acionados.  
  
- Normalmente, os eventos são usados para sinalizar ações do usuário, como cliques de botão ou seleções de menu em interfaces gráficas do usuário.  
  
- Quando um evento tem vários assinantes, os manipuladores de eventos são invocados sincronicamente quando um evento é acionado. Para invocar eventos de forma assíncrona, consulte [Chamando métodos síncronos assincronamente](../../../standard/asynchronous-programming-patterns/calling-synchronous-methods-asynchronously.md).  
  
- Na biblioteca de classes .NET Framework, os eventos são baseados no delegado <xref:System.EventHandler> e na classe base <xref:System.EventArgs>.  
  
## <a name="related-sections"></a>Seções relacionadas  
 Para obter mais informações, veja:  
  
- [Como realizar e cancelar a assinatura de eventos](./how-to-subscribe-to-and-unsubscribe-from-events.md)

- [Como publicar eventos que estão em conformidade com as diretrizes do .NET](./how-to-publish-events-that-conform-to-net-framework-guidelines.md)

- [Como acionar eventos de classe base em classes derivadas](./how-to-raise-base-class-events-in-derived-classes.md)

- [Como implementar eventos de interface](./how-to-implement-interface-events.md)

- [Como implementar acessadores de eventos personalizados](./how-to-implement-custom-event-accessors.md)

## <a name="c-language-specification"></a>Especificação da Linguagem C#  

Para obter mais informações, veja [Eventos](~/_csharplang/spec/classes.md#events) na [Especificação da linguagem C#](/dotnet/csharp/language-reference/language-specification/introduction). A especificação da linguagem é a fonte definitiva para a sintaxe e o uso de C#.
  
## <a name="featured-book-chapters"></a>Capítulos do Livro em Destaque  
 [Expressões lambda, eventos e delegados](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff518994%28v=orm.10%29) em [C# 3.0 Cookbook, Third Edition: More than 250 solutions for C# 3.0 programmers](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff518995%28v=orm.10%29)  
  
 [Delegados e eventos](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff652490%28v=orm.10%29) em [Learning C# 3.0: Master the fundamentals of C# 3.0](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff652493%28v=orm.10%29)  
  
## <a name="see-also"></a>Veja também

- <xref:System.EventHandler>
- [Guia de programação C#](../index.md)
- [Representantes](../delegates/index.md)
- [Criando manipuladores de eventos no Windows Forms](../../../framework/winforms/creating-event-handlers-in-windows-forms.md)
