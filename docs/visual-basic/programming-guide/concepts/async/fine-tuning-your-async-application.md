---
title: Ajustando seu aplicativo Async
ms.date: 07/20/2015
ms.assetid: 4c3e7997-a95f-4fbe-a6ac-60ba042d30b9
ms.openlocfilehash: e33f86177a3680d41ec04842dbc120713a48f61c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396643"
---
# <a name="fine-tuning-your-async-application-visual-basic"></a>Ajustando seu aplicativo assíncrono (Visual Basic)
É possível adicionar flexibilidade e precisão a seus aplicativos assíncronos usando os métodos e propriedades que o tipo <xref:System.Threading.Tasks.Task> disponibiliza. Os tópicos nesta seção mostram exemplos que usam <xref:System.Threading.CancellationToken> e métodos de `Task` importantes como <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> e <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType>.  
  
 Usando `WhenAny` e `WhenAll`, é possível, com facilidade, iniciar várias tarefas e aguardar sua conclusão monitorando uma única tarefa.  
  
- `WhenAny` retorna uma tarefa que é concluída quando qualquer tarefa em uma coleção for concluída.  
  
     Para obter exemplos que usam o `WhenAny` , consulte [Cancelar tarefas assíncronas restantes após uma conclusão (Visual Basic)](cancel-remaining-async-tasks-after-one-is-complete.md)e [iniciar várias tarefas assíncronas e processá-las à medida que elas forem concluídas (Visual Basic)](start-multiple-async-tasks-and-process-them-as-they-complete.md).  
  
- `WhenAll` retorna uma tarefa que é concluída quando todas as tarefas em uma coleção forem concluídas.  
  
     Para obter mais informações e um exemplo que usa `WhenAll` o, consulte [como: estender a explicação assíncrona usando Task. WhenAll (Visual Basic)](how-to-extend-the-async-walkthrough-by-using-task-whenall.md).  
  
 Esta seção inclui os seguintes exemplos.  
  
- [Cancelar uma tarefa assíncrona ou uma lista de tarefas (Visual Basic)](cancel-an-async-task-or-a-list-of-tasks.md).  
  
- [Cancelar tarefas assíncronas após um período (Visual Basic)](cancel-async-tasks-after-a-period-of-time.md)  
  
- [Cancelar as demais tarefas assíncronas depois que uma delas estiver concluída (Visual Basic)](cancel-remaining-async-tasks-after-one-is-complete.md)  
  
- [Iniciar várias tarefas assíncronas e processá-las na conclusão (Visual Basic)](start-multiple-async-tasks-and-process-them-as-they-complete.md)  
  
> [!NOTE]
> Para executar os exemplos, você precisa ter o Visual Studio 2012 ou uma versão mais recente e o .NET Framework 4.5 ou posterior instalados em seu computador.  
  
 Os projetos criam uma interface do usuário que contém um botão que inicia o processo e um botão que o cancela, como mostra a imagem a seguir. Os botões são chamados `startButton` e `cancelButton`.  
  
 ![Janela do WPF com o botão Cancelar](./media/fine-tuning-your-async-application/cancellation-and-start-button.png "Caixa de diálogo com um botão Iniciar e parar")  
  
 É possível baixar projetos completos do WPF (Windows Presentation Foundation) em [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea).  
  
## <a name="see-also"></a>Confira também

- [Programação assíncrona com Async e Await (Visual Basic)](index.md)
