---
title: Teste de unidade do Visual Basic no .NET Core com dotnet test e NUnit
description: Aprenda os conceitos de teste de unidade no .NET Core por meio de uma experiência interativa de criação passo a passo de uma solução de exemplo do Visual Basic usando NUnit.
author: rprouse
ms.date: 10/04/2018
ms.openlocfilehash: 4b807463d29f271d1a707b6254b7b5e66f745319
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656404"
---
# <a name="unit-testing-visual-basic-net-core-libraries-using-dotnet-test-and-nunit"></a>Teste de unidade de bibliotecas do .NET Core do Visual Basic usando dotnet test e NUnit

Este tutorial apresenta uma experiência interativa de compilação de uma solução de exemplo passo a passo para aprender os conceitos do teste de unidade. Se você preferir acompanhar o tutorial usando uma solução interna, [veja ou baixe o exemplo de código](https://github.com/dotnet/samples/tree/master/core/getting-started/unit-testing-vb-nunit/) antes de começar. Para obter instruções de download, consulte [Exemplos e tutoriais](../../samples-and-tutorials/index.md#view-and-download-samples).

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="prerequisites"></a>Pré-requisitos

- [SDK do .NET Core 2.1](https://dotnet.microsoft.com/download) ou versões posteriores.
- Um editor de texto ou editor de código de sua escolha.

## <a name="creating-the-source-project"></a>Criando o projeto de origem

Abra uma janela do shell. Crie um diretório chamado *unit-testing-vb-nunit* para armazenar a solução. Nesse diretório, execute o seguinte comando a fim de criar um novo arquivo de solução para a biblioteca de classes e o projeto de teste:

```dotnetcli
dotnet new sln
```

Em seguida, crie um diretório *PrimeService*. O seguinte contorno mostra a estrutura de arquivos até agora:

```console
/unit-testing-vb-nunit
    unit-testing-vb-nunit.sln
    /PrimeService
```

Torne *PrimeService* o diretório atual e execute o seguinte comando para criar o projeto de origem:

```dotnetcli
dotnet new classlib -lang VB
```

Renomeie *Class1.VB* para *PrimeService.VB*. Crie uma implementação com falha da classe `PrimeService`:

```vb
Namespace Prime.Services
    Public Class PrimeService
        Public Function IsPrime(candidate As Integer) As Boolean
            Throw New NotImplementedException("Please create a test first.")
        End Function
    End Class
End Namespace
```

Altere o diretório de volta para o diretório *unit-testing-vb-using-mstest*. Execute o seguinte comando para adicionar o projeto de biblioteca de classes à solução:

```dotnetcli
dotnet sln add .\PrimeService\PrimeService.vbproj
```

## <a name="creating-the-test-project"></a>Criando o projeto de teste

Em seguida, crie o diretório *PrimeService.Tests*. O seguinte esquema mostra a estrutura do diretório:

```console
/unit-testing-vb-nunit
    unit-testing-vb-nunit.sln
    /PrimeService
        Source Files
        PrimeService.vbproj
    /PrimeService.Tests
```

Torne o diretório *PrimeService.Tests* o diretório atual e crie um novo projeto usando o seguinte comando:

```dotnetcli
dotnet new nunit -lang VB
```

O comando [dotnet new](../tools/dotnet-new.md) cria um projeto de teste que usa o NUnit como a biblioteca de teste. O modelo gerado configura o executor de teste no arquivo *PrimeServiceTests.vbproj*:

[!code-xml[Packages](~/samples/snippets/core/testing/unit-testing-vb-nunit/vb/PrimeService.Tests/PrimeService.Tests.vbproj#Packages)]

O projeto de teste requer outros pacotes para criar e executar testes de unidade. O `dotnet new` na etapa anterior adicionou o NUnit e o adaptador de teste do NUnit. Agora, adicione a biblioteca de classes `PrimeService` como outra dependência ao projeto. Use o [`dotnet add reference`](../tools/dotnet-add-reference.md) comando:

```dotnetcli
dotnet add reference ../PrimeService/PrimeService.vbproj
```

Você pode ver o arquivo inteiro no [repositório de exemplos](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-vb-nunit/PrimeService.Tests/PrimeService.Tests.vbproj) no GitHub.

Você tem o seguinte layout de solução final:

```console
/unit-testing-vb-nunit
    unit-testing-vb-nunit.sln
    /PrimeService
        Source Files
        PrimeService.vbproj
    /PrimeService.Tests
        Test Source Files
        PrimeService.Tests.vbproj
```

Execute o seguinte comando no diretório *unit-testing-vb-nunit*:

```dotnetcli
dotnet sln add .\PrimeService.Tests\PrimeService.Tests.vbproj
```

## <a name="creating-the-first-test"></a>Criando o primeiro teste

Escreva um teste com falha, faça-o ser aprovado e, em seguida, repita o processo. No diretório *PrimeService.Tests*, renomeie o arquivo *UnitTest1.vb* para *PrimeService_IsPrimeShould.VB* e substitua todo o seu conteúdo pelo seguinte código:

```vb
Imports NUnit.Framework

Namespace PrimeService.Tests
    <TestFixture>
    Public Class PrimeService_IsPrimeShould
        Private _primeService As Prime.Services.PrimeService = New Prime.Services.PrimeService()

        <Test>
        Sub IsPrime_InputIs1_ReturnFalse()
            Dim result As Boolean = _primeService.IsPrime(1)

            Assert.False(result, "1 should not be prime")
        End Sub

    End Class
End Namespace
```

O atributo `<TestFixture>` indica uma classe que contém testes. O atributo `<Test>` indica um método que é executado pelo executor de teste. Em *unit-testing-vb-nunit*, execute [`dotnet test`](../tools/dotnet-test.md) para criar os testes e a biblioteca de classes e, em seguida, execute os testes. O executor de teste do NUnit contém o ponto de entrada do programa para executar os testes. `dotnet test` inicia o executor de teste usando o projeto de teste de unidade que você criou.

O teste falha. Você ainda não criou a implementação. Faça esse teste ser aprovado escrevendo o código mais simples na classe `PrimeService` que funciona:

```vb
Public Function IsPrime(candidate As Integer) As Boolean
    If candidate = 1 Then
        Return False
    End If
    Throw New NotImplementedException("Please create a test first.")
End Function
```

No diretório *unit-testing-vb-nunit*, execute `dotnet test` novamente. O comando `dotnet test` executa uma compilação para o projeto `PrimeService` e, depois, para o projeto `PrimeService.Tests`. Depois de compilar os dois projetos, ele executará esse teste único. Ele é aprovado.

## <a name="adding-more-features"></a>Adicionando mais recursos

Agora que você fez um teste ser aprovado, é hora de escrever mais. Existem alguns outros casos simples de números primos: 0, -1. Você pode adicionar esses casos como novos testes com o atributo `<Test>`, mas isso se torna entediante rapidamente. Há outros atributos de xUnit que permitem escrever um pacote de testes semelhantes.  Um atributo `<TestCase>` representa um pacote de testes que executa o mesmo código, mas têm diferentes argumentos de entrada. Você pode usar o atributo `<TestCase>` para especificar valores para essas entradas.

Em vez de criar novos testes, aplique esses dois atributos para criar uma série de testes que testa diversos valores menores que dois, que é o menor número primo:

[!code-vb[Sample_TestCode](../../../samples/snippets/core/testing/unit-testing-vb-nunit/vb/PrimeService.Tests/PrimeService_IsPrimeShould.vb?name=Sample_TestCode)]

Execute `dotnet test`, e dois desses testes falham. Para fazer com que todos os testes sejam aprovados, altere a cláusula `if` no início do método `Main` no arquivo *PrimeServices.cs*:

```vb
if candidate < 2
```

Continue iterando adicionando mais testes, mais teorias e mais código na biblioteca principal. Você tem a [versão concluída dos testes](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-vb-nunit/PrimeService.Tests/PrimeService_IsPrimeShould.vb) e a [implementação completa da biblioteca](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-vb-nunit/PrimeService/PrimeService.vb).

Você criou uma pequena biblioteca e um conjunto de testes de unidade para essa biblioteca. Você estruturou a solução para que a adição de novos pacotes e testes fizesse parte do fluxo de trabalho normal. Você concentrou grande parte do seu tempo e esforço em resolver as metas do aplicativo.
