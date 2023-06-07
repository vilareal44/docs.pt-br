---
title: Compilando aplicativos de console no .NET Framework
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- .NET Framework, building console applications
- application development [.NET Framework], console
- console applications
ms.assetid: c21fb997-9f0e-40a5-8741-f73bba376bd8
ms.openlocfilehash: 3c2031e2d038f32f6392a2eb734e4f8851d7b936
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291624"
---
# <a name="building-console-applications-in-the-net-framework"></a>Compilando aplicativos de console no .NET Framework
Os aplicativos do .NET Framework podem usar a classe <xref:System.Console?displayProperty=nameWithType> para ler e gravar caracteres no console. Os dados do console são lidos a partir do fluxo de entrada padrão, os dados para o console são gravados no fluxo de saída padrão e os dados de erro do console são gravados no fluxo de saída de erro padrão. Esses fluxos são automaticamente associados ao console quando o aplicativo é iniciado e são apresentados como as propriedades <xref:System.Console.In%2A>, <xref:System.Console.Out%2A> e <xref:System.Console.Error%2A>, respectivamente.  
  
 O valor da propriedade <xref:System.Console.In%2A?displayProperty=nameWithType> é um objeto <xref:System.IO.TextReader?displayProperty=nameWithType>, ao passo que os valores das propriedades <xref:System.Console.Out%2A?displayProperty=nameWithType> e <xref:System.Console.Error%2A?displayProperty=nameWithType> são objetos <xref:System.IO.TextWriter?displayProperty=nameWithType>. Você pode associar essas propriedades com fluxos que não representam o console, tornando possível apontar para o fluxo em um local diferente de entrada ou saída. Por exemplo, você pode redirecionar a saída para um arquivo ao definir a propriedade <xref:System.Console.Out%2A?displayProperty=nameWithType> para um <xref:System.IO.StreamWriter?displayProperty=nameWithType> que encapsula um <xref:System.IO.FileStream?displayProperty=nameWithType> pelo método <xref:System.Console.SetOut%2A?displayProperty=nameWithType>. As propriedades <xref:System.Console.In%2A?displayProperty=nameWithType> e <xref:System.Console.Out%2A?displayProperty=nameWithType> não precisam fazer referência ao mesmo fluxo.  
  
> [!NOTE]
> Para mais informações sobre a compilação de aplicativos de console, incluindo exemplos em C #, Visual Basic e C++, consulte a documentação da classe <xref:System.Console>.  
  
 Se o console não existir, como em um aplicativo baseado no Windows, a gravação de saída no fluxo de saída padrão não será visível, uma vez que não haverá nenhum console para gravar as informações. A gravação de informações em um console inacessível não gera uma exceção.  
  
 Como alternativa, para permitir que o console leia e grave em um aplicativo baseado no Windows que foi desenvolvido usando o Visual Studio, abra a caixa de diálogo **Propriedades** do projeto, clique na guia **Aplicativo** e defina **Tipo de aplicativo** como **Aplicativo de Console**.  
  
 Nos aplicativos de console falta uma bomba de mensagem iniciada por padrão. Portanto, as chamadas do console para os temporizadores do Microsoft Win32 podem falhar.  
  
 A classe **System.Console** tem métodos que podem ler caracteres individuais ou linhas inteiras do console. Outros métodos convertem dados e cadeias de formato e gravam as cadeias formatadas no console. Para mais informações sobre cadeias de caracteres de formatação, consulte [Tipos de formatação](base-types/formatting-types.md).  
  
## <a name="see-also"></a>Veja também

- <xref:System.Console?displayProperty=nameWithType>
- [Formatar tipos](base-types/formatting-types.md)
