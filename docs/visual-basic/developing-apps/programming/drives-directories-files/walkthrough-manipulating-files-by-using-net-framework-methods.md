---
title: manipular arquivos usando métodos do .NET Framework
ms.date: 07/20/2015
helpviewer_keywords:
- I/O [Visual Basic], walkthroughs
- text files [Visual Basic], writing to
- reading text files [Visual Basic]
- text, writing to files
- files [Visual Basic], searching
- StreamReader class, walkthroughs
- files [Visual Basic], accessing
- I/O [Visual Basic], writing text to files
- writing to files [Visual Basic], walkthroughs
- StreamWriter class, walkthroughs
- text files [Visual Basic], reading
- I/O [Visual Basic], reading text from files
ms.assetid: 7d2109eb-f98a-4389-b43d-30f384aaa7d5
ms.openlocfilehash: 9abb87f3f6cdefefef29eb37c2c2d4d15155e93d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406645"
---
# <a name="walkthrough-manipulating-files-by-using-net-framework-methods-visual-basic"></a>Instruções passo a passo: manipulando arquivos usando métodos do .NET Framework (Visual Basic)

Estas instruções passo a passo demonstram como abrir e ler um arquivo usando a classe <xref:System.IO.StreamReader>, verificar se um arquivo está sendo acessado, pesquisar uma cadeia de caracteres dentro de um arquivo lido com uma instância da classe <xref:System.IO.StreamReader> e gravar em um arquivo usando a classe <xref:System.IO.StreamWriter>.

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

## <a name="creating-the-application"></a>Criando o aplicativo

Inicie o Visual Studio e comece o projeto criando um formulário que o usuário pode usar para gravar no arquivo designado.

### <a name="to-create-the-project"></a>Para criar o projeto

1. No menu **Arquivo**, selecione **Novo Projeto**.

2. No painel **Novo Projeto**, clique em **Aplicativos do Windows**.

3. Na caixa **nome** , digite `MyDiary` e clique em **OK**.

     O Visual Studio adiciona o projeto a **Gerenciador de soluções**e o **Designer de formulários do Windows** é aberto.

4. Adicione os controles na tabela a seguir ao formulário e defina os valores correspondentes para as respectivas propriedades.

|**Objeto**|**Propriedades**|**Valor**|
|---|---|---|
|<xref:System.Windows.Forms.Button>|**Nome**<br /><br /> **Texto**|`Submit`<br /><br /> **Enviar entrada**|
|<xref:System.Windows.Forms.Button>|**Nome**<br /><br /> **Texto**|`Clear`<br /><br /> **Limpar entrada**|
|<xref:System.Windows.Forms.TextBox>|**Nome**<br /><br /> **Texto**<br /><br /> **Multilinha**|`Entry`<br /><br /> **Digite algo.**<br /><br /> `False`|

## <a name="writing-to-the-file"></a>Gravando no arquivo

Para adicionar a capacidade de gravar em um arquivo por meio do aplicativo, use a classe <xref:System.IO.StreamWriter>. <xref:System.IO.StreamWriter> foi criado para a saída de caracteres em uma determinada codificação, enquanto a classe <xref:System.IO.Stream> foi criada para entrada e saída em bytes. Use <xref:System.IO.StreamWriter> para gravar linhas de informações em um arquivo de texto padrão. Para saber mais sobre a classe <xref:System.IO.StreamWriter>, veja <xref:System.IO.StreamWriter>.

### <a name="to-add-writing-functionality"></a>Para adicionar a funcionalidade de gravação

1. No menu **Exibição**, escolha **Código** para abrir o Editor de código.

