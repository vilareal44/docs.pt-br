---
title: Código C# de teste de unidade no .NET Core usando dotnet test e xUnit
description: Aprenda os conceitos de teste de unidade no C# e .NET Core por meio de uma experiência interativa de criação passo a passo de uma solução de exemplo usando dotnet test e xUnit.
author: ardalis
ms.author: wiwagn
ms.date: 12/04/2019
ms.openlocfilehash: feff4cabbd10064ef4acca12d4f960f2a40a2b12
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656378"
---
# <a name="unit-testing-c-in-net-core-using-dotnet-test-and-xunit"></a>C# de teste de unidade no .NET Core usando dotnet test e xUnit

Este tutorial mostra como criar uma solução que contém um projeto de teste de unidade e um projeto de código-fonte. Para seguir o tutorial usando uma solução predefinida, [exiba ou baixe o código de exemplo](https://github.com/dotnet/samples/tree/master/core/getting-started/unit-testing-using-dotnet-test/). Para obter instruções de download, consulte [Exemplos e tutoriais](../../samples-and-tutorials/index.md#view-and-download-samples).

## <a name="create-the-solution"></a>Criar a solução

Nesta seção, é criada uma solução que contém os projetos de origem e de teste. A solução concluída tem a seguinte estrutura de diretório:

```
/unit-testing-using-dotnet-test
    unit-testing-using-dotnet-test.sln
    /PrimeService
        PrimeService.cs
        PrimeService.csproj
    /PrimeService.Tests
        PrimeService_IsPrimeShould.cs
        PrimeServiceTests.csproj
```

As instruções a seguir fornecem as etapas para criar a solução de teste. Consulte [comandos para criar solução de teste](#create-test-cmd) para obter instruções para criar a solução de teste em uma única etapa.

* Abra uma janela do shell.
* Execute o seguinte comando:

  ```dotnetcli
  dotnet new sln -o unit-testing-using-dotnet-test
  ```

  O [`dotnet new sln`](../tools/dotnet-new.md) comando cria uma nova solução no diretório *Unit-Testing-using-dotnet-Test* .
* Altere o diretório para a pasta *Unit-Testing-using-dotnet-Test* .
* Execute o seguinte comando:

  ```dotnetcli
  dotnet new classlib -o PrimeService
  ```

   O [`dotnet new classlib`](../tools/dotnet-new.md) comando cria um novo projeto de biblioteca de classes na pasta *PrimeService* . A nova biblioteca de classes conterá o código a ser testado.
* Renomeie *Class1.cs* como *PrimeService.cs*.
* Substitua o código em *PrimeService.cs* pelo seguinte código:
  
  ```csharp
  using System;

  namespace Prime.Services
  {
      public class PrimeService
      {
          public bool IsPrime(int candidate)
          {
              throw new NotImplementedException("Not implemented.");
          }
      }
  }
  ```

* O código anterior:
  * Gera um <xref:System.NotImplementedException> com uma mensagem indicando que não está implementado.
  * Será atualizado posteriormente no tutorial.

<!-- preceding code shows an english bias. Message makes no sense outside english -->

* No diretório *Unit-Testing-using-dotnet-Test* , execute o seguinte comando para adicionar o projeto de biblioteca de classes à solução:

  ```dotnetcli
  dotnet sln add ./PrimeService/PrimeService.csproj
  ```

* Crie o projeto *PrimeService. Tests* executando o seguinte comando:

  ```dotnetcli
  dotnet new xunit -o PrimeService.Tests
  ```

* O comando anterior:
  * Cria o projeto *PrimeService. Tests* no diretório *PrimeService. Tests* . O projeto de teste usa [xUnit](https://xunit.net/) como a biblioteca de teste.
  * Configura o executor de teste adicionando os seguintes `<PackageReference />` elementos ao arquivo de projeto:
    * "Microsoft. NET. Test. SDK"
    * xUnit
    * "xUnit. Runner. VisualStudio"

* Adicione o projeto de teste ao arquivo de solução executando o seguinte comando:

  ```dotnetcli
  dotnet sln add ./PrimeService.Tests/PrimeService.Tests.csproj
  ```

* Adicione a `PrimeService` biblioteca de classes como uma dependência ao projeto *PrimeService. Tests* :

  ```dotnetcli
  dotnet add ./PrimeService.Tests/PrimeService.Tests.csproj reference ./PrimeService/PrimeService.csproj  
  ```

<a name="create-test-cmd"></a>

### <a name="commands-to-create-the-solution"></a>Comandos para criar a solução

Esta seção resume todos os comandos na seção anterior. Ignore esta seção se você tiver concluído as etapas na seção anterior.

Os comandos a seguir criam a solução de teste em um computador Windows. Para macOS e UNIX, atualize o `ren` comando para a versão do sistema operacional do `ren` para renomear um arquivo:

```dotnetcli
dotnet new sln -o unit-testing-using-dotnet-test
cd unit-testing-using-dotnet-test
dotnet new classlib -o PrimeService
ren .\PrimeService\Class1.cs PrimeService.cs
dotnet sln add ./PrimeService/PrimeService.csproj
dotnet new xunit -o PrimeService.Tests
dotnet add ./PrimeService.Tests/PrimeService.Tests.csproj reference ./PrimeService/PrimeService.csproj
dotnet sln add ./PrimeService.Tests/PrimeService.Tests.csproj
```

Siga as instruções para "substituir o código em *PrimeService.cs* com o código a seguir" na seção anterior.

## <a name="create-a-test"></a>Criar um teste

Uma abordagem popular no TDD (desenvolvimento controlado por teste) é escrever um teste antes de implementar o código de destino. Este tutorial usa a abordagem TDD. O `IsPrime` método é chamável, mas não implementado. Uma chamada de teste para `IsPrime` falha. Com o TDD, um teste é escrito que é conhecido como falha. O código de destino é atualizado para fazer o teste passar. Você continua repetindo essa abordagem, escrevendo um teste com falha e, em seguida, atualizando o código de destino para passar.

Atualize o projeto *PrimeService. Tests* :

* Exclua *PrimeService. Tests/UnitTest1. cs*.
* Crie um arquivo *PrimeService. Tests/PrimeService_IsPrimeShould. cs*  .
* Substitua o código em *PrimeService_IsPrimeShould. cs* pelo código a seguir:

```csharp
using Xunit;
using Prime.Services;

namespace Prime.UnitTests.Services
{
    public class PrimeService_IsPrimeShould
    {
        private readonly PrimeService _primeService;

        public PrimeService_IsPrimeShould()
        {
            _primeService = new PrimeService();
        }

        [Fact]
        public void IsPrime_InputIs1_ReturnFalse()
        {
            var result = _primeService.IsPrime(1);

            Assert.False(result, "1 should not be prime");
        }
    }
}
```

O `[Fact]` atributo declara um método de teste que é executado pelo executor de teste. Na pasta *PrimeService. Tests* , execute `dotnet test` . O comando [dotnet Test](../tools/dotnet-test.md) compila ambos os projetos e executa os testes. O executor de teste do xUnit contém o ponto de entrada do programa para executar os testes. `dotnet test` inicia o executor de teste usando o projeto de teste de unidade.

O teste falha porque não `IsPrime` foi implementado. Usando a abordagem TDD, escreva apenas código suficiente para que esse teste passe. Atualize `IsPrime` com o seguinte código:

```csharp
public bool IsPrime(int candidate)
{
    if (candidate == 1)
    {
        return false;
    }
    throw new NotImplementedException("Not fully implemented.");
}
```

Execute `dotnet test`. O teste é aprovado.

### <a name="add-more-tests"></a>Adicionar mais testes

Adicionar testes de número primo para 0 e-1. Você pode copiar o teste anterior e alterar o código a seguir para usar 0 e-1:

```csharp
var result = _primeService.IsPrime(1);

Assert.False(result, "1 should not be prime");
```

Copiar o código de teste quando apenas um parâmetro altera os resultados na duplicação de código e no inchar de teste. Os seguintes atributos xUnit permitem escrever um conjunto de testes semelhantes:

- O `[Theory]` representa um pacote de testes que executa o mesmo código, mas têm diferentes argumentos de entrada.
- O atributo `[InlineData]` especifica valores para essas entradas.

Em vez de criar novos testes, aplique os atributos xUnit anteriores para criar uma única teoria. Substitua o código a seguir:

```csharp
[Fact]
public void IsPrime_InputIs1_ReturnFalse()
{
    var result = _primeService.IsPrime(1);

    Assert.False(result, "1 should not be prime");
}
```

pelo código a seguir:

[!code-csharp[Sample_TestCode](../../../samples/snippets/core/testing/unit-testing-using-dotnet-test/csharp/PrimeService.Tests/PrimeService_IsPrimeShould.cs?name=Sample_TestCode)]

No código anterior, `[Theory]` e `[InlineData]` habilite o teste de vários valores menores que dois. Dois é o menor número de primo.

Executar `dotnet test` , dois dos testes falham. Para fazer todos os testes serem aprovados, atualize o `IsPrime` método com o seguinte código:

```csharp
public bool IsPrime(int candidate)
{
    if (candidate < 2)
    {
        return false;
    }
    throw new NotImplementedException("Not fully implemented.");
}
```

Após a abordagem TDD, adicione mais testes com falha e, em seguida, atualize o código de destino. Consulte a [versão concluída dos testes](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-dotnet-test/PrimeService.Tests/PrimeService_IsPrimeShould.cs) e a [implementação completa da biblioteca](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-using-dotnet-test/PrimeService/PrimeService.cs).

O `IsPrime` método concluído não é um algoritmo eficiente para testar primality.

### <a name="additional-resources"></a>Recursos adicionais

- [Site oficial do xUnit.net](https://xunit.net)
- [Lógica do controlador de teste no ASP.NET Core](/aspnet/core/mvc/controllers/testing)
- [`dotnet add reference`](../tools/dotnet-add-reference.md)
