---
title: 'Tutorial: instalar e usar as ferramentas locais do .NET Core'
description: Saiba como instalar e usar uma ferramenta .NET como uma ferramenta local.
ms.topic: tutorial
ms.date: 02/12/2020
ms.openlocfilehash: 555497a71d54713e62e54f8f293afdf74ead1743
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88062666"
---
# <a name="tutorial-install-and-use-a-net-core-local-tool-using-the-net-core-cli"></a>Tutorial: instalar e usar uma ferramenta local do .NET Core usando o CLI do .NET Core

**Este artigo aplica-se a:** ✔️ SDK do .net Core 3,0 e versões posteriores

Este tutorial ensina como instalar e usar uma ferramenta local. Você usa uma ferramenta que você cria no [primeiro tutorial desta série](global-tools-how-to-create.md).

## <a name="prerequisites"></a>Pré-requisitos

* Conclua o [primeiro tutorial desta série](global-tools-how-to-create.md).
* Instale o tempo de execução do .NET Core 2,1.

  Para este tutorial, você instala e usa uma ferramenta que tem como alvo o .NET Core 2,1, portanto, você precisa ter esse tempo de execução instalado em seu computador. Para instalar o tempo de execução 2,1, vá para a [página de download do .NET Core 2,1](https://dotnet.microsoft.com/download/dotnet-core/2.1) e localize o link de instalação de tempo de execução na coluna **executar aplicativos-tempo de execução** .

## <a name="create-a-manifest-file"></a>Criar um arquivo de manifesto

Para instalar uma ferramenta somente para acesso local (para o diretório e subdiretórios atuais), ela deve ser adicionada a um arquivo de manifesto.

Na pasta *Microsoft. botsay* , navegue até um nível para a pasta do *repositório* :

```console
cd ..
```

Crie um arquivo de manifesto executando o comando [dotnet novo](dotnet-new.md) :

```dotnetcli
dotnet new tool-manifest
```

A saída indica a criação bem-sucedida do arquivo.

```console
The template "Dotnet local tool manifest file" was created successfully.
```

O *. config/dotnet-tools.jsno* arquivo ainda não tem ferramentas:

```json
{
  "version": 1,
  "isRoot": true,
  "tools": {}
}
```

As ferramentas listadas em um arquivo de manifesto estão disponíveis para o diretório e os subdiretórios atuais. O diretório atual é aquele que contém o diretório *. config* com o arquivo de manifesto.

Quando você usa um comando da CLI que se refere a uma ferramenta local, o SDK pesquisa um arquivo de manifesto no diretório atual e nos diretórios pai. Se encontrar um arquivo de manifesto, mas o arquivo não incluir a ferramenta referenciada, ele continuará a pesquisa por meio de diretórios pai. A pesquisa termina quando encontra a ferramenta referenciada ou encontra um arquivo de manifesto com `isRoot` definido como `true` .

## <a name="install-botsay-as-a-local-tool"></a>Instalar o botsay como uma ferramenta local

Instale a ferramenta do pacote que você criou no primeiro tutorial:

```dotnetcli
dotnet tool install --add-source ./microsoft.botsay/nupkg microsoft.botsay
```

Esse comando adiciona a ferramenta ao arquivo de manifesto que você criou na etapa anterior. A saída do comando mostra qual arquivo de manifesto está em sua ferramenta recém-instalada:

 ```console
 You can invoke the tool from this directory using the following command:
 'dotnet tool run botsay' or 'dotnet botsay'
 Tool 'microsoft.botsay' (version '1.0.0') was successfully installed.
 Entry is added to the manifest file /home/name/repository/.config/dotnet-tools.json
 ```

O *. config/dotnet-tools.jsno* arquivo agora tem uma ferramenta:

```json
{
  "version": 1,
  "isRoot": true,
  "tools": {
    "microsoft.botsay": {
      "version": "1.0.0",
      "commands": [
        "botsay"
      ]
    }
  }
}
```

## <a name="use-the-tool"></a>Usar a ferramenta

Invoque a ferramenta executando o `dotnet tool run` comando da pasta do *repositório* :

```dotnetcli
dotnet tool run botsay hello from the bot
```

## <a name="restore-a-local-tool-installed-by-others"></a>Restaurar uma ferramenta local instalada por outras pessoas

Normalmente, você instala uma ferramenta local no diretório raiz do repositório. Depois de fazer o check-in do arquivo de manifesto para o repositório, outros desenvolvedores poderão obter o arquivo de manifesto mais recente. Para instalar todas as ferramentas listadas no arquivo de manifesto, elas podem executar um único `dotnet tool restore` comando.

1. Abra o arquivo *. config/dotnet-tools.js* e substitua o conteúdo pelo JSON a seguir:

   ```json
   {
     "version": 1,
     "isRoot": true,
     "tools": {
       "microsoft.botsay": {
         "version": "1.0.0",
         "commands": [
           "botsay"
         ]
       },
       "dotnetsay": {
         "version": "2.1.3",
         "commands": [
           "dotnetsay"
         ]
       }
     }
   }
   ```

1. Substitua `<name>` pelo nome usado para criar o projeto.

1. Salve suas alterações.

   Fazer essa alteração é o mesmo que obter a versão mais recente do repositório depois que outra pessoa instalou o pacote `dotnetsay` para o diretório do projeto.

1. Execute o comando `dotnet tool restore`.

   ```dotnetcli
   dotnet tool restore
   ```

   O comando produz uma saída semelhante ao exemplo a seguir:

   ```console
   Tool 'microsoft.botsay' (version '1.0.0') was restored. Available commands: botsay
   Tool 'dotnetsay' (version '2.1.3') was restored. Available commands: dotnetsay
   Restore was successful.
   ```

1. Verifique se as ferramentas estão disponíveis:

   ```dotnetcli
   dotnet tool list
   ```

   A saída é uma lista de pacotes e comandos, semelhante ao exemplo a seguir:

   ```console
   Package Id      Version      Commands       Manifest
   --------------------------------------------------------------------------------------------
   microsoft.botsay 1.0.0        botsay         /home/name/repository/.config/dotnet-tools.json
   dotnetsay        2.1.3        dotnetsay      /home/name/repository/.config/dotnet-tools.json
   ```

1. Teste as ferramentas:

   ```dotnetcli
   dotnet tool run dotnetsay hello from dotnetsay
   dotnet tool run botsay hello from botsay
   ```

## <a name="update-a-local-tool"></a>Atualizar uma ferramenta local

A versão instalada da ferramenta local `dotnetsay` é 2.1.3.  A versão mais recente é 2.1.4. Use o comando [dotnet ferramenta de atualização](dotnet-tool-update.md) para atualizar a ferramenta para a versão mais recente.

```dotnetcli
dotnet tool update dotnetsay
```

A saída indica o novo número de versão:

```console
Tool 'dotnetsay' was successfully updated from version '2.1.3' to version '2.1.4'
(manifest file /home/name/repository/.config/dotnet-tools.json).
```

O comando Update localiza o primeiro arquivo de manifesto que contém a ID do pacote e o atualiza. Se não houver nenhuma ID de pacote em nenhum arquivo de manifesto que esteja no escopo da pesquisa, o SDK adicionará uma nova entrada ao arquivo de manifesto mais próximo. O escopo da pesquisa é feito por meio de diretórios pai até que um arquivo de manifesto com `isRoot = true` seja encontrado.

## <a name="remove-local-tools"></a>Remover ferramentas locais

Remova as ferramentas instaladas executando o comando [dotnet ferramenta de desinstalação](dotnet-tool-uninstall.md) :

```dotnetcli
dotnet tool uninstall microsoft.botsay
```

```dotnetcli
dotnet tool uninstall dotnetsay
```

## <a name="troubleshoot"></a>Solucionar problemas

Se você receber uma mensagem de erro ao seguir o tutorial, consulte [solucionar problemas de uso da ferramenta .NET Core](troubleshoot-usage-issues.md).

## <a name="see-also"></a>Consulte também

Para obter mais informações, consulte [Ferramentas do .NET Core](global-tools.md)
