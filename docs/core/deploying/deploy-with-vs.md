---
title: Implantar aplicativos .NET Core com o Visual Studio
description: Saiba como implantar um aplicativo .NET Core com o Visual Studio.
ms.date: 09/03/2018
dev_langs:
- csharp
- vb
ms.custom: vs-dotnet
ms.openlocfilehash: 73eee58a3d11f2f898a6d57cb282ccf4e802cdca
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656593"
---
# <a name="deploy-net-core-apps-with-visual-studio"></a>Implantar aplicativos .NET Core com o Visual Studio

Você pode implantar um aplicativo .NET Core como uma *implantação dependente de estrutura*, que inclui os binários do seu aplicativo, mas depende da presença do .NET Core no sistema de destino, ou como uma *implantação autocontida*, que inclui os binários do seu aplicativo e do .NET Core. Para obter uma visão geral da implantação de aplicativos .NET Core, consulte [Implantação de aplicativos .NET Core](index.md).

As seções a seguir mostram como usar o Microsoft Visual Studio para criar os seguintes tipos de implantações:

- Implantação dependente de estrutura
- Implantação dependente de estrutura com dependências de terceiros
- Implantação autocontida
- Implantação autocontida com dependências de terceiros

Para obter informações sobre como usar o Visual Studio para desenvolver aplicativos .NET Core, consulte [dependências e requisitos do .NET Core](../install/dependencies.md?pivots=os-windows).

## <a name="framework-dependent-deployment"></a>Implantação dependente de estrutura

Implantar uma implantação dependente de estrutura sem dependências de terceiros significa simplesmente compilar, testar e publicar o aplicativo. Um exemplo simples criado em C# ilustra o processo.

