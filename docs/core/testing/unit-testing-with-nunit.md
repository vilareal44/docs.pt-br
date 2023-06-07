---
title: Teste de unidade em C# com NUnit e .NET Core
description: Aprenda conceitos de teste de unidade no C# e .NET Core por meio de uma experiência interativa, criando passo a passo uma solução de exemplo, usando dotnet test e NUnit.
author: rprouse
ms.date: 08/31/2018
ms.openlocfilehash: 90fd917fd980db6689195026a7524e0cacfc92bc
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656365"
---
# <a name="unit-testing-c-with-nunit-and-net-core"></a>Teste de unidade em C# com NUnit e .NET Core

Este tutorial apresenta uma experiência interativa de compilação de uma solução de exemplo passo a passo para aprender os conceitos do teste de unidade. Se você preferir acompanhar o tutorial usando uma solução interna, [veja ou baixe o exemplo de código](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-nunit/) antes de começar. Para obter instruções de download, consulte [Exemplos e tutoriais](../../samples-and-tutorials/index.md#view-and-download-samples).

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="prerequisites"></a>Pré-requisitos

- [SDK do .NET Core 2.1](https://dotnet.microsoft.com/download) ou versões posteriores.
- Um editor de texto ou editor de código de sua escolha.

## <a name="creating-the-source-project"></a>Criando o projeto de origem

Abra uma janela do shell. Crie um diretório chamado *unit-testing-using-nunit* para armazenar a solução. Nesse diretório, execute o seguinte comando a fim de criar um novo arquivo de solução para a biblioteca de classes e o projeto de teste:

```dotnetcli
dotnet new sln
```

Em seguida, crie um diretório *PrimeService*. A seguinte estrutura de tópicos mostra a estrutura de arquivo e o diretório até aqui:

```console
/unit-testing-using-nunit
    unit-testing-using-nunit.sln
    /PrimeService
```

Torne *PrimeService* o diretório atual e execute o seguinte comando para criar o projeto de origem:

```dotnetcli
dotnet new classlib
```

Renomeie *Class1.cs* como *PrimeService.cs*. Crie uma implementação com falha da classe `PrimeService`:

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

Altere o diretório de volta para o diretório *unit-testing-using-nunit*. Execute o seguinte comando para adicionar o projeto de biblioteca de classes à solução:

```dotnetcli
dotnet sln add PrimeService/PrimeService.csproj
```

## <a name="creating-the-test-project"></a>Criando o projeto de teste

Em seguida, crie o diretório *PrimeService.Tests*. O seguinte esquema mostra a estrutura do diretório:

```console
/unit-testing-using-nunit
    unit-testing-using-nunit.sln
    /PrimeService
        Source Files
        PrimeService.csproj
    /PrimeService.Tests
```

Torne o diretório *PrimeService.Tests* o diretório atual e crie um novo projeto usando o seguinte comando:

```dotnetcli
dotnet new nunit
```

O comando [dotnet new](../tools/dotnet-new.md) cria um projeto de teste que usa o NUnit como a biblioteca de teste. O modelo gerado configura o executor de teste no arquivo *PrimeService.Tests.csproj*:

[!code-xml[Packages](~/samples/snippets/core/testing/unit-testing-using-nunit/csharp/PrimeService.Tests/PrimeService.Tests.csproj#Packages)]

O projeto de teste requer outros pacotes para criar e executar testes de unidade. O `dotnet new`, na etapa anterior, adicionou o SDK de teste da Microsoft, a estrutura de teste do NUnit e o adaptador de teste do NUnit. Agora, adicione a biblioteca de classes `PrimeService` como outra dependência ao projeto. Use o [`dotnet add reference`](../tools/dotnet-add-reference.md) comando:

```dotnetcli
dotnet add reference ../PrimeService/PrimeService.csproj
```

Você pode ver o arquivo inteiro no [repositório de exemplos](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-nunit/PrimeService.Tests/PrimeService.Tests.csproj) no GitHub.

O seguinte esquema mostra o layout da solução final:

```console
/unit-testing-using-nunit
    unit-testing-using-nunit.sln
    /PrimeService
        Source Files
        PrimeService.csproj
    /PrimeService.Tests
        Test Source Files
        PrimeService.Tests.csproj
```

Execute o seguinte comando no diretório *unit-testing-using-nunit*:

```dotnetcli
dotnet sln add ./PrimeService.Tests/PrimeService.Tests.csproj
```

## <a name="creating-the-first-test"></a>Criando o primeiro teste

Escreva um teste com falha, faça-o ser aprovado e, em seguida, repita o processo. No diretório *PrimeService.Tests*, renomeie o arquivo *UnitTest1.cs* para *PrimeService_IsPrimeShould.cs* e substitua todo o seu conteúdo pelo código a seguir:

[!code-csharp[Sample_FirstTest](~/samples/snippets/core/testing/unit-testing-using-nunit/csharp/PrimeService.Tests/PrimeService_IsPrimeShould.cs?name=Sample_FirstTest)]

O atributo `[TestFixture]` indica uma classe que contém testes de unidade. O atributo `[Test]` indica um método que é um método de teste.

Salve esse arquivo e execute [`dotnet test`](../tools/dotnet-test.md) para compilar os testes e a biblioteca de classes e, em seguida, execute os testes. O executor de teste do NUnit contém o ponto de entrada do programa para executar os testes. `dotnet test` inicia o executor de teste usando o projeto de teste de unidade que você criou.

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

No diretório *unit-testing-using-nunit*, execute `dotnet test` novamente. O comando `dotnet test` executa uma compilação para o projeto `PrimeService` e, depois, para o projeto `PrimeService.Tests`. Depois de compilar os dois projetos, ele executará esse teste único. Ele é aprovado.

## <a name="adding-more-features"></a>Adicionando mais recursos

Agora que você fez um teste ser aprovado, é hora de escrever mais. Existem alguns outros casos simples de números primos: 0, -1. Você pode adicionar novos testes com o atributo `[Test]`, mas isso se torna entediante rapidamente. Há outros atributos de NUnit que permitem que você grave um pacote com testes semelhantes.  Um atributo `[TestCase]` é usado para criar um pacote com testes que executa o mesmo código, mas têm diferentes argumentos de entrada. Você pode usar o atributo `[TestCase]` para especificar valores para essas entradas.

Em vez de criar novos testes, aplique esse atributo para criar um único teste controlado por dados. O teste controlado por dados é um método que testa vários valores menores que dois, que é o menor número primo:

[!code-csharp[Sample_TestCode](~/samples/snippets/core/testing/unit-testing-using-nunit/csharp/PrimeService.Tests/PrimeService_IsPrimeShould.cs?name=Sample_TestCode)]

Execute `dotnet test`, e dois desses testes falham. Para fazer com que todos os testes sejam aprovados, altere a cláusula `if` no início do método `Main` no arquivo *PrimeService.cs*:

```csharp
if (candidate < 2)
```

Continue iterando adicionando mais testes, mais teorias e mais código na biblioteca principal. Você tem a [versão concluída dos testes](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-nunit/PrimeService.Tests/PrimeService_IsPrimeShould.cs) e a [implementação completa da biblioteca](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-nunit/PrimeService/PrimeService.cs).

Você criou uma pequena biblioteca e um conjunto de testes de unidade para essa biblioteca. Você estruturou a solução para que a adição de novos pacotes e testes fizesse parte do fluxo de trabalho normal. Você concentrou grande parte do seu tempo e esforço em resolver as metas do aplicativo.
