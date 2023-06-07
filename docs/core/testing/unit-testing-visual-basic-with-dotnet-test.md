---
title: Teste de unidade do Visual Basic no .NET Core com dotnet test e xUnit
description: Aprenda os conceitos de teste de unidade no .NET Core por meio de uma experiência interativa de criação passo a passo de uma solução de exemplo do Visual Basic usando dotnet test e xUnit.
author: billwagner
ms.author: wiwagn
ms.date: 05/18/2020
ms.openlocfilehash: d384bf08f0b6031a519a8430c876eafc05d03a2e
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656417"
---
# <a name="unit-testing-visual-basic-net-core-libraries-using-dotnet-test-and-xunit"></a>Bibliotecas do .NET Core no Visual Basic de teste de unidade usando dotnet test e xUnit

Este tutorial mostra como criar uma solução que contém um projeto de teste de unidade e um projeto de biblioteca. Para seguir o tutorial usando uma solução predefinida, [exiba ou baixe o código de exemplo](https://github.com/dotnet/samples/tree/master/core/getting-started/unit-testing-using-dotnet-test/). Para obter instruções de download, consulte [Exemplos e tutoriais](../../samples-and-tutorials/index.md#view-and-download-samples).

## <a name="create-the-solution"></a>Criar a solução

Nesta seção, é criada uma solução que contém os projetos de origem e de teste. A solução concluída tem a seguinte estrutura de diretório:

```
/unit-testing-using-dotnet-test
    unit-testing-using-dotnet-test.sln
    /PrimeService
        PrimeService.vb
        PrimeService.vbproj
    /PrimeService.Tests
        PrimeService_IsPrimeShould.vb
        PrimeServiceTests.vbproj
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
  dotnet new classlib -o PrimeService --lang VB
  ```

   O [`dotnet new classlib`](../tools/dotnet-new.md) comando cria um novo projeto de biblioteca de classes na pasta *PrimeService* . A nova biblioteca de classes conterá o código a ser testado.
* Renomeie *Class1. vb* para *PrimeService. vb*.
* Substitua o código em *PrimeService. vb* pelo código a seguir:
  
  ```vb
  Imports System
  
  Namespace Prime.Services
      Public Class PrimeService
          Public Function IsPrime(candidate As Integer) As Boolean
              Throw New NotImplementedException("Not implemented.")
          End Function
      End Class
  End Namespace
  ```

* O código anterior:
  * Gera um <xref:System.NotImplementedException> com uma mensagem indicando que não está implementado.
  * Será atualizado posteriormente no tutorial.

<!-- preceding code shows an english bias. Message makes no sense outside english -->

* No diretório *Unit-Testing-using-dotnet-Test* , execute o seguinte comando para adicionar o projeto de biblioteca de classes à solução:

  ```dotnetcli
  dotnet sln add ./PrimeService/PrimeService.vbproj
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
  dotnet sln add ./PrimeService.Tests/PrimeService.Tests.vbproj
  ```

* Adicione a `PrimeService` biblioteca de classes como uma dependência ao projeto *PrimeService. Tests* :

  ```dotnetcli
  dotnet add ./PrimeService.Tests/PrimeService.Tests.vbproj reference ./PrimeService/PrimeService.vbproj  
  ```

<a name="create-test-cmd"></a>

### <a name="commands-to-create-the-solution"></a>Comandos para criar a solução

Esta seção resume todos os comandos na seção anterior. Ignore esta seção se você tiver concluído as etapas na seção anterior.

Os comandos a seguir criam a solução de teste em um computador Windows. Para macOS e UNIX, atualize o `ren` comando para a versão do sistema operacional do `ren` para renomear um arquivo:

```dotnetcli
dotnet new sln -o unit-testing-using-dotnet-test
cd unit-testing-using-dotnet-test
dotnet new classlib -o PrimeService
ren .\PrimeService\Class1.vb PrimeService.vb
dotnet sln add ./PrimeService/PrimeService.vbproj
dotnet new xunit -o PrimeService.Tests
dotnet add ./PrimeService.Tests/PrimeService.Tests.vbproj reference ./PrimeService/PrimeService.vbproj
dotnet sln add ./PrimeService.Tests/PrimeService.Tests.vbproj
```

Siga as instruções para "substituir o código em *PrimeService. vb* pelo código a seguir" na seção anterior.

## <a name="create-a-test"></a>Criar um teste

Uma abordagem popular no TDD (desenvolvimento controlado por teste) é escrever um teste antes de implementar o código de destino. Este tutorial usa a abordagem TDD. O `IsPrime` método é chamável, mas não implementado. Uma chamada de teste para `IsPrime` falha. Com o TDD, um teste é escrito que é conhecido como falha. O código de destino é atualizado para fazer o teste passar. Você continua repetindo essa abordagem, escrevendo um teste com falha e, em seguida, atualizando o código de destino para passar.

Atualize o projeto *PrimeService. Tests* :

* Exclua *PrimeService. Tests/UnitTest1. vb*.
* Crie um arquivo *PrimeService. Tests/PrimeService_IsPrimeShould. vb*  .
* Substitua o código em *PrimeService_IsPrimeShould. vb* pelo código a seguir:

```vb
Imports Xunit

Namespace PrimeService.Tests
    Public Class PrimeService_IsPrimeShould
        Private ReadOnly _primeService As Prime.Services.PrimeService

        Public Sub New()
            _primeService = New Prime.Services.PrimeService()
        End Sub


        <Fact>
        Sub IsPrime_InputIs1_ReturnFalse()
            Dim result As Boolean = _primeService.IsPrime(1)

            Assert.False(result, "1 should not be prime")
        End Sub

    End Class
End Namespace
```

O `[Fact]` atributo declara um método de teste que é executado pelo executor de teste. Na pasta *PrimeService. Tests* , execute `dotnet test` . O comando [dotnet Test](../tools/dotnet-test.md) compila ambos os projetos e executa os testes. O executor de teste do xUnit contém o ponto de entrada do programa para executar os testes. `dotnet test` inicia o executor de teste usando o projeto de teste de unidade.

O teste falha porque não `IsPrime` foi implementado. Usando a abordagem TDD, escreva apenas código suficiente para que esse teste passe. Atualize `IsPrime` com o seguinte código:

```vb
Public Function IsPrime(candidate As Integer) As Boolean
    If candidate = 1 Then
        Return False
    End If
    Throw New NotImplementedException("Not implemented.")
End Function
```

Execute `dotnet test`. O teste é aprovado.

### <a name="add-more-tests"></a>Adicionar mais testes

Adicionar testes de número primo para 0 e-1. Você pode copiar o teste anterior e alterar o código a seguir para usar 0 e-1:

```vb
Dim result As Boolean = _primeService.IsPrime(1)

Assert.False(result, "1 should not be prime")
```

Copiar o código de teste quando apenas um parâmetro altera os resultados na duplicação de código e no inchar de teste. Os seguintes atributos xUnit permitem escrever um conjunto de testes semelhantes:

- O `[Theory]` representa um pacote de testes que executa o mesmo código, mas têm diferentes argumentos de entrada.
- O atributo `[InlineData]` especifica valores para essas entradas.

Em vez de criar novos testes, aplique os atributos xUnit anteriores para criar uma única teoria. Substitua o código a seguir:

```vb
<Fact>
Sub IsPrime_InputIs1_ReturnFalse()
    Dim result As Boolean = _primeService.IsPrime(1)

    Assert.False(result, "1 should not be prime")
End Sub
```

pelo código a seguir:

```vb
<Theory>
<InlineData(-1)>
<InlineData(0)>
<InlineData(1)>
Sub IsPrime_ValuesLessThan2_ReturnFalse(ByVal value As Integer)
    Dim result As Boolean = _primeService.IsPrime(value)

    Assert.False(result, $"{value} should not be prime")
End Sub
```

No código anterior, `[Theory]` e `[InlineData]` habilite o teste de vários valores menores que dois. Dois é o menor número de primo.

Executar `dotnet test` , dois dos testes falham. Para fazer todos os testes serem aprovados, atualize o `IsPrime` método com o seguinte código:

```vb
Public Function IsPrime(candidate As Integer) As Boolean
    If candidate < 2 Then
        Return False
    End If
    Throw New NotImplementedException("Not fully implemented.")
End Function
```

Após a abordagem TDD, adicione mais testes com falha e, em seguida, atualize o código de destino. Consulte a [versão concluída dos testes](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-vb-dotnet-test/PrimeService.Tests/PrimeService_IsPrimeShould.vb) e a [implementação completa da biblioteca](https://github.com/dotnet/samples/blob/master/core/getting-started/unit-testing-vb-dotnet-test/PrimeService/PrimeService.vb).

O `IsPrime` método concluído não é um algoritmo eficiente para testar primality.

### <a name="additional-resources"></a>Recursos adicionais

- [Site oficial do xUnit.net](https://xunit.net/)
- [Lógica do controlador de teste no ASP.NET Core](/aspnet/core/mvc/controllers/testing)
- [`dotnet add reference`](../tools/dotnet-add-reference.md)
