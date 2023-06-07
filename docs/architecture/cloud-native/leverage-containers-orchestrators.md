---
title: Como aproveitar contêineres e orquestradores
description: Aproveitando contêineres do Docker e orquestradores kubernetes no Azure
ms.date: 05/31/2020
ms.openlocfilehash: 25e981e0fb7957e7180be09a19a406eddfe4e51b
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84446861"
---
# <a name="leveraging-containers-and-orchestrators"></a>Como aproveitar contêineres e orquestradores

Contêineres e orquestradores são projetados para resolver problemas comuns a abordagens de implantação monolítica.

## <a name="challenges-with-monolithic-deployments"></a>Desafios com implantações monolíticos

Tradicionalmente, a maioria dos aplicativos foi implantada como uma única unidade. Esses aplicativos são chamados de monolíticos. Essa abordagem geral de implantação de aplicativos como unidades únicas, mesmo se eles são compostos por vários módulos ou assemblies, é conhecida como arquitetura monolítica, como mostra a Figura 3-1.

![Arquitetura monolítica.](./media/monolithic-design.png)

**Figura 3-1**. Arquitetura monolítica.

Embora eles tenham o benefício da simplicidade, as arquiteturas monolíticos enfrentam vários desafios:

### <a name="deployment"></a>Implantação

Os aplicativos monolíticos exigem uma implantação completa de todo o aplicativo, mesmo que apenas uma pequena alteração tenha sido feita. As implantações completas podem ser caras e sujeitas a erros. Além disso, eles exigem uma reinicialização do aplicativo, o que afeta temporariamente a indisponibilidade.

### <a name="scaling"></a>Scaling

Um aplicativo monolítico é totalmente hospedado em uma única instância de computador, muitas vezes exigindo hardware de alta capacidade. Se qualquer parte do monolítica exigir o dimensionamento, outra cópia do aplicativo inteiro deverá ser implantada em outro computador. Com um monolítico, você não pode dimensionar componentes de aplicativos individualmente – é tudo ou nada. O dimensionamento de componentes que não exigem dimensionamento resulta em um uso de recursos ineficiente e dispendioso.

### <a name="environment"></a>Ambiente

Os aplicativos monolíticos normalmente são implantados em um ambiente de hospedagem com um sistema operacional pré-instalado, tempo de execução e dependências de biblioteca. Esse ambiente pode não corresponder ao que o aplicativo foi desenvolvido ou testado. Inconsistências entre ambientes de aplicativos são uma fonte comum de problemas para implantações monolíticos.

### <a name="coupling"></a>Acoplamento

Um aplicativo monolítico provavelmente experimentará um acoplamento alto em seus componentes funcionais. Sem limites rígidos, as alterações do sistema geralmente resultam em efeitos colaterais indesejados e dispendiosos. Novos recursos/correções se tornam complicados, demorados e caros de implementar. As atualizações exigem testes extensivos. O acoplamento também dificulta a refatoração de componentes ou a troca em implementações alternativas. Mesmo quando construída com uma separação estrita de preocupações, a arquitetura erosion define como a base de código monolítica deteriorada com "casos especiais" que não terminam.

### <a name="platform-lock-in"></a>Bloqueio de plataforma

Um aplicativo monolítico é construído com uma única pilha de tecnologia. Ao oferecer a uniformidade, esse compromisso pode se tornar uma barreira à inovação. Novos recursos e componentes serão criados usando a pilha atual do aplicativo, mesmo quando mais tecnologias modernas podem ser uma opção melhor. Um risco de longo prazo é que sua pilha de tecnologia se torna desatualizada e obsoleta. A reestruturação de um aplicativo inteiro para uma plataforma nova e mais moderna é a mais cara e arriscada.

## <a name="what-are-the-benefits-of-containers-and-orchestrators"></a>Quais são os benefícios de contêineres e orquestradores?

