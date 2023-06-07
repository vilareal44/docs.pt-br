---
title: Estruturas de destino em projetos em estilo SDK-.NET
description: Saiba mais sobre estruturas de destino para aplicativos e bibliotecas do .NET Core.
ms.date: 12/03/2019
ms.custom: updateeachrelease
ms.technology: dotnet-standard
ms.openlocfilehash: c1fd3a6fe07526d9f6828851c591ed0155c79a19
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164303"
---
# <a name="target-frameworks-in-sdk-style-projects"></a>Estruturas de destino em projetos no estilo SDK

Ao destinar a uma estrutura em um aplicativo ou uma biblioteca, você está especificando o conjunto de APIs que deseja disponibilizar para o aplicativo ou a biblioteca. Você especifica a estrutura de destino em seu arquivo de projeto usando os TFMs (Monikers da Estrutura de Destino).

Um aplicativo ou uma biblioteca pode ser destinada a uma versão do [.NET Standard](net-standard.md). As versões do .NET Standard representam conjuntos de APIs padronizadas entre todas as implementações do .NET. Por exemplo, uma biblioteca pode se destinar ao NET Standard 1.6 e obter acesso a APIs que funcionam no .NET Core e .NET Framework usando a mesma base de código.

Um aplicativo ou uma biblioteca também pode se destinar a uma implementação específica do .NET para obter acesso a APIs específicas de implementação. Por exemplo, um aplicativo que tem como destino o Xamarin.iOS (por exemplo, `Xamarin.iOS10`) obtém acesso aos wrappers da API fornecidos para Xamarin iOS do iOS 10 ou um aplicativo que tem como destino a UWP (Plataforma Universal do Windows, `uap10.0`) tem acesso a APIs que são compiladas para dispositivos que executam o Windows 10.

Para algumas estruturas de destino (por exemplo, o .NET Framework), as APIs são definidas pelos assemblies que a estrutura instala em um sistema e pode incluir as APIs da estrutura do aplicativo (por exemplo, ASP.NET).

Para estruturas de destino com base em pacote (por exemplo, .NET Standard e .NET Core), as APIs são definidas pelos pacotes incluídos no aplicativo ou na biblioteca. Um *metapacote* é um pacote NuGet que não tem nenhum conteúdo próprio, mas é uma lista de dependências (outros pacotes). Uma estrutura de destino com base em pacote NuGet especifica implicitamente um metapacote que faz referência a todos os pacotes que, juntos, compõem a estrutura.

## <a name="latest-target-framework-versions"></a>Versões mais recentes de estrutura de destino

A tabela a seguir define as estruturas de destino mais comuns, como elas são referenciadas e qual versão do [.NET Standard](net-standard.md) elas implementam. Estas versões de estrutura de destino são as versões estáveis mais recentes. As versões de pré-lançamento não são mostradas. Um TFM (Moniker da Estrutura de Destino) é um formato de token padronizado para especificar a estrutura de destino de um aplicativo ou uma biblioteca do .NET.

| Estrutura de Destino      | Mais Recente <br/> Versão estável | TFM (Moniker de Estrutura de Destino) | Implementado <br/> Versão do .NET Standard |
| :-------------------: | :-------------------------: | :----------------------------: | :-------------------------------------: |
| .NET Standard         | 2.1                         | netstandard 2.1                 | N/D                                     |
| .NET Core             | 3.1                         | netcoreapp 3.1                  | 2.1                                     |
| .NET Framework        | 4.8                         | net48                          | 2.0                                     |

## <a name="supported-target-framework-versions"></a>Versões de estrutura de destino com suporte

Normalmente, uma estrutura de destino é referenciada por um TFM. A tabela a seguir mostra as estruturas de destino com suporte pelo SDK do .NET Core e o cliente do NuGet. Equivalentes estão mostrados entre colchetes. Por exemplo, `win81` é um TFM equivalente ao `netcore451`.

| Estrutura de Destino           | TFM |
| -------------------------- | --- |
| .NET Standard              | netstandard1.0<br>netstandard1.1<br>netstandard1.2<br>netstandard1.3<br>netstandard1.4<br>netstandard1.5<br>netstandard1.6<br>netstandard2.0<br>netstandard 2.1 |
| .NET Core                  | netcoreapp1.0<br>netcoreapp1.1<br>netcoreapp2.0<br>netcoreapp2.1<br>netcoreapp2.2<br>netcoreapp 3.0<br>netcoreapp 3.1 |
| .NET Framework             | net11<br>net20<br>net35<br>net40<br>net403<br>net45<br>net451<br>net452<br>net46<br>net461<br>net462<br>net47<br>net471<br>net472<br>net48 |
| Windows Store              | netcore [netcore45]<br>netcore45 [win] [win8]<br>netcore451 [win81] |
| .NET Micro Framework       | netmf |
| Silverlight                | sl4<br>sl5 |
| Windows Phone              | wp [wp7]<br>wp7<br>wp75<br>wp8<br>wp81<br>wpa81 |
| Plataforma Universal do Windows | uap [uap10.0]<br>uap10.0 [win10] [netcore50] |

