---
title: Visão geral de BlockingCollection
description: Leia sobre BlockingCollection <T> , uma classe de coleção thread-safe no .net. Essa classe oferece recursos como a adição simultânea de & a execução de itens de vários threads.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- BlockingCollection, overview
ms.assetid: 987ea3d7-0ad5-4238-8b64-331ce4eb3f0b
ms.openlocfilehash: fc11f6c28a551e56d3bac4c5be9c08a396c0c6b1
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600796"
---
# <a name="blockingcollection-overview"></a>Visão geral de BlockingCollection
<xref:System.Collections.Concurrent.BlockingCollection%601> é uma classe de coleção thread-safe que fornece os seguintes recursos:  
  
- Uma implementação do padrão de produtor-consumidor.  
  
- Adição e remoção simultâneas de itens de vários threads.  
  
- Capacidade máxima opcional.  
  
- Operações de inserção e remoção que bloqueiam quando a coleção está vazia ou cheia.  
  
- Operações “try” de inserção e remoção que não bloqueiam ou bloqueiam até um período específico.  
  
- Encapsula qualquer tipo de coleção que implemente <xref:System.Collections.Concurrent.IProducerConsumerCollection%601>  
  
- Cancelamento com tokens de cancelamento.  
  
- Dois tipos de enumeração com `foreach` (`For Each` no Visual Basic):  
  
    1. Enumeração de somente leitura.  
  
    2. Enumeração que remove itens conforme eles são enumerados.  
  
