---
title: Fluxo de trabalho de desenvolvimento do loop interno para aplicativos do Docker
description: Saiba mais sobre o fluxo de trabalho de desenvolvimento de "loop interno" para aplicativos do Docker.
ms.date: 08/06/2020
ms.openlocfilehash: bf837ab53fff2b53cf141b2e621d484cff9b6889
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916174"
---
# <a name="inner-loop-development-workflow-for-docker-apps"></a>Fluxo de trabalho de desenvolvimento do loop interno para aplicativos do Docker

Antes de disparar o fluxo de trabalho de loop externo que abrange todo o ciclo de DevOps, tudo começa no computador de cada desenvolvedor, codificando o aplicativo em si, usando suas linguagens ou plataformas preferenciais e testando localmente (Figura 4-21). Mas, em todos os casos, você terá um ponto importante em comum, independentemente da linguagem, da estrutura ou das plataformas escolhidas. Neste fluxo de trabalho específico, você está sempre desenvolvendo e testando contêineres do Docker em nenhum outro ambiente, mas localmente.

![Diagrama mostrando o conceito de um ambiente de desenvolvimento de loop interno.](./media/docker-apps-inner-loop-workflow/inner-loop-development-context.png)

**Figura 4-21**. Contexto de desenvolvimento do loop interno

O contêiner ou instância de uma imagem de Docker terá estes componentes:

- Uma seleção de sistema operacional (por exemplo, uma distribuição do Linux ou Windows)

- Arquivos adicionados pelo desenvolvedor (por exemplo, binários do aplicativo)

- Configuração (por exemplo, configurações de ambiente e dependências)

- Instruções sobre quais processos devem ser executados pelo Docker

Você pode configurar o fluxo de trabalho de desenvolvimento do loop interno que utiliza o Docker como processo (descrito na próxima seção). Observe que as etapas iniciais para configurar o ambiente não estão incluídas, porque você só precisa fazer isso uma vez.

## <a name="building-a-single-app-within-a-docker-container-using-visual-studio-code-and-docker-cli"></a>Criando um único aplicativo dentro de um contêiner do Docker usando o Visual Studio Code e a CLI do Docker

Aplicativos são compostos por seus próprios serviços e por bibliotecas adicionais (dependências).

A Figura 4-22 mostra as etapas básicas que normalmente você precisaria executar ao criar um aplicativo do Docker, seguidas por descrições detalhadas de cada etapa.

![Diagrama mostrando as sete etapas necessárias para criar um aplicativo em contêineres.](./media/docker-apps-inner-loop-workflow/life-cycle-containerized-apps-docker-cli.png)

**Figura 4-22**. Fluxo de trabalho de alto nível do ciclo de vida de aplicativos em contêineres do Docker usando a CLI do Docker

### <a name="step-1-start-coding-in-visual-studio-code-and-create-your-initial-appservice-baseline"></a>Etapa 1: iniciar a codificação em Visual Studio Code e criar sua linha de base de aplicativo/serviço inicial

A maneira como você desenvolve seu aplicativo é semelhante à maneira como o faz sem o Docker. A diferença é que, durante o desenvolvimento, você está implantando e testando o aplicativo ou os serviços em execução dentro de contêineres do Docker colocados no ambiente local (como uma VM Linux ou Windows).

**Configurando o ambiente local**

Com as versões mais recentes do Docker desktop para Mac e Windows, é mais fácil do que nunca desenvolver aplicativos do Docker, e a configuração é simples.

> [!TIP]
> Para obter instruções sobre como configurar o Docker desktop para Windows, acesse <https://docs.docker.com/docker-for-windows/> .
>
> Para obter instruções sobre como configurar o Docker desktop para Mac, vá para <https://docs.docker.com/docker-for-mac/> .

Além disso, você precisará de um editor de código para que possa desenvolver o aplicativo enquanto estiver usando a CLI do Docker.

