---
title: Como controlar a versão do SDK e do Runtime do .NET Core
description: Este artigo ensina como controlar a versão do SDK do .NET Core (semelhante ao controle de versão semântico).
ms.date: 06/24/2020
ms.openlocfilehash: 5e315f49227f3c2ea40652a30fabbf566bdfe495
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619748"
---
# <a name="overview-of-how-net-core-is-versioned"></a>Visão geral de como o .NET Core tem a versão controlada

O .NET Core refere-se ao Runtime do .NET Core e ao SDK do .NET Core, que contém as ferramentas que necessárias para desenvolver aplicativos. Os SDKs do .NET Core são projetados para funcionar com qualquer versão anterior do Runtime do .NET Core. Este artigo explica o runtime e a estratégia de versão do SDK. Uma explicação de números de versão do .NET Standard pode ser encontrada no artigo que faz a introdução do [.NET Standard](../../standard/net-standard.md#net-implementation-support).

O Runtime do .NET Core e o SDK do .NET Core adicionam novos recursos em uma taxa diferente – em geral, o SDK do .NET Core fornece ferramentas atualizadas mais rapidamente do que o Runtime do .NET Core altera o runtime que você usa em produção.

## <a name="versioning-details"></a>Detalhes de controle de versão

O ".NET Core 2.1" refere-se ao número de versão do Runtime do .NET Core. O Runtime do .NET Core tem uma abordagem principal/secundária/de patch para o controle de versão que segue o [controle de versão semântico](#semantic-versioning).

O SDK do .NET Core não segue o controle de versão semântico. O SDK do .NET Core libera mais rapidamente e suas versões devem comunicar tanto o runtime alinhado quanto as versões de patch e a própria versão secundária do SDK. As duas primeiras posições da versão do SDK do .NET Core estão bloqueadas no Runtime do .NET Core com que foi lançado. Cada versão do SDK pode criar aplicativos para esse runtime ou qualquer versão inferior.

A terceira posição do número de versão do SDK comunica o número de patch e da versão secundária. A versão secundária é multiplicada por 100. Versão secundária 1, versão de patch 2 seria representada como 102. Os últimos dois dígitos representam o número de patch. Por exemplo, a versão do .NET Core 2.2 pode criar versões, como a tabela a seguir:

| Alteração                | Runtime do .NET Core | SDK do .NET Core ( \* ) |
|-----------------------|-------------------|-------------------|
| Versão inicial       | 2.2.0             | 2.2.100           |
| Patch do SDK             | 2.2.0             | 2.2.101           |
| Runtime e Patch do SDK | 2.2.1             | 2.2.102           |
| Alteração de Recurso do SDK    | 2.2.1             | 2.2.200           |

( \* ) Este gráfico usa o tempo de execução do .net core 2,2 como o exemplo porque um artefato histórico significava que o primeiro SDK para .NET Core 2,1 é 2.1.300. Para obter mais informações, veja a [seleção de versão do .NET Core](selection.md).

OBSERVAÇÕES:

- Se o SDK tiver 10 atualizações de recurso antes de uma atualização de recurso de runtime, os números de versão serão passados na série 1000 com números como 2.2.1000 de acordo com a versão do recurso 2.2.900 a seguir. Essa situação não deve ocorrer.
- As versões de patch 99 sem uma versão do recurso não ocorrerão. Se uma versão se aproximar desse número, ela forçará uma versão do recurso.

Você poderá ver mais detalhes na proposta inicial no repositório [dotnet/designs](https://github.com/dotnet/designs/pull/29).

## <a name="semantic-versioning"></a>Controle de versão semântico

O *Runtime* do .NET Core se adere ao [SemVer (Controle de Versão Semântico)](https://semver.org/), adotando o uso do controle de versão do `MAJOR.MINOR.PATCH`, usando as várias partes do número de versão para descrever o grau e o tipo de alteração.

```
MAJOR.MINOR.PATCH[-PRERELEASE-BUILDNUMBER]
```

As partes `PRERELEASE` e `BUILDNUMBER` opcionais nunca farão parte das versões compatíveis e existem apenas em builds noturnos, compilados localmente de destinos de origem e versões prévias sem suporte.

### <a name="understand-runtime-version-number-changes"></a>Entender as alterações do número de versão do runtime

`MAJOR` é incrementado quando:

- Ocorrem alterações significativas no produto ou uma nova direção de produto.
- Alterações da falha foram identificadas. Há um alto padrão para aceitar alterações da falha.
- Não há mais suporte para uma versão mais antiga.
- Uma versão `MAJOR` mais nova de uma dependência existente é adotada.

`MINOR` é incrementado quando:

- Uma área de superfície de API pública é adicionada.
- Um novo comportamento é adicionado.
- Uma versão `MINOR` mais nova de uma dependência existente é adotada.
- Uma nova dependência é introduzida.

`PATCH` é incrementado quando:

- Correções de bug são feitas.
- O suporte para uma plataforma mais recente é adicionado.
- Uma versão `PATCH` mais nova de uma dependência existente é adotada.
- Qualquer outra alteração que não se enquadre em um dos casos anteriores.

Quando há várias alterações, o elemento mais alto afetado por alterações individuais é incrementado e os seguintes são redefinidos como zero. Por exemplo, quando `MAJOR` é incrementado, `MINOR` e `PATCH` são redefinidos como zero. Quando `MINOR` é incrementado, `PATCH` é redefinido como zero enquanto `MAJOR` permanece inalterado.

## <a name="version-numbers-in-file-names"></a>Números de versão nos nomes de arquivo

Os arquivos baixados para .NET Core têm a versão, por exemplo, `dotnet-sdk-2.1.300-win10-x64.exe`.

### <a name="preview-versions"></a>Versões prévias

As versões prévias têm um `-preview[number]-([build]|"final")` anexado à versão. Por exemplo, `2.0.0-preview1-final`.

### <a name="servicing-versions"></a>Versões de manutenção

Depois que uma versão sai, os branches de versão geralmente param de produzir builds diários e, em vez disso, iniciam a produção de builds de manutenção. As versões de manutenção têm um `-servicing-[number]` anexado à versão. Por exemplo, `2.0.1-servicing-006924`.

## <a name="relationship-to-net-standard-versions"></a>Relacionamento com versões do .NET Standard

O .NET standard consiste em um assembly de referência do .NET. Há várias implementações específicas para cada plataforma. O assembly de referência contém a definição das APIs do .NET que fazem parte de uma determinada versão do .NET Standard. Cada implementação atende o contrato do .NET Standard na plataforma específica. Você pode aprender mais sobre o .NET Standard no artigo sobre [.NET Standard](../../standard/net-standard.md) no Guia do .NET.

O assembly de referência do .NET Standard usa um esquema de controle de versão `MAJOR.MINOR`. O nível `PATCH` não é útil para o .NET Standard porque expõe apenas uma especificação de API (nenhuma implementação) e por definição, qualquer alteração à API representaria uma alteração no conjunto de recursos e, portanto, uma nova versão `MINOR`.

As implementações em cada plataforma podem ser atualizadas, normalmente como parte da versão de plataforma e isso, portanto, não é evidente para os programadores que usam o .NET Standard nessa plataforma.

Cada versão do .NET Core implementa uma versão do .NET Standard. Implementar uma versão do .NET Standard implica o suporte para as versões anteriores do .NET Standard. Versão independente do .NET Standard e do .NET Core. É uma coincidência que o .NET Core 2.0 implemente o .NET Standard 2.0. O .NET Core 2.1 também implementa o .NET Standard 2.0. O .NET Core dará suporte a versões futuras do .NET Standard conforme se tornam disponíveis.

| .NET Core | .NET Standard |
|-----------|---------------|
| 1.0       | até 1.6     |
| 2,0       | até 2.0     |
| 2.1       | até 2.0     |
| 2.2       | até 2.0     |
| 3.0       | até 2,1     |
| 3.1       | até 2,1     |

Para uma tabela interativa das versões do .NET Standard e como elas correspondem às implementações do .NET, consulte [.net Standard versões](https://dotnet.microsoft.com/platform/dotnet-standard#versions).

## <a name="see-also"></a>Consulte também

- [Estruturas de destino](../../standard/frameworks.md)
- [Pacote de distribuição do .NET Core](../distribution-packaging.md)
- [Folha informativa sobre o ciclo de vida do suporte ao .NET Core](https://dotnet.microsoft.com/platform/support/policy)
- [.NET Core 2+ Associação de versão](https://github.com/dotnet/designs/issues/3)
- [Imagens do Docker para .NET Core](https://hub.docker.com/_/microsoft-dotnet-core/)
