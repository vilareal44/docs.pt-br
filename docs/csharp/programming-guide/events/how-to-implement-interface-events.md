---
title: Como implementar eventos de interface – guia de programação C#
description: Saiba como implementar eventos de interface em uma classe. Consulte exemplos de código e exiba recursos adicionais disponíveis.
ms.date: 07/20/2015
helpviewer_keywords:
- interfaces [C#], event implementation in classes
- events [C#], in interfaces
ms.assetid: 63527447-9535-4880-8e95-35e2075827df
ms.openlocfilehash: bd86aed4f8d8ac6e291c11fe441f87ac97593b03
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302120"
---
# <a name="how-to-implement-interface-events-c-programming-guide"></a>Como implementar eventos de interface (guia de programação C#)
Um [interface](../../language-reference/keywords/interface.md) pode declarar uma [evento](../../language-reference/keywords/event.md). O exemplo a seguir mostra como implementar eventos de interface em uma classe. Basicamente, as regras são as mesmas aplicadas à implementação de qualquer método ou propriedade de interface.  
  
## <a name="to-implement-interface-events-in-a-class"></a>Implementar eventos de interface em uma classe  
  
Declare o evento na classe e, em seguida, invoque-o nas áreas apropriadas.  
  
```csharp
namespace ImplementInterfaceEvents  
{  
    public interface IDrawingObject  
    {  
        event EventHandler ShapeChanged;  
    }  
    public class MyEventArgs : EventArgs
    {  
        // class members  
    }  
    public class Shape : IDrawingObject  
    {  
        public event EventHandler ShapeChanged;  
        void ChangeShape()  
        {  
            // Do something here before the event…  

            OnShapeChanged(new MyEventArgs(/*arguments*/));  

            // or do something here after the event.
        }  
        protected virtual void OnShapeChanged(MyEventArgs e)  
        {  
            ShapeChanged?.Invoke(this, e);  
        }  
    }  

}  
```  
  
## <a name="example"></a>Exemplo  
O exemplo a seguir mostra como lidar com a situação menos comum, na qual a classe herda de duas ou mais interfaces e cada interface tem um evento com o mesmo nome. Nessa situação, é necessário fornecer uma implementação explícita da interface para pelo menos um dos eventos. Ao gravar uma implementação explícita da interface de um evento, também é necessário gravar os acessadores de evento `add` e `remove`. Normalmente, eles são fornecidos pelo compilador, mas nesse caso o compilador não pode fornecê-los.  
  
Ao fornecer acessadores próprios, é possível especificar se os dois eventos são representados pelo mesmo evento na classe ou por eventos diferentes. Por exemplo, se os eventos forem gerados em horários diferentes, de acordo com as especificações da interface, será possível associar cada evento a uma implementação separada na classe. No exemplo a seguir, os assinantes determinam qual evento `OnDraw` receberão ao converter a referência de forma para um `IShape` ou um `IDrawingObject`.  
  
 [!code-csharp[csProgGuideEvents#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideEvents/CS/Events.cs#10)]
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Eventos](./index.md)
- [Representantes](../delegates/index.md)
- [Implementação de interface explícita](../interfaces/explicit-interface-implementation.md)
- [Como acionar eventos de classe base em classes derivadas](./how-to-raise-base-class-events-in-derived-classes.md)
