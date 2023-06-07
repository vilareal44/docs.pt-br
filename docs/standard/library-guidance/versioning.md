---
title: Bibliotecas de controle de versão e .NET
description: Recomendações de melhores práticas para controle de versão de bibliotecas do .NET.
ms.date: 12/10/2018
ms.openlocfilehash: ab15d56e40abedd842b681496b9e5ee737c8b1cd
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290117"
---
# <a name="versioning"></a>Controle de versão

Uma biblioteca de software raramente está completa na versão 1.0. As boas bibliotecas evoluem com o passar do tempo, adicionando recursos, corrigindo bugs e melhorando o desempenho. É importante que você possa liberar novas versões de uma biblioteca do .NET, fornecendo valor adicional a cada versão, sem interromper os usuários existentes.

## <a name="breaking-changes"></a>Alterações da falha

Para obter informações sobre como lidar com alterações da falha entre as versões, veja [Alterações da falha](./breaking-changes.md).

## <a name="version-numbers"></a>Números de versão

Uma biblioteca .NET tem várias maneiras de especificar uma versão. Essas versões são as mais importantes:

### <a name="nuget-package-version"></a>Versão do pacote NuGet

A [Versão do pacote NuGet](/nuget/reference/package-versioning) é exibida em NuGet.org, o gerenciador de pacotes do NuGet do Visual Studio e adicionada ao código-fonte quando o pacote é usado. A versão do pacote NuGet é o número de versão que os usuários normalmente verão e ao qual eles farão referência ao falarem sobre a versão de uma biblioteca que está sendo usada. A versão do pacote NuGet é usada pelo NuGet e não tem nenhum efeito sobre o comportamento de runtime.

```xml
<PackageVersion>1.0.0-alpha1</PackageVersion>
```

