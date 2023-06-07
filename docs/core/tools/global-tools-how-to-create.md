---
title: 'Tutorial: criar uma ferramenta do .NET Core'
description: Saiba como criar uma ferramenta do .NET Core. Uma ferramenta é um aplicativo de console que é instalado usando o CLI do .NET Core.
ms.topic: tutorial
ms.date: 02/12/2020
ms.openlocfilehash: c1c17368d8efdece73f5312899553bacf884cfb3
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88062776"
---
# <a name="tutorial-create-a-net-core-tool-using-the-net-core-cli"></a>Tutorial: criar uma ferramenta do .NET Core usando o CLI do .NET Core

**Este artigo aplica-se a:** ✔️ SDK do .net Core 2,1 e versões posteriores

Este tutorial ensina como criar e empacotar uma ferramenta do .NET Core. O CLI do .NET Core permite criar um aplicativo de console como uma ferramenta, que outras pessoas podem instalar e executar. As ferramentas do .NET Core são pacotes NuGet instalados a partir do CLI do .NET Core. Para obter mais informações sobre ferramentas, consulte [visão geral das ferramentas do .NET Core](global-tools.md).

A ferramenta que você criará é um aplicativo de console que recebe uma mensagem como entrada e exibe a mensagem junto com linhas de texto que criam a imagem de um robô.

Este é o primeiro de uma série de três tutoriais. Neste tutorial, você criará e empacotará uma ferramenta. Nos próximos dois tutoriais, você [usará a ferramenta como uma ferramenta global](global-tools-how-to-use.md) e [usará a ferramenta como uma ferramenta local](local-tools-how-to-use.md).

## <a name="prerequisites"></a>Pré-requisitos

