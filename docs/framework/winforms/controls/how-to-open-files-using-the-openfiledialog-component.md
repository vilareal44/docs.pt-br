---
title: 'Como: abrir arquivos com o componente OpenFileDialog'
ms.date: 02/11/2019
description: Saiba como usar o componente OpenFileDialog para abrir a caixa de diálogo do Windows para procurar e selecionar arquivos.
dev_langs:
- csharp
- vb
helpviewer_keywords:
- OpenFileDialog component [Windows Forms], opening files
- OpenFile method [Windows Forms], OpenFileDialog component
- files [Windows Forms], opening with OpenFileDialog component
ms.assetid: 9d88367a-cc21-4ffd-be74-89fd63767d35
ms.openlocfilehash: d571885011b0f0c723c73a417f294f30f96952f4
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904423"
---
# <a name="how-to-open-files-with-the-openfiledialog"></a>Como abrir arquivos com o OpenFileDialog

O <xref:System.Windows.Forms.OpenFileDialog?displayProperty=nameWithType> componente abre a caixa de diálogo do Windows para procurar e selecionar arquivos. Para abrir e ler os arquivos selecionados, você pode usar o <xref:System.Windows.Forms.OpenFileDialog.OpenFile%2A?displayProperty=nameWithType> método ou criar uma instância da <xref:System.IO.StreamReader?displayProperty=nameWithType> classe. Os exemplos a seguir mostram ambas as abordagens.

No .NET Framework, para obter ou definir a <xref:System.Windows.Forms.FileDialog.FileName%2A> propriedade, é necessário um nível de privilégio concedido pela <xref:System.Security.Permissions.FileIOPermission?displayProperty=nameWithType> classe. Os exemplos executam uma <xref:System.Security.Permissions.FileIOPermission> verificação de permissão e podem gerar uma exceção devido a privilégios insuficientes se forem executados em um contexto de confiança parcial. Para obter mais informações, consulte [noções básicas de segurança de acesso ao código](../../misc/code-access-security-basics.md).

Você pode criar e executar esses exemplos como .NET Framework aplicativos da linha de comando C# ou Visual Basic. Para obter mais informações, consulte [criação de linha de comando com csc.exe](../../../csharp/language-reference/compiler-options/command-line-building-with-csc-exe.md) ou [Build na linha de comando](../../../visual-basic/reference/command-line-compiler/building-from-the-command-line.md).

A partir do .NET Core 3,0, você também pode criar e executar os exemplos como aplicativos do Windows .NET Core de uma pasta que tem um arquivo de projeto Windows Forms * \<folder name> . csproj* do .NET Core.

## <a name="example-read-a-file-as-a-stream-with-streamreader"></a>Exemplo: ler um arquivo como um fluxo com StreamReader  
  
O exemplo a seguir usa o <xref:System.Windows.Forms.Button> manipulador de eventos do controle de Windows Forms <xref:System.Windows.Forms.Control.Click> para abrir o <xref:System.Windows.Forms.OpenFileDialog> com o <xref:System.Windows.Forms.CommonDialog.ShowDialog%2A> método. Depois que o usuário escolhe um arquivo e seleciona **OK**, uma instância da <xref:System.IO.StreamReader> classe lê o arquivo e exibe seu conteúdo na caixa de texto do formulário. Para obter mais informações sobre a leitura de fluxos de arquivos, consulte <xref:System.IO.FileStream.BeginRead%2A?displayProperty=nameWithType> e <xref:System.IO.FileStream.Read%2A?displayProperty=nameWithType> .  

 [!code-csharp[OpenFileDialog#1](~/samples/snippets/winforms/open-files/example1/cs/Form1.cs)]
 [!code-vb[OpenFileDialog#1](~/samples/snippets/winforms/open-files/example1/vb/Form1.vb)]  

## <a name="example-open-a-file-from-a-filtered-selection-with-openfile"></a>Exemplo: abrir um arquivo de uma seleção filtrada com o OpenFile

O exemplo a seguir usa o <xref:System.Windows.Forms.Button> manipulador de eventos do controle <xref:System.Windows.Forms.Control.Click> para abrir o <xref:System.Windows.Forms.OpenFileDialog> com um filtro que mostra apenas arquivos de texto. Depois que o usuário escolhe um arquivo de texto e seleciona **OK**, o <xref:System.Windows.Forms.OpenFileDialog.OpenFile%2A> método é usado para abrir o arquivo no bloco de notas.

 [!code-csharp[OpenFileDialog#2](~/samples/snippets/winforms/open-files/example2/cs/Form1.cs)]
 [!code-vb[OpenFileDialog#2](~/samples/snippets/winforms/open-files/example2/vb/Form1.vb)]  

## <a name="see-also"></a>Veja também

- <xref:System.Windows.Forms.OpenFileDialog>
- [Componente OpenFileDialog](openfiledialog-component-windows-forms.md)
