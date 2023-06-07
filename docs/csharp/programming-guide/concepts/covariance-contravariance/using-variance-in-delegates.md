---
title: Usando variação em delegações (C#)
description: Saiba como usar a variação em delegados usando os exemplos de código de covariância e contravariância incluídos.
ms.date: 07/20/2015
ms.assetid: 1638c95d-dc8b-40c1-972c-c2dcf84be55e
ms.openlocfilehash: 62b0555ee29c5e7d2ba0954a8949d61596122cc7
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105678"
---
# <a name="using-variance-in-delegates-c"></a>Usando variação em delegações (C#)
Quando você atribui um método a um delegado, a *covariância* e a *contravariância* fornece flexibilidade para corresponder um tipo de delegado a uma assinatura de método. A covariância permite que um método tenha o tipo de retorno mais derivado do que o definido no delegado. A contravariância permite que um método que tem tipos de parâmetro menos derivados do que no tipo delegado.  
  
## <a name="example-1-covariance"></a>Exemplo 1: covariância  
  
### <a name="description"></a>Descrição  
 Este exemplo demonstra como delegados podem ser usados com métodos que têm tipos de retorno que são derivados do tipo de retorno na assinatura do delegado. O tipo de dados retornado por `DogsHandler` é do tipo `Dogs`, que deriva do tipo `Mammals` definido no delegado.  
  
### <a name="code"></a>Código  
  
```csharp  
class Mammals {}  
class Dogs : Mammals {}  
  
class Program  
{  
    // Define the delegate.  
    public delegate Mammals HandlerMethod();  
  
    public static Mammals MammalsHandler()  
    {  
        return null;  
    }  
  
    public static Dogs DogsHandler()  
    {  
        return null;  
    }  
  
    static void Test()  
    {  
        HandlerMethod handlerMammals = MammalsHandler;  
  
        // Covariance enables this assignment.  
        HandlerMethod handlerDogs = DogsHandler;  
    }  
}  
```  
  
## <a name="example-2-contravariance"></a>Exemplo 2: contravariância  
  
### <a name="description"></a>Descrição

Este exemplo demonstra como representantes podem ser usados com métodos que têm parâmetros cujos tipos são tipos base do tipo de parâmetro de assinatura do representante. Com a contravariância, você pode usar um manipulador de eventos em vez de manipuladores separados. O seguinte exemplo usa dois representantes:

- Um representante <xref:System.Windows.Forms.KeyEventHandler> que define a assinatura do evento [Button.KeyDown](xref:System.Windows.Forms.Control.KeyDown). Sua assinatura é:

   ```csharp
   public delegate void KeyEventHandler(object sender, KeyEventArgs e)
   ```

- Um representante <xref:System.Windows.Forms.MouseEventHandler> que define a assinatura do evento [Button.MouseClick](xref:System.Windows.Forms.Control.MouseDown). Sua assinatura é:

   ```csharp
   public delegate void MouseEventHandler(object sender, MouseEventArgs e)
   ```

O exemplo define um manipulador de eventos com um parâmetro <xref:System.EventArgs> e o usa para manipular os eventos `Button.KeyDown` e `Button.MouseClick`. Ele pode fazer isso porque <xref:System.EventArgs> é um tipo base de <xref:System.Windows.Forms.KeyEventArgs> e <xref:System.Windows.Forms.MouseEventArgs>.
  
### <a name="code"></a>Código  
  
```csharp  
// Event handler that accepts a parameter of the EventArgs type.  
private void MultiHandler(object sender, System.EventArgs e)  
{  
    label1.Text = System.DateTime.Now.ToString();  
}  
  
public Form1()  
{  
    InitializeComponent();  
  
    // You can use a method that has an EventArgs parameter,  
    // although the event expects the KeyEventArgs parameter.  
    this.button1.KeyDown += this.MultiHandler;  
  
    // You can use the same method
    // for an event that expects the MouseEventArgs parameter.  
    this.button1.MouseClick += this.MultiHandler;  
  
}  
```  
  
## <a name="see-also"></a>Confira também

- [Variação em delegados (C#)](./variance-in-delegates.md)
- [Usando variação para delegados genéricos Func e Action (C#)](./using-variance-for-func-and-action-generic-delegates.md)
