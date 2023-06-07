---
title: Criar ASP.NET Core aplicativos implantados como contêineres do Linux em clusters AKS/kubernetes
description: Containerized Docker Application Lifecycle with Microsoft Platform and Tools (Ciclo de vida de aplicativo do Docker em contêineres com a plataforma e as ferramentas da Microsoft)
ms.date: 08/06/2020
ms.openlocfilehash: 4b04e5d56c73918c665ad6e2825205870aac9606
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916439"
---
# <a name="build-aspnet-core-applications-deployed-as-linux-containers-into-an-akskubernetes-orchestrator"></a>Crie ASP.NET Core aplicativos implantados como contêineres do Linux em um orquestrador AKS/kubernetes

O AKS (Serviço de Kubernetes do Azure) é um serviço de orquestração do Kubernetes gerenciado pelo Azure que simplifica o gerenciamento e a implantação de contêiner.

Os principais recursos do AKS são:

- Um plano de controle hospedado no Azure
- Atualizações automatizadas
- Autorrecuperação
- Dimensionamento configurável pelo usuário
- Experiência de usuário mais simples para desenvolvedores e operadores de cluster.

Os exemplos a seguir exploram a criação de um aplicativo ASP.NET Core 3,1 que é executado no Linux e é implantado em um cluster AKS no Azure, enquanto o desenvolvimento é feito usando o Visual Studio 2019.

## <a name="creating-the-aspnet-core-project-using-visual-studio-2019"></a>Criando o projeto de ASP.NET Core usando o Visual Studio 2019

O ASP.NET Core é uma plataforma de desenvolvimento de uso geral mantida pela Microsoft e pela comunidade .NET no GitHub. Ele é multiplataforma e dá suporte ao Windows, macOS e Linux, bem como pode ser usado em dispositivos, na nuvem e em cenários inseridos/de IoT.

Este exemplo usa alguns projetos simples baseados em modelos do Visual Studio, de modo que você não precisa de muito conhecimento adicional para criar o exemplo. Você só precisa criar o projeto usando um modelo padrão que inclui todos os elementos para executar um pequeno projeto com uma API REST e um aplicativo Web com páginas Razor, usando a tecnologia ASP.NET Core 3,1.

![Adicione uma nova janela de projeto no Visual Studio selecionando o aplicativo Web ASP.NET Core.](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/create-aspnet-core-application.png)

**Figura 4-35**. Criar um aplicativo Web ASP.NET Core no Visual Studio 2019.

Para criar o projeto de exemplo no Visual Studio, selecione **arquivo**  >  **novo**  >  **projeto**, selecione o tipo de projeto **Web** e, em seguida, o modelo de **aplicativo Web ASP.NET Core** . Você também pode pesquisar o modelo se precisar dele.

Em seguida, insira o nome do aplicativo e o local, conforme mostrado na próxima imagem.

![Insira o nome do projeto e o local.](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/enter-project-name-and-location.png)

**Figura 4-36**. Insira o nome do projeto e o local no Visual Studio 2019.

Verifique se você selecionou ASP.NET Core 3,1 como a estrutura. O .NET Core 3,1 está incluído na versão mais recente do Visual Studio 2019 e é automaticamente instalado e configurado para você quando você instala o Visual Studio.

![Caixa de diálogo do Visual Studio para selecionar o tipo de um aplicativo Web do ASP.NET Core com a opção de API selecionada.](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/create-web-api-application.png)

**Figura 4-37**. Selecionando o ASP.NET CORE 3,1 e o tipo de projeto de API Web

Observe que o suporte ao Docker não está habilitado agora, apenas para mostrar que ele pode ser feito após a criação do projeto.

Se você tiver qualquer versão anterior do .NET Core, poderá baixar e instalar a versão 3,1 do <https://dotnet.microsoft.com/download> .

Para mostrar que você pode "Encaixer" seu projeto a qualquer momento, você adicionará suporte ao Docker agora. Então, clique com o botão direito do mouse no nó do projeto em Gerenciador de soluções e selecione **Adicionar**  >  **suporte ao Docker** no menu de contexto.

![Opção de menu de contexto para adicionar suporte do Docker a um projeto existente: clique com o botão direito do mouse (no projeto) > adicionar > suporte do Docker.](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/add-docker-support-to-project.png)

**Figura 4-38**. Adicionando suporte ao Docker a um projeto existente

