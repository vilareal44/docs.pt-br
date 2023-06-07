---
title: Como gerar eventos de classe base em classes derivadas – guia de programação C#
description: Saiba como gerar eventos de classe base em classes derivadas. Consulte um exemplo de código e exiba recursos adicionais disponíveis.
ms.date: 07/20/2015
helpviewer_keywords:
- events [C#], in derived classes
ms.assetid: 2d20556a-0aad-46fc-845e-f85d86ea617a
ms.openlocfilehash: b0b0a16a1fd165e437fc79ccacb20d406f5cff63
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302094"
---
# <a name="how-to-raise-base-class-events-in-derived-classes-c-programming-guide"></a>Como gerar eventos de classe base em classes derivadas (guia de programação C#)
O exemplo simples a seguir mostra o modo padrão para declarar os eventos em uma classe base para que eles também possam ser gerados das classes derivadas. Esse padrão é usado extensivamente em Windows Forms classes nas bibliotecas de classes do .NET.  
  
 Quando você cria uma classe que pode ser usada como uma classe base para outras classes, deve considerar o fato de que os eventos são um tipo especial de delegado que pode ser invocado apenas de dentro da classe que os declarou. As classes derivadas não podem invocar diretamente eventos declarados dentro da classe base. Embora, às vezes, você possa desejar um evento que possa ser gerado apenas pela classe base, na maioria das vezes você deve habilitar a classe derivada para invocar os eventos de classe base. Para fazer isso, você pode criar um método de invocação protegido na classe base que encapsula o evento. Chamando ou substituindo esse método de invocação, as classes derivadas podem invocar o evento diretamente.  
  
> [!NOTE]
> Não declare eventos virtuais em uma classe base e substitua-os em uma classe derivada. O compilador C# não lida com eles corretamente e é imprevisível se um assinante do evento derivado realmente estará assinando o evento de classe base.  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[csProgGuideEvents#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideEvents/CS/Events.cs#1)]  
  
## <a name="see-also"></a>Confira também

- [Guia de programação C#](../index.md)
- [Eventos](./index.md)
- [Representantes](../delegates/index.md)
- [Modificadores de acesso](../classes-and-structs/access-modifiers.md)
- [Criando manipuladores de eventos no Windows Forms](../../../framework/winforms/creating-event-handlers-in-windows-forms.md)
