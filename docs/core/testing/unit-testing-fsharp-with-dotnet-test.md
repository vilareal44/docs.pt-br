---
title: Teste de unidade do F# no .NET Core com dotnet test e xUnit
description: Aprenda os conceitos de teste de unidade para F# no .NET Core por meio de uma experiência interativa de criação passo a passo de uma solução de exemplo usando dotnet test e xUnit.
author: billwagner
ms.author: wiwagn
ms.date: 08/30/2017
ms.openlocfilehash: e0f2b6f88a650f412148f51cc0236fa46ed8c618
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656547"
---
# <a name="unit-testing-f-libraries-in-net-core-using-dotnet-test-and-xunit"></a>Bibliotecas do F# de teste de unidade no .NET Core usando dotnet test e xUnit

Este tutorial apresenta uma experiência interativa de compilação de uma solução de exemplo passo a passo para aprender os conceitos do teste de unidade. Se você preferir acompanhar o tutorial usando uma solução interna, [veja ou baixe o exemplo de código](https://github.com/dotnet/samples/tree/master/core/getting-started/unit-testing-with-fsharp/) antes de começar. Para obter instruções de download, consulte [Exemplos e tutoriais](../../samples-and-tutorials/index.md#view-and-download-samples).

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="creating-the-source-project"></a>Criando o projeto de origem

Abra uma janela do shell. Crie um diretório chamado *unit-testing-with-fsharp* para armazenar a solução.
Nesse novo diretório, execute `dotnet new sln` para criar uma nova solução. Isso facilita o gerenciamento da biblioteca de classes e o projeto de teste de unidade.
No diretório da solução, crie um diretório *MathService*. A estrutura de arquivo e diretório até aqui é mostrada abaixo:

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
```

Torne o *MathService* o diretório atual e execute `dotnet new classlib -lang "F#"` para criar o projeto de origem. Você criará uma implementação com falha do serviço de matemática:

```fsharp
module MyMath =
    let squaresOfOdds xs = raise (System.NotImplementedException("You haven't written a test yet!"))
```

Altere o diretório de volta para o diretório *unit-testing-with-fsharp*. Execute `dotnet sln add .\MathService\MathService.fsproj` para adicionar o projeto de biblioteca de classes à solução.

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

Torne o diretório *MathService.Tests* o diretório atual e crie um novo projeto usando `dotnet new xunit -lang "F#"`. Isso cria um projeto de teste que usa o xUnit como a biblioteca de teste. O modelo gerado configura o executor de teste no *MathServiceTests.fsproj*:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.3.0-preview-20170628-02" />
  <PackageReference Include="xunit" Version="2.2.0" />
  <PackageReference Include="xunit.runner.visualstudio" Version="2.2.0" />
</ItemGroup>
```

O projeto de teste requer outros pacotes para criar e executar testes de unidade. `dotnet new` na etapa anterior adicionou xUnit e o executor de xUnit. Agora, adicione a biblioteca de classes `MathService` como outra dependência ao projeto. Use o `dotnet add reference` comando:

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
        MathServiceTests.fsproj
```

Execute `dotnet sln add .\MathService.Tests\MathService.Tests.fsproj` no diretório *unit-testing-with-fsharp*.

## <a name="creating-the-first-test"></a>Criando o primeiro teste

Escreva um teste com falha, faça-o ser aprovado e, em seguida, repita o processo. Abra *Tests.fs* e adicione o seguinte código:

```fsharp
[<Fact>]
let ``My test`` () =
    Assert.True(true)

[<Fact>]
let ``Fail every time`` () = Assert.True(false)
```

O atributo `[<Fact>]` indica um método de teste que é executado pelo executor de teste. Em *unit-testing-with-fsharp*, execute `dotnet test` para criar os testes e a biblioteca de classes e execute os testes. O executor de teste do xUnit contém o ponto de entrada do programa para executar os testes. `dotnet test` inicia o executor de teste usando o projeto de teste de unidade que você criou.

Esses dois testes mostram testes com aprovação e falha mais básicos. `My test` é aprovado e `Fail every time` falha. Agora, crie um teste para o método `squaresOfOdds`. O método `squaresOfOdds` retorna uma sequência dos quadrados de todos os valores inteiros ímpares que fazem parte da sequência de entrada. Em vez de tentar gravar todas as funções de uma vez, você pode criar testes iterativamente que validam a funcionalidade. Fazer com que cada teste passe significa criar a funcionalidade necessária para o método.

O teste mais simples que podemos escrever é chamar `squaresOfOdds` com todos os números pares, em que o resultado deve ser uma sequência vazia de inteiros.  Este é o teste:

```fsharp
[<Fact>]
let ``Sequence of Evens returns empty collection`` () =
    let expected = Seq.empty<int>
    let actual = MyMath.squaresOfOdds [2; 4; 6; 8; 10]
    Assert.Equal<Collections.Generic.IEnumerable<int>>(expected, actual)
```

O teste falha. Você ainda não criou a implementação. Faça esse teste ser aprovado escrevendo o código mais simples na classe `MathService` que funciona:

```fsharp
let squaresOfOdds xs =
    Seq.empty<int>
```

No diretório *unit-testing-with-fsharp*, execute `dotnet test` novamente. O comando `dotnet test` executa uma compilação para o projeto `MathService` e, depois, para o projeto `MathService.Tests`. Depois de compilar os dois projetos, ele executará esse teste único. Ele é aprovado.

## <a name="completing-the-requirements"></a>Como atender aos requisitos

Agora que você fez um teste ser aprovado, é hora de escrever mais. O próximo caso simples funciona com uma sequência cujo único número ímpar é `1`. O número 1 é mais fácil, pois o quadrado de 1 é 1. Este é o próximo teste:

```fsharp
[<Fact>]
let ``Sequences of Ones and Evens returns Ones`` () =
    let expected = [1; 1; 1; 1]
    let actual = MyMath.squaresOfOdds [2; 1; 4; 1; 6; 1; 8; 1; 10]
    Assert.Equal<Collections.Generic.IEnumerable<int>>(expected, actual)
```

A execução de `dotnet test` executa os testes e mostra que o novo teste falha. Agora, atualize o método `squaresOfOdds` para lidar com esse novo teste. Remova todos os números pares da sequência para que esse teste seja aprovado. Faça isso escrevendo uma pequena função de filtro e usando `Seq.filter`:

```fsharp
let private isOdd x = x % 2 <> 0

let squaresOfOdds xs =
    xs
    |> Seq.filter isOdd
```

Há mais uma etapa a ser executada: eleve ao quadrado cada um dos números ímpares. Comece escrevendo um novo teste:

```fsharp
[<Fact>]
let ``SquaresOfOdds works`` () =
    let expected = [1; 9; 25; 49; 81]
    let actual = MyMath.squaresOfOdds [1; 2; 3; 4; 5; 6; 7; 8; 9; 10]
    Assert.Equal(expected, actual)
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

- [dotnet new](../tools/dotnet-new.md)
- [dotnet sln](../tools/dotnet-new.md)
- [dotnet add reference](../tools/dotnet-add-reference.md)
- [dotnet test](../tools/dotnet-test.md)
