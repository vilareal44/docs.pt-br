---
title: Teste de unidade no .NET Core e no .NET Standard
description: Este artigo apresenta uma breve visão geral dos testes de unidade para projetos .NET Core e .NET Standard.
author: ardalis
ms.author: wiwagn
ms.date: 05/18/2020
zone_pivot_groups: unit-testing-framework-set-one
ms.openlocfilehash: e15f80b173389cdff86c6e62013e9c0f21171dd6
ms.sourcegitcommit: 0926684d8d34f4c6b5acce58d2193db093cb9cf2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83703098"
---
# <a name="unit-testing-in-net-core-and-net-standard"></a>Teste de unidade no .NET Core e no .NET Standard

O .NET Core torna mais fácil criar testes de unidade. Este artigo apresenta brevemente os testes de unidade e mostra como eles diferem de outros tipos de testes. Os recursos vinculados na parte inferior da página mostram como adicionar um projeto de teste à sua solução. Depois de configurar seu projeto de teste, você poderá executar testes de unidade usando a linha de comando ou o Visual Studio.

Se você estiver testando um projeto **ASP.NET Core** , consulte [testes de integração no ASP.NET Core](/aspnet/core/test/integration-tests#test-app-prerequisites).

O .NET Core 2.0 e posterior dá suporte ao [.NET Standard 2.0](../../standard/net-standard.md). Usamos as bibliotecas dele para demonstrar os testes de unidades.

Você é capaz de usar modelos de projeto de teste de unidade internos do .NET Core 2.0 e posteriores para C#, F# e Visual Basic como um ponto de partida para seu projeto pessoal.

## <a name="what-are-unit-tests"></a>O que são testes de unidade?

Testes automatizados são uma ótima maneira de garantir que um aplicativo de software faça o que seus autores pretendem. Existem vários tipos de testes para aplicativos de software. Isso inclui testes de integração, testes na Web, testes de carga e outros. **Testes de unidade** testam métodos e componentes de software individuais. Testes de unidade devem testar apenas o código dentro do controle do desenvolvedor. Eles não devem testar questões de infraestrutura. Questões de infraestrutura incluem bancos de dados, sistemas de arquivos e recursos de rede.

Além disso, tenha em mente que há práticas recomendadas para escrever testes. Por exemplo, um [TDD (Desenvolvimento Orientado por Testes)](https://deviq.com/test-driven-development/) é quando um teste de unidade é escrito antes do código que ele é destinado a verificar. O TDD é como criar um sumário para um livro antes de escrevê-lo. Ele destina-se a ajudar os desenvolvedores a escrever código mais legível, simples e eficiente.

> [!NOTE]
> A equipe do ASP.NET segue [estas convenções](https://github.com/dotnet/aspnetcore/wiki/Engineering-guidelines#unit-tests-and-functional-tests) para ajudar os desenvolvedores a inventar bons nomes para os métodos e classes de teste.

Tente não introduzir dependências na infraestrutura ao escrever testes de unidade. Elas tornam os testes lentos e frágeis, devendo ser reservadas para testes de integração. Você pode evitar essas dependências no aplicativo seguindo o [Princípio de Dependências Explícitas](https://deviq.com/explicit-dependencies-principle/) e usando a [Injeção de Dependência](/aspnet/core/fundamentals/dependency-injection). Você também pode manter seus testes de unidade em um projeto separado de seus testes de integração. Isso garante que seu projeto de teste de unidade não tenha dependências de pacotes de infraestrutura nem referências a eles.

## <a name="next-steps"></a>Próximas etapas

Mais informações sobre testes de unidade em projetos do .NET Core:

Projetos de testes de unidade do .NET Core são compatíveis com:

- [C#](../../csharp/index.yml)
- [Fixo #](../../fsharp/index.yml)
- [Visual Basic](../../visual-basic/index.yml)

Você também pode escolher entre várias estruturas de teste de unidade:

- [xUnit](https://xunit.net/)
- [NUnit](https://nunit.org)
- [MSTest](https://github.com/Microsoft/testfx-docs)

Você pode aprender mais nos guias passo a passo a seguir:

:::zone pivot="mstest"

- Criar testes de unidade usando [*MSTest* e *C#* com a CLI do .NET Core](unit-testing-with-mstest.md).
- Criar testes de unidade usando [*MSTest* e *F#* com a CLI do .NET Core](unit-testing-fsharp-with-mstest.md).
- Criar testes de unidade usando [*MSTest* e *Visual Basic* com a CLI do .NET Core](unit-testing-visual-basic-with-mstest.md).

:::zone-end
:::zone pivot="xunit"

- Criar testes de unidade usando [*xUnit* e *C#* com a CLI do .NET Core](unit-testing-with-dotnet-test.md).
- Criar testes de unidade usando [*xUnit* e *F#* com a CLI do .NET Core](unit-testing-fsharp-with-dotnet-test.md).
- Criar testes de unidade usando [*xUnit* e *Visual Basic* com a CLI do .NET Core](unit-testing-visual-basic-with-dotnet-test.md).

:::zone-end
:::zone pivot="nunit"

- Criar testes de unidade usando [*NUnit* e *C#* com a CLI do .NET Core](unit-testing-with-nunit.md).
- Criar testes de unidade usando [*NUnit* e *F#* com a CLI do .NET Core](unit-testing-fsharp-with-nunit.md).
- Criar testes de unidade usando [*NUnit* e *Visual Basic* com a CLI do .NET Core](unit-testing-visual-basic-with-nunit.md).

:::zone-end

Você pode saber mais nos seguintes artigos:

- O Visual Studio Enterprise oferece excelentes ferramentas de teste para o .NET Core. Para saber mais confira o [Live Unit Testing](/visualstudio/test/live-unit-testing) ou a [cobertura de código](https://github.com/Microsoft/vstest-docs/blob/master/docs/analyze.md#working-with-code-coverage).
- Para obter mais informações sobre como executar testes de unidade seletivos, veja [Executando testes de unidade seletivos](selective-unit-tests.md) ou [Incluindo e excluindo testes com o Visual Studio](/visualstudio/test/live-unit-testing#include-and-exclude-test-projects-and-test-methods).
- [Como usar o xUnit com o .NET Core e o Visual Studio](https://xunit.github.io/docs/getting-started-dotnet-core.html).
