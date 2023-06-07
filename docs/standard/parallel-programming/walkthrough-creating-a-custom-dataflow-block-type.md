---
title: 'Passo a passo: criando um tipo de bloco de fluxo de dados personalizado'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Task Parallel Library, dataflows
- TPL dataflow library, creating custom dataflow blocks
- dataflow blocks, creating custom in TPL
ms.assetid: a6147146-0a6a-4d9b-ab0f-237b3c1ac691
ms.openlocfilehash: 37857e465bf4089dbeecc4cfd532d0702f795495
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84284696"
---
# <a name="walkthrough-creating-a-custom-dataflow-block-type"></a>Passo a passo: criando um tipo de bloco de fluxo de dados personalizado
Embora a biblioteca de fluxo de dados TPL forneça vários tipos de bloco de fluxo de dados que permitem uma gama de funcionalidades, também é possível criar tipos de blocos personalizados. Este documento mostra como criar um tipo de bloco de fluxo de dados que implanta comportamentos personalizados.  
  
## <a name="prerequisites"></a>Pré-requisitos  
 Leia primeiro sobre o [Fluxo de dados](dataflow-task-parallel-library.md) antes de ler este documento.  

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]
  
## <a name="defining-the-sliding-window-dataflow-block"></a>Definir o bloco de fluxo de dados da janela deslizante  
 Considere um aplicativo de fluxo de dados que exija o armazenamento em buffer nos valores de entrada e uma saída à maneira de uma janela deslizante. Por exemplo, para os valores de entrada {0, 1, 2, 3, 4, 5} e um tamanho de janela de três, um bloco de fluxo de dados de janela deslizante produz as matrizes de saída {0, 1, 2}, {1, 2, 3}, {2, 3, 4} e {3, 4, 5}. As seções a seguir descrevem duas maneiras de criar um tipo de bloco de fluxo de dados que implanta este comportamento personalizado. A primeira técnica usa o método <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Encapsulate%2A> para combinar a funcionalidade de um objeto <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601> e um <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601> em um bloco propagador. A segunda técnica define uma classe que deriva de <xref:System.Threading.Tasks.Dataflow.IPropagatorBlock%602> e combina a funcionalidade existente para executar o comportamento personalizado.  
  
## <a name="using-the-encapsulate-method-to-define-the-sliding-window-dataflow-block"></a>Usar o método encapsular para definir o bloco de fluxo de dados de janela deslizante  
 O exemplo a seguir usa o método <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Encapsulate%2A> para criar um bloco propagador de uma origem e um destino. Um bloco propagador permite que um bloco de origem e um bloco de destino atuem como destinatários e remetentes de dados.  
  
 Essa técnica é útil quando você precisa de funcionalidades personalizadas de fluxo de dados, mas não precisa de um tipo que forneça campos, propriedades ou métodos adicionais.  
  
 [!code-csharp[TPLDataflow_SlidingWindowBlock#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_slidingwindowblock/cs/slidingwindowblock.cs#1)]
 [!code-vb[TPLDataflow_SlidingWindowBlock#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_slidingwindowblock/vb/slidingwindowblock.vb#1)]  
  
## <a name="deriving-from-ipropagatorblock-to-define-the-sliding-window-dataflow-block"></a>Derivar de IPropagatorBlock para definir o bloco de fluxo de dados de janela deslizante  
 O exemplo a seguir mostra a classe `SlidingWindowBlock`. Essa classe é derivada de <xref:System.Threading.Tasks.Dataflow.IPropagatorBlock%602> para que atue como origem e destino de dados. Como no exemplo anterior, a classe `SlidingWindowBlock` se baseia em tipos de blocos de fluxo de dados existentes. No entanto, a classe `SlidingWindowBlock` também implanta os métodos necessários para as interfaces <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601>, <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601> e <xref:System.Threading.Tasks.Dataflow.IDataflowBlock>. Todos esses métodos encaminham trabalho para os membros de tipo de bloco de fluxo de dados predefinidos. Por exemplo, o método `Post` transfere trabalho para o membro de dados `m_target`, que também é um objeto <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601>.  
  
 Essa técnica é útil quando você precisa de funcionalidades personalizadas de fluxo de dados e também de um tipo que forneça campos, propriedades ou métodos adicionais. Por exemplo, a classe `SlidingWindowBlock` também deriva de <xref:System.Threading.Tasks.Dataflow.IReceivableSourceBlock%601> para poder fornecer os métodos <xref:System.Threading.Tasks.Dataflow.IReceivableSourceBlock%601.TryReceive%2A> e <xref:System.Threading.Tasks.Dataflow.IReceivableSourceBlock%601.TryReceiveAll%2A>. A classe `SlidingWindowBlock` também demonstra extensibilidade, fornecendo a propriedade `WindowSize`, que recupera o número de elementos na janela deslizante.  
  
 [!code-csharp[TPLDataflow_SlidingWindowBlock#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_slidingwindowblock/cs/slidingwindowblock.cs#2)]
 [!code-vb[TPLDataflow_SlidingWindowBlock#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_slidingwindowblock/vb/slidingwindowblock.vb#2)]  
  
## <a name="the-complete-example"></a>O Exemplo Completo  
 O exemplo a seguir mostra o código completo dessa explicação passo a passo. Ele também demonstra como usar os dois blocos de janela deslizante em um método que grava o bloco, lê a partir dele e imprime os resultados para o console.  
  
 [!code-csharp[TPLDataflow_SlidingWindowBlock#100](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_slidingwindowblock/cs/slidingwindowblock.cs#100)]
 [!code-vb[TPLDataflow_SlidingWindowBlock#100](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_slidingwindowblock/vb/slidingwindowblock.vb#100)]  
  
## <a name="see-also"></a>Veja também

- [Fluxo de dados](dataflow-task-parallel-library.md)
