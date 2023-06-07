---
title: Ferramentas do Visual Studio para Docker no Windows
description: Conheça as ferramentas disponíveis do Docker no Visual Studio 2017 versão 15.7 e posteriores.
ms.date: 08/06/2020
ms.custom: vs-dotnet
ms.openlocfilehash: 74cffaae5885a7079ec774b1e8c68241cddda99a
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87915351"
---
# <a name="use-docker-tools-in-visual-studio-on-windows"></a>Usar ferramentas do Docker no Visual Studio no Windows

O fluxo de trabalho do desenvolvedor ao usar as ferramentas do Docker, inclusas no Visual Studio 2017 versão 15.7 e posteriores, é semelhante a usar o Visual Studio Code e a CLI do Docker (na verdade, ele se baseia na mesma CLI do Docker), mas é mais fácil de começar a usar, simplifica o processo e fornece maior produtividade para criar, executar e compor tarefas. Ele também pode executar e depurar seus contêineres por meio das teclas `F5` e `Ctrl+F5` usuais do Visual Studio. Você poderá até mesmo depurar toda a solução se seus contêineres estiverem definidos no mesmo arquivo `docker-compose.yml` no nível da solução.

## <a name="configure-your-local-environment"></a>Configurar o ambiente local

Com as versões mais recentes do Docker for Windows, é mais fácil do que nunca desenvolver aplicativos do Docker porque a instalação é simples, conforme explicado nas referências a seguir.

