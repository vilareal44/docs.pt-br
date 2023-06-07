---
title: 'Como: ler de arquivos de texto de largura fixa'
ms.date: 07/20/2015
helpviewer_keywords:
- fixed-width text file
- reading text files [Visual Basic], fixed-width
- files [Visual Basic], parsing
- text files [Visual Basic], tasks
- text files [Visual Basic], reading
ms.assetid: 99be5692-967a-4e85-993e-cd18139a5a69
ms.openlocfilehash: 77b2e0a4ebe36b68501f821ef5731935ee3b16a7
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411623"
---
# <a name="how-to-read-from-fixed-width-text-files-in-visual-basic"></a>Como ler de arquivos de texto de largura fixa no Visual Basic

O objeto `TextFieldParser` fornece uma maneira fácil e eficiente de analisar arquivos de texto estruturados, como logs.  
  
 A propriedade `TextFieldType` define se o arquivo analisado é um arquivo delimitado ou um arquivo com campos de texto de largura fixa. Em um arquivo de texto de largura fixa, o campo no final pode ter largura variável. Para especificar que o campo no final tenha largura variável, defina sua largura como menor ou igual a zero.  
  
### <a name="to-parse-a-fixed-width-text-file"></a>Para analisar um arquivo de texto de largura fixa  
  
1. Crie um `TextFieldParser`. O código a seguir cria o `TextFieldParser` chamado `Reader` e abre o arquivo `test.log`.  
  
     [!code-vb[VbFileIORead#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#9)]  
  
2. Defina a propriedade `TextFieldType` como `FixedWidth`, definindo a largura e o formato. O código a seguir define as colunas de texto; a primeira tem 5 caracteres de largura, a segunda tem 10, a terceira tem 11 e a quarta tem largura variável.  
  
     [!code-vb[VbFileIORead#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#10)]  
  
3. Percorra em loop os campos no arquivo. Se alguma linha estiver corrompida, relate um erro e continue com a análise.  
  
     [!code-vb[VbFileIORead#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#11)]  
  
4. Feche os blocos `While` e `Using` com `End While` e `End Using`.  
  
     [!code-vb[VbFileIORead#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#12)]  
  
## <a name="example"></a>Exemplo  

 Este exemplo lê do arquivo `test.log`.  
  
 [!code-vb[VbFileIORead#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#13)]  
  
## <a name="robust-programming"></a>Programação robusta  

 As seguintes condições podem causar uma exceção:  
  
- Não é possível analisar uma linha usando o formato especificado (<xref:Microsoft.VisualBasic.FileIO.MalformedLineException>). A mensagem de exceção especifica a linha que está causando a exceção, enquanto a propriedade <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.ErrorLine%2A> é atribuída ao texto contido na linha.  
  
- O arquivo especificado não existe (<xref:System.IO.FileNotFoundException>).  
  
- Uma situação de confiança parcial na qual o usuário não tem permissões suficientes para acessar o arquivo. (<xref:System.Security.SecurityException>).  
  
- O caminho é muito longo (<xref:System.IO.PathTooLongException>).  
  
- O usuário não tem permissões suficientes para acessar o arquivo (<xref:System.UnauthorizedAccessException>).  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser?displayProperty=nameWithType>
- [Como: ler de arquivos de texto separados por vírgula](how-to-read-from-comma-delimited-text-files.md)
- [Como: ler de arquivos de texto com vários formatos](how-to-read-from-text-files-with-multiple-formats.md)
- [Analisando arquivos de texto com o objeto TextFieldParser](parsing-text-files-with-the-textfieldparser-object.md)
- [Instruções passo a passo: manipulando arquivos e diretórios no Visual Basic](walkthrough-manipulating-files-and-directories.md)
- [Solução de problemas: ler e gravar em arquivos de texto](troubleshooting-reading-from-and-writing-to-text-files.md)
