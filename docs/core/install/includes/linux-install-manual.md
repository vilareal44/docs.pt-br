---
ms.openlocfilehash: ea2883912907843e4b6d65db5ba186af43f27aaa
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85805381"
---

<!-- Note, this content is copied in ../macos.md. Any fixes should be applied there too, though content may be different -->

Como alternativa para os gerenciadores de pacotes, você pode baixar e instalar manualmente o SDK e o tempo de execução. A instalação manual geralmente é executada como parte do teste de integração contínua ou em uma distribuição do Linux sem suporte. Para um desenvolvedor ou usuário, geralmente é melhor usar um Gerenciador de pacotes.

Se você instalar SDK do .NET Core, não será necessário instalar o tempo de execução correspondente. Primeiro, Baixe uma versão **binária** para o SDK ou o tempo de execução de um dos seguintes sites:

- ✔️ [downloads da versão prévia do .net 5,0](https://dotnet.microsoft.com/download/dotnet/5.0)
- downloads do ✔️ [.NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1)
- downloads do ✔️ [.NET Core 2,1](https://dotnet.microsoft.com/download/dotnet-core/2.1)
- [Todos os downloads do .NET Core](https://dotnet.microsoft.com/download/dotnet-core)

Em seguida, extraia o arquivo baixado e use o `export` comando para definir as variáveis usadas pelo .NET Core e, em seguida, verifique se o .NET Core está no caminho.

Para extrair o tempo de execução e tornar os comandos do CLI do .NET Core disponíveis no terminal, primeiro Baixe uma versão binária do .NET Core. Em seguida, abra um terminal e execute os seguintes comandos no diretório em que o arquivo foi salvo. O nome do arquivo morto pode ser diferente dependendo do que você baixou.

**Use o seguinte comando para extrair o tempo de execução**:

```bash
mkdir -p "$HOME/dotnet" && tar zxf aspnetcore-runtime-3.1.0-linux-x64.tar.gz -C "$HOME/dotnet"
export DOTNET_ROOT=$HOME/dotnet
export PATH=$PATH:$HOME/dotnet
```

**Use o seguinte comando para extrair o SDK**:

```bash
mkdir -p "$HOME/dotnet" && tar zxf dotnet-sdk-3.1.301-linux-x64.tar.gz -C "$HOME/dotnet"
export DOTNET_ROOT=$HOME/dotnet
export PATH=$PATH:$HOME/dotnet
```

> [!TIP]
> Os `export` comandos anteriores disponibilizam apenas os comandos CLI do .NET Core disponíveis para a sessão de terminal na qual ele foi executado.
>
> Você pode editar seu perfil de Shell para adicionar os comandos permanentemente. Há vários shells diferentes disponíveis para o Linux e cada um deles tem um perfil diferente. Por exemplo:
>
> - **Shell bash**: *~/. bash_profile*, *~/.bashrc*
> - **Shell Korn**: *~/.Kshrc* ou *. Profile*
> - **Shell Z**: *~/.zshrc* ou *. zprofile*
>
> Edite o arquivo de origem apropriado para o Shell e adicione `:$HOME/dotnet` ao final da `PATH` instrução existente. Se nenhuma `PATH` instrução for incluída, adicione uma nova linha com `export PATH=$PATH:$HOME/dotnet` .
>
> Além disso, adicione `export DOTNET_ROOT=$HOME/dotnet` ao final do arquivo.

Essa abordagem permite que você instale versões diferentes em locais separados e escolha explicitamente qual deles usar por qual aplicativo.
