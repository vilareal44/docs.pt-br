---
title: Visão geral das caixas de diálogo
description: Saiba mais sobre as variedades de caixas de diálogo na apresentação do Windows Foundation (WPF) que você pode usar para reunir e exibir informações.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- modeless dialog boxes [WPF]
- dialog boxes [WPF]
- message boxes [WPF]
- modal dialog boxes [WPF]
ms.assetid: 0d23d544-a393-4a02-a3aa-d8cd5d3d6511
ms.openlocfilehash: 822604dd694f2f6260496872fd7a1a8440a62535
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85618136"
---
# <a name="dialog-boxes-overview"></a>Visão geral das caixas de diálogo
Os aplicativos autônomos normalmente têm uma janela principal que exibe os dados principais sobre os quais o aplicativo opera e expõe a funcionalidade para processar esses dados por meio de [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] mecanismos como barras de menu, barras de ferramentas e barras de status. Um aplicativo não trivial também pode exibir janelas adicionais para fazer o seguinte:  
  
- Exibir informações específicas aos usuários.  
  
- Coletar informações de usuários.  
  
- Exibir e coletar informações.  
  
 Esses tipos de janelas são conhecidos como *caixas de diálogo*, e há dois tipos: modal e sem janela restrita.  
  
 Uma caixa de diálogo *modal* é exibida por uma função quando a função precisa de dados adicionais de um usuário para continuar. Como a função depende da caixa de diálogo modal para coletar dados, a caixa de diálogo modal também impede que um usuário ative outras janelas no aplicativo enquanto ele está aberto. Na maioria dos casos, uma caixa de diálogo modal permite que um usuário sinalize quando concluiu a caixa de diálogo modal pressionando um botão **OK** ou **Cancelar** . Pressionar o botão **OK** indica que um usuário inseriu dados e quer que a função continue o processamento com esses dados. Pressionar o botão **Cancelar** indica que um usuário deseja impedir que a função seja executada completamente. Os exemplos de caixas de diálogo modais mais comuns são mostrados para abrir, salvar e imprimir dados.  
  
 Uma caixa de diálogo *sem janela restrita* , por outro lado, não impede que um usuário ative outras janelas enquanto ele está aberto. Por exemplo, se um usuário desejar localizar ocorrências de uma palavra específica em um documento, uma janela principal geralmente abrirá uma caixa de diálogo para solicitar a um usuário qual palavra ele está procurando. No entanto, como a localização de uma palavra não impede que um usuário edite o documento, a caixa de diálogo não precisa ser restrita. Uma caixa de diálogo sem janela restrita, pelo menos, fornece um botão **fechar** para fechar a caixa de diálogo e pode fornecer botões adicionais para executar funções específicas, como um botão **Localizar próximo** para localizar a próxima palavra que corresponde aos critérios de localização de uma pesquisa do Word.  
  
 O Windows Presentation Foundation (WPF) permite que você crie vários tipos de caixas de diálogo, incluindo caixas de mensagens, caixas de diálogo comuns e caixas de diálogo personalizadas. Este tópico aborda cada um deles, e o [exemplo da caixa de diálogo](https://github.com/Microsoft/WPF-Samples/tree/master/Windows/DialogBox) fornece exemplos correspondentes.  

<a name="Message_Boxes"></a>
## <a name="message-boxes"></a>Caixas de mensagens  
 Uma *caixa de mensagem* é uma caixa de diálogo que pode ser usada para exibir informações textuais e permitir que os usuários tomem decisões com botões. A figura a seguir mostra uma caixa de mensagem que exibe informações textuais, faz uma pergunta e fornece ao usuário três botões para responder à pergunta.  
  
 ![Uma caixa de diálogo de processador de texto perguntando se você deseja salvar as alterações no documento antes de o aplicativo ser fechado.](./media/dialog-boxes-overview/word-processor-dialog.png)  
  
 Para criar uma caixa de mensagem, use a <xref:System.Windows.MessageBox> classe. <xref:System.Windows.MessageBox>permite que você configure o texto, o título, o ícone e os botões da caixa de mensagem, usando um código semelhante ao seguinte.  
  
 [!code-csharp[DialogBoxesOverviewSnippets#MsgBoxConfigureCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxesOverviewSnippets/CSharp/Window1.xaml.cs#msgboxconfigurecodebehind)]
 [!code-vb[DialogBoxesOverviewSnippets#MsgBoxConfigureCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxesOverviewSnippets/VisualBasic/window1.xaml.vb#msgboxconfigurecodebehind)]  
  
 Para mostrar uma caixa de mensagem, você chama o `static` <xref:System.Windows.MessageBox.Show%2A> método, conforme demonstrado no código a seguir.  
  
 [!code-csharp[DialogBoxesOverviewSnippets#MsgBoxShowCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxesOverviewSnippets/CSharp/Window1.xaml.cs#msgboxshowcodebehind)]
 [!code-vb[DialogBoxesOverviewSnippets#MsgBoxShowCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxesOverviewSnippets/VisualBasic/window1.xaml.vb#msgboxshowcodebehind)]  
  
 Quando o código que mostra uma caixa de mensagem precisa detectar e processar a decisão do usuário (qual botão foi pressionado), o código pode inspecionar o resultado da caixa de mensagem, conforme mostrado no código a seguir.  
  
 [!code-csharp[DialogBoxesOverviewSnippets#MsgBoxShowAndResultCODEBEHIND1](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxesOverviewSnippets/CSharp/Window1.xaml.cs#msgboxshowandresultcodebehind1)]
 [!code-vb[DialogBoxesOverviewSnippets#MsgBoxShowAndResultCODEBEHIND1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxesOverviewSnippets/VisualBasic/window1.xaml.vb#msgboxshowandresultcodebehind1)]  
  
 Para obter mais informações sobre como usar caixas de mensagens, consulte o exemplo <xref:System.Windows.MessageBox> [MessageBox](https://github.com/Microsoft/WPF-Samples/tree/master/Windows/MessageBox)e a [caixa de diálogo de exemplo](https://github.com/Microsoft/WPF-Samples/tree/master/Windows/DialogBox).  
  
 Embora <xref:System.Windows.MessageBox> possa oferecer uma experiência de usuário simples de caixa de diálogo, a vantagem de usar <xref:System.Windows.MessageBox> é que é o único tipo de janela que pode ser mostrado por aplicativos executados em uma área restrita de segurança de confiança parcial (consulte [segurança](../security-wpf.md)), como aplicativos de navegador XAML (XBAPs).  
  
 A maioria das caixas de diálogo exibe e coleta dados mais complexos que o resultado de uma caixa de mensagem, incluindo texto, seleção (caixas de seleção), seleção mutuamente exclusiva (botões de opção) e seleção de lista (caixas de listagem, caixas de combinação, caixas de listagem suspensas). Para isso, Windows Presentation Foundation (WPF) fornece várias caixas de diálogo comuns e permite que você crie suas próprias caixas de diálogo, embora o uso de seja limitado a aplicativos executados com confiança total.  
  
<a name="Common_Dialogs"></a>
## <a name="common-dialog-boxes"></a>Caixas de diálogo comuns  
 O Windows implementa uma variedade de caixas de diálogo reutilizáveis que são comuns a todos os aplicativos, incluindo caixas de diálogo para abrir arquivos, salvar arquivos e imprimir. Como essas caixas de diálogo são implementadas pelo sistema operacional, elas podem ser compartilhadas entre todos os aplicativos executados no sistema operacional, o que ajuda na consistência da experiência do usuário. Quando os usuários estiverem familiarizados com o uso de uma caixa de diálogo fornecida pelo sistema operacional em um aplicativo, eles não precisarão aprender a usar a caixa de diálogo em outros aplicativos. Como essas caixas de diálogo estão disponíveis para todos os aplicativos e como ajudam a fornecer uma experiência de usuário consistente, elas são conhecidas como *caixas de diálogo comuns*.  
  
 Windows Presentation Foundation (WPF) encapsula as caixas de diálogo abrir arquivo, salvar arquivo e imprimir comum e as expõe como classes gerenciadas para uso em aplicativos autônomos. Este tópico fornece uma visão geral breve de cada uma delas.  
  
<a name="Open_File_Dialog"></a>
### <a name="open-file-dialog"></a>Caixa de diálogo abrir arquivo  
 A caixa de diálogo Abrir Arquivo, mostrada na figura a seguir, é usada pela funcionalidade de abertura de arquivo para recuperar o nome de um arquivo a ser aberto.  
  
 ![Uma caixa de diálogo aberta mostrando o local para recuperar o arquivo.](./media/dialog-boxes-overview/open-file-dialog-box.png)  
  
 A caixa de diálogo arquivo aberto comum é implementada como a <xref:Microsoft.Win32.OpenFileDialog> classe e está localizada no <xref:Microsoft.Win32> namespace. O código a seguir mostra como criar, configurar e mostrar uma e como processar o resultado.  
  
 [!code-csharp[DialogBoxesOverviewSnippets#OpenFileDialogBoxCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxesOverviewSnippets/CSharp/Window1.xaml.cs#openfiledialogboxcodebehind)]
 [!code-vb[DialogBoxesOverviewSnippets#OpenFileDialogBoxCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxesOverviewSnippets/VisualBasic/window1.xaml.vb#openfiledialogboxcodebehind)]  
  
 Para obter mais informações sobre a caixa de diálogo abrir arquivo, consulte <xref:Microsoft.Win32.OpenFileDialog?displayProperty=nameWithType> .  
  
> [!NOTE]
> <xref:Microsoft.Win32.OpenFileDialog>pode ser usado para recuperar com segurança nomes de arquivo por aplicativos em execução com confiança parcial (consulte [segurança](../security-wpf.md)).  
  
<a name="Save_File_Dialog"></a>
### <a name="save-file-dialog-box"></a>caixa de diálogo Salvar Arquivo  
 A caixa de diálogo Salvar Arquivo, mostrada na figura a seguir, é usada pela funcionalidade de salvamento de arquivo para recuperar o nome de um arquivo a ser salvo.  
  
 ![Uma caixa de diálogo Salvar como mostrando o local para salvar o arquivo.](./media/dialog-boxes-overview/save-file-dialog-box.png)  
  
 A caixa de diálogo Salvar arquivo comum é implementada como a <xref:Microsoft.Win32.SaveFileDialog> classe e está localizada no <xref:Microsoft.Win32> namespace. O código a seguir mostra como criar, configurar e mostrar uma e como processar o resultado.  
  
 [!code-csharp[DialogBoxesOverviewSnippets#SaveFileDialogBoxCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxesOverviewSnippets/CSharp/Window1.xaml.cs#savefiledialogboxcodebehind)]
 [!code-vb[DialogBoxesOverviewSnippets#SaveFileDialogBoxCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxesOverviewSnippets/VisualBasic/window1.xaml.vb#savefiledialogboxcodebehind)]  
  
 Para obter mais informações sobre a caixa de diálogo Salvar arquivo, consulte <xref:Microsoft.Win32.SaveFileDialog?displayProperty=nameWithType> .  
  
<a name="Print_Dialog"></a>
### <a name="print-dialog-box"></a>caixa de diálogo Imprimir

A caixa de diálogo Imprimir, mostrada na figura a seguir, é usada pela funcionalidade de impressão para escolher e configurar a impressora na qual um usuário deseja imprimir os dados.  
  
![Captura de tela que mostra uma caixa de diálogo de impressão.](./media/dialog-boxes-overview/print-data-dialog-box.png)  
  
A caixa de diálogo Imprimir comum é implementada como a <xref:System.Windows.Controls.PrintDialog> classe e está localizada no <xref:System.Windows.Controls> namespace. O código a seguir mostra como criar, configurar e mostrar uma.  
  
 [!code-csharp[DialogBoxesOverviewSnippets#PrintDialogBoxCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxesOverviewSnippets/CSharp/Window1.xaml.cs#printdialogboxcodebehind)]
 [!code-vb[DialogBoxesOverviewSnippets#PrintDialogBoxCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxesOverviewSnippets/VisualBasic/window1.xaml.vb#printdialogboxcodebehind)]  
  
 Para obter mais informações sobre a caixa de diálogo Imprimir, consulte <xref:System.Windows.Controls.PrintDialog?displayProperty=nameWithType> . Para obter uma discussão detalhada sobre a impressão no WPF, consulte [visão geral da impressão](../advanced/printing-overview.md).  
  
<a name="Custom_Dialog_Boxes"></a>
## <a name="custom-dialog-boxes"></a>Caixas de diálogo personalizadas

Embora as caixas de diálogo comuns sejam úteis e devam ser usadas quando possível, elas não dão suporte aos requisitos de caixas de diálogo específicas ao domínio. Nesses casos, você precisa criar suas próprias caixas de diálogo. Como veremos, uma caixa de diálogo é uma janela com comportamentos especiais. <xref:System.Windows.Window>implementa esses comportamentos e, consequentemente, você usa <xref:System.Windows.Window> para criar caixas de diálogo personalizadas modais e sem janela restrita.  
  
<a name="Creating_a_Modal_Custom_Dialog_Box"></a>
### <a name="creating-a-modal-custom-dialog-box"></a>Criando uma caixa de diálogo personalizada modal

Este tópico mostra como usar <xref:System.Windows.Window> o para criar uma implementação de caixa de diálogo modal típica, usando a `Margins` caixa de diálogo como um exemplo (consulte a [caixa de diálogo](https://github.com/Microsoft/WPF-Samples/tree/master/Windows/DialogBox)de exemplo). A `Margins` caixa de diálogo é mostrada na figura a seguir.  
  
 ![Uma caixa de diálogo margens com campos para definir a margem esquerda, a margem superior, a margem direita e a margem inferior.](./media/dialog-boxes-overview/margin-size-dialog-box.png)  
  
#### <a name="configuring-a-modal-dialog-box"></a>Configurando uma caixa de diálogo modal

A interface do usuário de uma caixa de diálogo típica inclui o seguinte:  
  
- Os vários controles que são necessários para coletar os dados desejados.  
  
- Um botão **OK** em que os usuários clicam para fechar a caixa de diálogo, retornar à função e continuar o processamento.  
  
- Um botão **Cancelar** que os usuários clicam para fechar a caixa de diálogo e interromper a função de processamento adicional.  
  
- Um botão **fechar** na barra de título.  
  
- Um ícone.  
  
- Botões **minimizar**, **maximizar**e **restaurar** .  
  
- Um menu do **sistema** para minimizar, maximizar, restaurar e fechar a caixa de diálogo.  
  
- Uma posição acima e no centro da janela que abriu a caixa de diálogo.  
  
- A capacidade de ser redimensionada sempre que possível para impedir que a caixa de diálogo seja muito pequena e fornecer ao usuário um tamanho padrão útil. Isso requer que você defina as dimensões padrão e mínima.  
  
- A tecla ESC como um atalho de teclado que faz com que o botão **Cancelar** seja pressionado. Você faz isso definindo a <xref:System.Windows.Controls.Button.IsCancel%2A> Propriedade do botão de **cancelamento** como `true` .  
  
- A tecla ENTER (ou RETURN) como um atalho de teclado que faz com que o botão **OK** seja pressionado. Você faz isso definindo a <xref:System.Windows.Controls.Button.IsDefault%2A> Propriedade do botão **OK** `true` .  
  
O código a seguir demonstra essa configuração.  
  
[!code-xaml[MarginsDialogBox XAML file](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxSample/CSharp/MarginsDialogBox.xaml?range=1-16,106-112)]  

[!code-csharp[MarginsDialogBox C# code-behind](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxSample/CSharp/MarginsDialogBox.xaml.cs?range=1-12,67-68)]
[!code-vb[MarginsDialogBox VB code-behind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxSample/VisualBasic/MarginsDialogBox.xaml.vb?range=1-11,61-62)]  
  
A experiência do usuário em uma caixa de diálogo também se estende à barra de menus da janela que abre a caixa de diálogo. Quando um item de menu executar uma função que exige a interação do usuário por meio de uma caixa de diálogo antes que a função possa continuar, o item de menu da função terá reticências em seu cabeçalho, conforme mostrado aqui.  
  
[!code-xaml[Menu bar of MainWindow.Xaml file](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxSample/CSharp/MainWindow.xaml#L26-L27)]  
  
Quando um item de menu executar uma função que exibe uma caixa de diálogo que não exige a interação do usuário, como uma caixa de diálogo Sobre, as reticências não serão necessárias.  
  
#### <a name="opening-a-modal-dialog-box"></a>Abrindo uma caixa de diálogo modal

Normalmente, uma caixa de diálogo é mostrada como resultado da seleção por um usuário de um item de menu para executar uma função específica ao domínio, como definição das margens de um documento em um processador de texto. A exibição de uma janela como uma caixa de diálogo é semelhante à exibição de uma janela normal, embora ela exija uma configuração adicional específica à caixa de diálogo. O processo completo de criar uma instância, configurar e abrir uma caixa de diálogo é mostrado no código a seguir.  
  
[!code-csharp[Opening a modal dialog box](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxSample/CSharp/MainWindow.xaml.cs?range=1-11,78-88,193-195)]
[!code-vb[Opening a modal dialog box](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxSample/VisualBasic/MainWindow.xaml.vb?range=1-9,58-67,130-132)]  

Aqui, o código passa informações padrão (as margens atuais) para a caixa de diálogo. Ele também define a <xref:System.Windows.Window.Owner%2A?displayProperty=nameWithType> propriedade com uma referência à janela que está mostrando a caixa de diálogo. Em geral, você sempre deve definir o proprietário de uma caixa de diálogo para fornecer comportamentos relacionados ao estado da janela que são comuns a todas as caixas de diálogo (consulte [visão geral do WPF Windows](wpf-windows-overview.md) para obter mais informações).

> [!NOTE]
> Você deve fornecer um proprietário para dar suporte à automação da interface do usuário para caixas de diálogo (consulte [visão geral da automação da IU](../../ui-automation/ui-automation-overview.md)).

Depois que a caixa de diálogo é configurada, ela é mostrada modalmente chamando o <xref:System.Windows.Window.ShowDialog%2A> método.  
  
#### <a name="validating-user-provided-data"></a>Validando dados fornecidos pelo usuário

Quando uma caixa de diálogo é aberta e o usuário fornece os dados necessários, uma caixa de diálogo é responsável por garantir que os dados fornecidos são válidos pelos seguintes motivos:  
  
- De uma perspectiva de segurança, todas as informações devem ser validadas.  
  
- De uma perspectiva específica ao domínio, a validação de dados impede que dados incorretos sejam processados pelo código, o que potencialmente poderá gerar exceções.  
  
- De uma perspectiva da experiência do usuário, uma caixa de diálogo pode ajudar os usuários mostrando-lhes quais dados inseridos são inválidos.  
  
- De uma perspectiva do desempenho, a validação de dados em um aplicativo de várias camadas pode reduzir o número de viagens de ida e volta entre o cliente e as camadas do aplicativo, especialmente, quando o aplicativo é composto por serviços Web ou bancos de dados baseados em servidor.  

Para validar um controle ligado no WPF, você precisa definir uma regra de validação e associá-la à associação. Uma regra de validação é uma classe personalizada que deriva de <xref:System.Windows.Controls.ValidationRule> . O exemplo a seguir mostra uma regra de validação, `MarginValidationRule` , que verifica se um valor associado é a <xref:System.Double> e está dentro de um intervalo especificado.  

[!code-csharp[Margin validation rules](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxSample/CSharp/MarginValidationRule.cs)]
[!code-vb[Margin validation rules](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxSample/VisualBasic/MarginValidationRule.vb)]  

Nesse código, a lógica de validação de uma regra de validação é implementada substituindo o <xref:System.Windows.Controls.ValidationRule.Validate%2A> método, que valida os dados e retorna um apropriado <xref:System.Windows.Controls.ValidationResult> .  

Para associar a regra de validação ao controle associado, use a marcação a seguir.  
  
[!code-xaml[Associating a validation rule with a control](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxSample/CSharp/MarginsDialogBox.xaml?range=1-16,57-68,111-112)]

Depois que a regra de validação estiver associada, o WPF a aplicará automaticamente quando os dados forem inseridos no controle associado. Quando um controle contém dados inválidos, o WPF exibirá uma borda vermelha em torno do controle inválido, conforme mostrado na figura a seguir.  
  
![Uma caixa de diálogo margens com uma borda vermelha ao lado do valor inválido da margem esquerda.](./media/dialog-boxes-overview/invalid-left-margin-dialog.png)  

O WPF não restringe um usuário ao controle inválido até que eles tenham inserido dados válidos. Esse é bom comportamento para uma caixa de diálogo; um usuário poderá navegar livremente pelos controles em uma caixa de diálogo, independentemente de os dados serem válidos ou não. No entanto, isso significa que um usuário pode inserir dados inválidos e pressionar o botão **OK** . Por esse motivo, seu código também precisa validar todos os controles em uma caixa de diálogo quando o botão **OK** é pressionado manipulando o <xref:System.Windows.Controls.Primitives.ButtonBase.Click> evento.  
  
[!code-csharp[Validating all controls in a dialog box](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxSample/CSharp/MarginsDialogBox.xaml.cs?range=1-8,26-29,33-68)]
[!code-vb[Validating all controls in a dialog box](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxSample/VisualBasic/MarginsDialogBox.xaml.vb?range=1-8,27-29,33-62)]  

Esse código enumera todos os objetos de dependência em uma janela e, se algum deles for inválido (como retornado pelo <xref:System.Windows.Controls.Validation.GetHasError%2A> , o controle inválido obtém o foco, o `IsValid` método retorna `false` e a janela é considerada inválida.  
  
Depois que uma caixa de diálogo for válida, ela pode ser fechada e retornada com segurança. Como parte do processo de retorno, ela precisa retornar um resultado para a função de chamada.  
  
#### <a name="setting-the-modal-dialog-result"></a>Configurando o resultado da caixa de diálogo modal

Abrir uma caixa de diálogo usando <xref:System.Windows.Window.ShowDialog%2A> é fundamentalmente como chamar um método: o código que abriu a caixa de diálogo usando <xref:System.Windows.Window.ShowDialog%2A> espera até <xref:System.Windows.Window.ShowDialog%2A> retorna. Quando <xref:System.Windows.Window.ShowDialog%2A> retorna, o código que o chamou precisa decidir se deseja continuar processando ou parar o processamento, dependendo se o usuário pressionou o botão **OK** ou o botão **Cancelar** . Para facilitar essa decisão, a caixa de diálogo precisa retornar a opção do usuário como um <xref:System.Boolean> valor que é retornado do <xref:System.Windows.Window.ShowDialog%2A> método.  

Quando o botão **OK** é clicado, <xref:System.Windows.Window.ShowDialog%2A> deve retornar `true` . Isso é feito definindo a <xref:System.Windows.Window.DialogResult%2A> propriedade da caixa de diálogo quando o botão **OK** é clicado.  

[!code-csharp[Responding to the OK button](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxSample/CSharp/MarginsDialogBox.xaml.cs?range=1-8,25-27,32-33,67-68)]
[!code-vb[Responding to the OK button](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxSample/VisualBasic/MarginsDialogBox.xaml.vb?range=1-8,27,31-33,61-62)]  

Observe que a configuração da <xref:System.Windows.Window.DialogResult%2A> propriedade também faz com que a janela seja fechada automaticamente, o que alivia a necessidade de chamar explicitamente <xref:System.Windows.Window.Close%2A> .  
  
Quando o botão **Cancelar** é clicado, <xref:System.Windows.Window.ShowDialog%2A> deve retornar `false` , que também requer a definição da <xref:System.Windows.Window.DialogResult%2A> propriedade.  
  
[!code-csharp[Responding to the Cancel button](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxSample/CSharp/MarginsDialogBox.xaml.cs?range=1-8,19-24,67-68)]
[!code-vb[Responding to the Cancel button](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxSample/VisualBasic/MarginsDialogBox.xaml.vb?range=1-8,22-25,61-62)]  

Quando a propriedade de um botão <xref:System.Windows.Controls.Button.IsCancel%2A> é definida como `true` e o usuário pressiona o botão **Cancelar** ou a tecla ESC, <xref:System.Windows.Window.DialogResult%2A> é automaticamente definido como `false` . A marcação a seguir tem o mesmo efeito que o código anterior, sem a necessidade de lidar com o <xref:System.Windows.Controls.Primitives.ButtonBase.Click> evento.  
  
[!code-xaml[Markup instead of handling the Click event](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxSample/CSharp/MarginsDialogBox.xaml#L109-L109)]  

Uma caixa de diálogo retorna automaticamente `false` quando um usuário pressiona o botão **fechar** na barra de título ou escolhe o item de menu **Fechar** no menu do **sistema** .  

#### <a name="processing-data-returned-from-a-modal-dialog-box"></a>Processando dados retornados de uma caixa de diálogo modal  

Quando <xref:System.Windows.Window.DialogResult%2A> é definido por uma caixa de diálogo, a função que a abriu pode obter o resultado da caixa de diálogo inspecionando a <xref:System.Windows.Window.DialogResult%2A> propriedade quando <xref:System.Windows.Window.ShowDialog%2A> retorna.  
  
[!code-csharp[Processing data returned from the modal dialog box](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxSample/CSharp/MainWindow.xaml.cs?range=1-10,77-79,89-96,194-195)]
[!code-vb[Processing data returned from the modal dialog box](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxSample/VisualBasic/MainWindow.xaml.vb?range=1-9,58,69-73,131-132)]

Se o resultado da caixa de diálogo for `true` , a função usará isso como uma indicação para recuperar e processar os dados fornecidos pelo usuário.  
  
> [!NOTE]
> Depois <xref:System.Windows.Window.ShowDialog%2A> de retornado, uma caixa de diálogo não pode ser reaberta. Em vez disso, você precisa criar uma nova instância.

Se o resultado da caixa de diálogo for `false` , a função deverá terminar o processamento adequadamente.  
  
<a name="Creating_a_Modeless_Custom_Dialog_Box"></a>
### <a name="creating-a-modeless-custom-dialog-box"></a>Criando uma caixa de diálogo personalizada sem janela restrita

Uma caixa de diálogo sem janela restrita, como a Caixa de Diálogo Localizar mostrada na figura a seguir, tem a mesma aparência básica que a caixa de diálogo modal.  

![Captura de tela que mostra uma caixa de diálogo localizar.](./media/dialog-boxes-overview/find-modeless-dialog-box.png)  

No entanto, o comportamento é ligeiramente diferente, conforme descrito nas próximas seções.  
  
#### <a name="opening-a-modeless-dialog-box"></a>Abrindo uma caixa de diálogo sem janela restrita

Uma caixa de diálogo sem janela restrita é aberta chamando o <xref:System.Windows.Window.Show%2A> método.  

[!code-xaml[XAML to define a modeless dialog box](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxSample/CSharp/MainWindow.xaml#L21-L22)]  

[!code-csharp[Opening a modeless dialog box](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxSample/CSharp/MainWindow.xaml.cs?range=1-10,65-76,194-195)]
[!code-vb[Openng a modeless dialog box](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxSample/VisualBasic/MainWindow.xaml.vb?range=1-9,18-23,131,132)]  

Ao contrário <xref:System.Windows.Window.ShowDialog%2A> de, <xref:System.Windows.Window.Show%2A> retorna imediatamente. Consequentemente, a janela de chamada não pode saber quando a caixa de diálogo sem janela restrita é fechada e, portanto, não sabe quando procurar um resultado da caixa de diálogo nem quando obter dados da caixa de diálogo para processamento adicional. Em vez disso, a caixa de diálogo precisa criar uma maneira alternativa de retornar dados para a janela de chamada para o processamento.  
  
#### <a name="processing-data-returned-from-a-modeless-dialog-box"></a>Processando dados retornados de uma caixa de diálogo sem janela restrita  

Neste exemplo, o `FindDialogBox` pode retornar um ou mais resultados de find para a janela principal, dependendo do texto que está sendo pesquisado sem nenhuma frequência específica. Assim como ocorre com uma caixa de diálogo modal, uma caixa de diálogo sem janela restrita pode retornar resultados usando propriedades. No entanto, a janela que possui a caixa de diálogo precisa saber quando deve verificar essas propriedades. Uma maneira de habilitar essa opção é para que a caixa de diálogo implemente um evento que é acionado sempre que um texto é encontrado. `FindDialogBox`implementa o `TextFoundEvent` para essa finalidade, que primeiro requer um delegado.  

[!code-csharp[The TextFoundEventHandler delegate](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxSample/CSharp/TextFoundEventHandler.cs)]
[!code-vb[The TextFoundEventHandler delegate](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxSample/VisualBasic/TextFoundEventHandler.vb)]  

Usando o `TextFoundEventHandler` delegado, o `FindDialogBox` implementa o `TextFoundEvent` .
  
[!code-csharp[The TextFound event](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxSample/CSharp/FindDialogBox.xaml.cs?range=1-17,125-126)]
[!code-vb[The TextFound event](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxSample/VisualBasic/FindDialogBox.xaml.vb?range=1-15,102-103)]

Consequentemente, `Find` o pode gerar o evento quando um resultado da pesquisa for encontrado.  
  
[!code-csharp[Raising the TextFound event](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxSample/CSharp/FindDialogBox.xaml.cs?range=1-9,50-52,91-94,124-127)]
[!code-vb[Raising the TextFound event](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxSample/VisualBasic/FindDialogBox.xaml.vb?range=1-9,15,60-64,102-103)]  

A janela do proprietário precisa então se registrar e tratar esse evento.

[!code-csharp[Registering and handling the event](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxSample/CSharp/MainWindow.xaml.cs?range=1-10,184-195)]
[!code-vb[Registering and handling the event](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxSample/VisualBasic/MainWindow.xaml.vb?range=1-9,126-132)]  

#### <a name="closing-a-modeless-dialog-box"></a>Fechando uma caixa de diálogo sem janela restrita

Como <xref:System.Windows.Window.DialogResult%2A> o não precisa ser definido, uma caixa de diálogo sem janela restrita pode ser fechada usando mecanismos de fornecimento do sistema, incluindo o seguinte:  
  
- Clicando no botão **fechar** na barra de título.  
  
- Pressionar ALT + F4.  
  
- Escolhendo **fechar** no menu do **sistema** .  
  
Como alternativa, seu código pode chamar <xref:System.Windows.Window.Close%2A> quando o botão **fechar** é clicado.  

[!code-csharp[Calling the Close method](~/samples/snippets/csharp/VS_Snippets_Wpf/DialogBoxSample/CSharp/FindDialogBox.xaml.cs?range=1-9,119-126)]
[!code-vb[Calling the Close method](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DialogBoxSample/VisualBasic/FindDialogBox.xaml.vb?range=1-9,99-103)]  

## <a name="see-also"></a>Consulte também

- [Visão geral do pop-up](../controls/popup-overview.md)
- [Amostra de caixa de diálogo](https://github.com/Microsoft/WPF-Samples/tree/master/Windows/DialogBox)
