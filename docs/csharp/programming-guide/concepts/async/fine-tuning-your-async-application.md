---
title: Ajuste fino de seu aplicativo assíncrono (C#)
description: Esses exemplos em C# usam CancellationToken e métodos de tarefa importantes, como Task. WhenAll e Task. WhenAny, para ajustar os aplicativos assíncronos.
ms.date: 07/20/2015
ms.assetid: 97696eb9-81fc-4940-9655-84daa8eb4d5c
ms.openlocfilehash: 46457f889a78932e306d2bd21dca666625d744eb
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925313"
---
# <a name="fine-tuning-your-async-application-c"></a>Ajuste fino de seu aplicativo assíncrono (C#)
É possível adicionar flexibilidade e precisão a seus aplicativos assíncronos usando os métodos e propriedades que o tipo <xref:System.Threading.Tasks.Task> disponibiliza. Os tópicos nesta seção mostram exemplos que usam <xref:System.Threading.CancellationToken> e métodos de `Task` importantes como <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> e <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType>.  
  
 Usando `WhenAny` e `WhenAll`, é possível, com facilidade, iniciar várias tarefas e aguardar sua conclusão monitorando uma única tarefa.  
  
- `WhenAny` retorna uma tarefa que é concluída quando qualquer tarefa em uma coleção for concluída.  
  
     Para obter exemplos que usam `WhenAny`, consulte [Cancelar as demais tarefas assíncronas depois que uma delas estiver concluída (C#)](./cancel-remaining-async-tasks-after-one-is-complete.md) e [Iniciar várias tarefas assíncronas e processá-las na conclusão (C#)](./start-multiple-async-tasks-and-process-them-as-they-complete.md).  
  
- `WhenAll` retorna uma tarefa que é concluída quando todas as tarefas em uma coleção forem concluídas.  
  
     Para obter mais informações e um exemplo que usa `WhenAll` o, consulte [como estender a instrução assíncrona usando Task. WhenAll (C#)](./how-to-extend-the-async-walkthrough-by-using-task-whenall.md).
  
 Esta seção inclui os seguintes exemplos.  
  
- [Cancelar uma tarefa assíncrona ou uma lista de tarefas (C#)](./cancel-an-async-task-or-a-list-of-tasks.md).  
  
- [Cancelar tarefas assíncronas após um período de tempo (C#)](./cancel-async-tasks-after-a-period-of-time.md)  
  
- [Cancelar as demais tarefas assíncronas depois que uma delas estiver concluída (C#)](./cancel-remaining-async-tasks-after-one-is-complete.md)  
  
- [Iniciar várias tarefas assíncronas e processá-las na conclusão (C#)](./start-multiple-async-tasks-and-process-them-as-they-complete.md)  
  
> [!NOTE]
> Para executar os exemplos, você precisa ter o Visual Studio 2012 ou uma versão mais recente e o .NET Framework 4.5 ou posterior instalados em seu computador.  
  
 Os projetos criam uma interface do usuário que contém um botão que inicia o processo e um botão que o cancela, como mostra a imagem a seguir. Os botões são chamados `startButton` e `cancelButton`.  
  
 ![Janela do WPF com o botão Cancelar](./media/fine-tuning-your-async-application/cancellation-and-start-button.png "Caixa de diálogo com um botão Iniciar e parar")  
  
 É possível baixar projetos completos do WPF (Windows Presentation Foundation) em [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea).  
  
## <a name="see-also"></a>Veja também

- [Programação assíncrona com Async e Await (C#)](./index.md)
