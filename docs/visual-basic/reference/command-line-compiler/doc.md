---
title: -doc
ms.date: 03/10/2018
helpviewer_keywords:
- doc compiler option [Visual Basic]
- -doc compiler option [Visual Basic]
- /doc compiler option [Visual Basic]
ms.assetid: 5fc32ec9-a149-4648-994c-a8d0cccd0a65
ms.openlocfilehash: 57a81983278c26090c62995f4da55c5cbfd66047
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408667"
---
# <a name="-doc"></a>-doc
Processa comentários de documentação para um arquivo XML.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-doc[+ | -]  
```

ou  

```console
-doc:file  
```  
  
## <a name="arguments"></a>Argumentos  
  
|Termo|Definição|  
|---|---|  
|`+` &#124; `-`|Opcional. Especificar +, ou apenas `-doc`, faz com que o compilador gere informações sobre a documentação e as coloque em um arquivo XML. Especificar `-` é equivalente a não especificar `-doc`, fazendo com que nenhuma informação de documentação seja criada.|  
|`file`|Necessário se `-doc:` for usado. Especifica o arquivo XML de saída, que é preenchido com os comentários dos arquivos de código-fonte da compilação. Se o nome do arquivo contiver um espaço, coloque o nome entre aspas (" ").|  
  
## <a name="remarks"></a>Comentários  
 A opção `-doc` controla se o compilador gera um arquivo XML contendo os comentários de documentação. Se você usar a sintaxe `-doc:file`, o parâmetro `file` especifica o nome do arquivo XML. Se você usar `-doc` ou `-doc+`, o compilador pega o nome do arquivo XML do arquivo executável ou da biblioteca que o compilador está criando. Se você usar `-doc-` ou não especificar a opção `-doc`, o compilador não criará um arquivo XML.  
  
 Nos arquivos de código-fonte, os comentários de documentação podem preceder as seguintes definições:  
  
- Tipos definidos pelo usuário, como uma [classe](../../language-reference/statements/class-statement.md) ou [interface](../../language-reference/statements/interface-statement.md)  
  
- Membros, como um campo, [evento](../../language-reference/statements/event-statement.md), [propriedade](../../language-reference/statements/property-statement.md), [função](../../language-reference/statements/function-statement.md) ou [sub-rotina](../../language-reference/statements/sub-statement.md).  
  
 Para usar o arquivo XML gerado com o recurso [IntelliSense](/visualstudio/ide/using-intellisense) do Visual Studio, deixe o nome do arquivo XML ser o mesmo que o assembly para o qual você deseja dar suporte. Certifique-se de que o arquivo XML esteja no mesmo diretório que o assembly de modo que, ao referenciar o assembly no projeto do Visual Studio, o arquivo .xml também seja encontrado. Os arquivos de documentação XML não são necessários para o IntelliSense funcionar com o código dentro do projeto ou em projetos referenciados por um projeto.  
  
 A menos que você compile com `-target:module`, o arquivo XML contém as marcações `<assembly></assembly>`. Essas marcações especificam o nome do arquivo que contém o manifesto do assembly para o arquivo de saída da compilação.  
  
 Veja [Marcas de Comentário XML](../../language-reference/xmldoc/index.md) para descobrir as maneiras de gerar documentação de comentários em seu código.  
  
|Para configurar -doc no ambiente de desenvolvimento integrado do Visual Studio|  
|---|  
|1. ter um projeto selecionado em **Gerenciador de soluções**. No menu **Projeto** , clique em **Propriedades**. <br />2. Clique na guia **Compilar** .<br />3. defina o valor na caixa **gerar arquivo de documentação XML** .|  
  
## <a name="example"></a>Exemplo  
 Veja [Como documentar o código com XML](../../programming-guide/program-structure/documenting-your-code-with-xml.md) para obter um exemplo.  
  
## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [Documentando o código com XML](../../programming-guide/program-structure/documenting-your-code-with-xml.md)
