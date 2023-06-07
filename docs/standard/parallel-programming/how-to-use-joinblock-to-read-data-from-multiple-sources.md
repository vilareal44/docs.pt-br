---
title: 'Como: usar JoinBlock para ler dados de várias fontes'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Task Parallel Library, dataflows
- TPL dataflow library, joining blocks in
- dataflow blocks, joining in TPL
ms.assetid: e9c1ada4-ac57-4704-87cb-2f5117f8151d
ms.openlocfilehash: cd2f5c65f45d83ef23643dcc747a748bb8ba89d9
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290818"
---
# <a name="how-to-use-joinblock-to-read-data-from-multiple-sources"></a>Como: usar JoinBlock para ler dados de várias fontes
Este documento explica como usar a classe <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> para executar uma operação quando os dados estão disponíveis em várias fontes. Ele também demonstra como usar o modo não greedy para habilitar vários blocos de junção para compartilhar uma fonte de dados com mais eficiência.

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

## <a name="example"></a>Exemplo  
 O exemplo a seguir define três tipos de recursos, `NetworkResource`, `FileResource` e `MemoryResource`, e executa operações quando os recursos estiverem disponíveis. Este exemplo requer um par `NetworkResource` e `MemoryResource` para realizar a primeira operação e um par `FileResource` e `MemoryResource` para realizar a segunda operação. Para habilitar essas operações para que elas ocorram quando todos os recursos necessários estiverem disponíveis, este exemplo usa a classe <xref:System.Threading.Tasks.Dataflow.JoinBlock%602>. Quando um objeto <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> recebe dados de todas as fontes, ele propaga esses dados para o destino que, neste exemplo, é um objeto <xref:System.Threading.Tasks.Dataflow.ActionBlock%601>. Os dois objetos <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> leem de um pool compartilhado de objetos `MemoryResource`.  
  
 [!code-csharp[TPLDataflow_NonGreedyJoin#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_nongreedyjoin/cs/nongreedyjoin.cs#1)]
 [!code-vb[TPLDataflow_NonGreedyJoin#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_nongreedyjoin/vb/nongreedyjoin.vb#1)]  
  
 Para habilitar o uso eficiente do pool compartilhado de objetos `MemoryResource`, este exemplo especifica um objeto <xref:System.Threading.Tasks.Dataflow.GroupingDataflowBlockOptions> que tem a propriedade <xref:System.Threading.Tasks.Dataflow.GroupingDataflowBlockOptions.Greedy%2A> definida como `False` criar objetos <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> que atuam no modo não greedy. Um bloco de junção não greedy adia todas as mensagens de entrada até que uma esteja disponível em cada fonte. Se qualquer uma das mensagens adiadas forem aceitas pelo outro bloco, o bloco de junção reinicia o processo. O modo não greedy permite que os blocos de junção que compartilham um ou mais blocos de origem acelerem o andamento enquanto outros blocos aguardam pelos dados. Neste exemplo, se um objeto `MemoryResource` for adicionado ao pool `memoryResources`, o primeiro bloco de junção que receber sua segunda fonte de dados poderá acelerar o andamento. Se usássemos o modo greedy neste exemplo, que é o padrão, um bloco de junção poderia usar o objeto `MemoryResource` e aguardar até o segundo recurso ficar disponível. No entanto, se o outro bloco de junção tiver sua segunda fonte de dados disponível, ele não poderá acelerar o andamento já que o objeto `MemoryResource` foi usado pelo outro bloco de junção.  
  
## <a name="robust-programming"></a>Programação robusta  
 O uso de junções não greedy também pode ajudar a evitar deadlocks em seu aplicativo. Em um aplicativo de software, o *deadlock* ocorre quando cada um dos dois ou mais processos mantiver um recurso e mutuamente aguardar até que outro processo libere algum outro recurso. Considere um aplicativo que define dois objetos <xref:System.Threading.Tasks.Dataflow.JoinBlock%602>. Os dois objetos leem dados de dois blocos de origem compartilhados. No modo greedy, se um bloco de junção ler da primeira fonte e o segundo bloco de junção ler da segunda fonte, o aplicativo poderá causar o deadlock já que ambos os blocos de junção mutuamente aguardam que o outro libere seus recursos. No modo não greedy, cada bloco de junção lê de suas fontes somente quando todos os dados estão disponíveis e, portanto, o risco de deadlock é eliminado.  
  
## <a name="see-also"></a>Veja também

- [Fluxo de dados](dataflow-task-parallel-library.md)
