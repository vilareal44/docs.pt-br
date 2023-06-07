---
title: Teste de unidade do Visual Basic no .NET Core com dotnet test e MSTest
description: Aprenda os conceitos de teste de unidade no .NET Core por meio de uma experiência interativa de criação passo a passo de uma solução de exemplo do Visual Basic usando MSTest.
author: billwagner
ms.author: wiwagn
ms.date: 09/01/2017
ms.openlocfilehash: 9654d05754b83033bcaef6d8b8f24e3ba1d8bb2f
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656443"
---
# <a name="unit-testing-visual-basic-net-core-libraries-using-dotnet-test-and-mstest"></a>Bibliotecas do .NET Core no Visual Basic de teste de unidade usando dotnet test e MSTest

Este tutorial apresenta uma experiência interativa de compilação de uma solução de exemplo passo a passo para aprender os conceitos do teste de unidade. Se você preferir acompanhar o tutorial usando uma solução interna, [veja ou baixe o exemplo de código](https://github.com/dotnet/samples/tree/master/core/getting-started/unit-testing-vb-mstest/) antes de começar. Para obter instruções de download, consulte [Exemplos e tutoriais](../../samples-and-tutorials/index.md#view-and-download-samples).

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="creating-the-source-project"></a>Criando o projeto de origem

Abra uma janela do shell. Crie um diretório chamado *unit-testing-vb-mstest* para armazenar a solução.
Dentro desse novo diretório, execute [`dotnet new sln`](../tools/dotnet-new.md) para criar uma nova solução. Essa prática facilita gerenciar a biblioteca de classes e o projeto de teste de unidade.
No diretório da solução, crie um diretório *PrimeService*. Você tem a seguinte estrutura de arquivo e diretório até aqui:

```console
/unit-testing-vb-mstest
    unit-testing-vb-mstest.sln
    /PrimeService
```

Torne o *PrimeService* o diretório atual e execute [`dotnet new classlib -lang VB`](../tools/dotnet-new.md) para criar o projeto de origem. Renomeie *Class1.VB* para *PrimeService.VB*. Crie uma implementação com falha da classe `PrimeService`:

```vb
Namespace Prime.Services
    Public Class PrimeService
        Public Function IsPrime(candidate As Integer) As Boolean
            Throw New NotImplementedException("Please create a test first")
        End Function
    End Class
End Namespace
```

Altere o diretório de volta para o diretório *unit-testing-vb-using-mstest*. Execute [`dotnet sln add .\PrimeService\PrimeService.vbproj`](../tools/dotnet-sln.md) para adicionar o projeto de biblioteca de classes à solução.

## <a name="creating-the-test-project"></a>Criando o projeto de teste

Em seguida, crie o diretório *PrimeService.Tests*. O seguinte esquema mostra a estrutura do diretório:

```console
/unit-testing-vb-mstest
    unit-testing-vb-mstest.sln
    /PrimeService
        Source Files
        PrimeService.vbproj
    /PrimeService.Tests
```

Transforme o diretório *PrimeService. Tests* no diretório atual e crie um novo projeto usando [`dotnet new mstest -lang VB`](../tools/dotnet-new.md) . Esse comando cria um projeto de teste que usa o MSTest como a biblioteca de teste. O modelo gerado configura o executor de teste no *PrimeServiceTests.vbproj*:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.5.0" />
  <PackageReference Include="MSTest.TestAdapter" Version="1.1.18" />
  <PackageReference Include="MSTest.TestFramework" Version="1.1.18" />
</ItemGroup>
```

O projeto de teste requer outros pacotes para criar e executar testes de unidade. `dotnet new`, na etapa anterior, adicionou MSTest e o executor de MSTest. Agora, adicione a biblioteca de classes `PrimeService` como outra dependência ao projeto. Use o [`dotnet add reference`](../tools/dotnet-add-reference.md) comando:

```dotnetcli
dotnet add reference ../PrimeService/PrimeService.vbproj
```

Você pode ver o arquivo inteiro no [repositório de exemplos](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-vb-mstest/PrimeService.Tests/PrimeService.Tests.vbproj) no GitHub.

Você tem o seguinte layout de solução final:

```console
/unit-testing-vb-mstest
    unit-testing-vb-mstest.sln
    /PrimeService
        Source Files
        PrimeService.vbproj
    /PrimeService.Tests
        Test Source Files
        PrimeServiceTests.vbproj
```

Execute [`dotnet sln add .\PrimeService.Tests\PrimeService.Tests.vbproj`](../tools/dotnet-sln.md) no diretório *Unit-Testing-VB-MSTest* .

## <a name="creating-the-first-test"></a>Criando o primeiro teste

Escreva um teste com falha, faça-o ser aprovado e, em seguida, repita o processo. Remova *UnitTest1.vb* do diretório *PrimeService.Tests* e crie um novo arquivo do Visual Basic chamado *PrimeService_IsPrimeShould.VB*. Adicione os códigos a seguir:

```vb
Imports Microsoft.VisualStudio.TestTools.UnitTesting

Namespace PrimeService.Tests
    <TestClass>
    Public Class PrimeService_IsPrimeShould
        Private _primeService As Prime.Services.PrimeService = New Prime.Services.PrimeService()

        <TestMethod>
        Sub IsPrime_InputIs1_ReturnFalse()
            Dim result As Boolean = _primeService.IsPrime(1)

            Assert.IsFalse(result, "1 should not be prime")
        End Sub

    End Class
End Namespace
```

O atributo `<TestClass>` indica uma classe que contém testes. O atributo `<TestMethod>` indica um método que é executado pelo executor de teste. Em *unit-testing-vb-mstest*, execute [`dotnet test`](../tools/dotnet-test.md) para criar os testes e a biblioteca de classes e, em seguida, execute os testes. O executor de teste do MSTest contém o ponto de entrada do programa para executar os testes. `dotnet test` inicia o executor de teste usando o projeto de teste de unidade que você criou.

O teste falha. Você ainda não criou a implementação. Faça esse teste ser aprovado escrevendo o código mais simples na classe `PrimeService` que funciona:

```vb
Public Function IsPrime(candidate As Integer) As Boolean
    If candidate = 1 Then
        Return False
    End If
    Throw New NotImplementedException("Please create a test first.")
End Function
```

No diretório *unit-testing-vb-mstest*, execute `dotnet test` novamente. O comando `dotnet test` executa uma compilação para o projeto `PrimeService` e, depois, para o projeto `PrimeService.Tests`. Depois de compilar os dois projetos, ele executará esse teste único. Ele é aprovado.

## <a name="adding-more-features"></a>Adicionando mais recursos

Agora que você fez um teste ser aprovado, é hora de escrever mais. Existem alguns outros casos simples de números primos: 0, -1. Você pode adicionar esses casos como novos testes com o atributo `<TestMethod>`, mas isso se torna entediante rapidamente. Há outros atributos do MSTest que permitem escrever um pacote de testes semelhantes.  Um atributo `<DataTestMethod>` representa um pacote de testes que executa o mesmo código, mas têm diferentes argumentos de entrada. Você pode usar o atributo `<DataRow>` para especificar valores para essas entradas.

Em vez de criar novos testes, aplique esses dois atributos para criar uma única teoria. A teoria é um método que testa vários valores inferiores a dois, que é o número primo mais baixo:

[!code-vb[Sample_TestCode](../../../samples/snippets/core/testing/unit-testing-vb-mstest/vb/PrimeService.Tests/PrimeService_IsPrimeShould.vb?name=Sample_TestCode)]

Execute `dotnet test`, e dois desses testes falham. Para fazer todos os testes serem aprovados, altere a cláusula `if` no início do método:

```vb
if candidate < 2
```

Continue iterando adicionando mais testes, mais teorias e mais código na biblioteca principal. Você tem a [versão concluída dos testes](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-vb-mstest/PrimeService.Tests/PrimeService_IsPrimeShould.vb) e a [implementação completa da biblioteca](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-vb-mstest/PrimeService/PrimeService.vb).

Você criou uma pequena biblioteca e um conjunto de testes de unidade para essa biblioteca. Você estruturou a solução para que a adição de novos pacotes e testes fizesse parte do fluxo de trabalho normal. Você concentrou grande parte do seu tempo e esforço em resolver as metas do aplicativo.
