---
title: 'Passo a passo: usar um fluxo de dados em um aplicativo do Windows Forms'
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- TPL dataflow library, in Windows Forms
- Task Parallel Library, dataflows
- Windows Forms, and TPL
ms.assetid: 9c65cdf7-660c-409f-89ea-59d7ec8e127c
ms.openlocfilehash: 7cd82ffde5fccf938027a6ab6ea15fef226fef6f
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288427"
---
# <a name="walkthrough-using-dataflow-in-a-windows-forms-application"></a>Passo a passo: usar um fluxo de dados em um aplicativo do Windows Forms
Este documento demonstra como criar uma rede de blocos de fluxo de dados que executam o processamento de imagens em um Aplicativo do Windows Forms.  
  
 Este exemplo carrega arquivos de imagem da pasta especificada, cria uma imagem composta e exibe o resultado. O exemplo usa o modelo de fluxo de dados para encaminhar imagens por meio da rede. No modelo de fluxo de dados, componentes independentes de um programa se comunicam uns com os outros enviando mensagens. Quando um componente recebe uma mensagem, ele executa uma ação e, depois, passa o resultado para outro componente. Compare isso com o modelo de fluxo de controle, em que um aplicativo usa estruturas de controle, por exemplo, instruções condicionais, loops e assim por diante, para controlar a ordem das operações em um programa.  
  
## <a name="prerequisites"></a>Pré-requisitos  
 Leia sobre o [Fluxo de dados](dataflow-task-parallel-library.md) antes de iniciar essa explicação passo a passo.  

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

## <a name="sections"></a>Seções  
 Este passo a passo contém as seguintes seções:  
  