1. Crie o projeto.

   Selecione **Arquivo** > **Novo** > **Projeto**. Na caixa de diálogo **Novo projeto**, expanda as categorias de projeto (C# ou Visual Basic) de sua linguagem no painel de tipos de projeto **Instalados**, escolha **.NET Core** e, em seguida, selecione o modelo **Aplicativo de console (.NET Core)** no painel central. Insira um nome de projeto, como "FDD" na caixa de texto **Nome**. Selecione o botão **OK** .

1. Adicione o código-fonte do aplicativo.

   Abra o arquivo *Program.cs* ou *Program. vb* no editor e substitua o código gerado automaticamente pelo código a seguir. Ele solicitará que o usuário insira texto e exibirá as palavras individuais inseridas pelo usuário. Ele usa a expressão regular `\w+` para separar as palavras no texto de entrada.

   [!code-csharp[deployment#1](./snippets/deploy-with-vs/csharp/deployment-example.cs)]
   [!code-vb[deployment#1](./snippets/deploy-with-vs/vb/deployment-example.vb)]

1. Crie um build de depuração do seu aplicativo.

   Selecione **Compilar** > **Compilar Solução**. Você também pode compilar e executar a compilação de depuração de seu aplicativo selecionando **depurar**  >  **Iniciar Depuração**.

1. Implante seu aplicativo.

   Depois de ter depurado e testado o programa, crie os arquivos a serem implantados com seu aplicativo. Para publicar do Visual Studio, faça o seguinte:

      1. Altere a configuração da solução de **Depuração** para **Lançamento** na barra de ferramentas para compilar uma versão de lançamento (em vez de uma de depuração) do aplicativo.

      1. Clique com o botão direito do mouse no projeto (e não na solução) no **Gerenciador de Soluções** e selecione **Publicar**.

      1. Na guia **Publicar**, selecione **Publicar**. O Visual Studio grava os arquivos que compõem seu aplicativo no sistema de arquivos local.

      1. A guia **Publicar** agora mostra um único perfil **FolderProfile**. As configurações do perfil são mostradas na seção **Resumo** da guia.

   Os arquivos resultantes são colocados em um diretório nomeado `Publish` no Windows e `publish` em sistemas Unix que está em um subdiretório do subdiretório *.\bin\release\netcoreapp2.1* do seu projeto.

Junto com os arquivos do aplicativo, o processo de publicação emite um arquivo de banco de dados do programa (.pdb) que contém informações de depuração sobre seu aplicativo. O arquivo é útil principalmente para exceções de depuração. Você pode optar por não empacotá-lo com os arquivos do aplicativo. No entanto, você deve salvá-lo no caso de desejar depurar o build de lançamento para seu aplicativo.

Implante o conjunto completo de arquivos de aplicativo da maneira que desejar. Por exemplo, você pode empacotá-los em um arquivo zip, usar um simples comando `copy` ou implantá-los com qualquer pacote de instalação de sua escolha. Uma vez instalado, os usuários podem executar seu aplicativo usando o comando `dotnet` e fornecendo o nome de arquivo de aplicativo, como `dotnet fdd.dll`.

Além dos binários do aplicativo, o instalador deverá também agrupar o instalador da estrutura compartilhada ou procurar por ele como um pré-requisito como parte da instalação do aplicativo.  A instalação da estrutura compartilhada requer acesso de Administrador/raiz, pois se trata de todo o computador.

## <a name="framework-dependent-deployment-with-third-party-dependencies"></a>Implantação dependente de estrutura com dependências de terceiros

Implantar uma implantação dependente de estrutura com uma ou mais dependências de terceiros requer que todas as dependências estejam disponíveis para seu projeto. As etapas adicionais a seguir são necessárias antes de ser possível compilar seu aplicativo:

1. Use o **Gerenciador de Pacotes NuGet** para adicionar uma referência a um pacote NuGet ao projeto e se o pacote ainda não estiver disponível no sistema, instale-o. Para abrir o Gerenciador de pacotes, selecione **ferramentas**  >  **Gerenciador de pacotes NuGet**  >  **gerenciar pacotes NuGet para solução**.

1. Confirme se as dependências de terceiros (por exemplo, `Newtonsoft.Json` ) estão instaladas no sistema e, se não estiverem, instale-as. A guia **Instalados** lista os pacotes NuGet instalados no sistema. Se `Newtonsoft.Json` não estiver listado, selecione a guia **Procurar** e insira "Newtonsoft.Json" na caixa de pesquisa. Selecione `Newtonsoft.Json` e, no painel direito, selecione seu projeto antes de selecionar **Instalar**.

1. Se `Newtonsoft.Json` já estiver instalado no sistema, adicione-o ao projeto selecionando o projeto no painel direito da guia **Gerenciar Pacotes para a Solução**.

Uma implantação dependente da estrutura com dependências de terceiros é tão portável quanto suas dependências de terceiros. Por exemplo, se uma biblioteca de terceiros der suporte apenas a macOS, o aplicativo não será portátil para sistemas Windows. Isso acontecerá se a dependência de terceiros em si depender do código nativo. Um bom exemplo disso é o [servidor Kestrel](/aspnet/core/fundamentals/servers/kestrel), que requer uma dependência nativa no [libuv](https://github.com/libuv/libuv). Quando uma FDD é criada para um aplicativo com esse tipo de dependência de terceiros, a saída publicada contém uma pasta para cada [RID (Identificador de Runtime)](../rid-catalog.md) que dá suporte a dependência nativa (e que existe em seu pacote NuGet).

## <a name="self-contained-deployment-without-third-party-dependencies"></a><a name="simpleSelf"></a> Implantação autocontida sem dependências de terceiros

Implantar uma implantação autocontida sem dependências de terceiros inclui a criação do projeto, a modificação do arquivo *csproj*, a compilação, os testes e a publicação do aplicativo. Um exemplo simples criado em C# ilustra o processo. Comece criando, codificando e testando seu projeto como você faria com uma implantação que depende da estrutura:

1. Crie o projeto.

   Selecione **Arquivo** > **Novo** > **Projeto**. Na caixa de diálogo **Novo projeto**, expanda as categorias de projeto (C# ou Visual Basic) de sua linguagem no painel de tipos de projeto **Instalados**, escolha **.NET Core** e, em seguida, selecione o modelo **Aplicativo de console (.NET Core)** no painel central. Insira um nome de projeto, como "SCD" na caixa de texto **Nome** e selecione o botão **OK**.

1. Adicione o código-fonte do aplicativo.

   Abra o arquivo *Program.cs* ou *Program. vb* em seu editor e substitua o código gerado automaticamente pelo código a seguir. Ele solicitará que o usuário insira texto e exibirá as palavras individuais inseridas pelo usuário. Ele usa a expressão regular `\w+` para separar as palavras no texto de entrada.

   [!code-csharp[deployment#1](./snippets/deploy-with-vs/csharp/deployment-example.cs)]
   [!code-vb[deployment#1](./snippets/deploy-with-vs/vb/deployment-example.vb)]

1. Determine se você deseja usar o modo de invariável de globalização.

   Especialmente se seu aplicativo for destinado ao Linux, será possível reduzir o tamanho total da sua implantação usando o [modo invariável de globalização](https://github.com/dotnet/runtime/blob/master/docs/design/features/globalization-invariant-mode.md). O modo invariável de globalização é útil para aplicativos que não são conhecidos globalmente e que podem usar as convenções de formatação, de maiúsculas e minúsculas e a comparação de cadeia de caracteres e ordem de classificação da [cultura invariável](xref:System.Globalization.CultureInfo.InvariantCulture).

   Para habilitar o modo invariável, clique com o botão direito do mouse no seu projeto (não na solução) no **Gerenciador de Soluções** e selecione **Editar SCD.csproj** ou **Editar SCD.vbproj**. Em seguida, adicione as seguintes linhas realçadas ao arquivo:

   [!code-xml[globalization-invariant-mode](./snippets/deploy-with-vs/xml/invariant.csproj?highlight=7-9)]

1. Crie um build de depuração do seu aplicativo.

   Selecione **Compilar** > **Compilar Solução**. Você também pode compilar e executar a compilação de depuração de seu aplicativo selecionando **depurar**  >  **Iniciar Depuração**. Essa etapa de depuração permite identificar problemas com seu aplicativo quando ele está em execução em sua plataforma de host. Ainda será necessário testá-lo em cada uma de suas plataformas de destino.

   Se você tiver habilitado o modo invariável de globalização, certifique-se principalmente de testar se a ausência de dados que levam em conta a cultura é adequada para o seu aplicativo.

Após concluir a depuração, será possível publicar sua implantação independente:

<!-- markdownlint-disable MD025 -->

# <a name="visual-studio-156-and-earlier"></a>[Visual Studio 15.6 e versões anteriores](#tab/vs156)

Depois de ter depurado e testado o programa, crie os arquivos a serem implantados com seu aplicativo para cada plataforma à qual ele se destina.

Para publicar seu aplicativo do Visual Studio, faça o seguinte:

1. Defina as plataformas às quais seu aplicativo se destinará.

   1. Clique com o botão direito do mouse no projeto (e não na solução) no **Gerenciador de Soluções** e selecione **Editar SCD.csproj**.

   1. Crie uma marcação `<RuntimeIdentifiers>` na seção `<PropertyGroup>` de seu arquivo *csproj* que define as plataformas de destino do seu aplicativo e especifique o RID (identificador de runtime) de cada plataforma que você selecionar. Você também precisa adicionar um ponto e vírgula para separar os RIDs. Consulte [catálogo do identificador de tempo de execução](../rid-catalog.md) para obter uma lista de identificadores de tempo de execução.

   Por exemplo, o exemplo a seguir indica que o aplicativo é executado em sistemas operacionais Windows 10 de 64 bits e no sistema de operacional OS X Versão 10.11 de 64 bits.

   ```xml
   <PropertyGroup>
      <RuntimeIdentifiers>win10-x64;osx.10.11-x64</RuntimeIdentifiers>
   </PropertyGroup>
   ```

   O `<RuntimeIdentifiers>` elemento pode entrar em qualquer um `<PropertyGroup>` que você tenha em seu arquivo *csproj* . Um arquivo *csproj* de exemplo completo aparece mais adiante nesta seção.

1. Publique seu aplicativo.

   Depois de ter depurado e testado o programa, crie os arquivos a serem implantados com seu aplicativo para cada plataforma à qual ele se destina.

   Para publicar seu aplicativo do Visual Studio, faça o seguinte:

      1. Altere a configuração da solução de **Depuração** para **Lançamento** na barra de ferramentas para compilar uma versão de lançamento (em vez de uma de depuração) do aplicativo.

      1. Clique com o botão direito do mouse no projeto (e não na solução) no **Gerenciador de Soluções** e selecione **Publicar**.

      1. Na guia **Publicar**, selecione **Publicar**. O Visual Studio grava os arquivos que compõem seu aplicativo no sistema de arquivos local.

      1. A guia **Publicar** agora mostra um único perfil **FolderProfile**. As definições de configuração do perfil são mostradas na seção **Resumo** da guia. o **tempo de execução de destino** identifica qual tempo de execução foi publicado e o local de **destino** identifica onde os arquivos da implantação autônoma foram gravados.

      1. Por padrão, o Visual Studio grava todos os arquivos publicados em um único diretório. Para sua conveniência, é melhor criar perfis separados para cada runtime de destino e colocar os arquivos publicados em um diretório específico da plataforma. Isso envolve a criação de um perfil de publicação separado para cada plataforma de destino. Agora recompile o aplicativo para cada plataforma fazendo o seguinte:

         1. Selecione **Criar novo perfil** na caixa de diálogo **Publicar**.

         1. Na caixa de diálogo **Escolher um destino de publicação**, altere o local de **Escolher uma pasta** para *bin\Release\PublishOutput\win10-x64*. Selecione **OK**.

         1. Selecione o novo perfil (**FolderProfile1**) na lista de perfis e certifique-se de que o **Runtime de Destino** é `win10-x64`. Se não for, selecione **Configurações**. Na caixa de diálogo **Configurações de Perfil**, altere o **Runtime de Destino** para `win10-x64` e selecione **Salvar**. Caso contrário, selecione **Cancelar**.

         1. Selecione **Publicar** para publicar seu aplicativo para plataformas Windows 10 de 64 bits.

         1. Siga as etapas anteriores novamente para criar um perfil para a plataforma `osx.10.11-x64`. O **Local de Destino** é *bin\Release\PublishOutput\osx.10.11-x64* e o **Runtime de Destino** é `osx.10.11-x64`. O nome que o Visual Studio atribui a este perfil é **FolderProfile2**.

      Cada local de destino contém o conjunto completo de arquivos (seus arquivos de aplicativo e todos os arquivos do .NET Core) necessários para iniciar seu aplicativo.

Junto com os arquivos do aplicativo, o processo de publicação emite um arquivo de banco de dados do programa (.pdb) que contém informações de depuração sobre seu aplicativo. O arquivo é útil principalmente para exceções de depuração. Você pode optar por não empacotá-lo com os arquivos do aplicativo. No entanto, você deve salvá-lo no caso de desejar depurar o build de lançamento para seu aplicativo.

Implante os arquivos publicados na maneira que desejar. Por exemplo, você pode empacotá-los em um arquivo zip, usar um simples comando `copy` ou implantá-los com qualquer pacote de instalação de sua escolha.

A seguir está o arquivo *csproj* completo para esse projeto.

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.1</TargetFramework>
    <RuntimeIdentifiers>win10-x64;osx.10.11-x64</RuntimeIdentifiers>
  </PropertyGroup>
</Project>
```

# <a name="visual-studio-157-and-later"></a>[Visual Studio 15.7 e versões posteriores](#tab/vs157)

Depois de ter depurado e testado o programa, crie os arquivos a serem implantados com seu aplicativo para cada plataforma à qual ele se destina. Isso envolve a criação de um perfil separado para cada plataforma de destino.

Para cada plataforma que seu aplicativo direciona, faça o seguinte:

1. Crie um perfil para sua plataforma de destino.

   Se este for o primeiro perfil que você criou, clique com o botão direito do mouse no projeto (não na solução) no **Gerenciador de Soluções** e selecione **Publicar**.

   Se você já criou um perfil, clique com o botão direito do mouse no projeto para abrir a caixa de diálogo **Publicar** se ela ainda não estiver aberta. Em seguida, selecione **Novo perfil**.

   A caixa de diálogo **Escolher um destino de publicação** é aberta.

1. Selecione o local em que o Visual Studio publica seu aplicativo.

   Se você estiver apenas publicando em uma única plataforma, poderá aceitar o valor padrão na caixa de texto **escolher uma pasta** ; Isso publica a implantação dependente de estrutura do seu aplicativo no diretório * \<project-directory> \bin\Release\netcoreapp2.1\publish* .

   Se você estiver publicando em mais de uma plataforma, acrescente uma cadeia de caracteres que identifique a plataforma de destino. Por exemplo, se você acrescentar a cadeia de caracteres "Linux" ao caminho do arquivo, o Visual Studio publicará a implantação dependente da estrutura do seu aplicativo no diretório * \<project-directory> \bin\Release\netcoreapp2.1\publish\linux* .

1. Crie o perfil selecionando o ícone de lista suspensa ao lado do botão **Publicar** e selecionando **Criar perfil**. Em seguida, selecione o botão **Criar perfil** para criar o perfil.

1. Indique que você está publicando uma implantação independente e defina uma plataforma que seu aplicativo direcionará.

   1. Na caixa de diálogo **Publicar**, selecione o link **Configurar** para abrir a caixa de diálogo **Configurações de perfil**.

   1. Selecione **Independente** na caixa de listagem **Modo de implantação**.

   1. Na caixa de listagem **Runtime de destino**, selecione uma das plataformas que seu aplicativo direciona.

   1. Selecione **Salvar** para aceitar suas alterações e fechar a caixa de diálogo.

1. Nomeie seu perfil.

   1. Selecione **ações**  >  **renomear perfil** para nomear seu perfil.

   2. Atribua um nome ao seu perfil que identifique a plataforma de destino e, em seguida, selecione **Salvar*.

Repita essas etapas para definir quaisquer plataformas de destino adicionais que seu aplicativo direciona.

Você configurou seus perfis e agora está pronto para publicar seu aplicativo. Para fazer isso:

   1. Se a janela **Publicar** não estiver aberta no momento, clique com o botão direito do mouse no projeto (não na solução) no **Gerenciador de Soluções** e selecione **Publicar**.

   2. Selecione o perfil que você deseja publicar e, em seguida, selecione **Publicar**. Faça isso para cada perfil a ser publicado.

   Cada local de destino (no caso do nosso exemplo, bin\release\netcoreapp2.1\publish \\ *profile-name* contém o conjunto completo de arquivos (seus arquivos de aplicativo e todos os arquivos do .NET Core) necessários para iniciar seu aplicativo.

Junto com os arquivos do aplicativo, o processo de publicação emite um arquivo de banco de dados do programa (.pdb) que contém informações de depuração sobre seu aplicativo. O arquivo é útil principalmente para exceções de depuração. Você pode optar por não empacotá-lo com os arquivos do aplicativo. No entanto, você deve salvá-lo no caso de desejar depurar o build de lançamento para seu aplicativo.

Implante os arquivos publicados na maneira que desejar. Por exemplo, você pode empacotá-los em um arquivo zip, usar um simples comando `copy` ou implantá-los com qualquer pacote de instalação de sua escolha.

A seguir está o arquivo *csproj* completo para esse projeto.

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>
</Project>
```

Além disso, o Visual Studio cria um perfil de publicação separado (\*.pubxml) para cada plataforma que você direciona. Por exemplo, o arquivo para nosso perfil do linux (linux.pubxml) aparece da seguinte maneira:

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
https://go.microsoft.com/fwlink/?LinkID=208121.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <PublishProtocol>FileSystem</PublishProtocol>
    <Configuration>Release</Configuration>
    <Platform>Any CPU</Platform>
    <TargetFramework>netcoreapp2.1</TargetFramework>
    <PublishDir>bin\Release\netcoreapp2.1\publish\linux</PublishDir>
    <RuntimeIdentifier>win-x86</RuntimeIdentifier>
    <SelfContained>true</SelfContained>
    <_IsPortable>false</_IsPortable>
  </PropertyGroup>
</Project>
```

---

## <a name="self-contained-deployment-with-third-party-dependencies"></a>Implantação autocontida com dependências de terceiros

Implantar uma implantação autocontida com uma ou mais dependências de terceiros envolve adicionar as dependências. As etapas adicionais a seguir são necessárias antes de ser possível compilar seu aplicativo:

1. Use o **Gerenciador de Pacotes NuGet** para adicionar uma referência a um pacote NuGet ao projeto e se o pacote ainda não estiver disponível no sistema, instale-o. Para abrir o Gerenciador de pacotes, selecione **ferramentas**  >  **Gerenciador de pacotes NuGet**  >  **gerenciar pacotes NuGet para solução**.

1. Confirme se as dependências de terceiros (por exemplo, `Newtonsoft.Json` ) estão instaladas no sistema e, se não estiverem, instale-as. A guia **Instalados** lista os pacotes NuGet instalados no sistema. Se `Newtonsoft.Json` não estiver listado, selecione a guia **Procurar** e insira "Newtonsoft.Json" na caixa de pesquisa. Selecione `Newtonsoft.Json` e, no painel direito, selecione seu projeto antes de selecionar **Instalar**.

1. Se `Newtonsoft.Json` já estiver instalado no sistema, adicione-o ao projeto selecionando o projeto no painel direito da guia **Gerenciar Pacotes para a Solução**.

Este é o arquivo *csproj* completo para este projeto:

# <a name="visual-studio-156-and-earlier"></a>[Visual Studio 15.6 e versões anteriores](#tab/vs156)

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.1</TargetFramework>
    <RuntimeIdentifiers>win10-x64;osx.10.11-x64</RuntimeIdentifiers>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Newtonsoft.Json" Version="10.0.2" />
  </ItemGroup>
</Project>
```

# <a name="visual-studio-157-and-later"></a>[Visual Studio 15.7 e versões posteriores](#tab/vs157)

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Newtonsoft.Json" Version="10.0.2" />
  </ItemGroup>
</Project>
```

---

Quando você implanta seu aplicativo, todas as dependências de terceiros usadas em seu aplicativo também contém os arquivos do aplicativo. As bibliotecas de terceiros não são necessárias no sistema em que o aplicativo está em execução.

Você só pode implantar uma implantação independente com uma biblioteca de terceiros em plataformas com suporte nessa biblioteca. Isso é semelhante a ter dependências de terceiros com dependências nativas em sua implantação dependente de estrutura, em que as dependências nativas não existem na plataforma de destino a menos que elas tenham sido instaladas anteriormente.

## <a name="see-also"></a>Consulte também

- [Implantação do .NET Core Application](index.md)
- [Catálogo de RID (identificador de tempo de execução) do .NET Core](../rid-catalog.md)
