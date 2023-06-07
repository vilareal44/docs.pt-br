---
title: 'Como: evitar que uma tarefa filha se anexe ao pai dela'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, preventing attachments
ms.assetid: c0fb85d4-9e80-4905-9f65-29acc54201c4
ms.openlocfilehash: 5ff181344e6437179fa77f11872ae9a47745be93
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288154"
---
# <a name="how-to-prevent-a-child-task-from-attaching-to-its-parent"></a>Como: evitar que uma tarefa filha se anexe ao pai dela
Este documento demonstra como evitar que uma tarefa filho se anexe à tarefa principal. Impedir que uma tarefa filho seja anexada à tarefa pai é útil quando você chama um componente que é gravado por um terceiro e que também usa tarefas. Por exemplo, um componente de terceiros que usa a opção <xref:System.Threading.Tasks.TaskCreationOptions.AttachedToParent?displayProperty=nameWithType> para criar um objeto <xref:System.Threading.Tasks.Task> ou <xref:System.Threading.Tasks.Task%601> pode causar problemas em seu código se ele for de longa duração ou lançar uma exceção não controlada.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir compara os efeitos de usar as opções padrão com os efeitos de impedir que uma tarefa filho seja anexada ao pai. O exemplo cria um objeto <xref:System.Threading.Tasks.Task> que chama para uma biblioteca de terceiros que também usa um objeto <xref:System.Threading.Tasks.Task>. A biblioteca de terceiros usa a opção <xref:System.Threading.Tasks.TaskCreationOptions.AttachedToParent> para criar o objeto <xref:System.Threading.Tasks.Task>. O aplicativo usa a opção <xref:System.Threading.Tasks.TaskCreationOptions.DenyChildAttach?displayProperty=nameWithType> para criar a tarefa pai. Essa opção instrui o runtime para remover a especificação <xref:System.Threading.Tasks.TaskCreationOptions.AttachedToParent> em tarefas filho.  
  
 [!code-csharp[TPL_DenyChildAttach#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_denychildattach/cs/denychildattach.cs#1)]
 [!code-vb[TPL_DenyChildAttach#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_denychildattach/vb/denychildattach.vb#1)]  
  
 Como a tarefa pai não termina até que todas as tarefas filho sejam concluídas, uma tarefa filho de longa duração pode fazer com que o aplicativo geral tenha um baixo desempenho. Neste exemplo, quando o aplicativo usa as opções padrão para criar a tarefa pai, a tarefa filho deve terminar antes da conclusão da tarefa pai. Quando o aplicativo usa a opção <xref:System.Threading.Tasks.TaskCreationOptions.DenyChildAttach?displayProperty=nameWithType>, a tarefa filho não está anexada à tarefa pai. Portanto, o aplicativo pode executar trabalhos adicionais após a conclusão da tarefa pai e antes de aguardar a conclusão da tarefa filho.  
  
## <a name="see-also"></a>Veja também

- [Programação assíncrona baseada em tarefas](task-based-asynchronous-programming.md)
