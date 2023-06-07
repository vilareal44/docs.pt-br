---
title: Catálogo de RID (identificador de tempo de execução) do .NET Core
description: Saiba mais sobre o RID (Identificador de runtime) e como os RIDs são usados no .NET Core.
ms.date: 02/22/2019
ms.openlocfilehash: 903dd9c619008c9e3c6149a471ba814bdc9c97cc
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84903279"
---
# <a name="net-core-rid-catalog"></a>Catálogo de RIDs do .NET Core

RID é curto para o *identificador de tempo de execução*. Os valores do RID são usados para identificar plataformas de destino onde o aplicativo é executado.
Eles são usados por pacotes .NET para representar ativos específicos de plataforma em pacotes NuGet. Os seguintes valores são exemplos de RIDs: `linux-x64`, `ubuntu.14.04-x64`, `win7-x64` ou `osx.10.12-x64`.
Para os pacotes com dependências nativas, o RID designará as plataformas em que o pacote pode ser restaurado.

Um único RID pode ser definido no elemento `<RuntimeIdentifier>` do arquivo de projeto. Vários RIDs podem ser definidos como uma lista separada por ponto-e-vírgula no elemento `<RuntimeIdentifiers>` do arquivo de projeto. Eles também são usados por meio da opção `--runtime` com os seguintes [comandos da CLI do .NET Core](./tools/index.md):

- [dotnet build](./tools/dotnet-build.md)
- [dotnet clean](./tools/dotnet-clean.md)
- [dotnet pack](./tools/dotnet-pack.md)
- [dotnet publish](./tools/dotnet-publish.md)
- [dotnet restore](./tools/dotnet-restore.md)
- [dotnet run](./tools/dotnet-run.md)
- [dotnet store](./tools/dotnet-store.md)

RIDs que representem sistemas operacionais concretos geralmente seguem este padrão: `[os].[version]-[architecture]-[additional qualifiers]` em que:

- `[os]` é o moniker do sistema operacional/plataforma. Por exemplo, `ubuntu`.

- `[version]` é a versão do sistema operacional na forma de um separado de versão separado por ponto (`.`). Por exemplo, `15.10`.

  - A versão **não deve** ser uma versão de marketing, pois essas geralmente representam várias versões distintas do sistema operacional com diferentes áreas de superfície de API da plataforma.

- `[architecture]` é a arquitetura do processador. Por exemplo: `x86`, `x64`, `arm` ou `arm64`.

- `[additional qualifiers]` distingue diferentes plataformas. Por exemplo: `aot`.

## <a name="rid-graph"></a>Gráfico RID

