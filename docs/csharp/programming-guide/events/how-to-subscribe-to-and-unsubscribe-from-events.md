---
title: Como assinar e cancelar a assinatura de eventos – guia de programação C#
description: Saiba como assinar e cancelar a assinatura de eventos. Assine eventos usando o IDE do Visual Studio, programaticamente, ou usando um método anônimo.
ms.date: 07/20/2015
helpviewer_keywords:
- event handlers [C#], creating
- Code Editor, event handlers
- events [C#], creating using the IDE
ms.assetid: 6319f39f-282c-4173-8a62-6c4657cf51cd
ms.openlocfilehash: 4aecbbd58268e7b50a34f503160edd1eca4fe659
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063619"
---
# <a name="how-to-subscribe-to-and-unsubscribe-from-events-c-programming-guide"></a>Como assinar e cancelar a assinatura de eventos (guia de programação C#)
Você assina um evento publicado por outra classe quando quer escrever um código personalizado que é chamado quando esse evento é gerado. Por exemplo, você pode assinar o evento `click` de um botão para fazer com que seu aplicativo faça algo útil quando o usuário clicar no botão.  
  
### <a name="to-subscribe-to-events-by-using-the-visual-studio-ide"></a>Para assinar eventos usando o IDE do Visual Studio  
  
1. Se você não vir a janela **Propriedades**, no modo de exibição de **Design**, clique com o botão direito do mouse no formulário ou controle para o qual deseja criar um manipulador de eventos e selecione **Propriedades**.  
  
2. Na parte superior da janela **Propriedades**, clique no ícone **Eventos**.  
  
3. Clique duas vezes no evento que deseja criar, por exemplo, o evento `Load`.  
  
     O Visual C# cria um método de manipulador de eventos vazio e adiciona-o ao código. Como alternativa, você pode adicionar o código manualmente no modo de exibição **Código**. Por exemplo, as linhas de código a seguir declaram um método de manipulador de eventos que será chamado quando a classe `Form` gerar o evento `Load`.  
  
     [!code-csharp[csProgGuideEvents#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideEvents/CS/Events.cs#11)]  
  
     A linha de código que é necessária para assinar o evento também é gerada automaticamente no método `InitializeComponent` no arquivo Form1.Designer.cs em seu projeto. Ele é semelhante a isto:  
  
    ```csharp
    this.Load += new System.EventHandler(this.Form1_Load);  
    ```  
  
### <a name="to-subscribe-to-events-programmatically"></a>Para assinar eventos de forma programática  
  
1. Defina um método de manipulador de eventos cuja assinatura corresponda à assinatura do delegado do evento. Por exemplo, se o evento se basear no tipo de delegado <xref:System.EventHandler>, o código a seguir representará o stub do método:  
  
    ```csharp
    void HandleCustomEvent(object sender, CustomEventArgs a)  
    {  
       // Do something useful here.  
    }  
    ```  
  
2. Use o operador de atribuição de adição (`+=`) para anexar um manipulador de eventos ao evento. No exemplo a seguir, suponha que um objeto chamado `publisher` tem um evento chamado `RaiseCustomEvent`. Observe que a classe do assinante precisa de uma referência à classe do editor para assinar seus eventos.  
  
    ```csharp
    publisher.RaiseCustomEvent += HandleCustomEvent;  
    ```  
  
     Observe que a sintaxe anterior é nova no C# 2.0. Ela é exatamente equivalente à sintaxe C# 1.0, em que o delegado de encapsulamento deve ser criado explicitamente usando a palavra-chave `new`:  
  
    ```csharp
    publisher.RaiseCustomEvent += new CustomEventHandler(HandleCustomEvent);  
    ```  
  
     Você também pode usar uma [expressão lambda](../../language-reference/operators/lambda-expressions.md) para especificar um manipulador de eventos:
  
    ```csharp
    public Form1()  
    {  
        InitializeComponent();  
        this.Click += (s,e) =>
            {
                MessageBox.Show(((MouseEventArgs)e).Location.ToString());
            };
    }  
    ```  
  
### <a name="to-subscribe-to-events-by-using-an-anonymous-method"></a>Para assinar eventos usando um método anônimo  
  
- Se não precisar cancelar a assinatura de um evento posteriormente, você pode usar o operador de atribuição de adição (`+=`) para anexar um método anônimo ao evento. No exemplo a seguir, suponha que um objeto chamado `publisher` tenha um evento chamado `RaiseCustomEvent` e que uma classe `CustomEventArgs` também tenha sido definida para conter algum tipo de informação de evento específico. Observe que a classe do assinante precisa de uma referência a `publisher` para assinar seus eventos.  
  
    ```csharp
    publisher.RaiseCustomEvent += delegate(object o, CustomEventArgs e)  
    {  
      string s = o.ToString() + " " + e.ToString();  
      Console.WriteLine(s);  
    };  
    ```  
  
     É importante observar que você não pode, com facilidade, cancelar a assinatura de um evento se tiver usado uma função anônima para assiná-lo. Para cancelar a assinatura nesse cenário, é necessário voltar ao código em que você assinou o evento, armazenar o método anônimo em uma variável do delegado e, então, adicionar o delegado ao evento. De modo geral, é recomendável que você não use funções anônimas para assinar eventos se precisar cancelar a assinatura do evento posteriormente no código. Para obter mais informações sobre funções anônimas, consulte [Funções anônimas](../statements-expressions-operators/anonymous-functions.md).  
  
## <a name="unsubscribing"></a>Cancelando a assinatura  
 Para impedir que o manipulador de eventos seja invocado quando o evento for gerado, cancele a assinatura do evento. Para evitar perda de recursos, cancele a assinatura de eventos antes de descartar um objeto de assinante. Até que você cancele a assinatura de um evento, o delegado multicast subjacente ao evento no objeto de publicação terá uma referência ao delegado que encapsula o manipulador de eventos do assinante. Desde que o objeto de publicação contenha essa referência, a coleta de lixo não excluirá seu objeto de assinante.  
  
#### <a name="to-unsubscribe-from-an-event"></a>Para cancelar a assinatura de um evento  
  
- Use o operador de atribuição de subtração (`-=`) para cancelar a assinatura de um evento:  
  
    ```csharp
    publisher.RaiseCustomEvent -= HandleCustomEvent;  
    ```  
  
     Quando todos os assinantes tiverem cancelado a assinatura de um evento, a instância do evento na classe do publicador será definida como `null`.  
  
## <a name="see-also"></a>Consulte também

- [Eventos](./index.md)
- [event](../../language-reference/keywords/event.md)
- [Como publicar eventos que estão em conformidade com as diretrizes do .NET](./how-to-publish-events-that-conform-to-net-framework-guidelines.md)
- [-e-= operadores](../../language-reference/operators/subtraction-operator.md)
- [operadores + e + =](../../language-reference/operators/addition-operator.md)