Para concluir a adição de suporte ao Docker, você pode escolher Windows ou Linux. Nesse caso, selecione **Linux**.

![Caixa de diálogo de opção para selecionar o sistema operacional de destino para o Dockerfile.](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/select-linux-docker-support.png)

**Figura 4-39**. Como selecionar contêineres do Linux.

Com essas etapas simples, você tem seu aplicativo ASP.NET Core 3,1 em execução em um contêiner do Linux.

De forma semelhante, você também pode adicionar um projeto **webapp** muito simples (Figura 4-40) para consumir o ponto de extremidade da API Web, embora os detalhes não sejam discutidos aqui.

Depois disso, você adiciona suporte ao Orchestrator para seu projeto **WebApi** , conforme mostrado a seguir, na imagem 4-40.

![Adicionando suporte ao Orchestrator ao projeto WebApi](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/add-orchestrator-support.png)

**Figura 4-40**. Adicionando suporte ao Orchestrator ao projeto *WebApi* .

Quando você escolhe a `Docker Compose` opção, o que é adequado para o desenvolvimento local, o Visual Studio adiciona o projeto de composição do Docker, com os arquivos Docker-Compose, conforme mostrado na imagem 4-41.

![Docker-Compose adicionado à solução](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/docker-compose-project-in-visual-studio.png)

**Figura 4-41**. Adicionando suporte ao Orchestrator ao projeto *WebApi* .

Os arquivos iniciais adicionados são semelhantes a estes:

`docker-compose.yml`

```yml
version: "3.4"

services:
  webapi:
    image: ${DOCKER_REGISTRY-}webapi
    build:
      context: .
      dockerfile: WebApi/Dockerfile

  webapp:
    image: ${DOCKER_REGISTRY-}webapp
    build:
      context: .
      dockerfile: WebApp/Dockerfile
```

`docker-compose.override.yml`

```yml
version: "3.4"

services:
  webapi:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=https://+:443;http://+:80
    ports:
      - "80"
      - "443"
    volumes:
      - ${APPDATA}/Microsoft/UserSecrets:/root/.microsoft/usersecrets:ro
      - ${APPDATA}/ASP.NET/Https:/root/.aspnet/https:ro
  webapp:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=https://+:443;http://+:80
    ports:
      - "80"
      - "443"
    volumes:
      - ${APPDATA}/Microsoft/UserSecrets:/root/.microsoft/usersecrets:ro
      - ${APPDATA}/ASP.NET/Https:/root/.aspnet/https:ro
```

Para que seu aplicativo seja executado com Docker Compose você só precisa fazer alguns ajustes para`docker-compose.override.yml`

```yml
services:
  webapi:
    #...
    ports:
      - "51080:80"
      - "51443:443"
    #...
  webapp:
    environment:
      #...
      - WebApiBaseAddress=http://webapi
    ports:
      - "50080:80"
      - "50443:443"
    #...
```

Agora você pode executar seu aplicativo com a tecla **F5** ou usando o botão **Play** ou a tecla **Ctrl + F5** , selecionando o projeto Docker-Compose, conforme mostrado na imagem 4-42.

![Executando o projeto Docker-Compose com o Visual Studio](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/running-docker-compose-with-visual-studio.png)

**Figura 4-42**. Adicionando suporte ao Orchestrator ao projeto *WebApi* .

Ao executar o aplicativo Docker-Compose, conforme explicado, você obtém:

1. As imagens criadas e os contêineres criados de acordo com o arquivo Docker-Compose.
2. O navegador é aberto no endereço configurado na caixa de diálogo "Propriedades" do `docker-compose` projeto.
3. A janela do **contêiner** aberta (no Visual Studio 2019 versão 16,4 e posterior).
4. Suporte do depurador para todos os projetos na solução, conforme mostrado nas imagens a seguir.

Navegador aberto:

![Exibição de navegador com aplicativo Web em execução](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/browser-opened.png)

**Figura 4-43**. Janela do navegador com um aplicativo em execução em vários contêineres.

Janela contêineres:

![Janela "contêineres" do Visual Studio](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/visual-studio-containers-window.png)

**Figura 4-44**. Janela "contêineres" do Visual Studio

A janela **contêineres** permite exibir contêineres em execução, procurar imagens disponíveis, exibir variáveis de ambiente, logs e mapeamentos de porta, inspecionar o sistema de arquivos, anexar um depurador ou abrir uma janela de terminal dentro do ambiente de contêiner.