A Microsoft fornece Visual Studio Code, que é um editor de código leve com suporte no Windows, Linux e macOS, e fornece IntelliSense com [suporte para várias linguagens](https://code.visualstudio.com/docs/languages/overview) (JavaScript, .net, go, Java, Ruby, Python e linguagens mais modernas), [depuração](https://code.visualstudio.com/Docs/editor/debugging), [integração com git](https://code.visualstudio.com/Docs/editor/versioncontrol) e [suporte a extensões](https://code.visualstudio.com/docs/extensions/overview). Esse editor é uma ótima opção para desenvolvedores de macOS e Linux. No Windows, você também pode usar o Visual Studio.

> [!TIP]
> Para obter instruções sobre como instalar o Visual Studio Code para Windows, Linux ou macOS, vá para <https://code.visualstudio.com/docs/setup/setup-overview/> .
>
> Para obter instruções sobre como configurar o Docker para Mac, acesse <https://docs.docker.com/docker-for-mac/>.

Você pode trabalhar com a CLI do Docker e escrever seu código usando qualquer editor de código, mas usar o Visual Studio Code com a extensão do Docker torna mais fácil criar arquivos `Dockerfile` e `docker-compose.yml` em seu workspace. Você também pode executar tarefas e scripts do IDE do Visual Studio Code para executar comandos do Docker usando a CLI do Docker abaixo.

A extensão do Docker para o VS Code fornece os seguintes recursos:

- Geração automática de arquivos `Dockerfile` e `docker-compose.yml`

- Destaque de sintaxe e dicas ao passar o mouse para arquivos `docker-compose.yml` e `Dockerfile`

- IntelliSense (preenchimentos) para arquivos `Dockerfile` e `docker-compose.yml`

- Linting (erros e avisos) para arquivos `Dockerfile`

- Integração de paleta de comandos (F1) para os comandos do Docker mais comuns

- Integração do Explorer para gerenciar Imagens e Contêineres

- Implantar imagens do DockerHub e dos Registros de Contêiner do Azure no Serviço de Aplicativo do Azure

Para instalar a extensão do Docker, pressione Ctrl + Shift + P, digite `ext install` e, em seguida, execute o comando Instalar Extensão para abrir a lista de extensões do Marketplace. Em seguida, digite **docker** para filtrar os resultados e, então, selecione a extensão Docker Support, conforme ilustrado na Figura 4-23.

![Modo de exibição da extensão do Docker para VS Code.](media/docker-apps-inner-loop-workflow/install-docker-extension-vs-code.png)

**Figura 4-23**. Instalando a extensão do Docker no Visual Studio Code

### <a name="step-2-create-a-dockerfile-related-to-an-existing-image-plain-os-or-dev-environments-like-net-core-nodejs-and-ruby"></a>Etapa 2: criar um DockerFile relacionado a uma imagem existente (ambientes de sistema operacional simples ou de desenvolvimento como .NET Core, Node.js e Ruby)

Você precisará de um `DockerFile` por imagem personalizada a ser criada e por contêiner a ser implantado. Se seu aplicativo for composto por um único serviço personalizado, você precisará de um único `DockerFile` . No entanto, se o aplicativo for composto por vários serviços (por exemplo, em uma arquitetura de microsserviços), você precisará de um `Dockerfile` por serviço.

Normalmente, o `DockerFile` é colocado na pasta raiz do aplicativo ou serviço e contém os comandos necessários para que o Docker saiba como configurar e executar esse aplicativo ou serviço. Você pode criar seu `DockerFile` e adicioná-lo ao projeto junto com o código (Node.js, .NET Core etc.) ou, se você for novo no ambiente, veja a dica a seguir.

> [!TIP]
> Você pode usar a extensão do Docker para orientá-lo ao usar os arquivos `Dockerfile` e `docker-compose.yml` relacionados a seus contêineres do Docker. Eventualmente, você provavelmente gravará esses tipos de arquivos sem essa ferramenta, mas usar a extensão do Docker é um bom ponto de partida que acelerará sua curva de aprendizado.

Na Figura 4-24, você pode ver as etapas para adicionar os arquivos do Docker a um projeto usando a extensão do Docker para VS Code:

1. Abra a paleta de comandos, digite "**Docker**" e selecione "**Adicionar arquivos do Docker ao espaço de trabalho**".
2. Selecionar plataforma de aplicativo (ASP.NET Core)
3. Selecionar sistema operacional (Linux)
4. Incluir arquivos de Docker Compose opcionais
5. Insira as portas a serem publicadas (80, 443)
6. Selecione o projeto

![Etapas para adicionar arquivos do Docker com a extensão do Docker](media/docker-apps-inner-loop-workflow/add-docker-files-to-workspace-command.png)

**Figura 4-24**. Arquivos do Docker adicionados usando o comando **Adicionar arquivos do Docker ao espaço de trabalho**

Quando você adiciona um DockerFile, você especifica qual imagem do Docker base você usará (como usar `FROM mcr.microsoft.com/dotnet/core/aspnet` ). Normalmente, você criará sua imagem personalizada usando uma imagem de base que obteve de qualquer repositório oficial no [Registro do Docker Hub](https://hub.docker.com/) (como uma [imagem para .NET Core](https://hub.docker.com/_/microsoft-dotnet-core/) o a imagem [para Node.js](https://hub.docker.com/_/node/)).

> [!TIP]
> Você precisará repetir esse procedimento para cada projeto em seu aplicativo. No entanto, a extensão solicitará que o arquivo do Docker-compote gerado seja substituído após a primeira vez. Você deve responder a não substituí-lo, portanto, a extensão cria arquivos separados do Docker-Compose, que você pode mesclar manualmente, antes de executar o Docker-Compose.

**_Usar uma imagem oficial do Docker existente_**

Usar um repositório oficial de uma pilha de linguagem com um número de versão garante que recursos da mesma linguagem estejam disponíveis em todos os computadores (incluindo desenvolvimento, teste e produção).

A seguir, há um DockerFile de exemplo para um contêiner do .NET Core:

```Dockerfile
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
WORKDIR /src
COPY ["src/WebApi/WebApi.csproj", "src/WebApi/"]
RUN dotnet restore "src/WebApi/WebApi.csproj"
COPY . .
WORKDIR "/src/src/WebApi"
RUN dotnet build "WebApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebApi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApi.dll"]
```

Nesse caso, a imagem é baseada na versão 3,1 do oficial ASP.NET Core imagem do Docker (vários arcos para Linux e Windows), de acordo com a linha `FROM mcr.microsoft.com/dotnet/core/aspnet:3.1` . (Para obter mais informações sobre esse tópico, confira a página [Imagem do Docker do ASP.NET Core](https://hub.docker.com/_/microsoft-dotnet-core-aspnet/) e a página [Imagem do Docker do .NET Core](https://hub.docker.com/_/microsoft-dotnet-core/)).

No DockerFile, você também pode instruir o Docker para escutar a porta TCP que você usará no tempo de execução (como a porta 80 ou 443).

Você pode especificar mais definições de configurações no Dockerfile, dependendo da linguagem e da estrutura usadas. Por exemplo, a linha `ENTRYPOINT` com `["dotnet", "WebMvcApplication.dll"]` instrui o Docker a executar um aplicativo do .NET Core. Se você estiver usando o SDK e a CLI do .NET Core (`dotnet CLI`) para criar e executar o aplicativo do .NET, essa configuração será diferente. O essencial aqui é que a linha ENTRYPOINT e outras configurações dependem da linguagem e da plataforma que você escolher para seu aplicativo.

> [!TIP]
> Para obter mais informações sobre a criação de imagens do Docker para aplicativos .NET Core, acesse <https://docs.microsoft.com/dotnet/core/docker/building-net-docker-images>.
>
> Para saber mais sobre como criar suas próprias imagens, acesse <https://docs.docker.com/engine/tutorials/dockerimages/>.

**Usar repositórios de imagens com várias arquiteturas**

Um único nome de imagem em um repositório pode conter variantes de plataforma, como uma imagem do Linux e uma imagem do Windows. Esse recurso permite que fornecedores como a Microsoft (criadores de imagem base) criem um único repositório para abranger várias plataformas (ou seja, Linux e Windows). Por exemplo, o repositório [dotnet/core/aspnet](https://hub.docker.com/_/microsoft-dotnet-core-aspnet/), disponível no registro do Docker Hub, é compatível com Linux e Windows Nano Server usando o mesmo nome de imagem.

Efetuar pull da imagem [dotnet/core/aspnet](https://hub.docker.com/_/microsoft-dotnet-core-aspnet/) de um host do Windows efetua pull da variante do Windows, enquanto efetuar pull do mesmo nome de imagem de um host Linux efetua pull da variante do Linux.

**_Criar sua imagem base do zero_**

Você pode criar sua própria imagem base do Docker do zero, conforme explicado neste [artigo](https://docs.docker.com/engine/userguide/eng-image/baseimages/) do Docker. Esse cenário provavelmente não é recomendado para iniciantes no Docker, mas, se você quiser definir os bits específicos de sua própria imagem base, poderá fazer isso.

### <a name="step-3-create-your-custom-docker-images-embedding-your-service-in-it"></a>Etapa 3: criar suas imagens personalizadas do Docker inserindo seu serviço nele

Para cada serviço personalizado que compõe seu aplicativo, você precisará criar uma imagem relacionada. Se seu aplicativo for composto por um único serviço ou aplicativo Web, você precisará apenas de uma única imagem.

> [!NOTE]
> Ao levar em consideração o "fluxo de trabalho de DevOps de loop externo", as imagens serão criadas por um processo de build automatizado sempre que você efetuar push de seu código-fonte para um repositório Git (integração contínua), portanto, as imagens serão criadas no ambiente global de seu código-fonte.
>
> Mas antes de você considerar a saída dessa rota de loop externo, você precisa garantir que o aplicativo Docker esteja realmente funcionando corretamente para que eles não enviem um código que possa não funcionar corretamente para o sistema de controle do código-fonte (git, etc.).
>
> Sendo assim, cada desenvolvedor precisa primeiro realizar todo o processo de loop interno para testar localmente e continuar desenvolvendo até que queiram efetuar push de um recurso ou alteração completa para o sistema de controle do código-fonte.

Para criar uma imagem em seu ambiente local e usar o DockerFile, você pode usar o comando Docker Build, como mostrado na Figura 4-25, porque ele já marca a imagem para você e compila as imagens para todos os serviços no aplicativo com um comando simples.

![Captura de tela mostrando a saída do console do comando Docker-Compose Build.](media/docker-apps-inner-loop-workflow/run-docker-build-command.png)

**Figura 4-25**. Executando docker build

Opcionalmente, em vez de executar `docker build` diretamente da pasta do projeto, você pode primeiro gerar uma pasta implantável com as bibliotecas .NET necessárias usando o comando run `dotnet publish` e, em seguida, executando `docker build`.

Este exemplo cria uma imagem do Docker com o nome `explore-docker-vscode/webapi:latest` (`:latest` é uma marca, como uma versão específica). Você poderá realizar esta etapa para cada imagem personalizada que precisar criar para o seu aplicativo do Docker composto com vários contêineres. No entanto, veremos na próxima seção que é mais fácil fazer isso usando `docker-compose` .

Você pode encontrar as imagens existentes no seu repositório local (seu computador de desenvolvimento) usando o `docker images` comando, como ilustrado na figura 4-26.

![Saída do console do comando docker images, mostrando as imagens existentes.](media/docker-apps-inner-loop-workflow/view-existing-images-with-docker-images.png)

**Figura 4-26**. Exibindo imagens existentes usando imagens do Docker

### <a name="step-4-define-your-services-in-docker-composeyml-when-building-a-composed-docker-app-with-multiple-services"></a>Etapa 4: definir seus serviços em Docker-Compose. yml ao criar um aplicativo do Docker composto com vários serviços

Com o arquivo `docker-compose.yml`, você pode definir um conjunto de serviços relacionados para que sejam implantados como um aplicativo composto usando os comandos de implantação explicados na seção sobre a próxima etapa.

Crie esse arquivo na pasta principal ou raiz da solução; ele deve ter conteúdo semelhante ao mostrado neste arquivo `docker-compose.yml`:

```yml
version: "3.4"

services:
  webapi:
    image: webapi
    build:
      context: .
      dockerfile: src/WebApi/Dockerfile
    ports:
      - 51080:80
    depends_on:
      - redis
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=http://+:80

  webapp:
    image: webapp
    build:
      context: .
      dockerfile: src/WebApp/Dockerfile
    ports:
      - 50080:80
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=http://+:80
      - WebApiBaseAddress=http://webapi

  redis:
    image: redis
```

Nesse caso em particular, esse arquivo define três serviços: o serviço de API Web (seu serviço personalizado), um aplicativo Web e o serviço Redis (um serviço de cache popular). Cada serviço será implantado como um contêiner, portanto, você precisa usar uma imagem concreta do Docker para cada um. Para este aplicativo específico:

- O serviço de API Web é criado a partir do DockerFile no `src/WebApi/Dockerfile` diretório.

- A porta de host 51080 é encaminhada para a porta exposta 80 no `webapi` contêiner.

- O serviço API da Web depende do serviço Redis

- O aplicativo Web acessa o serviço de API Web usando o endereço interno: `http://webapi` .

- O serviço Redis usa a [imagem de Redis pública mais recente](https://hub.docker.com/_/redis/) extraída do registro do Hub do Docker. [Redis](https://redis.io/) é um sistema de cache popular para aplicativos do lado do servidor.

### <a name="step-5-build-and-run-your-docker-app"></a>Etapa 5: criar e executar o aplicativo Docker

Se seu aplicativo tiver apenas um contêiner, você precisará apenas executá-lo implantando-o no host do Docker (VM ou servidor físico). No entanto, se o aplicativo for composto por vários serviços, você também precisará _compô-lo_. Vejamos as diferentes opções.

**_Opção A: executar um único contêiner ou serviço_**

Você pode executar a imagem do Docker usando o comando docker run, conforme mostrado aqui:

```console
docker run -t -d -p 50080:80 explore-docker-vscode/webapp:latest
```

Para essa implantação específica, vamos redirecionar as solicitações enviadas para a porta 50080 no host para a porta interna 80.

**_Opção B: compor e executar um aplicativo de vários contêineres_**

Na maioria dos cenários empresariais, um aplicativo do Docker será composto por vários serviços. Nesses casos, você pode executar o `docker-compose up` comando (figura 4-27), que usará o arquivo Docker-Compose. yml que você criou anteriormente. A execução desse comando compila todas as imagens personalizadas e implanta o aplicativo composto com todos os seus contêineres relacionados.

![Saída de console do comando docker-compose up.](media/docker-apps-inner-loop-workflow/results-docker-compose-up.png)

**Figura 4-27**. Resultados da execução do comando "docker-compose up"

Depois de executar `docker-compose up`, implante seu aplicativo e os contêineres relacionados no Host do Docker, conforme ilustrado na Figura 4-28, na representação da VM.

![VM executando aplicativos de vários contêineres.](./media/docker-apps-inner-loop-workflow/vm-with-docker-containers-deployed.png)

**Figura 4-28**. VM com contêineres do Docker implantados

### <a name="step-6-test-your-docker-application-locally-in-your-local-cd-vm"></a>Etapa 6: testar o aplicativo Docker (localmente, em sua VM de CD local)

Esta etapa varia de acordo com o que seu aplicativo está fazendo.

Em um simples "Olá, Mundo" da API Web do .NET Core implantado como um único contêiner ou serviço, você precisará apenas acessar o serviço fornecendo a porta TCP especificada no DockerFile.

No host do Docker, abra um navegador e navegue até esse site. Você deverá ver seu aplicativo/serviço em execução, conforme demonstrado na Figura 4-29.

![Modo de exibição de navegador da resposta de localhost/API/ valores.](media/docker-apps-inner-loop-workflow/test-docker-app-locally-localhost.png)

**Figura 4-29**. Testando o aplicativo Docker localmente usando o navegador

Observe que ele está usando a porta 50080, mas internamente ela está sendo redirecionada para a porta 80, porque é assim que ela foi implantada `docker compose` , conforme explicado anteriormente.

Você pode testar isso usando o navegador usando a rotação do terminal, conforme descrito na Figura 4-30.

![O resultado da ondulação obtido dehttp://localhost:51080/WeatherForecast](media/docker-apps-inner-loop-workflow/test-docker-app-locally-curl.png)

**Figura 4-30**. Testando um aplicativo do Docker localmente usando CURL

**Depurando um contêiner em execução no Docker**

O Visual Studio Code dá suporte à depuração do Docker quando você está usando Node.js e outras plataformas, como contêineres do .NET Core.

Você também pode depurar contêineres do .NET Core ou .NET Framework no Docker ao usar o Visual Studio para Windows ou Mac, conforme descrito na próxima seção.

> [!TIP]
> Para saber mais sobre depuração Node.js contêineres do Docker, consulte <https://blog.docker.com/2016/07/live-debugging-docker/> e <https://docs.microsoft.com/archive/blogs/user_ed/visual-studio-code-new-features-13-big-debugging-updates-rich-object-hover-conditional-breakpoints-node-js-mono-more> .

> [!div class="step-by-step"]
> [Anterior](docker-apps-development-environment.md) 
>  [Avançar](visual-studio-tools-for-docker.md)