> [!TIP]
> Para saber mais sobre como instalar o Docker for Windows, confira (<https://docs.docker.com/docker-for-windows/>).

## <a name="docker-support-in-visual-studio"></a>Suporte ao Docker no Visual Studio

Há dois níveis de suporte do Docker que você pode adicionar a um projeto. Em projetos do ASP.NET Core, você pode adicionar um arquivo `Dockerfile` ao projeto habilitando o suporte do Docker. O próximo nível é o suporte à orquestração de contêiner, que adiciona um `Dockerfile` ao projeto (caso ainda não exista) e um arquivo `docker-compose.yml` no nível da solução. O suporte à orquestração de contêiner, por meio do Docker Compose, é adicionado por padrão no Visual Studio 2017 versões 15.0 a 15.7. O suporte à orquestração de contêiner é um recurso de aceitação no Visual Studio 2017 versão 15.8 ou posteriores. O Visual Studio 2019 e versões posteriores também dão suporte à implantação de **kubernetes/Helm** .

Os comandos **Adicionar > Suporte ao Docker** e **Adicionar > Suporte ao Orquestrador de Contêineres** estão localizados no menu de clique com o botão direito do mouse (ou menu de contexto) do nó do projeto para um projeto do ASP.NET Core no **Gerenciador de Soluções**, como mostrado na Figura 4-31:

![Adicionar a opção de menu de Suporte ao Docker no Visual Studio](media/add-docker-support-menu.png)

**Figura 4-31**. Adicionando o suporte do Docker a um projeto do Visual Studio 2019

### <a name="add-docker-support"></a>Adicionar suporte ao Docker

Além da opção de adicionar suporte do Docker a um aplicativo existente, conforme mostrado na seção anterior, você também pode habilitar o suporte do Docker durante a criação do projeto selecionando **habilitar suporte do Docker** na caixa de diálogo **novo ASP.NET Core aplicativo Web** que é aberta depois que você clica em **OK** na caixa de diálogo **novo projeto** , como mostrado na Figura 4-32.

![Habilitar o suporte ao Docker para o novo aplicativo Web do ASP.NET Core no Visual Studio](media/enable-docker-support-visual-studio.png)

**Figura 4-32**. Habilitar o suporte do Docker durante a criação do projeto no Visual Studio 2019

Quando você adiciona ou habilita o suporte do Docker, o Visual Studio adiciona um arquivo _Dockerfile_ ao projeto, que inclui referências a todo o projeto necessário da solução.

### <a name="add-container-orchestration-support"></a>Adicionar suporte à orquestração de contêiner

Se você deseja compor uma solução de vários contêineres, adicione o suporte à orquestração de contêiner aos seus projetos. Isso permite que você execute e depure um grupo de contêineres (uma solução inteira) ao mesmo tempo, como se eles estivessem definidos no mesmo arquivo _docker-compose.yml_.

Para adicionar o suporte à orquestração de contêiner, clique com o botão direito do mouse no nó do projeto ou na solução no **Gerenciador de Soluções** e escolha **Adicionar > Suporte à orquestração de contêiner**. Em seguida, escolha **kubernetes/Helm** ou **Docker Compose** para gerenciar os contêineres.

Depois de adicionar suporte de orquestração de contêiner ao seu projeto, você verá um Dockerfile adicionado ao projeto e uma pasta **Docker-Compose** adicionada à solução em **Gerenciador de soluções**, conforme mostrado na Figura 4-33:

![Arquivos do Docker no Gerenciador de Soluções no Visual Studio](media/docker-support-solution-explorer.png)

**Figura 4-33**. Arquivos do Docker no Gerenciador de Soluções no Visual Studio 2019

Se o _docker-compose.yml_ já existir, o Visual Studio adiciona nele apenas as linhas necessárias do código de configuração.

## <a name="configure-docker-tools"></a>Configurar as ferramentas do Docker

No menu principal, escolha **Ferramentas > Opções** e expanda **Ferramentas de contêiner > Configurações**. As configurações de ferramentas de contêiner aparecem.

![Opções das ferramentas do Docker do Visual Studio, mostrando três páginas: geral, projeto único e Docker Compose, detalhes no texto do artigo.](media/visual-studio-docker-tools-options.png)

**Figura 4-34**. Opções de ferramentas do Docker

A tabela a seguir pode ajudá-lo a decidir como definir essas opções.

| Página/configuração                                |  Configuração padrão   | Descrição                                                                                                                                                                                                                                                                                                                                                                                                           |
| ------------------------------------------- | :----------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Página geral**                            |
| Instalar o Docker desktop, se necessário            |     Avisar-me      |
| Iniciar o Docker desktop, se necessário              |     Avisar-me      |
| Confiar ASP.NET Core certificado SSL          |     Avisar-me      | Se o certificado SSL do localhost não tiver sido marcado como confiável (com `dotnet dev-certs https --trust` ), o Visual Studio será notificado sempre que você executar o projeto.                                                                                                                                                                                                                                                    |
| **Página de projeto único**                     |
| Efetuar pull de imagens do Docker necessárias no projeto aberto |        verdadeiro        | Para aumentar o desempenho ao executar o projeto, o Visual Studio iniciará uma operação de pull do Docker em segundo plano para que, quando você estiver pronto para executar seu código, a imagem já esteja baixada ou no processo de download. Se você estiver apenas carregando projetos e procurando código, será possível desativar isso para evitar o download de imagens de contêiner que você não precisa. Isso pode reduzir a experiência do usuário do projeto aberto. |
| Efetuar pull de imagens atualizadas do Docker na carga do projeto  | Projetos do .NET Core | Receba atualizações para imagens existentes para obter as atualizações mais recentes no projeto aberto. Isso pode reduzir a experiência do usuário do projeto aberto.                                                                                                                                                                                                                                                                                          |
| Remover contêineres no fechamento do projeto          |        verdadeiro        | Limpeza no fechamento do projeto, isso pode reduzir a experiência do usuário do projeto de fechamento, mas normalmente é rápido mesmo assim.                                                                                                                                                                                                                                                                                                            |
| Executar contêineres no projeto aberto              |        verdadeiro        | Para aumentar o desempenho ao executar o projeto, o Visual Studio iniciará todos os contêineres na solução. Isso pode reduzir a experiência do usuário do projeto aberto.                                                                                                                                                                                                                                                        |
| **Docker Compose**                          |                    | A página de Docker Compose contém as mesmas configurações da página do projeto único, mas elas se aplicam a soluções de vários contêineres.                                                                                                                                                                                                                                                                                           |

> [!WARNING]
> Se o certificado SSL do localhost não for confiável e você definir a opção como **nunca**, as solicitações da Web https poderão falhar em tempo de execução em seu aplicativo ou serviço. Nesse caso, defina o valor novamente para **avisar** ou, melhor de novo, confiar nos certificados em seu computador de desenvolvimento usando o comando `dotnet dev-certs https --trust` .

> [!TIP]
> Para obter mais detalhes sobre a implementação dos serviços e o uso das Ferramentas do Visual Studio para Docker, leia os seguintes artigos:
>
> Depurar aplicativos em um contêiner do Docker local:<https://docs.microsoft.com/azure/vs-azure-tools-docker-edit-and-refresh>
>
> Implantar um contêiner ASP.NET em um registro de contêiner usando o Visual Studio: <https://docs.microsoft.com/azure/vs-azure-tools-docker-hosting-web-apps-in-docker>

> [!div class="step-by-step"]
> [Anterior](docker-apps-inner-loop-workflow.md) 
>  [Avançar](set-up-windows-containers-with-powershell.md)
