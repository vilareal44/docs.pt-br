---
title: 'Solução de problemas: lendo e gravando em arquivos de texto'
ms.date: 07/20/2015
helpviewer_keywords:
- troubleshooting file I/O
- writing text to files [Visual Basic], troubleshooting
- troubleshooting Visual Basic, text files
- I/O [Visual Basic], troubleshooting text files
- writing to files [Visual Basic], troubleshooting
- reading text files [Visual Basic], troubleshooting
ms.assetid: a8e9b44d-facb-4718-8c0f-466537171182
ms.openlocfilehash: 8af4160d09f39f2622a007aef793173d614a8b44
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406619"
---
# <a name="troubleshooting-reading-from-and-writing-to-text-files-visual-basic"></a>Solução de problemas: lendo e gravando em arquivos de texto (Visual Basic)

Este tópico aborda problemas comuns encontrados ao trabalhar com arquivos de texto e sugere uma abordagem para cada um.  
  
## <a name="common-problems"></a>Problemas comuns  

 Os problemas mais comuns encontrados ao trabalhar com arquivos de texto incluem exceções de segurança, codificações de arquivo ou caminhos inválidos.  
  
### <a name="security-exceptions"></a>Exceções de segurança  

 A <xref:System.Security.SecurityException> é lançada quando ocorre um erro de segurança. Isso geralmente é um resultado do usuário não ter as permissões necessárias, o que pode ser resolvido adicionando permissões ou trabalhando com arquivos em um armazenamento isolado.  
  
### <a name="file-encodings"></a>Codificações de arquivo  

 As codificações de arquivo, também conhecidas como codificações de caracteres, especificam como representar caracteres no processamento de texto. Caracteres inesperados em um arquivo de texto podem resultar de uma codificação incorreta. Para a maioria dos arquivos, uma codificação pode ser preferível em relação a outra em termos de quais caracteres de idioma podem ou não ser manipulados, embora o Unicode geralmente seja o ideal. Para saber mais, confira [Codificações de arquivo](file-encodings.md) e <xref:System.Text.Encoding>.  
  
### <a name="incorrect-paths"></a>Caminhos incorretos  

 Durante a análise de caminhos de arquivo, particularmente caminhos relativos, é fácil fornecer os dados errados. Muitos problemas podem ser corrigidos certificando-se de que está fornecendo o caminho correto. Para obter mais informações, consulte [Como analisar demarcadores de arquivo](how-to-parse-file-paths.md).  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.FileIO.FileSystem>
- [Ler arquivos](reading-from-files.md)
- [Gravar em arquivos](writing-to-files.md)
- [Analisando arquivos de texto com o objeto TextFieldParser](parsing-text-files-with-the-textfieldparser-object.md)
