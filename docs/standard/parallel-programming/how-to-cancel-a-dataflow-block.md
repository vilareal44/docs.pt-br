---
title: 'Como: cancelar um bloco de fluxo de dados'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Task Parallel Library, dataflows
- dataflow blocks, canceling in TPL
- TPL dataflow library,canceling dataflow blocks
ms.assetid: fbddda0d-da3b-4ec8-a1d6-67ab8573fcd7
ms.openlocfilehash: 530c231deeaba007975849ab6dc41f4da6a859ea
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84285541"
---
# <a name="how-to-cancel-a-dataflow-block"></a>Como: cancelar um bloco de fluxo de dados
Este documento mostra como habilitar o cancelamento em seu aplicativo. Este exemplo usa o Windows Forms para mostrar onde os itens de trabalho estão ativos em um pipeline de fluxo de dados, assim como os efeitos de cancelamento.  

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]
  
## <a name="to-create-the-windows-forms-application"></a>Para criar o aplicativo do Windows Forms  
  
1. Crie um projeto de **aplicativo do Windows Forms** no C# ou Visual Basic. Nas etapas a seguir, o projeto é denominado `CancellationWinForms`.  
  
2. No designer de formulários do formulário principal, Form1.cs (Form1.vb para Visual Basic), adicione um controle <xref:System.Windows.Forms.ToolStrip>.  
  
3. Adicione um controle <xref:System.Windows.Forms.ToolStripButton> ao controle <xref:System.Windows.Forms.ToolStrip>. Defina a propriedade <xref:System.Windows.Forms.ToolStripItem.DisplayStyle%2A> como <xref:System.Windows.Forms.ToolStripItemDisplayStyle.Text> e a propriedade <xref:System.Windows.Forms.ToolStripItem.Text%2A> como **Adicionar itens de trabalho**.  
  
4. Adicione um segundo controle <xref:System.Windows.Forms.ToolStripButton> ao controle <xref:System.Windows.Forms.ToolStrip>. Defina a propriedade <xref:System.Windows.Forms.ToolStripItem.DisplayStyle%2A> como <xref:System.Windows.Forms.ToolStripItemDisplayStyle.Text>, a propriedade <xref:System.Windows.Forms.ToolStripItem.Text%2A> como **Cancelar** e a propriedade <xref:System.Windows.Forms.ToolStripItem.Enabled%2A> como `False`.  
  
5. Adicione quatro objetos <xref:System.Windows.Forms.ToolStripProgressBar> ao controle <xref:System.Windows.Forms.ToolStrip>.  
  
## <a name="creating-the-dataflow-pipeline"></a>Criar o Pipeline de fluxo de dados  
 Esta seção descreve como criar o pipeline de fluxo de dados que processa os itens de trabalho e atualiza as barras de progresso.  
  
### <a name="to-create-the-dataflow-pipeline"></a>Para criar o pipeline de fluxo de dados  
  
1. No seu projeto, adicione uma referência a System.Threading.Tasks.Dataflow.dll.  
  
