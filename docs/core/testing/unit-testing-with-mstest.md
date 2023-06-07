---
title: C# de teste de unidade com MSTest e .NET Core
description: Aprenda os conceitos de teste de unidade no C# e .NET Core por meio de uma experiência interativa de criação passo a passo de uma solução de exemplo usando dotnet test e MSTest.
author: ncarandini
ms.author: wiwagn
ms.date: 09/08/2017
ms.openlocfilehash: 765b57dce323c10dc5fcbf395cb7d52be76046c2
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656352"
---
# <a name="unit-testing-c-with-mstest-and-net-core"></a>C# de teste de unidade com MSTest e .NET Core

Este tutorial apresenta uma experiência interativa de compilação de uma solução de exemplo passo a passo para aprender os conceitos do teste de unidade. Se você preferir acompanhar o tutorial usando uma solução interna, [veja ou baixe o exemplo de código](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-mstest/) antes de começar. Para obter instruções de download, consulte [Exemplos e tutoriais](../../samples-and-tutorials/index.md#view-and-download-samples).

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="create-the-source-project"></a>Crie o projeto de origem

Abra uma janela do shell. Crie um diretório chamado *unit-testing-using-mstest* no qual manter a solução. Dentro desse novo diretório, execute [`dotnet new sln`](../tools/dotnet-new.md) para criar um novo arquivo de solução para a biblioteca de classes e o projeto de teste. Em seguida, crie um diretório *PrimeService*. O seguinte esquema mostra a estrutura de arquivo e diretório até aqui:

```console
/unit-testing-using-mstest
    unit-testing-using-mstest.sln
    /PrimeService
```

Torne o *PrimeService* o diretório atual e execute [`dotnet new classlib`](../tools/dotnet-new.md) para criar o projeto de origem. Renomeie *Class1.cs* como *PrimeService.cs*. Crie uma implementação com falha da classe `PrimeService`:

```csharp
using System;

namespace Prime.Services
{
    public class PrimeService
    {
        public bool IsPrime(int candidate)
        {
            throw new NotImplementedException("Please create a test first.");
        }
    }
}
```

Altere o diretório de volta para o diretório *unit-testing-using-mstest*. Execute [`dotnet sln add PrimeService/PrimeService.csproj`](../tools/dotnet-sln.md) para adicionar o projeto de biblioteca de classes à solução.

## <a name="create-the-test-project"></a>Criar um projeto de teste

Em seguida, crie o diretório *PrimeService.Tests*. O seguinte esquema mostra a estrutura do diretório:

```console
/unit-testing-using-mstest
    unit-testing-using-mstest.sln
    /PrimeService
        Source Files
        PrimeService.csproj
    /PrimeService.Tests
```

Transforme o diretório *PrimeService. Tests* no diretório atual e crie um novo projeto usando [`dotnet new mstest`](../tools/dotnet-new.md) . O comando dotnet new cria um projeto de teste que usa o MSTest como a biblioteca de teste. O modelo gerado configura o executor de teste no arquivo *PrimeServiceTests. csproj* :

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.3.0" />
  <PackageReference Include="MSTest.TestAdapter" Version="1.1.18" />
  <PackageReference Include="MSTest.TestFramework" Version="1.1.18" />
</ItemGroup>
```

O projeto de teste requer outros pacotes para criar e executar testes de unidade. `dotnet new` da etapa anterior adicionou o SDK do MSTest, a estrutura de teste do MSTest e o executor do MSTest. Agora, adicione a biblioteca de classes `PrimeService` como outra dependência ao projeto. Use o [`dotnet add reference`](../tools/dotnet-add-reference.md) comando:

```dotnetcli
dotnet add reference ../PrimeService/PrimeService.csproj
```

Você pode ver o arquivo inteiro no [repositório de exemplos](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-mstest/PrimeService.Tests/PrimeService.Tests.csproj) no GitHub.

O seguinte esquema mostra o layout da solução final:

```console
/unit-testing-using-mstest
    unit-testing-using-mstest.sln
    /PrimeService
        Source Files
        PrimeService.csproj
    /PrimeService.Tests
        Test Source Files
        PrimeServiceTests.csproj
```

Execute [`dotnet sln add .\PrimeService.Tests\PrimeService.Tests.csproj`](../tools/dotnet-sln.md) no diretório *Unit-Testing-using-MSTest* .

## <a name="create-the-first-test"></a>Crie o primeiro teste

Escreva um teste com falha, faça-o ser aprovado e, em seguida, repita o processo. Remova *UnitTest1.cs* do diretório *PrimeService. Tests* e crie um novo arquivo C# chamado *PrimeService_IsPrimeShould. cs* com o seguinte conteúdo:

```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Prime.Services;

namespace Prime.UnitTests.Services
{
    [TestClass]
    public class PrimeService_IsPrimeShould
    {
        private readonly PrimeService _primeService;

        public PrimeService_IsPrimeShould()
        {
            _primeService = new PrimeService();
        }

        [TestMethod]
        public void IsPrime_InputIs1_ReturnFalse()
        {
            var result = _primeService.IsPrime(1);

            Assert.IsFalse(result, "1 should not be prime");
        }
    }
}
```

O [atributo TestClass](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute) indica uma classe que contém testes de unidade. O [atributo TestMethod](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute) indica um método que é um método de teste.

Salve esse arquivo e execute [`dotnet test`](../tools/dotnet-test.md) para compilar os testes e a biblioteca de classes e, em seguida, execute os testes. O executor de teste do MSTest contém o ponto de entrada do programa para executar os testes. `dotnet test` inicia o executor de teste usando o projeto de teste de unidade que você criou.

O teste falha. Você ainda não criou a implementação. Faça esse teste ser aprovado escrevendo o código mais simples na classe `PrimeService` que funciona:

```csharp
public bool IsPrime(int candidate)
{
    if (candidate == 1)
    {
        return false;
    }
    throw new NotImplementedException("Please create a test first.");
}
```

No diretório *unit-testing-using-mstest*, execute `dotnet test` novamente. O comando `dotnet test` executa uma compilação para o projeto `PrimeService` e, depois, para o projeto `PrimeService.Tests`. Depois de compilar os dois projetos, ele executará esse teste único. Ele é aprovado.

## <a name="add-more-features"></a>Adicionar mais recursos

Agora que você fez um teste ser aprovado, é hora de escrever mais. Existem alguns outros casos simples de números primos: 0, -1. Você pode adicionar novos testes com o [atributo TestMethod](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute), mas isso se torna rapidamente entediante. Há outros atributos do MSTest que permitem escrever um pacote de testes semelhantes.  Um [atributo DataTestMethod](xref:Microsoft.VisualStudio.TestTools.UnitTesting.DataTestMethodAttribute) representa um pacote de testes que executa o mesmo código, mas com diferentes argumentos de entrada. Você pode usar o [atributo DataRow](xref:Microsoft.VisualStudio.TestTools.UnitTesting.DataRowAttribute) para especificar valores para essas entradas.

Em vez de criar novos testes, aplique esses dois atributos para criar um único teste orientado a dados. O teste controlado por dados é um método que testa vários valores menores que dois, que é o menor número primo:

[!code-csharp[Sample_TestCode](../../../samples/snippets/core/testing/unit-testing-using-mstest/csharp/PrimeService.Tests/PrimeService_IsPrimeShould.cs?name=Sample_TestCode)]

Execute `dotnet test`, e dois desses testes falham. Para fazer todos os testes serem aprovados, altere a cláusula `if` no início do método:

```csharp
if (candidate < 2)
```

Continue iterando adicionando mais testes, mais teorias e mais código na biblioteca principal. Você tem a [versão concluída dos testes](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-mstest/PrimeService.Tests/PrimeService_IsPrimeShould.cs) e a [implementação completa da biblioteca](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-mstest/PrimeService/PrimeService.cs).

Você criou uma pequena biblioteca e um conjunto de testes de unidade para essa biblioteca. Você estruturou a solução para que a adição de novos pacotes e testes fizesse parte do fluxo de trabalho normal. Você concentrou grande parte do seu tempo e esforço em resolver as metas do aplicativo.

## <a name="see-also"></a>Consulte também

- <xref:Microsoft.VisualStudio.TestTools.UnitTesting>
- [Usar a estrutura do MSTest em testes de unidade](/visualstudio/test/using-microsoft-visualstudio-testtools-unittesting-members-in-unit-tests)
- [Documentos da estrutura de teste do MSTest V2](https://github.com/Microsoft/testfx-docs)
