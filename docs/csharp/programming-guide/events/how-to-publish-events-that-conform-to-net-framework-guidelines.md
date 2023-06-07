---
title: Publicar eventos que estão em conformidade com as diretrizes do .NET – guia de programação C#
description: Saiba como publicar eventos que estão em conformidade com as diretrizes do .NET. Todos os eventos na biblioteca de classes de .NET Framework são baseados no delegado EventHandler.
ms.date: 05/26/2020
helpviewer_keywords:
- events [C#], implementation guidelines
ms.assetid: 9310ae16-8627-44a2-b08c-05e5976202b1
ms.openlocfilehash: 1b802e236026911b55bafcb3f48d487c43bba174
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302107"
---
# <a name="how-to-publish-events-that-conform-to-net-guidelines-c-programming-guide"></a>Como publicar eventos que estão em conformidade com as diretrizes do .NET (guia de programação C#)

O procedimento a seguir demonstra como adicionar eventos que seguem o padrão .NET padrão para suas classes e estruturas. Todos os eventos na biblioteca de classes do .NET Framework se baseiam no delegado <xref:System.EventHandler>, que é definido da seguinte maneira:

```csharp
public delegate void EventHandler(object sender, EventArgs e);
```

> [!NOTE]
> .NET Framework 2,0 apresenta uma versão genérica desse delegado, <xref:System.EventHandler%601> . Os exemplos a seguir mostram como usar as duas versões.

Embora eventos em classes que você define possam ser baseados em qualquer tipo delegado válido, até mesmo delegados que retornam um valor, geralmente é recomendável que você baseie seus eventos no padrão .NET usando <xref:System.EventHandler> , conforme mostrado no exemplo a seguir.

O nome `EventHandler` pode levar a um pouco de confusão, pois ela não lida realmente com o evento. Os <xref:System.EventHandler> genéricos, e <xref:System.EventHandler%601> são tipos delegados. Um método ou uma expressão lambda cuja assinatura corresponde à definição de delegado é o *manipulador de eventos* e será invocado quando o evento for gerado.

## <a name="publish-events-based-on-the-eventhandler-pattern"></a>Publicar eventos com base no padrão EventHandler

1. (Pule esta etapa e vá para a etapa 3a se você não precisar enviar dados personalizados com seu evento.) Declare a classe para seus dados personalizados em um escopo que seja visível para as classes de Publicador e de assinante. Em seguida, adicione os membros necessários para manter seus dados de evento personalizados. Neste exemplo, uma cadeia de caracteres simples é retornada.

    ```csharp
    public class CustomEventArgs : EventArgs
    {
        public CustomEventArgs(string message)
        {
            Message = message;
        }

        public string Message { get; set; }
    }
    ```

2. (Ignore esta etapa se você estiver usando a versão genérica do <xref:System.EventHandler%601> .) Declare um delegado em sua classe de publicação. Dê um nome que termine com `EventHandler` . O segundo parâmetro especifica o `EventArgs` tipo personalizado.

    ```csharp
    public delegate void CustomEventHandler(object sender, CustomEventArgs args);
    ```

3. Declare o evento em sua classe de publicação, usando uma das etapas a seguir.

    1. Se você não tiver uma classe EventArgs personalizada, o tipo de evento será o delegado EventHandler não genérico. Você não precisa declarar o delegado porque ele já está declarado no namespace <xref:System> que está incluído quando você cria seu projeto do C#. Adicione o seguinte código à sua classe publicadora.

        ```csharp
        public event EventHandler RaiseCustomEvent;
        ```

    2. Se você estiver usando a versão não genérica de <xref:System.EventHandler> e você tem uma classe personalizada derivada de <xref:System.EventArgs>, declare o evento dentro de sua classe de publicação e use o delegado da etapa 2 como o tipo.

        ```csharp
        public event CustomEventHandler RaiseCustomEvent;
        ```

    3. Se você estiver usando a versão genérica, não é necessário um delegado personalizado. Em vez disso, na sua classe de publicação, especifique o tipo de evento como `EventHandler<CustomEventArgs>`, substituindo o nome da sua própria classe entre os colchetes angulares.

        ```csharp
        public event EventHandler<CustomEventArgs> RaiseCustomEvent;
        ```

## <a name="example"></a>Exemplo

O exemplo a seguir demonstra as etapas anteriores, usando uma classe EventArgs personalizada e o <xref:System.EventHandler%601> como o tipo de evento.

[!code-csharp[csProgGuideEvents#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideEvents/CS/Events.cs#2)]

## <a name="see-also"></a>Confira também

- <xref:System.Delegate>
- [Guia de programação C#](../index.md)
- [Eventos](index.md)
- [Representantes](../delegates/index.md)