2. Verifique se o Form1.cs (Form1.vb para Visual Basic) contém as seguintes instruções `using` (`Imports` em Visual Basic).  
  
     [!code-csharp[TPLDataflow_CancellationWinForms#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_cancellationwinforms/cs/cancellationwinforms/form1.cs#1)]
     [!code-vb[TPLDataflow_CancellationWinForms#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_cancellationwinforms/vb/cancellationwinforms/form1.vb#1)]  
  
3. Adicione a classe `WorkItem` como um tipo interno da classe `Form1`.  
  
     [!code-csharp[TPLDataflow_CancellationWinForms#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_cancellationwinforms/cs/cancellationwinforms/form1.cs#2)]
     [!code-vb[TPLDataflow_CancellationWinForms#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_cancellationwinforms/vb/cancellationwinforms/form1.vb#2)]  
  
4. Adicione os membros de dados a seguir à classe `Form1`.  
  
     [!code-csharp[TPLDataflow_CancellationWinForms#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_cancellationwinforms/cs/cancellationwinforms/form1.cs#3)]
     [!code-vb[TPLDataflow_CancellationWinForms#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_cancellationwinforms/vb/cancellationwinforms/form1.vb#3)]  
  
5. Adicione o seguinte método, `CreatePipeline`, à classe `Form1`.  
  
     [!code-csharp[TPLDataflow_CancellationWinForms#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_cancellationwinforms/cs/cancellationwinforms/form1.cs#4)]
     [!code-vb[TPLDataflow_CancellationWinForms#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_cancellationwinforms/vb/cancellationwinforms/form1.vb#4)]  
  
 Como os blocos de fluxo de dados `incrementProgress` e `decrementProgress` atuam na interface do usuário, é importante que essas ações ocorram no thread da interface do usuário. Para isso, durante a construção cada objeto fornece um objeto <xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions> cuja propriedade <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.TaskScheduler%2A> está definida como <xref:System.Threading.Tasks.TaskScheduler.FromCurrentSynchronizationContext%2A?displayProperty=nameWithType>. O método <xref:System.Threading.Tasks.TaskScheduler.FromCurrentSynchronizationContext%2A?displayProperty=nameWithType> cria um objeto <xref:System.Threading.Tasks.TaskScheduler> que executa o trabalho no contexto de sincronização atual. Como o construtor `Form1` é chamado do thread de interface do usuário, as ações para os blocos de fluxo de dados `incrementProgress` e `decrementProgress` também são executadas no thread da interface do usuário.  
  
 Este exemplo define a propriedade <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.CancellationToken%2A> ao construir os membros do pipeline. Como a propriedade <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.CancellationToken%2A> cancela permanentemente a execução do bloco de fluxo de dados, o pipeline inteiro deve ser recriado depois que o usuário cancela a operação e, em seguida, deseja adicionar mais itens de trabalho ao pipeline. Veja um exemplo que demonstra uma maneira alternativa de cancelar um bloco de fluxo de dados para que outros trabalhos sejam executados após uma operação ser cancelada no [Passo a passo: usando o fluxo de dados em um aplicativo do Windows Forms](walkthrough-using-dataflow-in-a-windows-forms-application.md).  
  
## <a name="connecting-the-dataflow-pipeline-to-the-user-interface"></a>Conectar o Pipeline de fluxo de dados à interface do usuário  
 Esta seção descreve como conectar o pipeline de fluxo de dados à interface do usuário. O manipulador de eventos controla a criação do pipeline e a adição de itens de trabalho ao pipeline para o botão**Adicionar itens de trabalho**. O cancelamento é iniciado pelo botão **Cancelar**. Quando o usuário clica em um desses botões, a ação apropriada é iniciada de maneira assíncrona.  
  
### <a name="to-connect-the-dataflow-pipeline-to-the-user-interface"></a>Para conectar o Pipeline de fluxo de dados à interface do usuário  
  
1. No designer de formulários, para o formulário principal, crie um manipulador de eventos para o evento <xref:System.Windows.Forms.ToolStripItem.Click> para o botão **Adicionar itens de trabalho**.  
  
2. Implante o evento <xref:System.Windows.Forms.ToolStripItem.Click> para o botão **Adicionar itens de trabalho**.  
  
     [!code-csharp[TPLDataflow_CancellationWinForms#5](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_cancellationwinforms/cs/cancellationwinforms/form1.cs#5)]
     [!code-vb[TPLDataflow_CancellationWinForms#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_cancellationwinforms/vb/cancellationwinforms/form1.vb#5)]  
  
3. No designer de formulários, para o formulário principal, crie um manipulador de eventos <xref:System.Windows.Forms.ToolStripItem.Click> para o botão **Cancelar**.  
  
4. Implante o manipulador de eventos <xref:System.Windows.Forms.ToolStripItem.Click> para o botão **Cancelar**.  
  
     [!code-csharp[TPLDataflow_CancellationWinForms#6](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_cancellationwinforms/cs/cancellationwinforms/form1.cs#6)]
     [!code-vb[TPLDataflow_CancellationWinForms#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_cancellationwinforms/vb/cancellationwinforms/form1.vb#6)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra o código completo para o Form1.cs (Form1.vb para Visual Basic).  
  
 [!code-csharp[TPLDataflow_CancellationWinForms#100](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_cancellationwinforms/cs/cancellationwinforms/form1.cs#100)]
 [!code-vb[TPLDataflow_CancellationWinForms#100](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_cancellationwinforms/vb/cancellationwinforms/form1.vb#100)]  
  
 A ilustração a seguir mostra o aplicativo em execução.  
  
 ![O aplicativo Windows Forms](media/tpldataflow-cancellation.png "TPLDataflow_Cancellation")  

## <a name="see-also"></a>Veja também

- [Fluxo de dados](dataflow-task-parallel-library.md)
