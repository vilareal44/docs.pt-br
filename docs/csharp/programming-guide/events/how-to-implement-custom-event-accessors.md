---
title: Como implementar acessadores de eventos personalizados-guia de programação C#
description: Saiba como implementar acessadores de eventos personalizados. Consulte um exemplo de código e exiba recursos adicionais disponíveis.
ms.date: 07/20/2015
helpviewer_keywords:
- accessors [C#], event accessors
- add accessor [C#]
- events [C#], add accessor
- events [C#], remove accessor
- remove accessor [C#]
ms.assetid: bf903abf-03a4-4f7b-ab6b-b7e59bc2ee1e
ms.openlocfilehash: 4094aa1fedbceb68790b484608b3ea0ebc1e5cf6
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302133"
---
# <a name="how-to-implement-custom-event-accessors-c-programming-guide"></a>Como implementar acessadores de evento personalizados (guia de programação C#)
Um evento é um tipo especial de delegado multicast que só pode ser invocado de dentro da classe que ela está declarado. O código cliente assina o evento ao fornecer uma referência a um método que deve ser invocado quando o evento for disparado. Esses métodos são adicionados à lista de invocação do delegado por meio de acessadores de evento, que se assemelham aos acessadores de propriedade, com a exceção de que os acessadores de eventos são nomeados `add` e `remove`. Na maioria dos casos, não é necessário fornecer acessadores de eventos personalizados. Quando nenhum acessador de evento personalizado for fornecido no código, o compilador o adicionará automaticamente. No entanto, em alguns casos será necessário fornecer um comportamento personalizado. Um desses casos é mostrado no tópico [como implementar eventos de interface](./how-to-implement-interface-events.md).
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como implementar os acessadores de eventos personalizados adicionar e remover. Embora seja possível substituir qualquer código dentro dos acessadores, é recomendável que você bloqueie o evento antes de adicionar ou remover um novo método de manipulador de eventos.  
  
[!code-csharp[IDrawingObject.OnDraw](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideEvents/CS/Events.cs#IDrawingObjectOnDraw)]  
  
## <a name="see-also"></a>Veja também

- [Eventos](./index.md)
- [event](../../language-reference/keywords/event.md)
