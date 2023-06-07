---
title: Gerenciar dependências no .NET Core
description: Explica como gerenciar dependências de projeto para um aplicativo .NET Core.
no-loc:
- dotnet add package
- dotnet remove package
- dotnet list package
ms.topic: how-to
ms.date: 02/25/2020
ms.openlocfilehash: 2aeedb56f774b51076764c2772eb02b2fa095d92
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88062854"
---
# <a name="manage-dependencies-in-net-core-applications"></a>Gerenciar dependências em aplicativos .NET Core

Este artigo explica como adicionar e remover dependências editando o arquivo de projeto ou usando a CLI.

## <a name="the-packagereference-element"></a>O \<PackageReference> elemento

O `<PackageReference>` elemento de arquivo de projeto tem a seguinte estrutura:

```xml
<PackageReference Include="PACKAGE_ID" Version="PACKAGE_VERSION" />
```

O `Include` atributo especifica a ID do pacote a ser adicionado ao projeto. O `Version` atributo especifica a versão a ser obtida. As versões são especificadas de acordo com [as regras de versão do NuGet](/nuget/create-packages/dependency-versions#version-ranges).

> [!NOTE]
> Se você não estiver familiarizado com a sintaxe Project-File, consulte a documentação de [referência do projeto MSBuild](/visualstudio/msbuild/msbuild-project-file-schema-reference) para obter mais informações.

Use condições para adicionar uma dependência que está disponível somente em um destino específico, conforme mostrado no exemplo a seguir:

```xml
<PackageReference Include="PACKAGE_ID" Version="PACKAGE_VERSION" Condition="'$(TargetFramework)' == 'netcoreapp2.1'" />
```

A dependência no exemplo anterior só será válida se a compilação estiver acontecendo para o destino fornecido. O `$(TargetFramework)` na condição é uma propriedade do MSBuild que está sendo definida no projeto. Para aplicativos .NET Core mais comuns, você não precisa fazer isso.

## <a name="add-a-dependency-by-editing-the-project-file"></a>Adicionar uma dependência editando o arquivo de projeto

Para adicionar uma dependência, adicione um `<PackageReference>` elemento dentro de um `<ItemGroup>` elemento. Você pode adicionar um existente `<ItemGroup>` ou criar um novo. O exemplo a seguir usa o projeto de aplicativo de console padrão criado por `dotnet new console` :

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp3.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore" Version="3.1.2" />
  </ItemGroup>

</Project>
```

## <a name="add-a-dependency-by-using-the-cli"></a>Adicionar uma dependência usando a CLI

Para adicionar uma dependência, execute o [dotnet add package](dotnet-add-package.md) comando, conforme mostrado no exemplo a seguir:

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore
```

## <a name="remove-a-dependency-by-editing-the-project-file"></a>Remover uma dependência editando o arquivo de projeto

Para remover uma dependência, remova seu `<PackageReference>` elemento do arquivo de projeto.

## <a name="remove-a-dependency-by-using-the-cli"></a>Remover uma dependência usando a CLI

Para remover uma dependência, execute o [dotnet remove package](dotnet-remove-package.md) comando, conforme mostrado no exemplo a seguir:

```dotnetcli
dotnet remove package Microsoft.EntityFrameworkCore
```

## <a name="see-also"></a>Consulte também

* [Referências de pacotes em arquivos de projeto](../project-sdk/msbuild-props.md#reference-properties-and-items)
* [dotnet list packagelinha](dotnet-list-package.md)
