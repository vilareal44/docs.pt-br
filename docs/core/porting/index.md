---
title: Porte do .NET Framework para o .NET Core
description: Entenda o processo de compatibilidade e descubra ferramentas que podem ser úteis ao realizar a portabilidade de um projeto do .NET Framework para o .NET Core.
author: cartermp
ms.date: 10/22/2019
ms.openlocfilehash: 74fe4519e41a07bc78a4dc346f8d1b52b5c7d092
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502763"
---
# <a name="overview-of-porting-from-net-framework-to-net-core"></a>Visão geral da portabilidade do .NET Framework para o .NET Core

Você pode ter um código que atualmente é executado no .NET Framework que você está interessado em portar para o .NET Core. Esse artigo fornece:

* Uma visão geral do processo de portabilidade.
* Uma lista de ferramentas que você pode achar útil quando está portando seu código para o .NET Core.

## <a name="overview-of-the-porting-process"></a>Visão geral do processo de portabilidade

Portar para o .NET Core (ou .NET Standard) de .NET Framework para muitos projetos é relativamente simples. Há várias alterações que são necessárias, mas muitas delas seguem os padrões descritos abaixo. Os projetos em que o modelo de aplicativo está disponível no .NET Core (como bibliotecas, aplicativos de console e aplicativos de área de trabalho) geralmente exigem pouca alteração. Os projetos que exigem um novo modelo de aplicativo, como a mudança para ASP.NET Core de ASP.NET, exigem um pouco mais de trabalho, mas muitos padrões têm analogias que podem ser usadas durante a conversão. Este documento deve ajudar a identificar as principais estratégias que foram empregadas pelos usuários para converter com êxito suas bases de código para o destino .NET Standard ou o .NET Core e abordarão a conversão em dois níveis: toda a solução e o projeto específico. Consulte os links na parte inferior para obter instruções sobre conversões específicas de modelo de aplicativo.

Recomendamos que você use o processo a seguir ao portar seu projeto para o .NET Core. Cada uma dessas etapas introduz possíveis locais para alterações de comportamento, portanto, certifique-se de testar adequadamente sua biblioteca ou aplicativo antes de continuar em etapas posteriores. As primeiras etapas são preparar seu projeto para um comutador para .NET Standard ou para o .NET Core. Se você tiver testes de unidade, será melhor convertê-los primeiro para que você possa continuar testando as alterações no produto em que está trabalhando. Como a portabilidade para o .NET Core é uma alteração significativa na base de código, é altamente recomendável portar seus projetos de teste para que você possa executar testes à medida que você portar seu código. MSTest, xUnit e NUnit funcionam no .NET Core.

## <a name="getting-started"></a>Introdução

As ferramentas a seguir serão usadas em todo o processo:

- Visual Studio 2019
- Baixar o [.net Portability Analyzer](../../standard/analyzers/portability-analyzer.md)
- _Opcional_ Instalar [dotnet try – Convert](https://github.com/dotnet/try-convert)

## <a name="porting-a-solution"></a>Portando uma solução

Quando há vários projetos em uma solução, a portabilidade pode parecer mais complicada porque você deve tratar projetos em uma ordem específica. O processo de conversão deve ser uma abordagem de baixo para cima, em que os projetos sem dependências em outros projetos na solução são convertidos primeiro e continuam em toda a solução.

Para identificar a ordem em que os projetos devem ser migrados, você pode usar as seguintes ferramentas:

- Os [diagramas de dependência no Visual Studio](/visualstudio/modeling/create-layer-diagrams-from-your-code) podem criar um grafo direcionado do código em uma solução.
- Execute `msbuild _SolutionPath_ /t:GenerateRestoreGraphFile /p:RestoreGraphOutputPath=graph.dg.json` para gerar um documento JSON que inclui a lista de referências do projeto.
- Execute o [analisador de portabilidade do .net](../../standard/analyzers/portability-analyzer.md) com a `-r DGML` opção para recuperar um diagrama de dependência dos assemblies. Para saber mais, clique [aqui](../../standard/analyzers/portability-analyzer.md#solution-wide-view).

Depois que você tiver informações de dependência, poderá usar essas informações para começar nos nós folha e trabalhar com a árvore de dependência aplicando as etapas na próxima seção.

## <a name="per-project-steps"></a>Etapas por projeto

Recomendamos que você use o seguinte processo ao portar seu projeto para o .NET Core:

1. Converta todas as suas `packages.config` dependências para o formato [PackageReference](/nuget/consume-packages/package-references-in-project-files) com a [ferramenta de conversão no Visual Studio](/nuget/consume-packages/migrate-packages-config-to-package-reference).

   Esta etapa envolve a conversão de suas dependências do formato herdado `packages.config` . `packages.config`Não funciona no .NET Core, portanto, essa conversão será necessária se você tiver dependências de pacote. Ele também requer apenas as dependências que você está usando diretamente em um projeto, o que facilita mais as etapas, reduzindo o número de dependências que você deve gerenciar.

1. Converta o arquivo de projeto para a nova estrutura de arquivos em estilo SDK. Você pode criar novos projetos para o .NET Core e copiar sobre os arquivos de origem, ou tentar converter o arquivo de projeto existente com uma ferramenta.

   O .NET Core usa um formato de arquivo de [projeto](../tools/csproj.md) simplificado (e diferente) do que o .NET Framework. Você precisará converter os arquivos de projeto nesse formato para continuar. Este estilo de projeto permite que você também direcione .NET Framework, que neste ponto você ainda desejará direcionar.

   Você pode tentar portar soluções menores ou projetos individuais em uma operação para o formato de arquivo de projeto do .NET Core com a ferramenta [dotnet try-Convert](https://github.com/dotnet/try-convert) . `dotnet try-convert`Não tem garantia de funcionar para todos os seus projetos e pode causar alterações sutis no comportamento que você dependou. Use-o como um _ponto de partida_ que automatize as coisas básicas que podem ser automatizadas. Não é uma solução garantida para migrar um projeto, pois há muitas diferenças nos destinos usados pelos projetos de estilo do SDK em comparação com os arquivos de projeto de estilo antigo.

1. Redirecione todos os projetos que você deseja que a porta direcione .NET Framework 4.7.2 ou superior.

   Essa etapa garante que você possa usar alternativas de API para destinos específicos do .NET Framework quando o .NET Core não oferecer suporte a determinada API.

1. Atualize todas as dependências para a versão mais recente. Os projetos podem estar usando versões mais antigas de bibliotecas que podem não ter suporte .NET Standard. No entanto, as versões posteriores podem dar suporte a ela com um comutador simples. Isso pode exigir alterações de código se houver alterações significativas nas bibliotecas.

1. Use o [.net Portability Analyzer](../../standard/analyzers/portability-analyzer.md) para analisar seus assemblies e ver se eles são portáteis para o .NET Core.

   A ferramenta Analisador de portabilidade .NET analisa seus assemblies compilados e gera um relatório. Este relatório mostra um resumo de portabilidade de alto nível e uma análise de cada API que você está usando e que não está disponível no núcleo da rede. Ao usar a ferramenta, envie apenas o projeto individual que você está convertendo para se concentrar nas alterações da API que são potencialmente necessárias. Muitas das APIs têm disponibilidade equivalente no .NET Core, para a qual você vai querer alternar.

   Ao ler os relatórios gerados pelo analisador, as informações importantes são as APIs reais que estão sendo usadas e não necessariamente a porcentagem de suporte para a plataforma de destino. Muitas APIs têm opções equivalentes no .NET Standard/Core e, portanto, entender os cenários de que sua biblioteca ou aplicativo precisa para a API ajudará a determinar a implicação da portabilidade.

   Há alguns casos em que as APIs não são equivalentes e você precisará fazer algumas diretivas de pré-processador do compilador (ou seja, `#if NET45` ) para o caso especial das plataformas. Neste ponto, seu projeto ainda será direcionado .NET Framework. Para cada um desses casos de destino, é recomendável usar condicionais bem conhecidos que podem ser compreendidos como um cenário.  Por exemplo, o suporte a AppDomain no .NET Core é limitado, mas para o cenário de carregamento e descarregamento de assemblies, há uma nova API que não está disponível no .NET Core. Uma maneira comum de lidar com isso no código seria algo assim:

   ```csharp
   #if FEATURE_APPDOMAIN_LOADING
   // Code that uses appdomains
   #elif FEATURE_ASSEMBLY_LOAD_CONTEXT
   // Code that uses assembly load context
   #else
   #error Unsupported platform
   #endif
   ```

1. Instale o [.NET API Analyzer](../../standard/analyzers/api-analyzer.md) em seus projetos para identificar as APIs que lançam <xref:System.PlatformNotSupportedException> em algumas plataformas e outros problemas de compatibilidade em potencial.

   Essa ferramenta é semelhante ao analisador de portabilidade, mas em vez de analisar se o código pode ser criado no .NET Core, ele analisa se você está usando uma API de uma forma que gerará um <xref:System.PlatformNotSupportedException> em tempo de execução. Embora isso não seja comum se você estiver mudando de .NET Framework 4.7.2 ou superior, é bom verificar. Para obter mais informações sobre APIs que lançam exceções no .NET Core, consulte [APIs que sempre lançam exceções no .NET Core](../compatibility/unsupported-apis.md).

1. Neste ponto, você pode mudar para direcionar o .NET Core (geralmente para aplicativos) ou .NET Standard (para bibliotecas).

   A escolha entre o .NET Core e o .NET Standard é amplamente dependente de onde o projeto será executado. Se for uma biblioteca que será consumida por outros aplicativos ou distribuída via NuGet, a preferência geralmente será direcionada .NET Standard. No entanto, pode haver APIs que só estão disponíveis no .NET Core para fins de desempenho ou outros motivos; Se esse for o caso, o .NET Core deve ser direcionado com uma compilação potencialmente .NET Standard disponível, bem como desempenho ou funcionalidade reduzidos. Ao direcionar .NET Standard, o projeto estará pronto para ser executado em novas plataformas (como Webassembly). Se o projeto tiver dependências em estruturas de aplicativo específicas (como ASP.NET Core), o destino será limitado pelo que as dependências dão suporte.

   Se não houver diretivas de pré-processador para o código de compilação condicional para .NET Framework ou .NET Standard, isso será tão simples quanto encontrar o seguinte no arquivo de projeto:

   ```xml
   <TargetFramework>net472</TargetFramework>
   ```

   e alterne para a estrutura desejada. Para o .NET Core 3,1, isso seria:

   ```xml
   <TargetFramework>netcoreapp3.1</TargetFramework>
   ```

   No entanto, se esta for uma biblioteca para a qual você deseja continuar a dar suporte a compilações específicas de .NET Framework, você pode fazer [vários destinos](../../standard/library-guidance/cross-platform-targeting.md) substituindo-o pelo seguinte:

   ```xml
   <TargetFrameworks>net472;netstandard2.0</TargetFrameworks>
   ```

   Se você estiver usando APIs específicas do Windows (como o acesso ao registro), instale o [pacote de compatibilidade do Windows](./windows-compat-pack.md).

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Analisar dependências](third-party-deps.md) 
>  Pacote [NuGet](../deploying/creating-nuget-packages.md) 
>  do pacote [ASP.net para migração de ASP.NET Core](/aspnet/core/migration/proper-to-2x)