- [Criar o aplicativo do Windows Forms](#winforms)  
  
- [Criar a rede de fluxo de dados](#network)  
  
- [Conectar a rede de fluxo de dados à interface do usuário](#ui)  
  
- [O exemplo completo](#complete)  
  
<a name="winforms"></a>
## <a name="creating-the-windows-forms-application"></a>Criar o aplicativo do Windows Forms  
 Esta seção descreve como criar o aplicativo básico do Windows Forms e adicionar controles ao formulário principal.  
  
### <a name="to-create-the-windows-forms-application"></a>Para criar o aplicativo do Windows Forms  
  
1. No Visual Studio, crie um projeto de **aplicativo do Windows Forms** do Visual Basic ou do Visual C#. Neste documento, o projeto é chamado `CompositeImages`.  
  
2. No designer de formulários do formulário principal, Form1.cs (Form1.vb para Visual Basic), adicione um controle <xref:System.Windows.Forms.ToolStrip>.  
  
3. Adicione um controle <xref:System.Windows.Forms.ToolStripButton> ao controle <xref:System.Windows.Forms.ToolStrip>. Defina a propriedade <xref:System.Windows.Forms.ToolStripItem.DisplayStyle%2A> como <xref:System.Windows.Forms.ToolStripItemDisplayStyle.Text> e a propriedade <xref:System.Windows.Forms.ToolStripItem.Text%2A> como **Escolher Pasta**.  
  
4. Adicione um segundo controle <xref:System.Windows.Forms.ToolStripButton> ao controle <xref:System.Windows.Forms.ToolStrip>. Defina a propriedade <xref:System.Windows.Forms.ToolStripItem.DisplayStyle%2A> como <xref:System.Windows.Forms.ToolStripItemDisplayStyle.Text>, a propriedade <xref:System.Windows.Forms.ToolStripItem.Text%2A> como **Cancelar** e a propriedade <xref:System.Windows.Forms.ToolStripItem.Enabled%2A> como `False`.  
  
5. Adicione um objeto <xref:System.Windows.Forms.PictureBox> ao formulário principal. Defina a propriedade <xref:System.Windows.Forms.Control.Dock%2A> como <xref:System.Windows.Forms.DockStyle.Fill>.  
  
<a name="network"></a>
## <a name="creating-the-dataflow-network"></a>Criar a rede de fluxo de dados  
 Esta seção descreve como criar a rede de fluxo de dados que executa o processamento de imagem.  
  
### <a name="to-create-the-dataflow-network"></a>Para criar a rede de fluxo de dados  
  
1. Adicione uma referência a System.Threading.Tasks.Dataflow.dll ao seu projeto.  
  
2. Verifique se o Form1.cs (Form1.vb para Visual Basic) contém as seguintes instruções `using` (`Using` em Visual Basic):  
  
     [!code-csharp[TPLDataflow_CompositeImages#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_compositeimages/cs/compositeimages/form1.cs#1)]  
  
3. Adicione os membros de dados a seguir à classe `Form1`:  
  
     [!code-csharp[TPLDataflow_CompositeImages#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_compositeimages/cs/compositeimages/form1.cs#2)]  
  
4. Adicione o seguinte método, `CreateImageProcessingNetwork`, à classe `Form1`. Esse método cria a rede de processamento de imagem.  
  
     [!code-csharp[TPLDataflow_CompositeImages#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_compositeimages/cs/compositeimages/form1.cs#3)]  
  
5. Implementar o método de `LoadBitmaps` .  
  
     [!code-csharp[TPLDataflow_CompositeImages#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_compositeimages/cs/compositeimages/form1.cs#4)]  
  
6. Implementar o método de `CreateCompositeBitmap` .  
  
     [!code-csharp[TPLDataflow_CompositeImages#5](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_compositeimages/cs/compositeimages/form1.cs#5)]  
  
    > [!NOTE]
    > A versão em C# do método `CreateCompositeBitmap` usa ponteiros para habilitar o processamento eficiente dos objetos <xref:System.Drawing.Bitmap?displayProperty=nameWithType>. Portanto, você deve habilitar a opção **Permitir código não gerenciado** em seu projeto para usar a palavra-chave [unsafe](../../csharp/language-reference/keywords/unsafe.md). Para obter mais informações de como habilitar o código não seguro em um projeto em Visual C#, confira [Página Build, Designer de Projeto (C#)](/visualstudio/ide/reference/build-page-project-designer-csharp).  
  
 A tabela a seguir descreve os membros da rede.  
  
|Membro|Tipo|Description|  
|------------|----------|-----------------|  
|`loadBitmaps`|<xref:System.Threading.Tasks.Dataflow.TransformBlock%602>|Usa um caminho de pasta como entrada e produz uma coleção de objetos <xref:System.Drawing.Bitmap> como saída.|  
|`createCompositeBitmap`|<xref:System.Threading.Tasks.Dataflow.TransformBlock%602>|Usa uma coleção de objetos <xref:System.Drawing.Bitmap> como entrada e produz um bitmap composto como saída.|  
|`displayCompositeBitmap`|<xref:System.Threading.Tasks.Dataflow.ActionBlock%601>|Exibe o bitmap composto no formulário.|  
|`operationCancelled`|<xref:System.Threading.Tasks.Dataflow.ActionBlock%601>|Exibe uma imagem para indicar que a operação foi cancelada e permite que o usuário selecione outra pasta.|  
  
 Para conectar os blocos de fluxo de dados para formar uma rede, este exemplo usa o método <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601.LinkTo%2A>. O método <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601.LinkTo%2A> contém uma versão sobrecarregada que usa um objeto <xref:System.Predicate%601> que determina se o bloco de destino aceita ou rejeita uma mensagem. Esse mecanismo de filtragem permite que os blocos de mensagens recebam apenas determinados valores. Neste exemplo, a rede pode ser ramificada usando uma de duas maneiras. A ramificação principal carrega as imagens do disco, cria a imagem composta e exibe essa imagem no formulário. A ramificação alternativa cancela a operação atual. Os objetos <xref:System.Predicate%601> permitem que os blocos de fluxo de dados ao longo da ramificação principal alternem para a ramificação alternativa rejeitando determinadas mensagens. Por exemplo, se o usuário cancelar a operação, o bloco de fluxo de dados `createCompositeBitmap` produzirá `null` (`Nothing` em Visual Basic) como sua saída. O bloco de fluxo de dados `displayCompositeBitmap` rejeita valores de entrada `null` e, portanto, a mensagem é oferecida para `operationCancelled`. O bloco de fluxo de dados `operationCancelled` aceita todas as mensagens e, portanto, exibe uma imagem para indicar que a operação foi cancelada.  
  
 A seguinte ilustração mostra a rede de processamento de imagem:  
  
 ![Ilustração que mostra a rede de processamento de imagem.](./media/walkthrough-using-dataflow-in-a-windows-forms-application/dataflow-winforms-image-processing.png)  
  
 Como os blocos de fluxo de dados `displayCompositeBitmap` e `operationCancelled` atuam na interface do usuário, é importante que essas ações ocorram no thread da interface do usuário. Para isso, durante a construção cada objeto fornece um objeto <xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions> cuja propriedade <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.TaskScheduler%2A> está definida como <xref:System.Threading.Tasks.TaskScheduler.FromCurrentSynchronizationContext%2A?displayProperty=nameWithType>. O método <xref:System.Threading.Tasks.TaskScheduler.FromCurrentSynchronizationContext%2A?displayProperty=nameWithType> cria um objeto <xref:System.Threading.Tasks.TaskScheduler> que executa o trabalho no contexto de sincronização atual. Como o método `CreateImageProcessingNetwork` é chamado do manipulador do botão **Escolher Pasta**, executado no thread da interface de usuário, as ações para os blocos de fluxo de dados `displayCompositeBitmap` e `operationCancelled` também são executadas no thread da interface do usuário.  
  
 Este exemplo usa um token de cancelamento compartilhado em vez de configurar a propriedade <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.CancellationToken%2A>, pois a propriedade <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.CancellationToken%2A> cancela permanentemente a execução do bloco de fluxo de dados. Um token de cancelamento permite que esse exemplo reutilize a mesma rede de fluxo de dados várias vezes, mesmo quando o usuário cancela uma ou mais operações. Para obter um exemplo que usa <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.CancellationToken%2A> para cancelar permanentemente a execução de um bloco de fluxo de dados, confira [Como cancelar um bloco de fluxo de dados](how-to-cancel-a-dataflow-block.md).  
  
<a name="ui"></a>
## <a name="connecting-the-dataflow-network-to-the-user-interface"></a>Conectar a rede de fluxo de dados à interface do usuário  
 Esta seção descreve como conectar a rede de fluxo de dados à interface do usuário. A criação de uma imagem composta e o cancelamento da operação começa com os botões **Escolher Pasta** e **Cancelar**. Quando o usuário escolhe um desses botões, a ação apropriada é iniciada de maneira assíncrona.  
  
### <a name="to-connect-the-dataflow-network-to-the-user-interface"></a>Para conectar a rede de fluxo de dados à interface do usuário  
  
1. No designer de formulários, do formulário principal, crie um manipulador de eventos para o evento <xref:System.Windows.Forms.ToolStripItem.Click> para o botão **Escolher Pasta**.  
  
2. Implemente o evento <xref:System.Windows.Forms.ToolStripItem.Click> para o botão **Escolher Pasta**.  
  
     [!code-csharp[TPLDataflow_CompositeImages#6](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_compositeimages/cs/compositeimages/form1.cs#6)]  
  
3. No designer de formulários, para o formulário principal, crie um manipulador de eventos para o evento <xref:System.Windows.Forms.ToolStripItem.Click> para o botão **Cancelar**.  
  
4. Implante o evento <xref:System.Windows.Forms.ToolStripItem.Click> para o botão **Cancelar**.  
  
     [!code-csharp[TPLDataflow_CompositeImages#7](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_compositeimages/cs/compositeimages/form1.cs#7)]  
  
<a name="complete"></a>
## <a name="the-complete-example"></a>O Exemplo Completo  
 O exemplo a seguir mostra o código completo dessa explicação passo a passo.  
  
 [!code-csharp[TPLDataflow_CompositeImages#100](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_compositeimages/cs/compositeimages/form1.cs#100)]  
  
 A ilustração a seguir mostra a saída típica para a pasta \Sample Pictures\ comum.  
  
 ![O aplicativo Windows Forms](media/tpldataflow-compositeimages.gif "TPLDataflow_CompositeImages")  

## <a name="see-also"></a>Veja também

- [Fluxo de dados](dataflow-task-parallel-library.md)