- [SDK do .NET Core 3,1](https://dotnet.microsoft.com/download) ou uma versão posterior.

  Este tutorial e o tutorial a seguir [para ferramentas globais](global-tools-how-to-use.md) se aplicam ao SDK do .NET Core 2,1 e versões posteriores porque as ferramentas globais estão disponíveis a partir dessa versão. Mas este tutorial pressupõe que você tenha instalado o 3,1 ou posterior para que você tenha a opção de continuar no [tutorial de ferramentas locais](local-tools-how-to-use.md). As ferramentas locais estão disponíveis a partir do SDK do .NET Core 3,0. Os procedimentos para criar uma ferramenta são os mesmos se você usá-la como uma ferramenta global ou como uma ferramenta local.
  
- Um editor de texto ou editor de código de sua escolha.

## <a name="create-a-project"></a>Criar um projeto

1. Abra um prompt de comando e crie uma pasta chamada *Repository*.

1. Navegue até a pasta *repositório* e insira o seguinte comando:

   ```dotnetcli
   dotnet new console -n microsoft.botsay
   ```

   O comando cria uma nova pasta chamada *Microsoft. botsay* na pasta *Repository* .

1. Navegue até a pasta *Microsoft. botsay* .

   ```console
   cd microsoft.botsay
   ```

## <a name="add-the-code"></a>Adicionar o código

1. Abra o `Program.cs` arquivo com seu editor de código.

1. Adicione a seguinte `using` diretiva à parte superior do arquivo:

   ```csharp
   using System.Reflection;
   ```

1. Substitua o `Main` método pelo código a seguir para processar os argumentos de linha de comando para o aplicativo.

   ```csharp
   static void Main(string[] args)
   {
       if (args.Length == 0)
       {
           var versionString = Assembly.GetEntryAssembly()
                                   .GetCustomAttribute<AssemblyInformationalVersionAttribute>()
                                   .InformationalVersion
                                   .ToString();

           Console.WriteLine($"botsay v{versionString}");
           Console.WriteLine("-------------");
           Console.WriteLine("\nUsage:");
           Console.WriteLine("  botsay <message>");
           return;
       }

       ShowBot(string.Join(' ', args));
   }
   ```

   Se nenhum argumento for passado, será exibida uma mensagem de ajuda curta. Caso contrário, todos os argumentos são concatenados em uma única cadeia de caracteres e impressos chamando o `ShowBot` método que você cria na próxima etapa.

1. Adicione um novo método chamado `ShowBot` que usa um parâmetro de cadeia de caracteres. O método imprime a mensagem e uma imagem de um robô usando linhas de texto.

   ```csharp
   static void ShowBot(string message)
   {
       string bot = $"\n        {message}";
       bot += @"
       __________________
                         \
                          \
                             ....
                             ....'
                              ....
                           ..........
                       .............'..'..
                    ................'..'.....
                  .......'..........'..'..'....
                 ........'..........'..'..'.....
                .'....'..'..........'..'.......'.
                .'..................'...   ......
                .  ......'.........         .....
                .    _            __        ......
               ..    #            ##        ......
              ....       .                 .......
              ......  .......          ............
               ................  ......................
               ........................'................
              ......................'..'......    .......
           .........................'..'.....       .......
        ........    ..'.............'..'....      ..........
      ..'..'...      ...............'.......      ..........
     ...'......     ...... ..........  ......         .......
    ...........   .......              ........        ......
   .......        '...'.'.              '.'.'.'         ....
   .......       .....'..               ..'.....
      ..       ..........               ..'........
             ............               ..............
            .............               '..............
           ...........'..              .'.'............
          ...............              .'.'.............
         .............'..               ..'..'...........
         ...............                 .'..............
          .........                        ..............
           .....
   ";
       Console.WriteLine(bot);
   }
   ```

1. Salve suas alterações.

## <a name="test-the-application"></a>Testar o aplicativo

Execute o projeto e veja a saída. Experimente essas variações na linha de comando para ver os resultados diferentes:

```dotnetcli
dotnet run
dotnet run -- "Hello from the bot"
dotnet run -- Hello from the bot
```

Todos os argumentos após o delimitador `--` são passados para o aplicativo.

## <a name="package-the-tool"></a>Empacotar a ferramenta

Antes de poder empacotar e distribuir o aplicativo como uma ferramenta, você precisa modificar o arquivo de projeto.

1. Abra o arquivo *Microsoft. botsay. csproj* e adicione três novos nós XML ao final do `<PropertyGroup>` nó:

   ```xml
   <PackAsTool>true</PackAsTool>
   <ToolCommandName>botsay</ToolCommandName>
   <PackageOutputPath>./nupkg</PackageOutputPath>
   ```

   `<ToolCommandName>`é um elemento opcional que especifica o comando que invocará a ferramenta após sua instalação. Se esse elemento não for fornecido, o nome do comando para a ferramenta será o nome do arquivo de projeto sem a extensão *. csproj* .

   `<PackageOutputPath>`é um elemento opcional que determina onde o pacote NuGet será produzido. O pacote NuGet é o que o CLI do .NET Core usa para instalar sua ferramenta.

   O arquivo de projeto agora é semelhante ao exemplo a seguir:

   ```xml
   <Project Sdk="Microsoft.NET.Sdk">
  
     <PropertyGroup>

       <OutputType>Exe</OutputType>
       <TargetFramework>netcoreapp3.1</TargetFramework>
  
       <PackAsTool>true</PackAsTool>
       <ToolCommandName>botsay</ToolCommandName>
       <PackageOutputPath>./nupkg</PackageOutputPath>
  
     </PropertyGroup>

   </Project>
   ```

1. Crie um pacote NuGet executando o comando [dotnet Pack](dotnet-pack.md) :

   ```dotnetcli
   dotnet pack
   ```

   O arquivo *Microsoft. botsay. 1.0.0. nupkg* é criado na pasta identificada pelo `<PackageOutputPath>` valor do arquivo *Microsoft. botsay. csproj* , que neste exemplo é a pasta *./nupkg* .
  
   Quando você quiser liberar uma ferramenta publicamente, poderá carregá-la no `https://www.nuget.org` . Depois que a ferramenta estiver disponível no NuGet, os desenvolvedores poderão instalar a ferramenta usando o comando [dotnet ferramenta de instalação](dotnet-tool-install.md) . Para este tutorial, você instala o pacote diretamente da pasta *nupkg* local, portanto, não é necessário carregar o pacote no NuGet.

## <a name="troubleshoot"></a>Solucionar problemas

Se você receber uma mensagem de erro ao seguir o tutorial, consulte [solucionar problemas de uso da ferramenta .NET Core](troubleshoot-usage-issues.md).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você criou um aplicativo de console e o empacotava como uma ferramenta. Para saber como usar a ferramenta como uma ferramenta global, avance para o próximo tutorial.

> [!div class="nextstepaction"]
> [Instalar e usar uma ferramenta global](global-tools-how-to-use.md)

Se preferir, você pode ignorar o tutorial de ferramentas globais e ir diretamente para o tutorial de ferramentas locais.

> [!div class="nextstepaction"]
> [Instalar e usar uma ferramenta local](local-tools-how-to-use.md)
