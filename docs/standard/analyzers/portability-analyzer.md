---
title: O .NET Portability Analyzer – .NET
description: Saiba como usar a ferramenta .NET Portability Analyzer para avaliar a portabilidade do seu código entre as várias implementações de .NET, incluindo .NET Core, .NET Standard, UWP e Xamarin.
ms.date: 09/13/2019
ms.technology: dotnet-standard
ms.assetid: 0375250f-5704-4993-a6d5-e21c499cea1e
ms.openlocfilehash: d2a9551565e9ef0a2ed76960c869829fc2e86a1f
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84903604"
---
# <a name="the-net-portability-analyzer"></a>O .NET Portability Analyzer

Deseja fazer suas bibliotecas serem compatíveis com multiplataforma? Deseja ver quanto trabalho é necessário para fazer seu .NET Framework aplicativo ser executado no .NET Core? O [.net Portability Analyzer](https://github.com/microsoft/dotnet-apiport) é uma ferramenta que analisa os assemblies e fornece um relatório detalhado sobre as APIs do .NET que estão faltando para que os aplicativos ou bibliotecas sejam portáteis em suas plataformas .net de destino especificadas. O analisador de portabilidade é oferecido como uma [extensão do Visual Studio](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer), que analisa um assembly por projeto e como um [aplicativo de console ApiPort](https://aka.ms/apiportdownload), que analisa os assemblies por arquivos ou diretórios especificados.

Depois de converter seu projeto para direcionar a nova plataforma, como o .NET Core, você pode usar a [ferramenta Analisador de API](api-analyzer.md) baseada em Roslyn para identificar as APIs que geram <xref:System.PlatformNotSupportedException> exceções e outros problemas de compatibilidade.

## <a name="common-targets"></a>Destinos comuns

- [.NET Core](../../core/index.yml): tem um design modular, dá suporte à instalação lado a lado e tem como alvo cenários de plataforma cruzada. A instalação lado a lado permite que você adote novas versões do .NET Core sem interromper outros aplicativos. Se seu objetivo é portar seu aplicativo para o .NET Core e oferecer suporte a várias plataformas, esse é o destino recomendado.
- . [Net Standard](../net-standard.md): inclui as APIs de .net Standard disponíveis em todas as implementações do .net. Se seu objetivo é fazer com que sua biblioteca seja executada em todas as plataformas com suporte do .NET, esse é o destino recomendado.
- [ASP.NET Core](/aspnet/core): uma estrutura da Web moderna criada no .NET Core. Se sua meta é portar seu aplicativo Web para o .NET Core para dar suporte a várias plataformas, esse é o destino recomendado.
- [Extensões de plataforma](../../core/porting/windows-compat-pack.md).NET Core +: inclui as APIs do .NET Core, além do pacote de compatibilidade do Windows, que fornece muitas das .NET Framework tecnologias disponíveis. Esse é um destino recomendado para portar seu aplicativo do .NET Framework para o .NET Core no Windows.
- .NET Standard + [extensões de plataforma](../../core/porting/windows-compat-pack.md): inclui as APIs de .net Standard além do pacote de compatibilidade do Windows, que fornece muitas das .NET Framework tecnologias disponíveis. Esse é um destino recomendado para portar sua biblioteca do .NET Framework para o .NET Core no Windows.

## <a name="how-to-use-the-net-portability-analyzer"></a>Como usar o Analisador de Portabilidade do .NET

Para começar a usar o Analisador de Portabilidade do .NET no Visual Studio, primeiro é necessário baixar e instalar a extensão do [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer). Ele funciona no Visual Studio 2017 e versões posteriores. Configure-o no Visual Studio **usando**  >  **as configurações** do analisador de portabilidade e selecione suas plataformas de destino, que são as plataformas .net/versões que você deseja avaliar as lacunas de portabilidade que são comparadas com a plataforma/versão com a qual seu assembly atual foi criado.

![Captura de tela do analisador de portabilidade.](./media/portability-analyzer/portability-screenshot.png)

Também é possível usar o aplicativo de console ApiPort, baixando-o do [repositório ApiPort](https://aka.ms/apiportdownload). É possível usar a opção de comando `listTargets` para exibir a lista de destino disponível e escolher as plataformas de destino especificando a opção de comando `-t` ou `--target`.

### <a name="solution-wide-view"></a>Visão ampla da solução

Uma etapa útil na análise de uma solução com muitos projetos seria Visualizar as dependências para entender qual subconjunto de assemblies dependem do que. A recomendação geral é aplicar os resultados da análise em uma abordagem de baixo para cima, começando com os nós folha em um grafo de dependência.

Para recuperar isso, você pode executar o seguinte comando:

```
ApiPort.exe analyze -r DGML -f [directory or file]
```

Um resultado disso seria semelhante ao seguinte quando aberto no Visual Studio:

![Captura de tela da análise de DGML.](./media/portability-analyzer/dgml-example.png)

### <a name="analyze-portability"></a>Analisar portabilidade
Para analisar todo o projeto no Visual Studio, clique com o botão direito do mouse no projeto no **Gerenciador de Soluções** e selecione **Analisar Portabilidade do Assembly**. Caso contrário, acesse o menu **Analyze** e selecione **Analisar Portabilidade do Assembly**. A partir daí, selecione o executável ou a DLL do seu projeto.

![Captura de tela do analisador de portabilidade do Gerenciador de Soluções.](./media/portability-analyzer/portability-solution-explorer.png)

Também é possível usar o [aplicativo de console ApiPort](https://aka.ms/apiportdownload).

- Digite o seguinte comando para analisar o diretório atual: `ApiPort.exe analyze -f .`
- Para analisar uma lista específica de arquivos. dll, digite o seguinte comando: `ApiPort.exe analyze -f first.dll -f second.dll -f third.dll`
- Execute `ApiPort.exe -?` para obter mais ajuda

É recomendável que você inclua todos os arquivos exe e dll relacionados que você tem e que deseje portar e exclua os arquivos do qual seu aplicativo depende, mas que você não tem e não pode portar. Isso fará com que o relatório de portabilidade seja o mais relevante.

### <a name="view-and-interpret-portability-result"></a>Exibir e interpretar o resultado da portabilidade

Apenas as APIs que não são compatíveis com uma Plataforma de Destino são exibidas no relatório.
Após executar a análise no Visual Studio, você verá o link do arquivo do relatório de portabilidade do .NET aparecer. Se você usou o [aplicativo de console ApiPort](https://aka.ms/apiportdownload), o relatório de portabilidade do .NET é salvo como um arquivo no formato especificado. O padrão está em um arquivo Excel (*.xlsx*) em seu diretório atual.

#### <a name="portability-summary"></a>Resumo de Portabilidade

![Captura de tela do Resumo de portabilidade.](./media/portability-analyzer/api-catalog-portablility-summary.png)

A seção Resumo de Portabilidade do relatório mostra o percentual de portabilidade para cada assembly incluído na execução. No exemplo anterior, 71,24% das APIs do .NET Framework usadas no aplicativo `svcutil` estão disponíveis no .NET Core + Extensões de plataforma. Se você executar a ferramenta Analisador de Portabilidade do .NET em vários assemblies, cada um deles deverá ter uma linha no relatório Resumo de Portabilidade.

#### <a name="details"></a>Detalhes

![Captura de tela dos detalhes de portabilidade.](./media/portability-analyzer/api-catalog-portablility-details.png)

A seção de **detalhes** do relatório lista as APIs ausentes em qualquer uma das **plataformas de destino**selecionadas.

- Tipo de destino: o tipo tem uma API ausente de uma Plataforma de Destino
- Membro de destino: o método está ausente de uma Plataforma de Destino
- Nome do assembly: o assembly do .NET Framework no qual a API ausente reside.
- Cada uma das plataformas de destino selecionadas é uma coluna, como ".NET Core": o valor "sem suporte" significa que a API não tem suporte nesta plataforma de destino.
- Alterações recomendadas: a API ou a tecnologia recomendada para alterar. Atualmente, esse campo está vazio ou desatualizado para muitas APIs. Devido ao grande número de APIs, temos um desafio significativo para mantê-lo atualizado. Estamos examinando soluções alternativas para fornecer informações úteis aos clientes.

#### <a name="missing-assemblies"></a>Assemblies Ausentes

![Captura de tela de assemblies ausentes.](./media/portability-analyzer/api-catalog-missing-assemblies.png)

Você pode encontrar uma seção Assemblies Ausentes em seu relatório. Esta seção contém uma lista de assemblies que são referenciados por seus assemblies analisados e não foram analisados. Se for um assembly que você possui, inclua-o na execução do analisador de portabilidade de API para que você possa obter um relatório detalhado de portabilidade no nível de API para ele. Se for uma biblioteca de terceiros, verifique se há uma versão mais recente que dê suporte à sua plataforma de destino e considere mover para a versão mais recente. Eventualmente, a lista deve incluir todos os assemblies de terceiros dos quais seu aplicativo depende que tenham uma versão que dê suporte à sua plataforma de destino.

Para obter mais informações sobre o Analisador de Portabilidade do .NET, acesse a [documentação do GitHub](https://github.com/Microsoft/dotnet-apiport#documentation) e assista ao vídeo [A Brief Look at the .NET Portability Analyzer](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer) (Uma análise breve do Analisador de Portabilidade do .NET) do Channel 9.