Introduzimos contêineres no capítulo 1. Destacamos como a CNCF (nuvem Native Computing Foundation) classifica a Containerização como a primeira etapa em seu [mapa de rastros de nuvem nativa](https://raw.githubusercontent.com/cncf/trailmap/master/CNCF_TrailMap_latest.png) -orientação para empresas que começam sua jornada nativa de nuvem. Nesta seção, discutiremos os benefícios dos contêineres.

O Docker é a plataforma de gerenciamento de contêiner mais popular. Ele funciona com contêineres no Linux ou no Windows. Os contêineres fornecem ambientes de aplicativos separados, mas reproduzíveis, que são executados da mesma maneira em qualquer sistema. Esse aspecto torna-os perfeitos para desenvolver e hospedar serviços nativos de nuvem. Os contêineres são isolados um do outro. Dois contêineres no mesmo hardware de host podem ter versões diferentes do software, sem causar conflitos.

Os contêineres são definidos por arquivos simples baseados em texto que se tornam artefatos do projeto e são verificados no controle do código-fonte. Embora os servidores completos e as máquinas virtuais exijam um esforço manual para atualizar, os contêineres são facilmente controlados por versão. Aplicativos criados para execução em contêineres podem ser desenvolvidos, testados e implantados usando ferramentas automatizadas como parte de um pipeline de compilação.

Os contêineres são imutáveis. Depois de definir um contêiner, você pode recriá-lo e executá-lo exatamente da mesma maneira. Essa imutabilidade se presta ao design baseado em componentes. Se algumas partes de um aplicativo evoluem de forma diferente do que outras, por que reimplantar todo o aplicativo quando você pode simplesmente implantar as partes que mudam com mais frequência? Diferentes recursos e preocupações abrangentes de um aplicativo podem ser divididos em unidades separadas. A Figura 3-2 mostra como um aplicativo monolítico pode aproveitar os contêineres e os microserviços delegando determinados recursos ou funcionalidades. A funcionalidade restante no aplicativo também foi contida em contêineres.

![Dividir um aplicativo monolítico para usar os microserviços no back-end.](./media/cloud-native-design.png)

**Figura 3-2**. Decompondo um aplicativo monolítico para abraçar os microserviços.

Cada serviço nativo de nuvem é criado e implantado em um contêiner separado. Cada uma pode ser atualizada conforme necessário. Serviços individuais podem ser hospedados em nós com recursos apropriados para cada serviço. O ambiente em que cada serviço é executado é imutável, compartilhado entre ambientes de desenvolvimento, teste e produção e com controle de versão com facilidade. O acoplamento entre diferentes áreas do aplicativo ocorre explicitamente como chamadas ou mensagens entre serviços, não as dependências de tempo de compilação dentro do monolítico. Você também pode escolher a tecnologia que melhor suites um determinado recurso sem a necessidade de alterações no restante do aplicativo.

Os serviços em contêineres exigem gerenciamento automatizado. Não seria possível administrar manualmente um grande conjunto de contêineres implantados de forma independente. Por exemplo, considere as seguintes tarefas:

- Como as instâncias de contêiner serão provisionadas em um cluster de vários computadores?
- Uma vez implantado, como os contêineres irão descobrir e se comunicar entre si?
- Como os contêineres podem ser expandidos ou reduzidos sob demanda?
- Como monitorar a integridade de cada contêiner?
- Como proteger um contêiner contra falhas de hardware e software?
- Como atualizar contêineres para um aplicativo em tempo real sem tempo de inatividade?

Os orquestradores de contêiner abordam e automatizam essas e outras preocupações.

No sistema de eco nativo de nuvem, kubernetes tornou-se o orquestrador de contêiner de fato. É uma plataforma de software livre gerenciada pela CNCF (nuvem Native Computing Foundation). O kubernetes automatiza a implantação, o dimensionamento e as preocupações operacionais de cargas de trabalho em contêineres em um cluster de máquina. No entanto, a instalação e o gerenciamento do kubernetes são notoriamente complexos.

Uma abordagem muito melhor é aproveitar o kubernetes como um serviço gerenciado de um fornecedor de nuvem. A nuvem do Azure apresenta uma plataforma kubernetes totalmente gerenciada chamada [AKs (serviço kubernetes do Azure)](https://azure.microsoft.com/services/kubernetes-service/). O AKS abstrai a complexidade e a sobrecarga operacional do gerenciamento de kubernetes. Você consome kubernetes como um serviço de nuvem; A Microsoft assume a responsabilidade por gerenciar e dar suporte a ela. O AKS também se integra perfeitamente com outros serviços do Azure e ferramentas de desenvolvimento.

AKS é uma tecnologia baseada em cluster. Um pool de máquinas virtuais federadas, ou nós, é implantado na nuvem do Azure. Juntos, eles formam um ambiente altamente disponível ou cluster. O cluster aparece como uma entidade simples e direta para seu aplicativo nativo de nuvem. Nos bastidores, o AKS implanta seus serviços em contêineres entre esses nós seguindo uma estratégia predefinida que distribui uniformemente a carga.

## <a name="what-are-the-scaling-benefits"></a>Quais são os benefícios de dimensionamento?

Os serviços criados nos contêineres podem aproveitar os benefícios de dimensionamento fornecidos pelas ferramentas de orquestração como o kubernetes. Os contêineres de design só sabem sobre si mesmos. Depois de ter vários contêineres que precisam trabalhar juntos, você deve organizá-los em um nível mais alto. Organizar grandes números de contêineres e suas dependências compartilhadas, como a configuração de rede, é onde as ferramentas de orquestração entram para economizar o dia! O kubernetes cria uma camada de abstração em grupos de contêineres e os organiza em *pods*. O pods é executado em computadores de trabalho conhecidos como *nós*. Essa estrutura organizada é conhecida como um *cluster*. A Figura 3-3 mostra os diferentes componentes de um cluster kubernetes.

![Componentes do cluster kubernetes. ](./media/kubernetes-cluster-components.png)
 **Figura 3-3**. Componentes do cluster kubernetes.

Dimensionar cargas de trabalho em contêineres é um recurso importante de orquestradores de contêiner. O AKS dá suporte ao dimensionamento automático em duas dimensões: instâncias de contêiner e nós de computação. Juntos, eles oferecem ao AKS a capacidade de responder de maneira rápida e eficiente a picos de demanda e adicionar recursos adicionais. Discutiremos o dimensionamento em AKS mais adiante neste capítulo.

### <a name="declarative-versus-imperative"></a>Declarativa versus imperativo

O kubernetes dá suporte à configuração declarativa e imperativa. A abordagem imperativa envolve a execução de vários comandos que dizem ao kubernetes o que fazer em cada etapa do caminho. Executar esta imagem. Excluir este Pod. Expor esta porta. Com a abordagem declarativa, você cria um arquivo de configuração, chamado de manifesto, para descrever o que você deseja em vez do que fazer. Kubernetes lê o manifesto e transforma o estado final desejado no estado de término real.

Os comandos imperativos são ótimos para aprendizado e experimentação interativa. No entanto, você desejará criar declarativamente arquivos de manifesto kubernetes para adotar uma infraestrutura como abordagem de código, fornecendo implantações confiáveis e reproduzíveis. O arquivo de manifesto se torna um artefato de projeto e é usado em seu pipeline de CI/CD para automatizar implantações de kubernetes.

Se você já tiver configurado o cluster usando comandos imperativos, poderá exportar um manifesto declarativo usando o `kubectl get svc SERVICENAME -o yaml > service.yaml` . Esse comando produz um manifesto semelhante a um mostrado abaixo:

```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: "2019-09-13T13:58:47Z"
  labels:
    component: apiserver
    provider: kubernetes
  name: kubernetes
  namespace: default
  resourceVersion: "153"
  selfLink: /api/v1/namespaces/default/services/kubernetes
  uid: 9b1fac62-d62e-11e9-8968-00155d38010d
spec:
  clusterIP: 10.96.0.1
  ports:
  - name: https
    port: 443
    protocol: TCP
    targetPort: 6443
  sessionAffinity: None
  type: ClusterIP
status:
  loadBalancer: {}
```

Ao usar a configuração declarativa, você pode visualizar as alterações que serão feitas antes de confirmá-las usando `kubectl diff -f FOLDERNAME` a pasta em que os arquivos de configuração estão localizados. Quando tiver certeza de que deseja aplicar as alterações, execute `kubectl apply -f FOLDERNAME` . Adicionar `-R` para processar recursivamente uma hierarquia de pastas.

Você também pode usar a configuração declarativa com outros recursos do kubernetes, um dos quais sendo implantações. As implantações declarativas ajudam a gerenciar versões, atualizações e dimensionamento. Eles instruem o controlador de implantação kubernetes sobre como implantar novas alterações, escalar horizontalmente a carga ou reverter para uma revisão anterior. Se um cluster estiver instável, uma implantação declarativa retornará automaticamente o cluster de volta para um estado desejado. Por exemplo, se um nó deve falhar, o mecanismo de implantação reimplantará uma substituição para atingir o estado desejado

O uso da configuração declarativa permite que a infraestrutura seja representada como código que pode ser verificado e com controle de versão junto com o código do aplicativo. Ele fornece controle de alterações aprimorado e melhor suporte para implantação contínua usando um pipeline de compilação e implantação.

## <a name="what-scenarios-are-ideal-for-containers-and-orchestrators"></a>Quais cenários são ideais para contêineres e orquestradores?

Os cenários a seguir são ideais para usar contêineres e orquestradores.

### <a name="applications-requiring-high-uptime-and-scalability"></a>Aplicativos que exigem alto tempo de atividade e escalabilidade

Os aplicativos individuais que têm altos requisitos de tempo de atividade e escalabilidade são candidatos ideais para arquiteturas nativas de nuvem usando microservices, contêineres e orquestradores. Eles podem ser desenvolvidos em contêineres, testados em ambientes com controle de versão e implantados em produção com tempo de inatividade zero. O uso de clusters kubernetes garante que esses aplicativos também possam ser dimensionados sob demanda e recuperados automaticamente de falhas de nó.

### <a name="large-numbers-of-applications"></a>Grandes números de aplicativos

As organizações que implantam e mantêm um grande número de aplicativos se beneficiam de contêineres e orquestradores. O esforço antecipado de configurar ambientes em contêineres e clusters kubernetes é, principalmente, um custo fixo. Implantar, manter e atualizar aplicativos individuais tem um custo que varia de acordo com o número de aplicativos. Além de um pequeno número de aplicativos, a complexidade da manutenção manual de aplicativos personalizados excede o custo da implementação de uma solução usando contêineres e orquestradores.

## <a name="when-should-you-avoid-using-containers-and-orchestrators"></a>Quando você deve evitar o uso de contêineres e orquestradores?

Se não for possível criar seu aplicativo seguindo os princípios do aplicativo de doze fatores, considere evitar contêineres e orquestradores. Nesses casos, considere uma plataforma de hospedagem baseada em VM ou possivelmente algum sistema híbrido. Com ele, você sempre pode girar determinadas partes de funcionalidade em contêineres separados ou mesmo em funções sem servidor.

## <a name="development-resources"></a>Recursos de desenvolvimento

Esta seção mostra uma lista curta de recursos de desenvolvimento que podem ajudá-lo a começar a usar contêineres e orquestradores para seu próximo aplicativo. Se você estiver procurando orientação sobre como projetar seu aplicativo de arquitetura de microserviços nativos de nuvem, leia o complemento deste livro, [microservices .net: arquitetura para aplicativos .net em contêineres](https://dotnet.microsoft.com/download/thank-you/microservices-architecture-ebook).

### <a name="local-kubernetes-development"></a>Desenvolvimento de kubernetes local

As implantações kubernetes fornecem um ótimo valor em ambientes de produção, mas também podem ser executadas localmente em seu computador de desenvolvimento. Embora você possa trabalhar em microserviços individuais de forma independente, pode haver ocasiões em que você precisará executar todo o sistema localmente, assim como ele será executado quando implantado na produção. Há várias ferramentas que podem ajudar: Minikube e Docker desktop. O Visual Studio também fornece ferramentas para o desenvolvimento do Docker.

### <a name="minikube"></a>Minikube

O que é o Minikube? O projeto Minikube diz "Minikube implementa um cluster kubernetes local no macOS, Linux e Windows." Suas metas principais são "para ser a melhor ferramenta para o desenvolvimento de aplicativos kubernetes local e para dar suporte a todos os recursos do kubernetes que se ajustam". A instalação do Minikube é separada do Docker, mas o Minikube dá suporte a diferentes hipervisores do que o Docker desktop dá suporte. Atualmente, há suporte para os seguintes recursos de kubernetes pelo Minikube:

- DNS
- NodePorts
- ConfigMaps e segredos
- Painéis
- Tempos de execução de contêiner: Docker, RKT, CRI-O e em contêineres
- Habilitando o CNI (interface de rede de contêiner)
- Entrada

Depois de instalar o Minikube, você pode começar rapidamente a usá-lo executando o `minikube start` comando, que baixa uma imagem e inicia o cluster kubernetes local. Depois que o cluster é iniciado, você interage com ele usando os comandos padrão do kubernetes `kubectl` .

### <a name="docker-desktop"></a>Docker Desktop

Você também pode trabalhar com kubernetes diretamente da área de trabalho do Docker no Windows. É sua única opção se você estiver usando contêineres do Windows e também é uma ótima opção para contêineres que não são do Windows. A Figura 3-4 mostra como habilitar o suporte a kubernetes local ao executar o Docker desktop.

![Configurando o kubernetes no Docker desktop](./media/docker-desktop-kubernetes.png)

**Figura 3-4**. Configurando o kubernetes no Docker desktop.

O Docker desktop é a ferramenta mais popular para configurar e executar aplicativos em contêineres localmente. Ao trabalhar com o Docker desktop, você pode desenvolver localmente no mesmo conjunto de imagens de contêiner do Docker que você implantará na produção. O Docker desktop foi projetado para "Compilar, testar e enviar" aplicativos em contêineres localmente. Ele dá suporte a contêineres do Linux e do Windows. Depois de enviar suas imagens por push para um registro de imagem, como o registro de contêiner do Azure ou o Hub do Docker, o AKS pode efetuar pull e implantá-las na produção.

### <a name="visual-studio-docker-tooling"></a>Ferramentas do Docker do Visual Studio

O Visual Studio dá suporte ao desenvolvimento do Docker para aplicativos baseados na Web. Ao criar um novo aplicativo ASP.NET Core, você tem a opção de configurá-lo com o suporte do Docker, como mostra a Figura 3-5.

![Habilitar o suporte do Docker ao Visual Studio](./media/visual-studio-enable-docker-support.png)

**Figura 3-5**. Habilitar o suporte do Docker ao Visual Studio

Quando essa opção é selecionada, o projeto é criado com um `Dockerfile` em sua raiz, que pode ser usado para criar e hospedar o aplicativo em um contêiner do Docker. Um exemplo de Dockerfile é mostrado na Figura 3 -6. git

```docker
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["eShopWeb/eShopWeb.csproj", "eShopWeb/"]
RUN dotnet restore "eShopWeb/eShopWeb.csproj"
COPY . .
WORKDIR "/src/eShopWeb"
RUN dotnet build "eShopWeb.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "eShopWeb.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "eShopWeb.dll"]
```

**Figura 3-6**. Dockerfile gerado pelo Visual Studio

O comportamento padrão quando o aplicativo é executado é configurado para usar o Docker também. A Figura 3-7 mostra as diferentes opções de execução disponíveis em um novo projeto ASP.NET Core criado com o suporte do Docker adicionado.

![Opções de execução do Docker do Visual Studio](./media/visual-studio-docker-run-options.png)

**Figura 3-7**. Opções de execução do Docker do Visual Studio

Além do desenvolvimento local, o [Azure dev Spaces](https://docs.microsoft.com/azure/dev-spaces/) fornece uma maneira conveniente para vários desenvolvedores trabalharem com suas próprias configurações de kubernetes no Azure. Como você pode ver na Figura 3-7, você também pode executar o aplicativo em Azure Dev Spaces.

Além disso, a qualquer momento você pode adicionar o suporte do Docker a um aplicativo ASP.NET Core existente. No Gerenciador de soluções do Visual Studio, clique com o botão direito do mouse no projeto e **adicione**  >  **suporte ao Docker**, conforme mostrado na Figura 3-8.

![Adicionar suporte ao Docker ao Visual Studio](./media/visual-studio-add-docker-support.png)

**Figura 3-8**. Adicionando suporte ao Docker ao Visual Studio

Você também pode adicionar suporte à orquestração de contêiner, também mostrada na Figura 3-8. Por padrão, o orquestrador usa kubernetes e Helm. Depois de escolher o orquestrador, um `azds.yaml` arquivo é adicionado à raiz do projeto e uma `charts` pasta é adicionada contendo os gráficos Helm usados para configurar e implantar o aplicativo no kubernetes. A Figura 3-9 mostra os arquivos resultantes em um novo projeto.

![Adicionar suporte ao Orchestrator no Visual Studio](./media/visual-studio-add-orchestrator-support.png)

**Figura 3-9**. Adicionando suporte de orquestração ao Visual Studio

### <a name="visual-studio-code-docker-tooling"></a>Visual Studio Code ferramentas do Docker

Há várias extensões disponíveis para Visual Studio Code que dão suporte ao desenvolvimento do Docker.

A Microsoft fornece o [Docker para a extensão de Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker). Essa extensão simplifica o processo de adição de suporte a contêineres a aplicativos. Ele aplica Scaffold arquivos necessários, cria imagens do Docker e permite que você depure seu aplicativo dentro de um contêiner. A extensão apresenta um Visual Explorer que torna mais fácil executar ações em contêineres e imagens como iniciar, parar, inspecionar, remover e muito mais. A extensão também dá suporte a Docker Compose permitindo que você gerencie vários contêineres em execução como uma única unidade.

>[!div class="step-by-step"]
>[Anterior](scale-applications.md) 
> [Avançar](leverage-serverless-functions.md)