## <a name="bounding-and-blocking-support"></a>Suporte a delimitação e bloqueio  
 <xref:System.Collections.Concurrent.BlockingCollection%601> dá suporte à delimitação e ao bloqueio. Delimitação significa que você pode definir a capacidade máxima da coleção. A delimitação é importante em determinados cenários, porque permite que você controle o tamanho máximo da coleção na memória e impede que os threads de produção se distanciem muito a frente dos threads de consumo.  
  
 Várias tarefas ou threads podem adicionar itens à coleção simultaneamente e se a coleção atingir sua capacidade máxima especificada, os threads de produção serão bloqueados até que um item seja removido. Vários consumidores podem remover itens simultaneamente e, se a coleção ficar vazia, os threads de consumo serão bloqueados até que um produtor adicione um item. Um thread de produção pode chamar <xref:System.Collections.Concurrent.BlockingCollection%601.CompleteAdding%2A> para indicar que não serão adicionados mais itens. Os consumidores monitoram a propriedade <xref:System.Collections.Concurrent.BlockingCollection%601.IsCompleted%2A> para saber quando a coleção está vazia e não serão adicionados mais itens. O exemplo a seguir mostra uma BlockingCollection simples com uma capacidade limitada igual a 100. Uma tarefa de produtor adiciona itens à coleção contanto que algumas condições sejam verdadeiras e, em seguida, chama <xref:System.Collections.Concurrent.BlockingCollection%601.CompleteAdding%2A>. A tarefa de consumidor tira itens até que a propriedade <xref:System.Collections.Concurrent.BlockingCollection%601.IsCompleted%2A> seja true.  
  
 [!code-csharp[CDS_BlockingCollection#04](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/blockingcollection.cs#04)]
 [!code-vb[CDS_BlockingCollection#04](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_blockingcollection/vb/introsnippetsbc.vb#04)]  
  
 Para obter um exemplo completo, consulte [Como adicionar e tirar itens individualmente de uma BlockingCollection](how-to-add-and-take-items.md).  
  
## <a name="timed-blocking-operations"></a>Operações de bloqueio cronometrado  
 Nas operações <xref:System.Collections.Concurrent.BlockingCollection%601.TryAdd%2A> e <xref:System.Collections.Concurrent.BlockingCollection%601.TryTake%2A> de bloqueio cronometrado em coleções limitadas, o método tenta adicionar ou tirar um item. Se houver um item disponível, ele será colocado na variável que foi passada por referência e o método retornará true. Se nenhum item for recuperado após um período de tempo limite especificado, o método retornará false. O thread fica, então, livre para realizar outro trabalho útil antes de tentar acessar a coleção novamente. Para obter um exemplo de acesso com bloqueio cronometrado, consulte o segundo exemplo em [Como adicionar e tirar itens individualmente de uma BlockingCollection](how-to-add-and-take-items.md).  
  
## <a name="cancelling-add-and-take-operations"></a>Cancelando as operações Add e Take  
 As operações Add e Take normalmente são realizadas em um loop. Você pode cancelar um loop, passando um <xref:System.Threading.CancellationToken> para o método <xref:System.Collections.Concurrent.BlockingCollection%601.TryAdd%2A> ou <xref:System.Collections.Concurrent.BlockingCollection%601.TryTake%2A> e, em seguida, verificando o valor da propriedade <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> do token em cada iteração. Se o valor for true, caberá a você responder à solicitação de cancelamento limpando os recursos e saindo do loop. O exemplo a seguir mostra uma sobrecarga de <xref:System.Collections.Concurrent.BlockingCollection%601.TryAdd%2A> que usa um token de cancelamento e o código que ele usa:  
  
 [!code-csharp[CDS_BlockingCollection#05](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/blockingcollection.cs#05)]
 [!code-vb[CDS_BlockingCollection#05](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_blockingcollection/vb/introsnippetsbc.vb#05)]  
  
 Para obter um exemplo de como adicionar suporte a cancelamento, veja o segundo exemplo em [Instruções: adicionar e remover itens individualmente de uma BlockingCollection](how-to-add-and-take-items.md).  
  
## <a name="specifying-the-collection-type"></a>Especificando o tipo de coleção  
 Ao criar um <xref:System.Collections.Concurrent.BlockingCollection%601>, você pode especificar não apenas a capacidade limitada, mas também o tipo de coleção a ser usado. Por exemplo, seria possível especificar um <xref:System.Collections.Concurrent.ConcurrentQueue%601> para o comportamento PEPS (primeiro a entrar, primeiro a sair) ou um <xref:System.Collections.Concurrent.ConcurrentStack%601> para o comportamento UEPS (último a entrar, primeiro a sair). Você pode usar qualquer classe de coleção que implemente a interface <xref:System.Collections.Concurrent.IProducerConsumerCollection%601>. O tipo de coleção padrão para <xref:System.Collections.Concurrent.BlockingCollection%601> é <xref:System.Collections.Concurrent.ConcurrentQueue%601>. O exemplo de código a seguir mostra como criar um <xref:System.Collections.Concurrent.BlockingCollection%601> de cadeias de caracteres que tenha uma capacidade de 1000 e use um <xref:System.Collections.Concurrent.ConcurrentBag%601>:  
  
```vb  
Dim bc = New BlockingCollection(Of String)(New ConcurrentBag(Of String()), 1000)  
```  
  
```csharp  
BlockingCollection<string> bc = new BlockingCollection<string>(new ConcurrentBag<string>(), 1000 );  
```  
  
 Para obter mais informações, consulte [Instruções: adicionar as funcionalidades de limitação e bloqueio a uma coleção](how-to-add-bounding-and-blocking.md).  
  
## <a name="ienumerable-support"></a>Suporte a IEnumerable  
 O <xref:System.Collections.Concurrent.BlockingCollection%601> fornece um método <xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A> que permite que os consumidores usem `foreach` (`For Each` em Visual Basic) para remover itens até que a coleção seja concluída, ou seja, que ela esteja vazia e que não sejam adicionados mais itens. Para obter mais informações, confira [Como usar ForEach para remover itens de uma BlockingCollection](how-to-use-foreach-to-remove.md).  
  
## <a name="using-many-blockingcollections-as-one"></a>Usando vários BlockingCollections como um  
 Para cenários em que um consumidor precisa remover itens de várias coleções simultaneamente, é possível criar matrizes de <xref:System.Collections.Concurrent.BlockingCollection%601> e usar os métodos estáticos como <xref:System.Collections.Concurrent.BlockingCollection%601.TakeFromAny%2A> e <xref:System.Collections.Concurrent.BlockingCollection%601.AddToAny%2A> que adicionarão ou retirarão de qualquer uma das coleções na matriz. Se uma coleção for de bloqueio, o método imediatamente tenta outra até encontrar uma que possa realizar a operação. Para obter mais informações, confira [Como usar matrizes de coleções Blocking em um pipeline](how-to-use-arrays-of-blockingcollections.md).  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- [Coleções e estruturas de dados](../index.md)
- [Coleções com segurança de thread](index.md)
