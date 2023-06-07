---
title: 'Como: ler de arquivos de texto delimitados por vírgula'
ms.date: 07/20/2015
helpviewer_keywords:
- files [Visual Basic], parsing
- text files [Visual Basic], tasks
- reading text files [Visual Basic], comma-delimited
- text files [Visual Basic], reading
ms.assetid: a8413fe4-0dba-49c8-8692-44fb67a9ec4f
ms.openlocfilehash: ba62a0226a1a83326cbc7ab393d135763a7c7cb2
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411636"
---
# <a name="how-to-read-from-comma-delimited-text-files-in-visual-basic"></a>Como ler em arquivos de texto separados por vírgulas no Visual Basic

O objeto `TextFieldParser` fornece uma maneira fácil e eficiente de analisar arquivos de texto estruturados, como logs. A propriedade `TextFieldType` define se ele é um arquivo delimitado ou um arquivo com campos de texto de largura fixa.  
  
### <a name="to-parse-a-comma-delimited-text-file"></a>Para analisar um arquivo de texto delimitado por vírgulas  
  
1. Crie um `TextFieldParser`. O código a seguir cria o `TextFieldParser` chamado `MyReader` e abre o arquivo `test.txt`.  
  
     [!code-vb[VbFileIORead#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#15)]  
  
2. Defina o delimitador e o tipo `TextField`. O código a seguir define a propriedade `TextFieldType` como `Delimited` e o delimitador como “,”.  
  
     [!code-vb[VbFileIORead#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#16)]  
  
3. Percorra em loop os campos no arquivo. Se alguma linha estiver corrompida, relate um erro e continue a análise. O código a seguir faz um loop pelo arquivo, exibindo cada campo por sua vez e relatando quaisquer campos que estejam formatados incorretamente.  
  
     [!code-vb[VbFileIORead#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#17)]  
  
4. Feche os blocos `While` e `Using` com `End While` e `End Using`.  
  
     [!code-vb[VbFileIORead#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#18)]  
  
## <a name="example"></a>Exemplo  

 Este exemplo lê do arquivo `test.txt`.  
  
 [!code-vb[VbFileIORead#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#19)]  
  
## <a name="robust-programming"></a>Programação robusta  

 As seguintes condições podem causar uma exceção:  
  
- Não é possível analisar uma linha usando o formato especificado (<xref:Microsoft.VisualBasic.FileIO.MalformedLineException>). A mensagem de exceção especifica a linha causando a exceção, enquanto a propriedade <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.ErrorLine%2A> é atribuída ao texto contido na linha.  
  
- O arquivo especificado não existe (<xref:System.IO.FileNotFoundException>).  
  
- Uma situação de confiança parcial na qual o usuário não tem permissões suficientes para acessar o arquivo. (<xref:System.Security.SecurityException>).  
  
- O caminho é muito longo (<xref:System.IO.PathTooLongException>).  
  
- O usuário não tem permissões suficientes para acessar o arquivo (<xref:System.UnauthorizedAccessException>).  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser?displayProperty=nameWithType>
- [Como: ler de arquivos de texto de largura fixa](how-to-read-from-fixed-width-text-files.md)
- [Como: ler de arquivos de texto com vários formatos](how-to-read-from-text-files-with-multiple-formats.md)
- [Analisando arquivos de texto com o objeto TextFieldParser](parsing-text-files-with-the-textfieldparser-object.md)
- [Instruções passo a passo: manipulando arquivos e diretórios no Visual Basic](walkthrough-manipulating-files-and-directories.md)
- [Solução de problemas: ler e gravar em arquivos de texto](troubleshooting-reading-from-and-writing-to-text-files.md)
