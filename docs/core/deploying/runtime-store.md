---
title: Repositório de pacotes de runtime
description: Saiba como usar o repositório de pacotes de runtime para direcionar a manifestos usados pelo .NET Core.
ms.date: 08/12/2017
ms.openlocfilehash: e9e27ef535dbd9e7197c323f7e49a9960aeff0f9
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608351"
---
# <a name="runtime-package-store"></a>Repositório de pacotes de runtime

A partir do .NET Core 2.0, é possível empacotar e implantar aplicativos com relação a um conjunto conhecido de pacotes que existem no ambiente de destino. Os benefícios são implantações mais rápidas, menor uso de espaço em disco e desempenho aprimorado de inicialização em alguns casos.

Esse recurso é implementado como um *repositório de pacotes de runtime*, que é um diretório em disco no qual os pacotes são armazenados (normalmente em */usr/local/share/dotnet/store* em macOS/Linux e *C:/Arquivos de Programas/dotnet/store* no Windows). Nesse diretório, há subdiretórios para arquiteturas e [estruturas de destino](../../standard/frameworks.md). O layout do arquivo é semelhante à maneira como os [ativos do NuGet são dispostos no disco](/nuget/create-packages/supporting-multiple-target-frameworks#framework-version-folder-structure):

```
\dotnet
    \store
        \x64
            \netcoreapp2.0
                \microsoft.applicationinsights
                \microsoft.aspnetcore
                ...
        \x86
            \netcoreapp2.0
                \microsoft.applicationinsights
                \microsoft.aspnetcore
                ...
```

Um arquivo de *manifesto de destino* lista os pacotes no repositório de pacotes de runtime. Os desenvolvedores podem direcionar esse manifesto ao publicar seu aplicativo. Normalmente, o manifesto de destino é fornecido pelo proprietário do ambiente de produção direcionado.

## <a name="preparing-a-runtime-environment"></a>Preparação de um ambiente de runtime

O administrador de um ambiente de runtime pode otimizar aplicativos para oferecer implantações mais rápidas e menor uso do espaço em disco criando um repositório de pacotes de runtime e o manifesto de destino correspondente.

A primeira etapa é criar um *manifesto de repositório de pacotes* que lista os pacotes que compõem o repositório de pacotes de runtime. Esse formato de arquivo é compatível com o formato de arquivo de projeto (*csproj*).

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <ItemGroup>
    <PackageReference Include="NUGET_PACKAGE" Version="VERSION" />
    <!-- Include additional packages here -->
  </ItemGroup>
</Project>
```

**Exemplo**

O exemplo do manifesto do repositório de pacotes a seguir (*packages.csproj*) é usado para adicionar [`Newtonsoft.Json`](https://www.nuget.org/packages/Newtonsoft.Json/) e [`Moq`](https://www.nuget.org/packages/moq/) a um repositório de pacotes de runtime:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <ItemGroup>
    <PackageReference Include="Newtonsoft.Json" Version="10.0.3" />
    <PackageReference Include="Moq" Version="4.7.63" />
  </ItemGroup>
</Project>
```

Provisione o repositório de pacotes de runtime executando `dotnet store` com o manifesto, runtime e estrutura do repositório de pacotes:

```dotnetcli
dotnet store --manifest <PATH_TO_MANIFEST_FILE> --runtime <RUNTIME_IDENTIFIER> --framework <FRAMEWORK>
```

**Exemplo**

```dotnetcli
dotnet store --manifest packages.csproj --runtime win10-x64 --framework netcoreapp2.0 --framework-version 2.0.0
```

Você pode passar vários caminhos de manifesto do repositório de pacotes de destino para um único [`dotnet store`](../tools/dotnet-store.md) comando repetindo a opção e o caminho no comando.

Por padrão, a saída do comando é um repositório de pacotes no subdiretório *.dotnet/store* do perfil do usuário. É possível especificar um local diferente usando a opção `--output <OUTPUT_DIRECTORY>`. O diretório raiz do repositório contém um arquivo de manifesto de destino *artifact.xml*. Esse arquivo pode ser disponibilizado para download e ser usado por autores de aplicativos que desejam direcionar esse repositório durante a publicação.

**Exemplo**

O arquivo *artifact.xml* a seguir é produzido depois de executar o exemplo anterior. Observe que [`Castle.Core`](https://www.nuget.org/packages/Castle.Core/) é uma dependência de `Moq` , portanto, é incluída automaticamente e aparece no arquivo de manifesto *artifacts.xml* .

```xml
<StoreArtifacts>
  <Package Id="Newtonsoft.Json" Version="10.0.3" />
  <Package Id="Castle.Core" Version="4.1.0" />
  <Package Id="Moq" Version="4.7.63" />
</StoreArtifacts>
```

## <a name="publishing-an-app-against-a-target-manifest"></a>Publicação de um aplicativo em um manifesto de destino

Se você tiver um arquivo de manifesto de destino em disco, especifique o caminho para o arquivo ao publicar seu aplicativo com o [`dotnet publish`](../tools/dotnet-publish.md) comando:

```dotnetcli
dotnet publish --manifest <PATH_TO_MANIFEST_FILE>
```

**Exemplo**

```dotnetcli
dotnet publish --manifest manifest.xml
```

Implante o aplicativo publicado resultante em um ambiente que tem os pacotes descritos no manifesto de destino. Se isso não for feito, o aplicativo falhará ao iniciar.

Especifique vários manifestos de destino ao publicar um aplicativo, repetindo a opção e o caminho (por exemplo, `--manifest manifest1.xml --manifest manifest2.xml`). Quando você fizer isso, o aplicativo será cortado para a união de pacotes especificada nos arquivos de manifesto de destino fornecidos para o comando.

Se você implantar um aplicativo com uma dependência de manifesto presente na implantação (o assembly está presente na pasta *bin*), o repositório de pacotes de runtime *não será usado* no host desse assembly. O assembly da pasta *bin* é usado, independentemente de sua presença no repositório de pacotes de runtime no host.

A versão da dependência indicada no manifesto deve corresponder à versão da dependência no repositório de pacotes de runtime. Se você tiver uma incompatibilidade de versão entre a dependência no manifesto de destino e a versão que existe no repositório de pacotes de runtime e o aplicativo não incluir a versão necessária do pacote em sua implantação, a inicialização do aplicativo falhará. A exceção inclui o nome do manifesto de destino chamado que chamou o assembly do repositório de pacotes de runtime, que ajuda você a solucionar problemas de incompatibilidade.

Quando a implantação é *cortada* na publicação, somente as versões específicas dos pacotes de manifesto indicadas são retidas na saída publicada. Os pacotes nas versões indicadas devem estar presentes no host do aplicativo a ser iniciado.

## <a name="specifying-target-manifests-in-the-project-file"></a>Especificação de manifestos de destino no arquivo de projeto

Uma alternativa para especificar manifestos de destino com o [`dotnet publish`](../tools/dotnet-publish.md) comando é especificá-los no arquivo de projeto como uma lista de caminhos separados por ponto e vírgula em uma **\<TargetManifestFiles>** marca.

```xml
<PropertyGroup>
  <TargetManifestFiles>manifest1.xml;manifest2.xml</TargetManifestFiles>
</PropertyGroup>
```

Especifique os manifestos de destino no arquivo de projeto somente quando o ambiente de destino para o aplicativo for bem conhecido, como para projetos .NET Core. Esse não é o caso para projetos de software livre. Normalmente, os usuários de um projeto de software livre o implantam em diferentes ambientes de produção. Geralmente, esses ambientes de produção têm diferentes conjuntos de pacotes pré-instalados. Você não pode fazer suposições sobre o manifesto de destino em tais ambientes, portanto, você deve usar a `--manifest` opção de [`dotnet publish`](../tools/dotnet-publish.md) .

## <a name="aspnet-core-implicit-store-net-core-20-only"></a>ASP.NET Core repositório implícito (somente .NET Core 2,0)

O repositório implícito do ASP.NET Core é aplicável apenas ao ASP.NET Core 2.0. Recomendamos que os aplicativos usem o ASP.NET Core 2.1 e posterior, que **não** usa o repositório implícito. O ASP.NET Core 2.1 e posterior usam a estrutura compartilhada.

Para o .NET Core 2,0, o recurso de armazenamento de pacotes de tempo de execução é usado implicitamente por um aplicativo ASP.NET Core quando o aplicativo é implantado como um aplicativo de [implantação dependente de estrutura](index.md#publish-framework-dependent) . Os destinos no [`Microsoft.NET.Sdk.Web`](https://github.com/aspnet/websdk) incluem manifestos que fazem referência ao repositório de pacotes implícito no sistema de destino. Além disso, qualquer aplicativo dependente da estrutura que dependa do `Microsoft.AspNetCore.All` pacote resulta em um aplicativo publicado que contém apenas o aplicativo e seus ativos, e não os pacotes listados no `Microsoft.AspNetCore.All` metapacote. Pressupõe-se que esses pacotes estão presentes no sistema de destino.

O repositório de pacotes de runtime é instalado no host quando o SDK de .NET Core é instalado. Talvez outros instaladores forneçam o repositório de pacotes de runtime, incluindo instalações Zip/tarball do SDK do .NET Core, `apt-get`, Red Hat Yum, o pacote de hospedagem do Windows Server do .NET Core e instalações manuais de repositório de pacotes de runtime.

Ao implantar um aplicativo [de implantação dependente de estrutura](index.md#publish-framework-dependent) , verifique se o ambiente de destino tem o SDK do .NET Core instalado. Se o aplicativo for implantado em um ambiente que não inclui ASP.NET Core, você poderá recusar o repositório implícito especificando  **\<PublishWithAspNetCoreTargetManifest>** definir como `false` no arquivo de projeto, como no exemplo a seguir:

```xml
<PropertyGroup>
  <PublishWithAspNetCoreTargetManifest>false</PublishWithAspNetCoreTargetManifest>
</PropertyGroup>
```

> [!NOTE]
> Para aplicativos de [implantação independentes](index.md#publish-self-contained) , supõe-se que o sistema de destino não contenha necessariamente os pacotes de manifesto necessários. Portanto, **\<PublishWithAspNetCoreTargetManifest>** não pode ser definido como `true` para um aplicativo independente.

## <a name="see-also"></a>Confira também

- [dotnet-publish](../tools/dotnet-publish.md)
- [dotnet-store](../tools/dotnet-store.md)
