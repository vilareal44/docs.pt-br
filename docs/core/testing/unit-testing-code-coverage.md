---
title: Usar cobertura de código para teste de unidade
description: Saiba como usar os recursos de cobertura de código para testes de unidade .NET.
author: IEvangelist
ms.author: dapine
ms.date: 07/01/2020
ms.openlocfilehash: af64116e86c3f46f37c8d5d079b9c86084095485
ms.sourcegitcommit: b6a1869f97a37f11a68c90afde1a520a6887dcbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853906"
---
# <a name="use-code-coverage-for-unit-testing"></a>Usar cobertura de código para teste de unidade

Os testes de unidade ajudam a garantir a funcionalidade e fornecem um meio de verificação para esforços de refatoração. A cobertura de código é uma medida da quantidade de código que é executada por testes de unidade: ambas as linhas, ramificações ou métodos. Por exemplo, se você tiver um aplicativo simples com apenas duas ramificações condicionais de código (_Branch a_e _Branch b_), um teste de unidade que verifica _a ramificação_ condicional que relatará a cobertura de código de ramificação de 50%.

Este artigo aborda o uso da cobertura de código para testes de unidade com coverlet e geração de relatórios usando o ReportGenerator. Embora este artigo se concentre em C# e xUnit como a estrutura de teste, o MSTest e o NUnit também funcionariam. O coverlet é um projeto de software livre [no GitHub](https://github.com/coverlet-coverage/coverlet) que fornece uma estrutura de cobertura de código de plataforma cruzada para C#. O [coverlet](https://dotnetfoundation.org/projects/coverlet) faz parte do .net Foundation. O coverlet coleta dados de execução de teste de cobertura de cobertura, que são usados para geração de relatórios.

