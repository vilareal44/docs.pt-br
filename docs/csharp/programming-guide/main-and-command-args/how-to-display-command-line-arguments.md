---
title: Como exibir argumentos de linha de comando – guia de programação C#
description: Saiba como exibir argumentos de linha de comando. Consulte um exemplo de código e exiba recursos adicionais disponíveis.
ms.date: 07/20/2015
helpviewer_keywords:
- command-line arguments [C#], displaying
ms.assetid: b8479f2d-9e05-4d38-82da-2e61246e5437
ms.openlocfilehash: 1ac5dc5a5f4e974c9202d2ce23f61071494e1977
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381808"
---
# <a name="how-to-display-command-line-arguments-c-programming-guide"></a>Como exibir argumentos de linha de comando (guia de programação C#)
Os argumentos fornecidos a um executável na linha de comando podem ser acessados por meio de um parâmetro opcional para `Main` . Os argumentos são fornecidos na forma de uma matriz de cadeias de caracteres. Cada elemento da matriz contém um argumento. O espaço em branco entre os argumentos é removido. Por exemplo, considere essas invocações de linha de comando de um executável fictício:  
  
|Entrada na linha de comando|Matriz de cadeias de caracteres passada a Main|  
|----------------------------|-------------------------------------|  
|**executável.exe a b c**|"a"<br /><br /> "b"<br /><br /> "c"|  
|**executável.exe um dois**|"um"<br /><br /> "dois"|  
|**executável.exe "um dois" três**|"one two"<br /><br /> "three"|  
  
> [!NOTE]
> Quando estiver executando um aplicativo no Visual Studio, você pode especificar argumentos de linha de comando na [Página de depuração, Designer de Projeto](/visualstudio/ide/reference/debug-page-project-designer).  
  
## <a name="example"></a>Exemplo  
 Este exemplo exibe os argumentos de linha de comando passados para um aplicativo de linha de comando. A saída mostrada é para a primeira entrada da tabela acima.  
  
 [!code-csharp[csProgGuideMain#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class1.cs#9)]  
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Compilando pela linha de comando com csc.exe](../../language-reference/compiler-options/command-line-building-with-csc-exe.md)
- [Argumentos Main () e de linha de comando](./index.md)
- [Valores de retorno de Main()](./main-return-values.md)
