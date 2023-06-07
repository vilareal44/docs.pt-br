---
title: 'Como: percorrer uma árvore binária com tarefas paralelas'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, how to traverse a tree
ms.assetid: 4265d169-6c69-4f36-b10d-b7ae7f72f4df
ms.openlocfilehash: 5ac81a61691ec20daafc9e18978ba5814a150383
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288063"
---
# <a name="how-to-traverse-a-binary-tree-with-parallel-tasks"></a>Como: percorrer uma árvore binária com tarefas paralelas
O exemplo a seguir mostra duas maneiras pelas quais tarefas paralelas podem ser usadas para percorrer uma estrutura de dados de árvore. A criação da árvore em si permanece como um exercício.  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[TPL#16](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl/cs/tpl.cs#16)]
 [!code-vb[TPL#16](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl/vb/treewalk.vb#16)]  
  
 Os dois métodos mostrados são funcionalmente equivalentes. Usando o método <xref:System.Threading.Tasks.TaskFactory.StartNew%2A> para criar e executar as tarefas, você obter um identificador de tarefas que pode ser usado para aguardar as tarefas e manipular exceções.  
  
## <a name="see-also"></a>Veja também

- [Biblioteca de tarefas paralelas (TPL)](task-parallel-library-tpl.md)