2. Como o aplicativo faz referência ao namespace <xref:System.IO>, adicione as seguintes instruções ao início do seu código, antes da declaração de classe para o formulário, que inicia `Public Class Form1`.

     [!code-vb[VbVbcnMyFileSystem#35](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#35)]

     Antes de gravar no arquivo, você precisa criar uma instância de uma classe <xref:System.IO.StreamWriter>.

3. No menu **Exibição**, escolha **Designer** para voltar para o **Designer de Formulários do Windows**. Clique duas vezes no botão `Submit` para criar um manipulador de eventos <xref:System.Windows.Forms.Control.Click> para o botão e adicione o código a seguir.

     [!code-vb[VbVbcnMyFileSystem#36](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#36)]

> [!NOTE]
> O IDE (ambiente de desenvolvimento integrado) do Visual Studio retornará ao Editor de código e posicionará o ponto de inserção dentro do manipulador de eventos, no qual você deve adicionar o código.

1. Para gravar o arquivo, use o método <xref:System.IO.StreamWriter.Write%2A> da classe <xref:System.IO.StreamWriter>. Adicione o código a seguir diretamente após `Dim fw As StreamWriter`. Você não precisa se preocupar se uma exceção será gerada caso o arquivo não seja encontrado, porque ele será criado se ainda não existir.

     [!code-vb[VbVbcnMyFileSystem#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#37)]

2. Certifique-se de que o usuário não possa enviar uma entrada em branco adicionando o código a seguir diretamente após `Dim ReadString As String`.

     [!code-vb[VbVbcnMyFileSystem#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#38)]

3. Como este é uma diário, o usuário desejará atribuir uma data a cada entrada. Insira o código a seguir após `fw = New StreamWriter("C:\MyDiary.txt", True)` para definir a variável `Today` como a data atual.

     [!code-vb[VbVbcnMyFileSystem#39](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#39)]

4. Por fim, anexe o código para limpar a <xref:System.Windows.Forms.TextBox>. Adicione o seguinte código ao evento <xref:System.Windows.Forms.Control.Click> do botão `Clear`.

     [!code-vb[VbVbcnMyFileSystem#40](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#40)]

## <a name="adding-display-features-to-the-diary"></a>Adicionando recursos de exibição ao diário

Nesta seção, você adiciona um recurso que exibe a última entrada em `DisplayEntry`<xref:System.Windows.Forms.TextBox>. Você também pode adicionar uma <xref:System.Windows.Forms.ComboBox> que exiba várias entradas e a partir da qual um usuário pode selecionar uma entrada para exibir o `DisplayEntry`<xref:System.Windows.Forms.TextBox>. Uma instância da classe <xref:System.IO.StreamReader> é lida de `MyDiary.txt`. Como a classe <xref:System.IO.StreamWriter>, o <xref:System.IO.StreamReader> deve ser usado com arquivos de texto.

Para esta seção do passo a passo, adicione os controles na tabela a seguir ao formulário e defina os valores correspondentes para as respectivas propriedades.

|Control|Propriedades|Valores|
|-------------|----------------|------------|
|<xref:System.Windows.Forms.TextBox>|**Nome**<br /><br /> **Visível**<br /><br /> **Tamanho**<br /><br /> **Multilinha**|`DisplayEntry`<br /><br /> `False`<br /><br /> `120,60`<br /><br /> `True`|
|<xref:System.Windows.Forms.Button>|**Nome**<br /><br /> **Texto**|`Display`<br /><br /> **Exibição**|
|<xref:System.Windows.Forms.Button>|**Nome**<br /><br /> **Texto**|`GetEntries`<br /><br /> **Obter entradas**|
|<xref:System.Windows.Forms.ComboBox>|**Nome**<br /><br /> **Texto**<br /><br /> **Enabled**|`PickEntries`<br /><br /> **Selecionar uma entrada**<br /><br /> `False`|

### <a name="to-populate-the-combo-box"></a>Para popular a caixa de combinação

1. A `PickEntries`<xref:System.Windows.Forms.ComboBox> é usada para exibir as datas em que um usuário envia cada entrada, para que o usuário possa selecionar uma entrada de uma data específica. Crie um identificador de evento <xref:System.Windows.Forms.Control.Click> para o botão `GetEntries` e adicione o seguinte código.

     [!code-vb[VbVbcnMyFileSystem#41](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#41)]

2. Para testar seu código, pressione F5 para compilar o aplicativo e, então, clique em **Obter entradas**. Clique na seta do menu suspenso no <xref:System.Windows.Forms.ComboBox> para exibir as datas de entrada.

### <a name="to-choose-and-display-individual-entries"></a>Para escolher e exibir entradas individuais

1. Crie um manipulador de eventos <xref:System.Windows.Forms.Control.Click> para o botão `Display` e adicione o seguinte código.

     [!code-vb[VbVbcnMyFileSystem#42](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#42)]

2. Para testar seu código, pressione F5 para compilar o aplicativo e, então, envie uma entrada. Clique em **Obter Entradas**, selecione uma entrada da <xref:System.Windows.Forms.ComboBox> e clique em **Exibir**. Os conteúdos da entrada selecionada são exibidos no `DisplayEntry`<xref:System.Windows.Forms.TextBox>.

## <a name="enabling-users-to-delete-or-modify-entries"></a>Habilitando usuários a excluir ou modificar entradas

Por fim, você pode incluir uma funcionalidade adicional que permite que os usuários excluam ou modifiquem uma entrada usando os botões `DeleteEntry` e `EditEntry`. Os dois botões permanecem desabilitados a menos que uma entrada seja exibida.

Adicione os controles na tabela a seguir ao formulário e defina os valores correspondentes para as respectivas propriedades.

|Control|Propriedades|Valores|
|-------------|----------------|------------|
|<xref:System.Windows.Forms.Button>|**Nome**<br /><br /> **Texto**<br /><br /> **Enabled**|`DeleteEntry`<br /><br /> **Excluir entrada**<br /><br /> `False`|
|<xref:System.Windows.Forms.Button>|**Nome**<br /><br /> **Texto**<br /><br /> **Enabled**|`EditEntry`<br /><br /> **Editar entrada**<br /><br /> `False`|
|<xref:System.Windows.Forms.Button>|**Nome**<br /><br /> **Texto**<br /><br /> **Enabled**|`SubmitEdit`<br /><br /> **Enviar edição**<br /><br /> `False`|

### <a name="to-enable-deletion-and-modification-of-entries"></a>Para habilitar a exclusão e modificação de entradas

1. Adicione o seguinte código ao evento <xref:System.Windows.Forms.Control.Click> do botão `Display` , depois de `DisplayEntry.Text = ReadString`.

     [!code-vb[VbVbcnMyFileSystem#43](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#43)]

2. Crie um manipulador de eventos <xref:System.Windows.Forms.Control.Click> para o botão `DeleteEntry` e adicione o seguinte código.

     [!code-vb[VbVbcnMyFileSystem#44](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#44)]

3. Quando um usuário exibe uma entrada, o botão `EditEntry` fica habilitado. Adicione o seguinte código ao evento <xref:System.Windows.Forms.Control.Click> do botão `Display`, depois de `DisplayEntry.Text = ReadString`.

     [!code-vb[VbVbcnMyFileSystem#45](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#45)]

4. Crie um manipulador de eventos <xref:System.Windows.Forms.Control.Click> para o botão `EditEntry` e adicione o seguinte código.

     [!code-vb[VbVbcnMyFileSystem#46](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#46)]

5. Crie um manipulador de eventos <xref:System.Windows.Forms.Control.Click> para o botão `SubmitEdit` e adicione o seguinte código

     [!code-vb[VbVbcnMyFileSystem#47](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#47)]

Para testar seu código, pressione F5 para compilar o aplicativo. Clique em **Obter Entradas**, selecione uma entrada e clique em **Exibir**. A entrada aparece na `DisplayEntry`<xref:System.Windows.Forms.TextBox>. Clique em **Editar entrada**. A entrada aparece na `Entry`<xref:System.Windows.Forms.TextBox>. Edite a entrada no `Entry` <xref:System.Windows.Forms.TextBox> e clique em **Enviar editar**. Abra o arquivo `MyDiary.txt` para confirmar a correção. Agora, selecione uma entrada e clique em **Excluir entrada**. Quando o <xref:System.Windows.Forms.MessageBox> solicita confirmação, clique em **OK**. Feche o aplicativo e abra `MyDiary.txt` para confirmar a exclusão.

## <a name="see-also"></a>Confira também

- <xref:System.IO.StreamReader>
- <xref:System.IO.StreamWriter>
- [Passo a passo](../../../walkthroughs.md)
