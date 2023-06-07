---
title: Como implementar um provedor
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- observer design pattern [.NET Framework], implementing providers
- providers [.NET Framework], in observer design pattern
- observables [.NET Framework], in observer design pattern
ms.assetid: 790b5d8b-d546-40a6-beeb-151b574e5ee5
ms.openlocfilehash: 4f8a213c0df3ef3c633106b7249a4947fe77c0d2
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84280017"
---
# <a name="how-to-implement-a-provider"></a>Como implementar um provedor
O padrão de design do observador requer uma divisão entre um provedor, que monitora os dados e envia notificações e um ou mais observadores, que recebem notificações (retornos de chamada) do provedor. Este tópico discute como criar um provedor. Um tópico relacionado, [Como implementar um observador](how-to-implement-an-observer.md), descreve como criar um observador.  
  
### <a name="to-create-a-provider"></a>Para criar um provedor  
  
1. Defina os dados que o provedor é responsável por enviar para os observadores. Embora o provedor e os dados que ele envia para observadores possam ser um tipo único, geralmente são representados por tipos diferentes. Por exemplo, em um aplicativo de monitoramento de temperatura, a estrutura `Temperature` define os dados que o provedor (que é representado pela classe `TemperatureMonitor` definida na próxima etapa) monitora e quais observadores assinar.  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/data.cs#1)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/data.vb#1)]  
  
2. Defina o provedor de dados, que é um tipo que implementa a interface <xref:System.IObservable%601?displayProperty=nameWithType>. O argumento de tipo genérico do provedor é o tipo que o provedor envia a observadores. O exemplo a seguir define uma classe `TemperatureMonitor`, que é uma implementação construída <xref:System.IObservable%601?displayProperty=nameWithType> com um argumento de tipo genérico de `Temperature`.  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/provider.cs#2)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/provider.vb#2)]  
  
3. Determine como o provedor armazena as referências a observadores para que cada observador possa ser notificado quando apropriado. Normalmente, um objeto de coleção como um objeto genérico <xref:System.Collections.Generic.List%601> é usado para essa finalidade. O exemplo a seguir define um objeto particular <xref:System.Collections.Generic.List%601> que é instanciado no constructo de classe `TemperatureMonitor`.  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/provider.cs#3)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/provider.vb#3)]  
  
4. Defina uma implementação <xref:System.IDisposable> que o provedor possa retornar para os assinantes para que eles possam interromper o recebimento de notificações a qualquer momento. O exemplo a seguir define uma classe `Unsubscriber` aninhada para a qual é passada uma referência à coleção de assinantes e ao assinante quando a classe é instanciada. Esse código permite que o assinante chame a implementação <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> do objeto para se remover da coleção de assinantes.  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/provider.cs#4)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/provider.vb#4)]  
  
5. Implementar o método de <xref:System.IObservable%601.Subscribe%2A?displayProperty=nameWithType> . O método recebe uma referência para a interface <xref:System.IObserver%601?displayProperty=nameWithType> e deve ser armazenado no objeto criado para essa finalidade na etapa 3. O método deve retornar a implementação <xref:System.IDisposable> desenvolvida na etapa 4. O exemplo a seguir mostra a implementação do método <xref:System.IObservable%601.Subscribe%2A> na classe `TemperatureMonitor`.  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/provider.cs#5)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/provider.vb#5)]  
  
6. Notificar os observadores conforme apropriado, chamando as implementações <xref:System.IObserver%601.OnNext%2A?displayProperty=nameWithType>, <xref:System.IObserver%601.OnError%2A?displayProperty=nameWithType> e <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType>. Em alguns casos, um provedor não pode chamar o método <xref:System.IObserver%601.OnError%2A> quando ocorre um erro. Por exemplo, o método `GetTemperature` a seguir simula um monitor que lê dados de temperatura a cada cinco segundos e notifica os observadores se a temperatura foi alterada em pelo menos 0,1 grau desde a leitura anterior. Se o dispositivo não relatar uma temperatura (ou seja, se o valor for nulo), o provedor notificará observadores de que a transmissão foi concluída. Observe que, além de chamar o método <xref:System.IObserver%601.OnCompleted%2A> de cada observador, o método `GetTemperature` limpa a coleção <xref:System.Collections.Generic.List%601>. Nesse caso, o provedor não torna nenhuma chamada para o método <xref:System.IObserver%601.OnError%2A> de seus observadores.  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/provider.cs#6)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/provider.vb#6)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir contém o código-fonte completo para definir uma implementação <xref:System.IObservable%601> para uma aplicativo de monitoramento de temperatura. Ele inclui a estrutura `Temperature`, os dados enviados para observadores e a classe `TemperatureMonitor`, que é a implementação <xref:System.IObservable%601>.  
  
 [!code-csharp[Conceptual.ObserverDesign.HowTo#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/provider.cs#7)]
 [!code-vb[Conceptual.ObserverDesign.HowTo#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/provider.vb#7)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.IObservable%601>
- [Padrão de design do observador](observer-design-pattern.md)
- [Como: implementar um observador](how-to-implement-an-observer.md)
- [Práticas recomendadas para o padrão de design do observador](observer-design-pattern-best-practices.md)