O identificador de pacote do NuGet combinado com a versão do pacote NuGet é usado para identificar um pacote no NuGet. Por exemplo, `Newtonsoft.Json` + `11.0.2`. Um pacote com um sufixo é um pacote de pré-lançamento e tem um comportamento especial que o torna ideal para teste. Para obter mais informações, veja [Pacotes pré-lançamento](./nuget.md#pre-release-packages).

Como a versão do pacote NuGet é a versão mais visível para os desenvolvedores, é uma boa ideia atualizá-la usando [SemVer (Controle de Versão de Semântica)](https://semver.org/). SemVer indica a importância das alterações entre as versões e ajuda os desenvolvedores a tomarem uma decisão bem informada ao escolherem qual versão usar. Por exemplo, ir de `1.0` para `2.0` indica que potencialmente há alterações significativas.

✔️ CONSIDERAR o uso do [SemVer 2.0.0](https://semver.org/) para criar a versão do seu pacote NuGet.

✔️ FAÇA uso da versão do pacote NuGet na documentação pública, uma vez que é o número de versão que os usuários verão normalmente.

✔️ FAÇA a inclusão de um sufixo de pré-lançamento ao liberar um pacote não estável.

> Os usuários devem aceitar receberem pacotes de pré-lançamento, assim, eles entenderão que o pacote não está concluído.

### <a name="assembly-version"></a>Versão do assembly

A versão do assembly é o que o CLR usa em tempo de execução para selecionar qual versão de um assembly carregar. Selecionar um assembly usando o controle de versão só se aplica aos assemblies com um nome forte.

```xml
<AssemblyVersion>1.0.0.0</AssemblyVersion>
```

O .NET Framework CLR exige uma correspondência exata para carregar um assembly de nome forte. Por exemplo, `Libary1, Version=1.0.0.0` foi compilado com uma referência ao `Newtonsoft.Json, Version=11.0.0.0`. .NET Framework carregará apenas essa versão exata `11.0.0.0` . Para carregar uma versão diferente em tempo de execução, um redirecionamento de associação deve ser adicionado ao arquivo de configuração do aplicativo .NET.

Nomenclatura forte combinada com a versão do assembly habilita [carregamento de versão do assembly estrita](../assembly/versioning.md). Embora o nome forte de uma biblioteca tenha vários benefícios, ele geralmente resulta em exceções de tempo de execução que um assembly não pode encontrar e [requer redirecionamentos de associação](../../framework/configure-apps/redirect-assembly-versions.md) no `app.config` ou `web.config` para ser corrigido. No .NET Core, o carregamento do assembly é mais relaxado. O tempo de execução do .NET Core carrega automaticamente os assemblies com uma versão superior em tempo de execução.

✔️ CONSIDERE incluir apenas uma versão principal em AssemblyVersion.

> Por exemplo, Biblioteca 1.0 e Biblioteca 1.0.1 têm ambas uma AssemblyVersion de `1.0.0.0`, enquanto a Biblioteca 2.0 tem uma AssemblyVersion de `2.0.0.0`. Quando a versão do assembly é alterada com menos frequência, ela reduz os redirecionamentos de associação.

✔️ CONSIDERE manter o número de versão principal do AssemblyVersion e a versão do pacote NuGet em sincronia.

> A versão do AssemblyVersion é incluída em algumas mensagens informativas exibidas ao usuário, por exemplo, o nome do assembly e os nomes de tipo qualificados do assembly em mensagens de exceção. Manter uma relação entre as versões fornece mais informações para desenvolvedores sobre qual versão eles estão usando.

❌Não tem um AssemblyVersion fixo.

> Embora um AssemblyVersion inalterável evite a necessidade de redirecionamentos de associação, isso significa que apenas uma única versão do assembly pode ser instalada no GAC (Cache de Assembly Global). Além disso, os aplicativos que fazem referência ao assembly no GAC serão interrompidos se outro aplicativo atualizar o assembly do GAC com alterações significativas.

### <a name="assembly-file-version"></a>Versão do arquivo do assembly

A versão do arquivo de assembly é usada para exibir uma versão de arquivo no Windows e não tem nenhum efeito sobre o comportamento de tempo de execução. Configurar esta versão é opcional. Ele ficará visível na caixa de diálogo Propriedades do Arquivo no Windows Explorer:

```xml
<FileVersion>11.0.2.21924</FileVersion>
```

![Windows Explorer](./media/versioning/win-properties.png "Windows Explorer")

✔️ CONSIDERE incluir um número de build de integração contínua como a revisão AssemblyFileVersion.

> Por exemplo, você está criando a versão 1.0.0 do seu projeto e o número de build de integração contínua é 99, portanto, sua AssemblyFileVersion é 1.0.0.99.

✔️ Use o formato `Major.Minor.Build.Revision` para a versão de arquivo.

> Embora a versão de arquivo nunca seja usada pelo .NET, o [Windows espera que a versão de arquivo](/windows/desktop/menurc/versioninfo-resource) esteja no formato `Major.Minor.Build.Revision`. Um aviso é acionado se a versão não segue esse formato.

### <a name="assembly-informational-version"></a>Versão informativa do assembly

A versão informativa do assembly é usada para registrar informações adicionais de versão e não tem nenhum efeito sobre o comportamento de runtime. Configurar esta versão é opcional. Se você estiver usando o Source Link, essa versão será definida no build com a versão do pacote NuGet, além de uma versão de controle do código-fonte. Por exemplo, `1.0.0-beta1+204ff0a` inclui o hash de confirmação do código-fonte do qual o assembly foi criado. Para obter mais informações, confira [Source Link](./sourcelink.md).

```xml
<AssemblyInformationalVersion>The quick brown fox jumped over the lazy dog.</AssemblyInformationalVersion>
```

> [!NOTE]
> As versões mais antigas do Visual Studio acionam um aviso de build caso essa versão não siga o formato `Major.Minor.Build.Revision`. O aviso pode ser ignorado com segurança.

❌Evite definir a versão informativa do assembly por conta própria.

> Permita a SourceLink gerar automaticamente a versão que contém metadados de controle do código-fonte e NuGet.

>[!div class="step-by-step"]
>[Anterior](publish-nuget-package.md) 
> [Avançar](breaking-changes.md)
