---
title: Escolher pastas com o componente FolderBrowserDialog
description: Saiba como usar o componente Windows Forms FolderBrowserDialog em aplicativos do Windows que você cria para solicitar que os usuários selecionem uma pasta.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- directories [Windows Forms], choosing
- FolderBrowserDialog component [Windows Forms], choosing directories
- folders [Windows Forms], selecting
- folders [Windows Forms], choosing
- directories [Windows Forms], selecting
ms.assetid: 4593670e-7c7d-4661-b46b-4ffb63258adb
ms.openlocfilehash: 11d01bbaec3b82bc221960ebab5e33ca1aa302db
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84903669"
---
# <a name="how-to-choose-folders-with-the-windows-forms-folderbrowserdialog-component"></a>Como escolher pastas com o componente FolderBrowserDialog dos Windows Forms

Muitas vezes, dentro de aplicativos do Windows que você criou, será necessário solicitar que os usuários selecionem uma pasta e com mais frequência que eles salvem um conjunto de arquivos. O <xref:System.Windows.Forms.FolderBrowserDialog> componente Windows Forms permite que você realize essa tarefa facilmente.

### <a name="to-choose-folders-with-the-folderbrowserdialog-component"></a>Escolher pastas com o componente FolderBrowserDialog

1. Em um procedimento, verifique a <xref:System.Windows.Forms.FolderBrowserDialog> Propriedade do componente <xref:System.Windows.Forms.Form.DialogResult%2A> para ver como a caixa de diálogo foi fechada e obter o valor da <xref:System.Windows.Forms.FolderBrowserDialog> Propriedade do componente <xref:System.Windows.Forms.FolderBrowserDialog.SelectedPath%2A> .

2. Se você precisar definir a pasta superior que aparecerá dentro do modo de exibição de árvore da caixa de diálogo, defina a <xref:System.Windows.Forms.FolderBrowserDialog.RootFolder%2A> propriedade, que usa um membro da <xref:System.Environment.SpecialFolder> enumeração.

3. Além disso, você pode definir a <xref:System.Windows.Forms.FolderBrowserDialog.Description%2A> propriedade, que especifica a cadeia de caracteres de texto que aparece na parte superior da exibição de árvore de navegador de pasta.

    No exemplo a seguir, o <xref:System.Windows.Forms.FolderBrowserDialog> componente é usado para selecionar uma pasta, semelhante a quando você cria um projeto no Visual Studio e é solicitado a selecionar uma pasta na qual salvá-la. Neste exemplo, o nome da pasta é exibido em um <xref:System.Windows.Forms.TextBox> controle no formulário. É uma boa ideia posicionar o local em uma área editável, como um <xref:System.Windows.Forms.TextBox> controle, para que os usuários possam editar sua seleção em caso de erro ou outros problemas. Este exemplo pressupõe um formulário com um <xref:System.Windows.Forms.FolderBrowserDialog> componente e um <xref:System.Windows.Forms.TextBox> controle.

    ```vb
    Public Sub ChooseFolder()
        If FolderBrowserDialog1.ShowDialog() = DialogResult.OK Then
            TextBox1.Text = FolderBrowserDialog1.SelectedPath
        End If
    End Sub
    ```

    ```csharp
    public void ChooseFolder()
    {
        if (folderBrowserDialog1.ShowDialog() == DialogResult.OK)
        {
            textBox1.Text = folderBrowserDialog1.SelectedPath;
        }
    }
    ```

    ```cpp
    public:
       void ChooseFolder()
       {
          if (folderBrowserDialog1->ShowDialog() == DialogResult::OK)
          {
             textBox1->Text = folderBrowserDialog1->SelectedPath;
          }
       }
    ```

    > [!IMPORTANT]
    > Para usar essa classe, seu assembly requer um nível de privilégio concedido pela <xref:System.Security.Permissions.FileIOPermissionAttribute.PathDiscovery%2A> propriedade, que faz parte da <xref:System.Security.Permissions.FileIOPermissionAccess> enumeração. Se você estiver executando em um contexto de confiança parcial, o processo poderá gerar uma exceção em razão dos privilégios insuficientes. Para obter mais informações, consulte [Noções Básicas da Segurança de Acesso do Código](../../misc/code-access-security-basics.md).

Para obter informações sobre como salvar arquivos, consulte [Como salvar arquivos usando o componente SaveFileDialog](how-to-save-files-using-the-savefiledialog-component.md).

## <a name="see-also"></a>Veja também

- <xref:System.Windows.Forms.FolderBrowserDialog>
- [Visão geral do componente FolderBrowserDialog (Windows Forms)](folderbrowserdialog-component-overview-windows-forms.md)
- [Componente FolderBrowserDialog](folderbrowserdialog-component-windows-forms.md)
