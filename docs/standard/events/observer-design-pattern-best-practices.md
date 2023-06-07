---
title: Práticas recomendadas para o padrão de design do observador
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- observer design pattern [.NET Framework], best practices
- best practices [.NET Framework], observer design pattern
ms.assetid: c834760f-ddd4-417f-abb7-a059679d5b8c
ms.openlocfilehash: b4f8e568dcb6790dac1dc8fc5c969d6fa1367c4e
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288453"
---
# <a name="observer-design-pattern-best-practices"></a>Práticas recomendadas para o padrão de design do observador
No .NET Framework, o padrão de design de observador é implementado como um conjunto de interfaces. A interface <xref:System.IObservable%601?displayProperty=nameWithType> representa o provedor de dados, que também é responsável por fornecer uma implementação <xref:System.IDisposable> que permite que os observadores cancelem a assinatura de notificações. A interface <xref:System.IObserver%601?displayProperty=nameWithType> representa o observador. Este tópico descreve as práticas recomendadas que os desenvolvedores devem seguir ao implementar o padrão de design de observador usando essas interfaces.  
  
## <a name="threading"></a>Threading  
 Normalmente, um provedor implementa o método <xref:System.IObservable%601.Subscribe%2A?displayProperty=nameWithType> adicionando um observador específico a uma lista de assinantes que é representada por algum objeto de coleção e implementa o método <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> removendo um determinado observador da lista de assinantes. Um observador pode chamar esses métodos a qualquer momento. Além disso, como o contrato de provedor/observador não especifica quem é responsável por cancelar a assinatura após o método de retorno de chamada <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType>, o provedor e o observador podem tentar ambos remover o mesmo membro da lista. Devido a essa possibilidade, tanto o método <xref:System.IObservable%601.Subscribe%2A> quanto o método <xref:System.IDisposable.Dispose%2A> devem ser thread-safe. Normalmente, isso envolve o uso de uma [coleção simultânea](../parallel-programming/data-structures-for-parallel-programming.md) ou um bloqueio. As implementações que não são thread-safe devem documentar explicitamente esse fato.  
  
 Quaisquer garantias adicionais devem ser especificadas em uma camada no início do contrato de provedor/observador. Os implementadores devem chamar claramente ao imporem requisitos adicionais para evitar confusão do usuário sobre o contrato do observador.  
  
## <a name="handling-exceptions"></a>Tratando exceções  
 Devido ao fraco acoplamento entre um provedor de dados e um observador, as exceções no padrão de design do observador devem ser informativas. Isso afeta como os provedores e observadores manipulam exceções no padrão de design do observador.  
  
### <a name="the-provider----calling-the-onerror-method"></a>O provedor – Chamando o método OnError  
 O método <xref:System.IObserver%601.OnError%2A> serve como uma mensagem informativa para observadores, da mesma forma que o método <xref:System.IObserver%601.OnNext%2A?displayProperty=nameWithType>. No entanto, o método <xref:System.IObserver%601.OnNext%2A> foi projetado para fornecer a um observador dados atuais ou atualizados, enquanto o método <xref:System.IObserver%601.OnError%2A> foi projetado para indicar que o provedor não é capaz de fornecer dados válidos.  
  
 O provedor deve seguir essas práticas recomendadas ao manipular exceções e chamar o método <xref:System.IObserver%601.OnError%2A>:  
  
- O provedor deverá manipular suas próprias exceções se houver algum requisito específico.  
  
- O provedor não deve esperar ou exigir que os observadores manipulem exceções de alguma maneira específica.  
  
- O provedor deve chamar o método <xref:System.IObserver%601.OnError%2A> ao manipular uma exceção que comprometa sua capacidade de fornecer atualizações. Informações sobre essas exceções podem ser passadas para o observador. Em outros casos, não há necessidade de notificar os observadores com relação a uma exceção.  
  
 Uma vez que o provedor chame o método <xref:System.IObserver%601.OnError%2A> ou <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType>, não deverá haver nenhuma notificação adicional e o provedor poderá cancelar a assinatura de seus observadores. No entanto, os observadores podem também cancelar eles próprios sua assinatura a qualquer momento, inclusive antes e depois de receberem uma notificação <xref:System.IObserver%601.OnError%2A> ou <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType>. O padrão de design do observador não determina se o provedor ou o observador é responsável pelo cancelamento da assinatura; portanto, é possível que ambos tentem cancelar a assinatura. Normalmente, quando os observadores cancelam a assinatura, eles são removidos de uma coleção de assinantes. Em um aplicativo de thread único, a implementação <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> deve garantir que uma referência de objeto seja válida e que o objeto seja um membro da coleção de assinantes antes de tentar removê-lo. Em um aplicativo com multithread, deve-se usar um objeto de coleção thread-safe, tal como um objeto <xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType>.  
  
### <a name="the-observer----implementing-the-onerror-method"></a>O observador – Implementando o método OnError  
 Quando um observador recebe uma notificação de erro de um provedor, o observador deve tratar a exceção como informativa e não deve ser necessária qualquer ação específica.  
  
 O observador deve seguir essas práticas recomendadas ao responder a uma chamada de método <xref:System.IObserver%601.OnError%2A> de um provedor:  
  
- O observador não deve lançar exceções de suas implementações de interface, tais como <xref:System.IObserver%601.OnNext%2A> ou <xref:System.IObserver%601.OnError%2A>. No entanto, se o observador lançar exceções, ele deverá esperar que essas exceções fiquem sem tratamento.  
  
- Para preservar a pilha de chamadas, um observador que deseja gerar um objeto <xref:System.Exception>, que foi passado para o seu método <xref:System.IObserver%601.OnError%2A>, deveria encapsular a exceção antes de lançá-la. Um objeto de exceção padrão deve ser usado para essa finalidade.  
  
## <a name="additional-best-practices"></a>Práticas recomendadas adicionais  
 A tentativa de cancelar registro no método <xref:System.IObservable%601.Subscribe%2A?displayProperty=nameWithType> pode resultar em uma referência nula. Portanto, é recomendável que você evite essa prática.  
  
 Embora seja possível anexar um observador para vários provedores, o padrão recomendado é anexar uma <xref:System.IObserver%601> instância a uma única instância <xref:System.IObservable%601>.  
  
## <a name="see-also"></a>Veja também

- [Padrão de design do observador](observer-design-pattern.md)
- [Como: implementar um observador](how-to-implement-an-observer.md)
- [Como implementar um provedor](how-to-implement-a-provider.md)
