---
title: Escrever um programa paralelo simples usando Parallel.ForEach
description: Neste artigo, saiba como habilitar o paralelismo de dados no .NET. Escreva um loop Parallel. ForEach em qualquer fonte de dados IEnumerable ou IEnumerable <T> .
ms.date: 02/14/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- foreach, parallel version
- parallel programming, foreach
ms.assetid: cb5fab92-1c19-499e-ae91-8b7525dd875f
ms.openlocfilehash: 59c8710a8e3fc878b2ceded8595f7f3319d4c953
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84447193"
---
# <a name="how-to-write-a-simple-parallelforeach-loop"></a>Como: gravar um loop Parallel. ForEach simples

Este exemplo mostra como usar um loop <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> para permitir o paralelismo de dados em relação a qualquer fonte de dados <xref:System.Collections.IEnumerable?displayProperty=nameWithType> ou <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType>.

> [!NOTE]
> Esta documentação usa expressões lambda para definir representantes na PLINQ. Se você não estiver familiarizado com expressões lambda em C# ou Visual Basic, confira [Expressões Lambda em PLINQ e TPL](lambda-expressions-in-plinq-and-tpl.md).

## <a name="example"></a>Exemplo

Este exemplo considera que você tenha vários arquivos .jpg em uma pasta *C:\Users\Public\Pictures\Sample Pictures* e crie uma subpasta chamada *Modified*. Quando você executa o exemplo, ele gira cada imagem .jpg em *Amostras de Imagens* e salva-a em *Modified*. Você pode modificar os dois caminhos, conforme o necessário.

[!code-csharp[TPL_Parallel#03](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/simpleforeach.cs#03)]
[!code-vb[TPL_Parallel#03](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/simpleforeach.vb#03)]

Um loop <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> funciona como um loop <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType>. O loop particiona a coleção de origem e agenda o trabalho em vários threads com base no ambiente do sistema. Quanto mais processadores houver no sistema, mais rápido o método paralelo será executado. Para algumas coleções de origem, um loop sequencial pode ser mais rápido, dependendo do tamanho da origem e do tipo de trabalho que o loop executa. Para obter mais informações sobre o desempenho, consulte [possíveis armadilhas em dados e paralelismo de tarefas](potential-pitfalls-in-data-and-task-parallelism.md).

Para obter mais informações sobre loops paralelos, consulte [como: gravar um loop Parallel. for simples](how-to-write-a-simple-parallel-for-loop.md).

Para usar <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> com uma coleção não genérica, você pode usar o método de extensão <xref:System.Linq.Enumerable.Cast%2A?displayProperty=nameWithType> para converter a coleção em uma coleção genérica, conforme mostra o exemplo a seguir:

[!code-csharp[TPL_Parallel#07](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/nongeneric.cs#07)]
[!code-vb[TPL_Parallel#07](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/nongeneric.vb#07)]

Você também pode usar o LINQ Paralelo (PLINQ) para efetuar a paralelização de processamento de fontes de dados <xref:System.Collections.Generic.IEnumerable%601>. O PLINQ permite que você use a sintaxe de consulta declarativa para expressar o comportamento do loop. Para obter mais informações, consulte [PLINQ (Parallel LINQ)](introduction-to-plinq.md).

## <a name="compile-and-run-the-code"></a>Compilar e executar o código

Você pode compilar o código como um aplicativo de console do .NET Framework ou do .NET Core.

No Visual Studio, há modelos de aplicativo de console do Visual Basic e do C# para a área de trabalho do Windows e o .NET Core.

Na linha de comando, você pode usar os comandos de CLI do .NET Core (por exemplo, `dotnet new console` ou `dotnet new console -lang vb` ), ou pode criar o arquivo e usar o compilador de linha de comando para um aplicativo .NET Framework.

Para um projeto do .NET Core, você precisa referenciar o pacote **System.Drawing.Common** do NuGet. No Visual Studio, use o Gerenciador de Pacotes do NuGet para instalar o pacote. Como alternativa, você pode adicionar uma referência ao pacote no arquivo \*.csproj* ou \*.vbproj*:

```xml
<ItemGroup>
     <PackageReference Include="System.Drawing.Common" Version="4.5.1" />
</ItemGroup>
```

Para executar um aplicativo de console do .NET Core na linha de comando, use `dotnet run` da pasta que contém o aplicativo.

Para executar o aplicativo de console do Visual Studio, pressione **F5**.

## <a name="see-also"></a>Confira também

- [Paralelismo de dados](data-parallelism-task-parallel-library.md)
- [Programação paralela](index.md)
- [LINQ paralelo (PLINQ)](introduction-to-plinq.md)