O gráfico RID ou gráfico de fallback de runtime é uma lista de RIDs que são compatíveis entre si. Os RIDs são definidos no pacote [Microsoft.NETCore.Platforms](https://www.nuget.org/packages/Microsoft.NETCore.Platforms/). Você pode ver a lista de RIDs com suporte e o grafo RID na [*runtime.jsno*](https://github.com/dotnet/runtime/blob/master/src/libraries/pkg/Microsoft.NETCore.Platforms/runtime.json) arquivo, que está localizado no `dotnet/runtime` repositório. Nesse arquivo, você pode ver todos os RIDs, exceto para a base um, que contém uma instrução `"#import"`. Essas instruções indicam RIDs compatíveis.

Quando o NuGet restaura pacotes, ele tenta encontrar uma correspondência exata para o runtime especificado.
Se uma correspondência exata não for encontrada, o NuGet voltará ao gráfico até encontrar o sistema compatível mais próximo de acordo com o gráfico RID.

O exemplo a seguir é a entrada real para o RID `osx.10.12-x64`:

```json
"osx.10.12-x64": {
    "#import": [ "osx.10.12", "osx.10.11-x64" ]
}
```

O RID acima especifica que `osx.10.12-x64` importa `osx.10.11-x64`. Desse modo, quando o NuGet restaura pacotes, ele tenta encontrar uma correspondência exata para `osx.10.12-x64` no pacote. Se o NuGet não puder encontrar o runtime específico, ele poderá restaurar pacotes que especificam runtimes `osx.10.11-x64`, por exemplo.

O seguinte exemplo mostra um gráfico RID ligeiramente maior, também definido no arquivo *runtime.json*:

```
    win7-x64    win7-x86
       |   \   /    |
       |   win7     |
       |     |      |
    win-x64  |  win-x86
          \  |  /
            win
             |
            any
```

Todos os RIDs eventualmente mapeiam para a raiz de `any` RID.

Há algumas considerações sobre RIDs das quais você precisa se lembrar ao trabalhar com eles:

- Os RIDs são **cadeias de caracteres opacas** e devem ser tratados como caixas pretas.
- Não crie RIDs de modo programático.
- Use RIDs que já estão definidos para a plataforma.
- Os RIDs precisam ser específicos, portanto, não presuma nada usando o valor RID real.

## <a name="using-rids"></a>Usando RIDs

Para poder usar RIDs, você precisa saber quais RIDs existem. Novos RIDs são adicionados regularmente à plataforma.
Para obter a versão mais recente e completa, consulte o [runtime.jsem](https://github.com/dotnet/runtime/blob/master/src/libraries/pkg/Microsoft.NETCore.Platforms/runtime.json) arquivo no `dotnet/runtime` repositório.

O SDK do .NET Core 2.0 apresenta o conceito de RIDs portáteis. Eles são novos valores adicionados ao gráfico RID que não estão associados a uma versão específica ou distribuição de SO e são a opção preferida ao usar o .NET Core 2.0 e posterior. Eles são particularmente úteis ao lidar com várias distribuições Linux, já que a maioria dos RIDs de distribuição são mapeados para os RIDs portáteis.

A lista a seguir mostra um pequeno subconjunto dos RIDs mais comuns usados para cada SO.

## <a name="windows-rids"></a>RIDs do Windows

Apenas os valores comuns são listados. Para obter a versão mais recente e completa, consulte o [runtime.jsem](https://github.com/dotnet/runtime/blob/master/src/libraries/pkg/Microsoft.NETCore.Platforms/runtime.json) arquivo no `dotnet/runtime` repositório.

- Portátil (.NET Core 2.0 ou versões posteriores)
  - `win-x64`
  - `win-x86`
  - `win-arm`
  - `win-arm64`
- Windows 7 / Windows Server 2008 R2
  - `win7-x64`
  - `win7-x86`
- Windows 8.1 / Windows Server 2012 R2
  - `win81-x64`
  - `win81-x86`
  - `win81-arm`
- Windows 10 / Windows Server 2016
  - `win10-x64`
  - `win10-x86`
  - `win10-arm`
  - `win10-arm64`

Para obter mais informações, consulte [dependências e requisitos do .NET Core](install/dependencies.md?pivots=os-windows).

## <a name="linux-rids"></a>RIDs do Linux

Apenas os valores comuns são listados. Para obter a versão mais recente e completa, consulte o [runtime.jsem](https://github.com/dotnet/runtime/blob/master/src/libraries/pkg/Microsoft.NETCore.Platforms/runtime.json) arquivo no `dotnet/runtime` repositório. Os dispositivos que executam uma distribuição não listada abaixo podem funcionar com um dos RIDs Portáteis. Por exemplo, os dispositivos Raspberry Pi executando uma distribuição Linux não listada podem ser direcionados com `linux-arm`.

- Portátil (.NET Core 2.0 ou versões posteriores)
  - `linux-x64`(A maioria das distribuições de área de trabalho como CentOS, Debian, Fedora, Ubuntu e derivativos)
  - `linux-musl-x64` (Distribuições leves usando [musl](https://wiki.musl-libc.org/projects-using-musl.html), como o Alpine Linux)
  - `linux-arm`(Distribuições do Linux em execução no ARM, como Raspbian no Raspberry Pi Model 2 +)
  - `linux-arm64`(Distribuições do Linux em execução no ARM de 64 bits, como o Ubuntu Server 64-bit no modelo Raspberry Pi 3 +)
- Red Hat Enterprise Linux
  - `rhel-x64` (Substituído por `linux-x64` para RHEL acima da versão 6)
  - `rhel.6-x64` (.NET Core 2.0 ou versões posteriores)
- Tizen (.NET Core 2.0 ou versões posteriores)
  - `tizen`
  - `tizen.4.0.0`
  - `tizen.5.0.0`

Para obter mais informações, consulte [dependências e requisitos do .NET Core](install/dependencies.md?pivots=os-linux).

## <a name="macos-rids"></a>RIDs do macOS

Os RIDs do macOS usam a identidade visual “OSX” mais antiga. Apenas os valores comuns são listados. Para obter a versão mais recente e completa, consulte o [runtime.jsem](https://github.com/dotnet/runtime/blob/master/src/libraries/pkg/Microsoft.NETCore.Platforms/runtime.json) arquivo no `dotnet/runtime` repositório.

- Portátil (.NET Core 2.0 ou versões posteriores)
  - `osx-x64` (A versão mínima do sistema operacional é macOS 10.12 Sierra)
- macOS 10.10 Yosemite
  - `osx.10.10-x64`
- macOS 10.11 El Capitan
  - `osx.10.11-x64`
- macOS 10.12 Sierra (.NET Core 1.1 ou versões posteriores)
  - `osx.10.12-x64`
- macOS 10.13 High Sierra (.NET Core 1.1 ou versões posteriores)
  - `osx.10.13-x64`
- macOS 10.14 Mojave (.NET Core 1.1 ou versões posteriores)
  - `osx.10.14-x64`

Para obter mais informações, consulte [dependências e requisitos do .NET Core](install/dependencies.md?pivots=os-macos).

## <a name="see-also"></a>Confira também

- [IDs de Runtime](https://github.com/dotnet/runtime/blob/master/src/libraries/pkg/Microsoft.NETCore.Platforms/readme.md)
