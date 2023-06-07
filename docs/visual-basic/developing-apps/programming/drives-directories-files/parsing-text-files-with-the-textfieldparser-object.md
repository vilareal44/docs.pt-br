---
title: Analisando arquivos de texto com o objeto TextFieldParser
ms.date: 07/20/2015
helpviewer_keywords:
- TextFieldParser object, using
- I/O [Visual Basic], parsing files
- files [Visual Basic], parsing
ms.assetid: fc31d6e6-af0c-403f-8a00-d556b2c57567
ms.openlocfilehash: 7b2cb0dd39d14dd94db661cc9cbacf99edf0e778
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406671"
---
# <a name="parsing-text-files-with-the-textfieldparser-object-visual-basic"></a>Analisando arquivos de texto com o objeto TextFieldParser (Visual Basic)

O objeto `TextFieldParser` permite analisar e processar arquivos muito grandes estruturados como colunas de texto de largura delimitada, como arquivos de log ou informações de banco de dados herdadas. Analisar um arquivo de texto com `TextFieldParser` é semelhante a iterar em um arquivo de texto, enquanto o método de análise para extrair campos de texto é semelhante a métodos de manipulação de cadeias de caracteres usados para criar tokens para cadeias de caracteres delimitadas.  
  
## <a name="parsing-different-types-of-text-files"></a>Analisando tipos diferentes de arquivos de texto  

 Arquivos de texto podem ter campos com larguras variadas, delimitados por um caractere como uma vírgula ou um espaço de tabulação. Defina `TextFieldType` e o delimitador, como no exemplo a seguir, que usa o método `SetDelimiters` para definir um arquivo de texto delimitado por tabulação:  
  
 [!code-vb[VbVbalrTextFieldParser#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTextFieldParser/VB/Class1.vb#21)]  
  
 Outros arquivos de texto podem ter larguras de campo fixas. Nesses casos, você precisa definir o `TextFieldType` como `FixedWidth` e definir as larguras de cada campo, como no exemplo a seguir. Este exemplo usa o método `SetFieldWidths` para definir as colunas de texto: a primeira coluna tem 5 caracteres de largura, a segunda tem 10, a terceira tem 11 e a quarta tem largura variável.  
  
 [!code-vb[VbVbalrTextFieldParser#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTextFieldParser/VB/Class1.vb#22)]  
  
 Após o formato ser definido, você pode executar um loop no arquivo, usando o método `ReadFields` para processar uma linha por vez.  
  
 Se um campo não coincidir com o formato especificado, uma exceção <xref:Microsoft.VisualBasic.FileIO.MalformedLineException> será lançada. Quando essas exceções são geradas, as propriedades `ErrorLine` e `ErrorLineNumber` retêm o texto causador da exceção e o número de linha do texto.  
  
## <a name="parsing-files-with-multiple-formats"></a>Analisando arquivos com vários formatos  

 O método `PeekChars` do objeto `TextFieldParser` pode ser usado para verificar cada campo antes da leitura, permitindo definir vários formatos para os campos e reagir adequadamente. Para obter mais informações, consulte [Como ler de arquivos de texto com vários formatos](how-to-read-from-text-files-with-multiple-formats.md).  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.OpenTextFieldParser%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.PeekChars%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.ReadFields%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.CommentTokens%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.Delimiters%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.ErrorLine%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.ErrorLineNumber%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.FieldWidths%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.HasFieldsEnclosedInQuotes%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.LineNumber%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.TextFieldType%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.TrimWhiteSpace%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.SetDelimiters%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.SetFieldWidths%2A>
