---
title: Comando dotnet nuget push
description: O comando dotnet nuget push efetua push de um pacote no servidor e o publica.
author: karann-msft
ms.date: 02/14/2020
ms.openlocfilehash: 50a4a542c2d192bfbd927845489d04fd1b6c6cf3
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87555117"
---
# <a name="dotnet-nuget-push"></a>dotnet nuget push

**Este artigo aplica-se a:** ✔️ SDK do .NET Core 2. x e versões posteriores

## <a name="name"></a>Nome

`dotnet nuget push` – Envia um pacote ao servidor e os publica.

## <a name="synopsis"></a>Sinopse

```dotnetcli
dotnet nuget push [<ROOT>] [-d|--disable-buffering] [--force-english-output]
    [--interactive] [-k|--api-key <API_KEY>] [-n|--no-symbols true]
    [--no-service-endpoint] [-s|--source <SOURCE>] [--skip-duplicate]
    [-sk|--symbol-api-key <API_KEY>] [-ss|--symbol-source <SOURCE>]
    [-t|--timeout <TIMEOUT>]

dotnet nuget push -h|--help
```

## <a name="description"></a>Descrição

O comando `dotnet nuget push` envia um pacote ao servidor e os publica. O comando push usa o servidor e detalhes de credencial encontradas no arquivo de configuração do sistema NuGet ou uma cadeia de arquivos de configuração. Para obter mais informações sobre arquivos de configuração, consulte [Configurando o comportamento do NuGet](/nuget/consume-packages/configuring-nuget-behavior). A configuração de padrão do NuGet é obtida ao carregar *%AppData%\NuGet\NuGet.config* (Windows) ou *$HOME/.local/share* (Linux/macOS) e, em seguida, carregar qualquer *nuget.config* ou *.nuget\nuget.config* da raiz da unidade e terminar no diretório atual.

O comando envia por push um pacote existente. Ele não cria um pacote. Para criar um pacote, use [`dotnet pack`](dotnet-pack.md) .

## <a name="arguments"></a>Argumentos

- **`ROOT`**

  Especifica o caminho do arquivo para o pacote a ser enviado por push.

## <a name="options"></a>Opções

- **`-d|--disable-buffering`**

  Desabilita o buffer durante o push para um servidor HTTP(S) a fim de reduzir o uso de memória.

- **`--force-english-output`**

  Força a execução do aplicativo usando uma cultura invariável com base em inglês.

- **`-h|--help`**

  Imprime uma ajuda breve para o comando.

- **`--interactive`**

  Permite que o comando bloqueie e exija ação manual para operações como a autenticação. Opção disponível desde o SDK 2.2 do .NET Core.

- **`-k|--api-key <API_KEY>`**

  A chave da API para o servidor.

- **`-n|--no-symbols true`**

  Não envia símbolos por push (mesmo se estiver presente).

- **`--no-service-endpoint`**

  Não acrescenta "api/v2/package" à URL de origem. Opção disponível desde o SDK do .NET Core 2.1.

- **`-s|--source <SOURCE>`**

  Especifica a URL do servidor. Essa opção será obrigatória, a menos que o valor de configuração `DefaultPushSource` seja definido no arquivo de configuração do NuGet.

- **`--skip-duplicate`**

  Ao enviar vários pacotes para um servidor HTTP (S), o trata qualquer resposta de conflito 409 como um aviso para que o envio por push possa continuar. Disponível desde o SDK do .NET Core 3,1.

- **`-sk|--symbol-api-key <API_KEY>`**

  A chave da API para o servidor de símbolos.

- **`-ss|--symbol-source <SOURCE>`**

  Especifica a URL do servidor de símbolos.

- **`-t|--timeout <TIMEOUT>`**

  Especifica o tempo limite para enviar para um servidor em segundos. O padrão é 300 segundos (5 minutos). Especificar 0 (zero segundos) aplica o valor padrão.

## <a name="examples"></a>Exemplos

- Envie por push *foo. nupkg* para a fonte Push padrão, especificando uma chave de API:

  ```dotnetcli
  dotnet nuget push foo.nupkg -k 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a
  ```

- Envie por push o *foo. nupkg* para o servidor do NuGet oficial, especificando uma chave de API:

  ```dotnetcli
  dotnet nuget push foo.nupkg -k 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -s https://api.nuget.org/v3/index.json
  ```
  
  * Envia por push *foo.nupkg* à `https://customsource` de origem de push personalizada, especificando uma chave de API:

  ```dotnetcli
  dotnet nuget push foo.nupkg -k 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -s https://customsource/
  ```

- Envie por push *foo. nupkg* para a fonte Push padrão:

  ```dotnetcli
  dotnet nuget push foo.nupkg
  ```

- Envie por push *foo. Symbols. nupkg* para a fonte de símbolos padrão:

  ```dotnetcli
  dotnet nuget push foo.symbols.nupkg
  ```

- Envie por push *foo. nupkg* para a fonte Push padrão, especificando um tempo limite de 360 segundos:

  ```dotnetcli
  dotnet nuget push foo.nupkg --timeout 360
  ```

- Envie por push todos os arquivos *. nupkg* no diretório atual para a fonte Push padrão:

  ```dotnetcli
  dotnet nuget push "*.nupkg"
  ```

  > [!NOTE]
  > Se esse comando não funcionar, talvez seja devido a um bug que existia em versões mais antigas do SDK (SDK do .NET Core 2.1 e versões anteriores).
  > Para corrigir esse problema, atualize sua versão do SDK ou execute o seguinte comando em vez disso: `dotnet nuget push "**/*.nupkg"`
  
  > [!NOTE]
  > As aspas de circunscrição são necessárias para shells como bash que executam o mascaramento de arquivos. Para obter mais informações, consulte [NuGet/Home # 4393](https://github.com/NuGet/Home/issues/4393#issuecomment-667618120).

- Envie todos os arquivos *. nupkg* mesmo se uma resposta de conflito 409 for retornada por um servidor http (S):

  ```dotnetcli
  dotnet nuget push "*.nupkg" --skip-duplicate
  ```

- Envie por push todos os arquivos *. nupkg* no diretório atual para um diretório de feed local:

  ```dotnetcli
  dotnet nuget push "*.nupkg" -s c:\mydir
  ```

  Esse comando não armazena pacotes em uma estrutura de pastas hierárquicas, o que é recomendado para otimizar o desempenho. Para obter mais informações, consulte [feeds locais](/nuget/hosting-packages/local-feeds).  
