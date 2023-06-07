---
title: Selecionar qual versão do .NET Core usar
description: Saiba como o .NET Core localiza e escolhe automaticamente versões de runtime para o seu programa. Além disso, este artigo ensina como forçar uma versão específica.
author: adegeo
ms.author: adegeo
ms.date: 03/24/2020
ms.openlocfilehash: 82b5522601b0ed5d3f4faf6e6c6c970ba285b11f
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608194"
---
# <a name="select-the-net-core-version-to-use"></a>Selecionar a versão do .NET Core a ser usada

Este artigo explica as políticas usadas pelas ferramentas do .NET Core, pelo SDK e pelo runtime para a seleção de versões. Essas políticas fornecem um equilíbrio entre a execução de aplicativos usando as versões especificadas e a possibilidade de fácil atualização dos computadores de desenvolvedores e usuários finais. Essas políticas executam as seguintes ações:

- Implantação fácil e eficiente do .NET Core, incluindo atualizações de segurança e de confiabilidade.
- Uso das ferramentas e dos comandos mais recentes independentes do runtime de destino.

A seleção de versão ocorre:

- Quando você executa um comando do SDK, [o SDK usa a versão mais recente instalada](#the-sdk-uses-the-latest-installed-version).
- Quando você compila um assembly [os monikers da estrutura de destino definem as APIs do tempo de build](#target-framework-monikers-define-build-time-apis).
- Quando você executa um aplicativo .NET Core, [os aplicativos dependentes da estrutura de destino efetuam roll forward](#framework-dependent-apps-roll-forward).
- Quando você publica um aplicativo autossuficiente, [as implantações autossuficientes incluem o runtime selecionado](#self-contained-deployments-include-the-selected-runtime).

O restante deste documento examina esses quatro cenários.

## <a name="the-sdk-uses-the-latest-installed-version"></a>O SDK usa a versão mais recente instalada

Os comandos do SDK incluem `dotnet new` e `dotnet run`. A CLI do .NET Core precisa escolher uma versão do SDK para todos os comandos `dotnet`. Por padrão, ela usa o SDK mais recente instalado no computador, mesmo se:

- O projeto se destinar a uma versão anterior do runtime do .NET Core.
- A versão mais recente do SDK do .NET Core for uma versão prévia.

Você pode aproveitar os recursos e as melhorias mais recentes do SDK mesmo ao direcionar a versões anteriores de runtime do .NET Core. Você pode direcionar a várias versões de runtime do .NET Core em projetos diferentes, usando as mesmas ferramentas do SDK para todos os projetos.

Em raras ocasiões, talvez você precise usar uma versão anterior do SDK. Nesse caso, especifique essa versão em um [arquivo *global. JSON*](../tools/global-json.md). A política "usar versão mais recente" significa usar o *global.json* somente para especificar uma versão do SDK do .NET Core anterior à versão mais recente instalada.

O *global.json* pode ser colocado em qualquer lugar na hierarquia de arquivos. A CLI procura no diretório do projeto em diante até encontrar o primeiro *global.json*. Você controla a quais projetos um determinado *global.json* se aplica a pelo seu lugar no sistema de arquivos. A CLI do .NET procura um arquivo *global.json* navegando iterativamente do caminho do diretório de trabalho atual em diante. O primeiro arquivo *global.json* encontrado especifica a versão usada. Se essa versão do SDK estiver instalada, essa versão será usada. Se o SDK especificado no *global.jsem* não for encontrado, a CLI do .NET usará [regras de correspondência](../tools/global-json.md#matching-rules) para selecionar um SDK compatível ou falhará se nenhum for encontrado.

O exemplo a seguir mostra a sintaxe *global.json*:

``` json
{
  "sdk": {
    "version": "3.0.0"
  }
}
```

O processo para selecionar uma versão do SDK é:

1. O `dotnet` procura um arquivo *global.json* navegando inversamente de forma iterativa no caminho do diretório de trabalho atual em diante.
1. O `dotnet` usa o SDK especificado no primeiro *global.json* encontrado.
1. O `dotnet` usará a versão mais recente do SDK instalada se nenhum *global.json* for encontrado.

Saiba mais sobre como selecionar uma versão do SDK na seção [Regras de correspondência](../tools/global-json.md#matching-rules) do artigo sobre o *global.json*.

## <a name="target-framework-monikers-define-build-time-apis"></a>Os Monikers da Estrutura de Destino definem as APIs de tempo de build

Você compila seu projeto em relação às APIs definidas em um **TFM** (Moniker da Estrutura de Destino). Você especifica a [estrutura de destino](../../standard/frameworks.md) no arquivo de projeto. Defina o elemento `TargetFramework` no arquivo de projeto, conforme mostrado no exemplo a seguir:

``` xml
<TargetFramework>netcoreapp3.0</TargetFramework>
```

Você pode compilar o projeto em relação a várias TFMs. A definição de várias estruturas de destino é mais comum em bibliotecas, mas também pode ocorrer com aplicativos. Especifique uma propriedade `TargetFrameworks` (plural de `TargetFramework`). As estruturas de destino são delimitadas por ponto e vírgula, conforme mostrado no exemplo a seguir:

``` xml
<TargetFrameworks>netcoreapp3.0;net47</TargetFrameworks>
```

Um determinado SDK dá suporte a um conjunto fixo de estruturas, limitado à estrutura de destino do runtime com o qual é fornecido. Por exemplo, o SDK do .NET Core 3,0 inclui o tempo de execução do .NET Core 3,0, que é uma implementação da `netcoreapp3.0` estrutura de destino. O SDK do .NET Core 3,0 dá suporte a `netcoreapp2.1` ,, `netcoreapp2.2` `netcoreapp3.0` , mas não `netcoreapp3.1` (ou superior). Você instala o SDK do .NET Core 3,1 para compilar para o `netcoreapp3.1` .

As estruturas de destino do .NET Standard também são limitadas à estrutura de destino do runtime com o qual o SDK é fornecido. O SDK do .NET Core 3,1 está limitado ao `netstandard2.1` . Para obter mais informações, confira [.NET Standard](../../standard/net-standard.md).

## <a name="framework-dependent-apps-roll-forward"></a>Roll foward de aplicativos dependentes da estrutura

Quando você executa um aplicativo de origem com [`dotnet run`](../tools/dotnet-run.md) , de uma [**implantação dependente de estrutura**](../deploying/index.md#publish-framework-dependent) com o [`dotnet myapp.dll`](../tools/dotnet.md#description) , ou de um [**executável dependente de estrutura**](../deploying/index.md#publish-framework-dependent) com `myapp.exe` , o `dotnet` executável é o **host** para o aplicativo.

O host escolhe a versão de patch mais recente instalada no computador. Por exemplo, se você especificar `netcoreapp3.0` em seu arquivo de projeto e `3.0.2` for o runtime mais recente do .NET instalado, o runtime `3.0.2` será usado.

Se nenhuma versão `3.0.*` aceitável for encontrada, uma nova versão `3.*` será usada. Por exemplo, se você especificar `netcoreapp3.0` e só o `3.1.0` estiver instalado, o aplicativo será executado usando o runtime `3.1.0`. Esse comportamento é conhecido como "roll forward de versão secundária". As versões inferiores também não serão consideradas. Quando nenhum runtime aceitável estiver instalado, o aplicativo não será executado.

Alguns exemplos de uso demonstram o comportamento, se você visar 3,0:

- ✔️ 3,0 está especificado. 3.0.3 é a versão de patch mais alta instalada. 3.0.3 é usado.
- ❌ 3,0 está especificado. Nenhuma versão 3,0. * está instalada. 2.1.1 é o tempo de execução mais alto instalado. Uma mensagem de erro é exibida.
- ✔️ 3,0 está especificado. Nenhuma versão 3,0. * está instalada. 3.1.0 é a versão de tempo de execução mais alta instalada. 3.1.0 é usado.
- ❌ 2,0 está especificado. Não há nenhuma versão 2.x instalada. 3.0.0 é o tempo de execução mais alto instalado. Uma mensagem de erro é exibida.

O roll forward de versão secundária tem um efeito colateral que pode afetar os usuários finais. Considere o cenário a seguir.

1. O aplicativo especifica que 3,0 é necessário.
2. Quando executado, a versão 3,0. * não está instalada, no entanto, 3.1.0 é. A versão 3.1.0 será usada.
3. Posteriormente, o usuário instala o 3.0.3 e executa o aplicativo novamente, 3.0.3 será usado agora.

É possível que 3.0.3 e 3.1.0 se comportem de forma diferente, especialmente em cenários como a serialização de dados binários.

## <a name="self-contained-deployments-include-the-selected-runtime"></a>As implantações autossuficientes incluem o runtime selecionado

É possível publicar um aplicativo como uma [**distribuição autossuficiente**](../deploying/index.md#publish-self-contained). Essa abordagem inclui o runtime e as bibliotecas do .NET Core com seu aplicativo. As implantações autossuficientes não são dependentes dos ambientes de runtime. A seleção da versão do runtime ocorre no momento da publicação, não no runtime.

O processo de publicação seleciona a versão de patch mais recente da família de determinado runtime. Por exemplo, `dotnet publish` selecionará o .NET Core 3.0.3 se for a versão de patch mais recente na família de tempo de execução do .NET Core 3,0. A estrutura de destino (incluindo os patches de segurança mais recentes instalados) é empacotada com o aplicativo.

É um erro quando a versão mínima especificada para um aplicativo não é atendida. O `dotnet publish` vincula-se à última versão de patch de runtime (em uma determinada família de versão principal.secundária). `dotnet publish` não dá suporte à semântica de roll forward de `dotnet run`. Para obter mais informações sobre patches e implantações autossuficientes, consulte o artigo sobre [seleção de patch de runtime](../deploying/runtime-patch-selection.md) na implantação de aplicativos .NET Core.

As implantações autossuficientes podem exigir uma versão de patch específica. Você pode substituir a versão de patch mínima do runtime (para versões superiores ou inferiores) no arquivo de projeto, conforme é mostrado no exemplo a seguir:

``` xml
<RuntimeFrameworkVersion>3.0.3</RuntimeFrameworkVersion>
```

O elemento `RuntimeFrameworkVersion` substitui a política de versão padrão. Para implantações autossuficientes, o `RuntimeFrameworkVersion` especifica a versão *exata* da estrutura do runtime. Para aplicativos dependentes da estrutura, o `RuntimeFrameworkVersion` especifica a versão *mínima* da estrutura de runtime necessária.

## <a name="see-also"></a>Confira também

- [Baixe e instale o .NET Core](../install/index.yml).
- [Como remover o SDK e o tempo de execução do .NET Core](../install/remove-runtime-sdk-versions.md).