Além disso, este artigo fornece detalhes sobre como usar as informações de cobertura de código coletadas de uma execução de teste do coverlet para gerar um relatório. A geração de relatórios é possível usando outro [projeto de código-fonte aberto no GitHub-ReportGenerator](https://github.com/danielpalme/ReportGenerator). O ReportGenerator converte relatórios de cobertura gerados por cobertura entre muitos outros, em relatórios legíveis por humanos em vários formatos.

Este artigo se baseia no [projeto de código-fonte de exemplo](https://docs.microsoft.com/samples/dotnet/samples/unit-testing-code-coverage-cs), disponível no navegador de exemplos.

## <a name="system-under-test"></a>Sistema em teste

O "sistema em teste" refere-se ao código no qual você está escrevendo testes de unidade, pode ser um objeto, serviço ou qualquer outra coisa que expõe a funcionalidade de teste. Para a finalidade deste artigo, você criará uma biblioteca de classes que será o sistema em teste e dois projetos de teste de unidade correspondentes.

### <a name="create-a-class-library"></a>Criar uma biblioteca de classes

Em um prompt de comando em um novo diretório chamado `UnitTestingCodeCoverage` , crie uma nova biblioteca de classes padrão .NET usando o [`dotnet new classlib`](../tools/dotnet-new.md#classlib) comando:

```dotnetcli
dotnet new classlib -n Numbers
```

O trecho de código a seguir define uma `PrimeService` classe simples que fornece funcionalidade para verificar se um número é primo. Copie o trecho abaixo e substitua o conteúdo do arquivo *Class1.cs* que foi criado automaticamente no diretório *Numbers* . Renomeie o arquivo *Class1.cs* para *PrimeService.cs*.

```csharp
namespace System.Numbers
{
    public class PrimeService
    {
        public bool IsPrime(int candidate)
        {
            if (candidate < 2)
            {
                return false;
            }

            for (int divisor = 2; divisor <= Math.Sqrt(candidate); ++divisor)
            {
                if (candidate % divisor == 0)
                {
                    return false;
                }
            }
            return true;
        }
    }
}
```

> [!TIP]
> Vale a pena mencionar que a `Numbers` biblioteca de classes foi adicionada intencionalmente ao `System` namespace. Isso permite que o para <xref:System.Math?displayProperty=fullName> seja acessível sem uma `using System;` declaração de namespace. Para obter mais informações, consulte [namespace (referência C#)](../../csharp/language-reference/keywords/namespace.md).

### <a name="create-test-projects"></a>Criar projetos de teste

Crie dois modelos novos de **projeto de teste do xUnit (.NET Core)** no mesmo prompt de comando usando o [`dotnet new xunit`](../tools/dotnet-new.md#test) comando:

```dotnetcli
dotnet new xunit -n XUnit.Coverlet.Collector
```

```dotnetcli
dotnet new xunit -n XUnit.Coverlet.MSBuild
```

Os dois projetos de teste xUnit criados recentemente precisam adicionar uma referência de projeto da biblioteca de classes *Numbers* . Isso é para que os projetos de teste tenham acesso ao *PrimeService* para teste. No prompt de comando, use o [`dotnet add`](../tools/dotnet-add-reference.md) comando:

```dotnetcli
dotnet add XUnit.Coverlet.Collector\XUnit.Coverlet.Collector.csproj reference Numbers\Numbers.csproj
```

```dotnetcli
dotnet add XUnit.Coverlet.MSBuild\XUnit.Coverlet.MSBuild.csproj reference Numbers\Numbers.csproj
```

O projeto *MSBuild* é nomeado adequadamente, pois dependerá do pacote NuGet [coverlet. MSBuild](https://www.nuget.org/packages/coverlet.msbuild) . Adicione essa dependência do pacote executando o [`dotnet add package`](../tools/dotnet-add-package.md) comando:

```dotnetcli
cd XUnit.Coverlet.MSBuild && dotnet add package coverlet.msbuild && cd ..
```

O comando anterior alterou os diretórios efetivamente com escopo para o projeto de teste do *MSBuild* e, em seguida, adicionou o pacote NuGet. Quando isso foi feito, ele alterava os diretórios, passando um nível para cima.

Abra os dois arquivos *UnitTest1.cs* e substitua seu conteúdo pelo trecho a seguir. Renomeie os arquivos *UnitTest1.cs* para *PrimeServiceTests.cs*.

```csharp
using System.Numbers;
using Xunit;

namespace XUnit.Coverlet
{
    public class PrimeServiceTests
    {
        readonly PrimeService _primeService;

        public PrimeServiceTests() => _primeService = new PrimeService();

        [
            Theory,
            InlineData(-1), InlineData(0), InlineData(1)
        ]
        public void IsPrime_ValuesLessThan2_ReturnFalse(int value) =>
            Assert.False(_primeService.IsPrime(value), $"{value} should not be prime");

        [
            Theory,
            InlineData(2), InlineData(3), InlineData(5), InlineData(7)
        ]
        public void IsPrime_PrimesLessThan10_ReturnTrue(int value) =>
            Assert.True(_primeService.IsPrime(value), $"{value} should be prime");

        [
            Theory,
            InlineData(4), InlineData(6), InlineData(8), InlineData(9)
        ]
        public void IsPrime_NonPrimesLessThan10_ReturnFalse(int value) =>
            Assert.False(_primeService.IsPrime(value), $"{value} should not be prime");
    }
}
```

### <a name="create-a-solution"></a>Criar uma solução

No prompt de comando, crie uma nova solução para encapsular a biblioteca de classes e os dois projetos de teste. Usando o [`dotnet sln`](../tools/dotnet-sln.md) comando:

```dotnetcli
dotnet new sln -n XUnit.Coverage
```

Isso criará um novo nome de arquivo `XUnit.Coverage` de solução no diretório *UnitTestingCodeCoverage* . Adicione os projetos à raiz da solução.

## <a name="linux"></a>[Linux](#tab/linux)

```dotnetcli
dotnet sln XUnit.Coverage.sln add **/*.csproj --in-root
```

## <a name="windows"></a>[Windows](#tab/windows)

```dotnetcli
dotnet sln XUnit.Coverage.sln add (ls **/*.csproj) --in-root
```

---

Compile a solução usando o [`dotnet build`](../tools/dotnet-build.md) comando:

```dotnetcli
dotnet build
```

Se a compilação for bem-sucedida, você terá criado os três projetos, adequadamente os projetos e pacotes referenciados e atualizado o código-fonte corretamente. Muito bem!

## <a name="tooling"></a>Ferramentas

Há dois tipos de ferramentas de cobertura de código:

- **Datacoletores:** Datacoletores monitoram a execução de teste e coletam informações sobre execuções de teste. Eles relatam as informações coletadas em vários formatos de saída, como XML e JSON. Para obter mais informações, consulte [seu primeiro datacoletor](https://github.com/Microsoft/vstest-docs/blob/master/docs/extensions/datacollector.md).
- **Geradores de relatórios:** Use os dados coletados das execuções de teste para gerar relatórios, geralmente como HTML com estilo.

Nesta seção, o foco está nas ferramentas do coletor de dados. Para usar o coverlet para cobertura de código, um projeto de teste de unidade existente deve ter as dependências de pacote apropriadas ou, alternativamente, contar com as [ferramentas globais do .net](../tools/global-tools.md) e o pacote NuGet do [coverlet. console](https://www.nuget.org/packages/coverlet.console) correspondente.

## <a name="integrate-with-net-test"></a>Integrar com o teste .NET

O modelo de projeto de teste do xUnit já se integra ao [coverlet. Collector](https://www.nuget.org/packages/coverlet.collector) por padrão.
No prompt de comando, altere os diretórios para o projeto *xUnit. coverlet. Collector* e execute o [`dotnet test`](../tools/dotnet-test.md) comando:

```dotnetcli
cd XUnit.Coverlet.Collector && dotnet test --collect:"XPlat Code Coverage"
```

> [!NOTE]
> O `"XPlat Code Coverage"` argumento é um nome amigável que corresponde aos coletores de dados de coverlet. Esse nome é necessário, mas não diferencia maiúsculas de minúsculas.

Como parte da `dotnet test` execução, um arquivo *coverage.cobertura.xml* resultante é apresentado para o diretório *TestResults* . O arquivo XML contém os resultados. Essa é uma opção de plataforma cruzada que depende do CLI do .NET Core, e é excelente para sistemas de compilação em que o MSBuild não está disponível.

Veja abaixo o arquivo de *coverage.cobertura.xml* de exemplo.

```xml
<?xml version="1.0" encoding="utf-8"?>
<coverage line-rate="1" branch-rate="1" version="1.9" timestamp="1592248008"
          lines-covered="12" lines-valid="12" branches-covered="6" branches-valid="6">
  <sources>
    <source>C:\</source>
  </sources>
  <packages>
    <package name="Numbers" line-rate="1" branch-rate="1" complexity="6">
      <classes>
        <class name="Numbers.PrimeService" line-rate="1" branch-rate="1" complexity="6"
               filename="Numbers\PrimeService.cs">
          <methods>
            <method name="IsPrime" signature="(System.Int32)" line-rate="1"
                    branch-rate="1" complexity="6">
              <lines>
                <line number="8" hits="11" branch="False" />
                <line number="9" hits="11" branch="True" condition-coverage="100% (2/2)">
                  <conditions>
                    <condition number="7" type="jump" coverage="100%" />
                  </conditions>
                </line>
                <line number="10" hits="3" branch="False" />
                <line number="11" hits="3" branch="False" />
                <line number="14" hits="22" branch="True" condition-coverage="100% (2/2)">
                  <conditions>
                    <condition number="57" type="jump" coverage="100%" />
                  </conditions>
                </line>
                <line number="15" hits="7" branch="False" />
                <line number="16" hits="7" branch="True" condition-coverage="100% (2/2)">
                  <conditions>
                    <condition number="27" type="jump" coverage="100%" />
                  </conditions>
                </line>
                <line number="17" hits="4" branch="False" />
                <line number="18" hits="4" branch="False" />
                <line number="20" hits="3" branch="False" />
                <line number="21" hits="4" branch="False" />
                <line number="23" hits="11" branch="False" />
              </lines>
            </method>
          </methods>
          <lines>
            <line number="8" hits="11" branch="False" />
            <line number="9" hits="11" branch="True" condition-coverage="100% (2/2)">
              <conditions>
                <condition number="7" type="jump" coverage="100%" />
              </conditions>
            </line>
            <line number="10" hits="3" branch="False" />
            <line number="11" hits="3" branch="False" />
            <line number="14" hits="22" branch="True" condition-coverage="100% (2/2)">
              <conditions>
                <condition number="57" type="jump" coverage="100%" />
              </conditions>
            </line>
            <line number="15" hits="7" branch="False" />
            <line number="16" hits="7" branch="True" condition-coverage="100% (2/2)">
              <conditions>
                <condition number="27" type="jump" coverage="100%" />
              </conditions>
            </line>
            <line number="17" hits="4" branch="False" />
            <line number="18" hits="4" branch="False" />
            <line number="20" hits="3" branch="False" />
            <line number="21" hits="4" branch="False" />
            <line number="23" hits="11" branch="False" />
          </lines>
        </class>
      </classes>
    </package>
  </packages>
</coverage>
```

> [!TIP]
> Como alternativa, você pode usar o pacote do MSBuild se o seu sistema de compilação já usa o MSBuild. No prompt de comando, altere os diretórios para o projeto *xUnit. coverlet. MSBuild* e execute o `dotnet test` comando:
>
> ```dotnetcli
> dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=cobertura
> ```
>
> O arquivo resultante *coverage.cobertura.xml* é a saída.  
> Você pode seguir o guia de integração do MSBuild [aqui](https://github.com/coverlet-coverage/coverlet/blob/master/Documentation/MSBuildIntegration.md)

## <a name="generate-reports"></a>Gerar relatórios

Agora que você é capaz de coletar dados de execuções de teste de unidade, você pode gerar relatórios usando [ReportGenerator](https://github.com/danielpalme/ReportGenerator). Para instalar o pacote NuGet do [ReportGenerator](https://www.nuget.org/packages/dotnet-reportgenerator-globaltool) como uma [ferramenta global do .net](../tools/global-tools.md), use o [`dotnet tool install`](../tools/dotnet-tool-install.md) comando:

```dotnetcli
dotnet tool install -g dotnet-reportgenerator-globaltool
```

Execute a ferramenta e forneça as opções desejadas, considerando o arquivo de *coverage.cobertura.xml* de saída da execução de teste anterior.

```console
reportgenerator
"-reports:Path\To\TestProject\TestResults\{guid}\coverage.cobertura.xml"
"-targetdir:coveragereport"
-reporttypes:Html
```

Depois de executar esse comando, um arquivo HTML representa o relatório gerado.

:::image type="content" source="media/test-report.png" lightbox="media/test-report.png" alt-text="Relatório gerado por teste de unidade":::

## <a name="see-also"></a>Confira também

- [Cobertura da capa de teste de unidade do Visual Studio](/visualstudio/test/using-code-coverage-to-determine-how-much-code-is-being-tested)
- [Repositório GitHub-coverlet](https://github.com/coverlet-coverage/coverlet)
- [Repositório GitHub-ReportGenerator](https://github.com/danielpalme/ReportGenerator)
- [Site do projeto ReportGenerator](https://danielpalme.github.io/ReportGenerator)
- [CLI do .NET Core comando de teste](../tools/dotnet-test.md)
- [Exemplo de código fonte](https://docs.microsoft.com/samples/dotnet/samples/unit-testing-code-coverage-cs)

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Melhores práticas de teste de unidade](unit-testing-best-practices.md)