## <a name="how-to-specify-target-frameworks"></a>Como especificar estruturas de destino

As estruturas de destino são especificadas no arquivo de projeto. Quando uma única estrutura de destino é especificada, use o elemento **TargetFramework**. O arquivo de projeto de aplicativo de console a seguir demonstra como direcionar o .NET Core 3,0:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp3.0</TargetFramework>
  </PropertyGroup>

</Project>
```

Ao especificar várias estruturas de destino, você pode fazer referência a assemblies para cada estrutura de destino de forma condicional. Em seu código, você pode condicionalmente compilar em relação a esses assemblies usando símbolos de pré-processador com a lógica *if-then-else*.

O seguinte arquivo de projeto de biblioteca tem como destino as APIs do .NET Standard (`netstandard1.4`) e APIs do .NET Framework (`net40` e `net45`). Use o elemento **TargetFrameworks** plural com várias estruturas de destino. Observe como os atributos `Condition` incluem pacotes específicos de implementação quando a biblioteca é compilada para os dois TFMs de .NET Framework:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFrameworks>netstandard1.4;net40;net45</TargetFrameworks>
  </PropertyGroup>

  <!-- Conditionally obtain references for the .NET Framework 4.0 target -->
  <ItemGroup Condition=" '$(TargetFramework)' == 'net40' ">
    <Reference Include="System.Net" />
  </ItemGroup>

  <!-- Conditionally obtain references for the .NET Framework 4.5 target -->
  <ItemGroup Condition=" '$(TargetFramework)' == 'net45' ">
    <Reference Include="System.Net.Http" />
    <Reference Include="System.Threading.Tasks" />
  </ItemGroup>

</Project>
```

No aplicativo ou na biblioteca, você escreve código condicional para compilar para cada estrutura de destino:

```csharp
public class MyClass
{
    static void Main()
    {
#if NET40
        Console.WriteLine("Target framework: .NET Framework 4.0");
#elif NET45  
        Console.WriteLine("Target framework: .NET Framework 4.5");
#else
        Console.WriteLine("Target framework: .NET Standard 1.4");
#endif
    }
}
```

O sistema de compilação reconhece os símbolos de pré-processador que representam as estruturas de destino mostradas na tabela de [versões do Framework de destino com suporte](#supported-target-framework-versions) quando você está usando projetos em estilo SDK. Ao usar um símbolo que representa um TFM do .NET Standard ou .NET Core, substitua o ponto por um sublinhado e altere as letras minúsculas por maiúsculas (por exemplo, o símbolo de `netstandard1.4` é `NETSTANDARD1_4`).

A lista completa de símbolos de pré-processador para estruturas de destino do .NET Core é:

[!INCLUDE [Preprocessor symbols](../../includes/preprocessor-symbols.md)]

## <a name="deprecated-target-frameworks"></a>Estruturas de destino preteridas

As seguintes estruturas de destino estão preteridas. Os pacotes direcionados a essas estruturas de destino devem ser migrados para as substituições indicadas.

| TFM preterido                                                                             | Substituição |
| ------------------------------------------------------------------------------------------ | ----------- |
| aspnet50<br>aspnetcore50<br>dnxcore50<br>dnx<br>dnx45<br>dnx451<br>dnx452                  | netcoreapp  |
| dotnet<br>dotnet50<br>dotnet51<br>dotnet52<br>dotnet53<br>dotnet54<br>dotnet55<br>dotnet56 | netstandard |
| netcore50                                                                                  | uap10.0     |
| win                                                                                        | netcore45   |
| win8                                                                                       | netcore45   |
| win81                                                                                      | netcore451  |
| win10                                                                                      | uap10.0     |
| winrt                                                                                      | netcore45   |

## <a name="see-also"></a>Confira também

- [Desenvolver bibliotecas com as ferramentas de plataforma cruzada](../core/tutorials/libraries.md)
- [.NET Standard](net-standard.md)
- [Controle de versão do .NET Core](../core/versions/index.md)
- [Repositório GitHub dotnet/standard](https://github.com/dotnet/standard)
- [Repositório GitHub de Ferramentas NuGet](https://github.com/joelverhagen/NuGetTools)
- [Perfis de estrutura no .NET](https://blog.stephencleary.com/2012/05/framework-profiles-in-net.html)
