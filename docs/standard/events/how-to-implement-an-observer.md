---
title: Como implementar um observador
description: Implemente um observador no .NET. O padrão de design observador requer uma divisão entre um observador, que se registra para notificações e um provedor.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- observers [.NET Framework], observer design pattern
- observer design pattern [.NET Framework], implementing observers
ms.assetid: 8ecfa9f5-b500-473d-bcf0-5652ffb1e53d
ms.openlocfilehash: 43236ead15be0777f4284ba553a2f2f5e09d0a73
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768989"
---
# <a name="how-to-implement-an-observer"></a>Como implementar um observador
O padrão de design do observador exige uma divisão entre um observador, que registra as notificações, e um provedor, que monitora os dados e envia notificações e um ou mais observadores. Este tópico discute como criar um observador. Um tópico relacionado, [Como implementar um provedor](how-to-implement-a-provider.md), descreve como criar um provedor.  
  
### <a name="to-create-an-observer"></a>Para criar um observador  
  
1. Defina o observador, que é um tipo que implementa a interface <xref:System.IObserver%601?displayProperty=nameWithType>. Por exemplo, o código a seguir define um tipo chamado `TemperatureReporter`, que é uma implementação <xref:System.IObserver%601?displayProperty=nameWithType> construída com um argumento de tipo genérico de `Temperature`.  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/observer.cs#8)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/observer.vb#8)]  
  
2. Se o observador puder interromper o recebimento de notificações antes de o provedor chamar sua implementação de <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType>, defina uma variável privada que armazenará a implementação de <xref:System.IDisposable> retornada pelo método <xref:System.IObservable%601.Subscribe%2A?displayProperty=nameWithType> do provedor. Você também deve definir um método de assinatura que chama o método <xref:System.IObservable%601.Subscribe%2A> do provedor e armazena o objeto <xref:System.IDisposable> retornado. Por exemplo, o código a seguir define uma variável privada chamada `unsubscriber`, e define um método `Subscribe` que chama o método <xref:System.IObservable%601.Subscribe%2A> do provedor e atribui o objeto retornado à variável `unsubscriber`.  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/observer.cs#9)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/observer.vb#9)]  
  
3. Defina um método que permite ao observador interromper o recebimento de notificações antes de o provedor chamar sua implementação de <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType>, se esse recurso for necessário. O exemplo a seguir define um método `Unsubscribe`.  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/observer.cs#10)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/observer.vb#10)]  
  
4. Forneça implementações dos três métodos definidos pela interface <xref:System.IObserver%601>: <xref:System.IObserver%601.OnNext%2A?displayProperty=nameWithType>, <xref:System.IObserver%601.OnError%2A?displayProperty=nameWithType> e <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType>. Dependendo do provedor e das necessidades do aplicativo, os métodos <xref:System.IObserver%601.OnError%2A> e <xref:System.IObserver%601.OnCompleted%2A> podem ser implementações de stub. Observe que o método <xref:System.IObserver%601.OnError%2A> não deve tratar do objeto <xref:System.Exception> transmitido como uma exceção, e o método <xref:System.IObserver%601.OnCompleted%2A> é livre para chamar a implementação <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> do provedor. O exemplo a seguir mostra a implementação <xref:System.IObserver%601> da classe `TemperatureReporter`.  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/observer.cs#11)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/observer.vb#11)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir contém o código-fonte completo para a classe `TemperatureReporter`, que fornece a implementação <xref:System.IObserver%601> para uma aplicativo de monitoramento de temperatura.  
  
 [!code-csharp[Conceptual.ObserverDesign.HowTo#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/observer.cs#12)]
 [!code-vb[Conceptual.ObserverDesign.HowTo#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/observer.vb#12)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.IObserver%601>
- [Padrão de design do observador](observer-design-pattern.md)
- [Como implementar um provedor](how-to-implement-a-provider.md)
- [Práticas recomendadas para o padrão de design do observador](observer-design-pattern-best-practices.md)
