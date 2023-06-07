---
title: Teste de unidade do F# no .NET Core com dotnet test e NUnit
description: Aprenda os conceitos de teste de unidade para F# no .NET Core por meio de uma experiência interativa de criação passo a passo de uma solução de exemplo usando dotnet test e NUnit.
author: rprouse
ms.date: 10/04/2018
dev_langs:
- fsharp
ms.openlocfilehash: 9a1b4bb7e5e705cf1c71cd44827d4a2e0a3f8cf6
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656469"
---
# <a name="unit-testing-f-libraries-in-net-core-using-dotnet-test-and-nunit"></a>Teste de unidade de bibliotecas do F# no .NET Core usando dotnet test e NUnit

Este tutorial apresenta uma experiência interativa de compilação de uma solução de exemplo passo a passo para aprender os conceitos do teste de unidade. Se você preferir acompanhar o tutorial usando uma solução interna, [veja ou baixe o exemplo de código](https://github.com/dotnet/samples/tree/master/core/getting-started/unit-testing-with-fsharp-nunit/) antes de começar. Para obter instruções de download, consulte [Exemplos e tutoriais](../../samples-and-tutorials/index.md#view-and-download-samples).

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="prerequisites"></a>Pré-requisitos

- [SDK do .NET Core 2.1](https://dotnet.microsoft.com/download) ou versões posteriores.
- Um editor de texto ou editor de código de sua escolha.

## <a name="creating-the-source-project"></a>Criando o projeto de origem

Abra uma janela do shell. Crie um diretório chamado *unit-testing-with-fsharp* para armazenar a solução.
Nesse diretório, execute o seguinte comando a fim de criar um novo arquivo de solução para a biblioteca de classes e o projeto de teste:

```dotnetcli
dotnet new sln
```

Em seguida, crie um diretório *MathService*. A seguinte estrutura de tópicos mostra a estrutura de arquivo e o diretório até aqui:

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
```

Torne *MathService* o diretório atual e execute o seguinte comando para criar o projeto de origem:

```dotnetcli
dotnet new classlib -lang "F#"
```

Crie uma implementação com falha do serviço de matemática:

```fsharp
module MyMath =
    let squaresOfOdds xs = raise (System.NotImplementedException("You haven't written a test yet!"))
```

Altere o diretório de volta para o diretório *unit-testing-with-fsharp*. Execute o seguinte comando para adicionar o projeto de biblioteca de classes à solução:

```dotnetcli
dotnet sln add .\MathService\MathService.fsproj
```

## <a name="creating-the-test-project"></a>Criando o projeto de teste

Em seguida, crie o diretório *MathService.Tests*. O seguinte esquema mostra a estrutura do diretório:

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
        Source Files
        MathService.fsproj
    /MathService.Tests
```

Torne o diretório *MathService.Tests* o diretório atual e crie um novo projeto usando o seguinte comando:

```dotnetcli
dotnet new nunit -lang "F#"
```

Isso cria um projeto de teste que usa o NUnit como a estrutura de teste. O modelo gerado configura o executor de teste no *MathServiceTests.fsproj*:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.5.0" />
  <PackageReference Include="NUnit" Version="3.9.0" />
  <PackageReference Include="NUnit3TestAdapter" Version="3.9.0" />
</ItemGroup>
```

O projeto de teste requer outros pacotes para criar e executar testes de unidade. O `dotnet new` na etapa anterior adicionou o NUnit e o adaptador de teste do NUnit. Agora, adicione a biblioteca de classes `MathService` como outra dependência ao projeto. Use o `dotnet add reference` comando:

```dotnetcli
dotnet add reference ../MathService/MathService.fsproj
```

Você pode ver o arquivo inteiro no [repositório de exemplos](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-with-fsharp/MathService.Tests/MathService.Tests.fsproj) no GitHub.

Você tem o seguinte layout de solução final:

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
        Source Files
        MathService.fsproj
    /MathService.Tests
        Test Source Files
        MathService.Tests.fsproj
```

Execute o seguinte comando no diretório *unit-testing-with-fsharp*:

```dotnetcli
dotnet sln add .\MathService.Tests\MathService.Tests.fsproj
```

## <a name="creating-the-first-test"></a>Criando o primeiro teste

Escreva um teste com falha, faça-o ser aprovado e, em seguida, repita o processo. Abra *UnitTest1.fs* e adicione o seguinte código:

```fsharp
namespace MathService.Tests

open System
open NUnit.Framework
open MathService

[<TestFixture>]
type TestClass () =

    [<Test>]
    member this.TestMethodPassing() =
        Assert.True(true)

    [<Test>]
     member this.FailEveryTime() = Assert.True(false)
```

O atributo `[<TestFixture>]` indica uma classe que contém testes. O atributo `[<Test>]` indica um método de teste que é executado pelo executor de teste. No diretório *unit-testing-with-fsharp*, execute `dotnet test` para criar os testes e a biblioteca de classes e execute os testes. O executor de teste do NUnit contém o ponto de entrada do programa para executar os testes. `dotnet test` inicia o executor de teste usando o projeto de teste de unidade que você criou.

Esses dois testes mostram testes com aprovação e falha mais básicos. `My test` é aprovado e `Fail every time` falha. Agora, crie um teste para o método `squaresOfOdds`. O método `squaresOfOdds` retorna uma sequência dos quadrados de todos os valores inteiros ímpares que fazem parte da sequência de entrada. Em vez de tentar gravar todas as funções de uma vez, você pode criar testes iterativamente que validam a funcionalidade. Fazer com que cada teste passe significa criar a funcionalidade necessária para o método.

O teste mais simples que podemos escrever é chamar `squaresOfOdds` com todos os números pares, em que o resultado deve ser uma sequência vazia de inteiros.  Este é o teste:

```fsharp
[<Test>]
member this.TestEvenSequence() =
    let expected = Seq.empty<int>
    let actual = MyMath.squaresOfOdds [2; 4; 6; 8; 10]
    Assert.That(actual, Is.EqualTo(expected))
```

Observe que a sequência `expected` foi convertida em uma lista. A estrutura do NUnit depende de muitos tipos padrão de .NET. Essa dependência significa que sua interface pública e os resultados esperados dão suporte a <xref:System.Collections.ICollection> em vez de <xref:System.Collections.IEnumerable>.

Ao executar o teste, você verá que ele falha. Você ainda não criou a implementação. Faça esse teste passar escrevendo o código mais simples na classe *Library.fs* em seu projeto MathService que funciona:

```fsharp
let squaresOfOdds xs =
    Seq.empty<int>
```

No diretório *unit-testing-with-fsharp*, execute `dotnet test` novamente. O comando `dotnet test` executa uma compilação para o projeto `MathService` e, depois, para o projeto `MathService.Tests`. Depois de compilar os dois projetos, ele executa os seus testes. Agora, dois testes são aprovados.

## <a name="completing-the-requirements"></a>Como atender aos requisitos

Agora que você fez um teste ser aprovado, é hora de escrever mais. O próximo caso simples funciona com uma sequência cujo único número ímpar é `1`. O número 1 é mais fácil, pois o quadrado de 1 é 1. Este é o próximo teste:

```fsharp
[<Test>]
member public this.TestOnesAndEvens() =
    let expected = [1; 1; 1; 1]
    let actual = MyMath.squaresOfOdds [2; 1; 4; 1; 6; 1; 8; 1; 10]
    Assert.That(actual, Is.EqualTo(expected))
```

A execução de `dotnet test` falha no novo teste. Você deve atualizar o método `squaresOfOdds` para lidar com esse novo teste. É preciso remover todos os números pares da sequência para que esse teste seja aprovado. Faça isso escrevendo uma pequena função de filtro e usando `Seq.filter`:

```fsharp
let private isOdd x = x % 2 <> 0

let squaresOfOdds xs =
    xs
    |> Seq.filter isOdd
```

Observe a chamada a `Seq.toList`. Isso cria uma lista, que implementa a interface <xref:System.Collections.ICollection>.

Há mais uma etapa a ser executada: eleve ao quadrado cada um dos números ímpares. Comece escrevendo um novo teste:

```fsharp
[<Test>]
member public this.TestSquaresOfOdds() =
    let expected = [1; 9; 25; 49; 81]
    let actual = MyMath.squaresOfOdds [1; 2; 3; 4; 5; 6; 7; 8; 9; 10]
    Assert.That(actual, Is.EqualTo(expected))
```

Você pode corrigir o teste redirecionando a sequência filtrada por meio de uma operação de mapa para calcular o quadrado de cada número ímpar:

```fsharp
let private square x = x * x
let private isOdd x = x % 2 <> 0

let squaresOfOdds xs =
    xs
    |> Seq.filter isOdd
    |> Seq.map square
```

Você criou uma pequena biblioteca e um conjunto de testes de unidade para essa biblioteca. Você estruturou a solução para que a adição de novos pacotes e testes fizesse parte do fluxo de trabalho normal. Você concentrou grande parte do seu tempo e esforço em resolver as metas do aplicativo.

## <a name="see-also"></a>Consulte também

- [dotnet add reference](../tools/dotnet-add-reference.md)
- [dotnet test](../tools/dotnet-test.md)