Como você pode ver, a integração entre o Visual Studio 2019 e o Docker é totalmente orientada à produtividade do desenvolvedor.

É claro que você também pode listar as imagens usando o `docker images` comando. Você deve ver as `webapi` `webapp` imagens e com as `dev` marcas criadas pela implantação automática do nosso projeto com o Visual Studio 2019.

```console
docker images
```

![A saída do console do comando imagens do Docker mostra uma lista com: repositório, marca, ID da imagem, criado (Data) e tamanho.](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/docker-images-command.png)

**Figura 4-45**. Exibição de imagens do Docker

## <a name="register-the-solution-in-an-azure-container-registry-acr"></a>Registrar a solução em um ACR (registro de contêiner do Azure)

Você pode carregar as imagens no [ACR (registro de contêiner do Azure)](https://azure.microsoft.com/services/container-registry/), mas também pode usar o Hub do Docker ou qualquer outro registro, para que as imagens possam ser implantadas no cluster AKs desse registro.

### <a name="create-an-acr-instance"></a>Criar uma instância de ACR

Execute o seguinte comando na **CLI do AZ**:

```powershell
az acr create --name exploredocker --resource-group explore-docker-aks-rg --sku basic --admin-enabled
```

### <a name="create-the-image-in-release-mode"></a>Criar a imagem no modo de versão

Agora, você criará a imagem no modo de **liberação** (pronto para produção) alterando para **lançamento**, conforme mostrado na Figura 4-46 e executando o aplicativo como fazia antes.

![Opção de barra de ferramentas no VS para build no modo de versão.](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/select-release-mode.png)

**Figura 4-46**. Como selecionar o modo de versão

Se você executar o `docker images` comando, verá as duas imagens criadas, uma para `debug` (**dev**) e outra para o `release` modo (**mais recente**).

### <a name="create-a-new-tag-for-the-image"></a>Criar uma marca para a imagem

Cada imagem de contêiner precisa ser marcada com o nome do `loginServer` do registro. Essa marca é usada para roteamento ao enviar imagens de contêiner por push a um registro da imagem.

Você pode exibir o nome `loginServer` do portal do Azure com as informações do Registro de Contêiner do Azure

![Exibição de navegador do nome do registro de contêiner do Azure, no canto superior direito.](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/loginServer-name.png)

**Figura 4-47**. Exibição do nome do Registro

Ou executando o seguinte comando:

```console
az acr list --resource-group <resource-group-name> --query "[].{acrLoginServer:loginServer}" --output table
```

![Saída do console do comando acima.](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/az-cli-loginServer-name.png)

**Figura 4-48**. Obter o nome do registro usando **AZ CLI**

Em ambos os casos, você obterá o nome. Em nosso exemplo, `exploredocker.azurecr.io`.

Agora você pode marcar a imagem, usando a imagem mais recente (a imagem de versão), com o comando:

```console
docker tag <image-name>:latest <login-server-name>/<image-name>:v1
```

Depois de executar o comando `docker tag`, liste as imagens com o comando `docker images` e você deverá ver a imagem com a nova marca.

![Saída de console do comando de imagens do Docker.](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/tagged-docker-images-list.png)

**Figura 4-49**. Exibição de imagens marcadas

### <a name="push-the-image-into-the-azure-acr"></a>Efetue push da imagem no ACR do Azure

Faça logon no Registro de Contêiner do Azure

```console
az acr login --name exploredocker
```

Efetue push da imagem no ACR do Azure usando o seguinte comando:

```console
docker push <login-server-name>/<image-name>:v1
```

Esse comando demora um pouco para carregar as imagens, mas fornece comentários sobre o processo. Na imagem a seguir, você pode ver a saída de uma imagem concluída e outra em andamento.

![Saída do console do comando Docker Push.](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/uploading-docker-images-complete.png)

**Figura 4-50**. Saída do console do comando Push.

Para implantar seu aplicativo de vários contêineres em seu cluster do AKS, você precisa `.yaml` de alguns arquivos de manifesto que tenham, a maioria das propriedades obtidas dos `docker-compose.yml` `docker-compose.override.yml` arquivos e.

#### `deploy-webapi.yml`

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapi
  labels:
    app: weather-forecast
spec:
  replicas: 1
  selector:
    matchLabels:
      service: webapi
  template:
    metadata:
      labels:
        app: weather-forecast
        service: webapi
    spec:
      containers:
        - name: webapi
          image: exploredocker.azurecr.io/webapi:v1
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 80
              protocol: TCP
          env:
            - name: ASPNETCORE_URLS
              value: http://+:80
---
apiVersion: v1
kind: Service
metadata:
  name: webapi
  labels:
    app: weather-forecast
    service: webapi
spec:
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
  selector:
    service: webapi
```

#### `deploy-webapp.yml`

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp
  labels:
    app: weather-forecast
spec:
  replicas: 1
  selector:
    matchLabels:
      service: webapp
  template:
    metadata:
      labels:
        app: weather-forecast
        service: webapp
    spec:
      containers:
        - name: webapp
          image: exploredocker.azurecr.io/webapp:v1
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 80
              protocol: TCP
          env:
            - name: ASPNETCORE_URLS
              value: http://+:80
            - name: WebApiBaseAddress
              value: http://webapi
---
apiVersion: v1
kind: Service
metadata:
  name: webapp
  labels:
    app: weather-forecast
    service: webapp
spec:
  type: LoadBalancer
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
  selector:
    service: webapp
```

> [!NOTE]
> Os `.yml` arquivos anteriores só habilitam as `HTTP` portas, usando o `ASPNETCORE_URLS` parâmetro, para evitar problemas com o certificado ausente no aplicativo de exemplo.

> [!TIP]
> Você pode ver como criar o Cluster do AKS para esta amostra na seção [**Implantar no AKS (Serviço de Kubernetes do Azure)**](deploy-azure-kubernetes-service.md) neste guia.

Agora você está quase pronto para implantar usando **kubectl**, mas primeiro você deve obter as credenciais do cluster AKs com este comando:

```console
az aks get-credentials --resource-group explore-docker-aks-rg --name explore-docker-aks
```

![Saída do console do comando acima: "Explore-Docker-AKS" mesclado como o contexto atual em C:\Users\Miguel.kube\config](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/getting-aks-credentials.png)

**Figura 4-51**. Obter credenciais de AKS para o ambiente kubectl.

Você também precisa permitir que o cluster AKS Extraia imagens do ACR, usando este comando:

```console
az aks update --name explore-docker-aks --resource-group explore-docker-aks-rg --attach-acr exploredocker
```

O comando anterior pode levar alguns minutos para ser concluído. Em seguida, use o `kubectl apply` comando para iniciar as implantações e, em seguida, `kubectl get all` obtenha a lista de objetos de cluster.

```console
kubectl apply -f deploy-webapi.yml
kubectl apply -f deploy-webapp.yml

kubectl get all
```

![Saída do console dos comandos acima: implantações aplicadas. serviços criados.](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/kubectl-apply-command.png)

**Figura 4-52**. Implantação em kubernetes

Você precisará aguardar um pouco até que o balanceador de carga obtenha o IP externo, verificando `kubectl get services` e, em seguida, o aplicativo deve estar disponível nesse endereço, conforme mostrado na próxima imagem:

![Exibição de navegador do aplicativo implantado no AKS](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/aks-deployed-application.png)

**Figura 4-53**. Implantação em kubernetes

Quando a implantação for concluída, você poderá acessar a [interface do usuário da Web do amKubernetes](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) com um proxy local, usando um túnel SSH.

Primeiro, você deve criar um ClusterRoleBinding com o seguinte comando:

```console
kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```

E, em seguida, esse comando para iniciar o proxy:

```console
az aks browse --resource-group exploredocker-aks-rg --name explore-docker-aks
```

Uma janela do navegador deve ser aberta em `http://127.0.0.1:8001` com uma exibição semelhante a esta:

![Exibição de navegador do painel do Kubernetes mostrando Implantações, Pods, Conjuntos de Réplicas e Serviços.](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/kubernetes-cluster-information.png)

**Figura 4-54**. Exibição de informações de cluster do Kubernetes

Agora você tem seu aplicativo ASP.NET Core, executado em contêineres do Linux e implantado em um cluster AKS no Azure.

> [!NOTE]
> Para obter mais informações sobre implantação com o Kubernetes, consulte: <https://kubernetes.io/docs/reference/kubectl/cheatsheet/>

> [!div class="step-by-step"]
> [Anterior](set-up-windows-containers-with-powershell.md) 
>  [Avançar](../docker-devops-workflow/index.md)
