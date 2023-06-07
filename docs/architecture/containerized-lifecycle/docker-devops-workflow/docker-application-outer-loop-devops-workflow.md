---
title: Etapas no fluxo de trabalho de DevOps loop externo para um aplicativo de Docker
description: Aprenda as etapas do "loop externo" do fluxo de trabalho de DevOps
ms.date: 08/06/2020
ms.openlocfilehash: 5515c204b09cecba323540572c6769c65c6c93ab
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87915268"
---
# <a name="steps-in-the-outer-loop-devops-workflow-for-a-docker-application"></a>Etapas no fluxo de trabalho de DevOps loop externo para um aplicativo de Docker

A Figura 5-1 traz uma representação de ponta a ponta das etapas que compõem o fluxo de trabalho de loop externo de DevOps. Ele mostra o "loop externo" de DevOps. Quando o código é enviado por push ao repositório, um pipeline de CI é iniciado, seguido pelo pipeline de CD, em que o aplicativo é implantado. As métricas coletadas dos aplicativos implantados são fornecidas para a carga de trabalho de desenvolvimento, na qual ocorre o "loop interno", para que as equipes de desenvolvimento tenham dados reais para responder às necessidades de negócios e dos usuários.

![Diagrama mostrando as 6 etapas do fluxo de trabalho de loop externo DevOps.](./media/docker-application-outer-loop-devops-workflow/overview-dev-ops-outter-loop-workflow.png)

**Figura 5-1**. Fluxo de trabalho do loop externo de DevOps para aplicativos do Docker com ferramentas da Microsoft

Agora, vamos examinar cada uma dessas etapas com mais detalhes.

## <a name="step-1-inner-loop-development-workflow"></a>Etapa 1: fluxo de trabalho de desenvolvimento de loop interno

Esta etapa é explicada em detalhes no Capítulo 4. Entretanto, para recapitular, aqui é onde o loop externo começa, no momento em que um desenvolvedor enviar código por push para o sistema de gerenciamento de controle do código-fonte (como o Git), iniciando ações do pipeline de CI.

## <a name="step-2-source-code-control-integration-and-management-with-azure-devops-services-and-git"></a>Etapa 2: código-fonte controle integração e gerenciamento com Azure DevOps Services e git

Nesta etapa, você precisa ter um sistema de controle de versão para reunir uma versão consolidada de todo o código proveniente de diferentes desenvolvedores na equipe.

Ainda que o SCC (controle do código-fonte) e o gerenciamento de código-fonte possam parecer naturais para a maioria dos desenvolvedores, ao criar aplicativos do Docker em um ciclo de vida de DevOps, é essencial enfatizar que você não deve enviar as imagens do Docker com o aplicativo diretamente para o Registro do Docker global (como o Registro de Contêiner do Azure ou o Docker Hub) do computador do desenvolvedor. Pelo contrário, as imagens do Docker a serem lançadas e implantadas em ambientes de produção devem ser criadas apenas no código-fonte que está sendo integrado em seu build global ou pipeline de CI, com base em seu repositório de código-fonte (como o Git).

As imagens locais, geradas por desenvolvedores, devem ser usadas por eles apenas ao testar dentro de seus próprios computadores. Por isso, é essencial ter o pipeline de DevOps ativado no código de SCC.

O Azure DevOps Services e o Team Foundation Server dão suporte ao Git e ao Controle de Versão do Team Foundation. Você pode escolher entre eles e usá-los para ter uma experiência de ponta a ponta da Microsoft. No entanto, você também pode gerenciar seu código em repositórios externos (como GitHub, repositórios git locais ou subversão) e ainda conseguir se conectar a ele e obter o código como ponto de partida para o pipeline de CI DevOps.

## <a name="step-3-build-ci-integrate-and-test-with-azure-devops-services-and-docker"></a>Etapa 3: Compilar, CI, integrar e testar com o Azure DevOps Services e o Docker

A CI surgiu como um padrão para testar e fornecer softwares modernos. A solução do Docker mantém uma separação clara entre as preocupações das equipes de desenvolvimento e de operações. A imutabilidade das imagens do Docker garante uma implantação repetível entre o que é desenvolvido, testado por meio de CI e executado em produção. O Mecanismo do Docker implantado nos laptops e na infraestrutura de teste do desenvolvedor torna os contêineres portáteis entre ambientes.

Neste ponto, quando tiver um sistema de controle de versão com o código correto enviado, você precisará de um *serviço de build* para usar o código e executar o build e os testes globais.

O fluxo de trabalho interno desta etapa (CI, build, teste) é focado na construção de um pipeline de CI composto pelo repositório de código (Git etc.), pelo servidor de build (Azure DevOps Services), o Mecanismo do Docker e um Registro do Docker.

Você pode usar o Azure DevOps Services como base para criar seus aplicativos e configurar seu pipeline de CI, bem como para publicar os "artefatos" criados em um "repositório de artefatos", o que é explicado na próxima etapa.

Ao usar o Docker para a implantação, os "artefatos finais" a serem implantados são imagens do Docker com seu aplicativo ou serviços inseridos. Essas imagens são enviadas por push ou publicadas em um *Registro do Docker* (um repositório privado, como os que você pode ter no Registro de Contêiner do Azure, ou um público, como o Registro do Docker Hub, que costuma ser usado para imagens de base oficiais).

Este é o conceito básico: o pipeline de CI será iniciado por uma confirmação para um repositório SCC, como o git. A confirmação ("commit") fará com que o Azure DevOps Services execute um trabalho de build dentro de um contêiner do Docker e, após a conclusão bem-sucedida do trabalho, efetuar push de uma imagem do Docker para o Registro do Docker, conforme ilustrado na Figura 5-2. A primeira parte do loop externo envolve as etapas de 1 a 3, de código, execução, depuração e validação, depois o repositório de código até a etapa compilar e testar CI.

![Diagrama mostrando as três etapas envolvidas no fluxo de trabalho de CI.](./media/docker-application-outer-loop-devops-workflow/continuous-integration-steps.png)

**Figura 5-2**. As etapas envolvidas na CI

Estas são as etapas básicas do fluxo de trabalho de CI com o Docker e o Azure DevOps Services:

1. O desenvolvedor envia por push uma confirmação ("commit") para um repositório de SCC (Git/Azure DevOps Services, GitHub etc.).

2. Se você está usando o Azure DevOps Services ou o Git, a CI é interna, o que significa que ela é tão simples quanto marcar uma caixa de seleção no Azure DevOps Services. Se você estiver usando um SCC externo (como o GitHub), um `webhook` notificará o Azure DevOps Services da atualização ou do push para o Git/GitHub.

3. O Azure DevOps Services efetua pull do repositório de SCC, incluindo o Dockerfile descrevendo a imagem, bem como do código do aplicativo e de teste.

4. O Azure DevOps Services cria uma imagem do Docker e a rotula com um número de build.

5. O Azure DevOps Services cria uma instância do contêiner do Docker no Host do Docker provisionado e executa os testes apropriados.

6. Se os testes forem bem-sucedidos, primeiro a imagem será rotulada novamente com um nome significativo, para que você saiba que se trata de um "build privilegiado" (como "/ 1.0.0" ou qualquer outro rótulo) e, em seguida, enviada por push para seu Registro do Docker (Docker Hub, Registro de Contêiner do Azure, DTR etc.)

### <a name="implementing-the-ci-pipeline-with-azure-devops-services-and-the-docker-extension-for-azure-devops-services"></a>Implementando o pipeline de CI com o Azure DevOps Services e a extensão do Docker para Azure DevOps Services

O Azure DevOps Services do Visual Studio contém modelos de Build e Versão que você pode usar no pipeline de CI/CD com que você pode criar imagens do Docker, efetuar push de imagens do Docker para um registro do Docker autenticado, executar imagens do Docker ou executar outras operações oferecidas pela CLI do Docker. Ele também adiciona uma tarefa de Docker Compose que pode ser usada para efetuar push de aplicativos do Docker de vários contêineres e criá-los e executá-los ou executar outras operações oferecidas pela CLI do Docker Compose, conforme mostrado na Figura 5-3.

![Captura de tela do pipeline do Docker CI no Azure DevOps.](./media/docker-application-outer-loop-devops-workflow/docker-ci-pipeline-azure-devops.png)

**Figura 5-3**. O pipeline de CI do Docker no Azure DevOps Services, incluindo modelos de Build e Versão e as tarefas associadas.

Você pode usar esses modelos e tarefas para construir seus artefatos de CI/CD para build/teste e implantação no Service Fabric do Azure, no Serviço de Kubernetes do Azure e em ofertas semelhantes.

Com essas tarefas do Visual Studio Team Services, um Host/VM de build do Linux-Docker provisionado no Azure e o registro do Docker de sua preferência (Registro de Contêiner do Azure, Docker Hub, DTR privado do Docker ou qualquer outro registro do Docker), você pode montar seu pipeline de CI do Docker de maneira muito consistente.

***Requisitos:***

- Azure DevOps Services ou, para instalações locais, Team Foundation Server 2015, Atualização 3 ou posterior.

- Um agente do Azure DevOps Services que tem os binários de Docker.

  Uma maneira fácil de criar um desses agentes é usar o Docker para executar um contêiner baseado na imagem do Docker de agente do Azure DevOps Services.

> [!INFORMAÇÕES] Para ler mais sobre como montar um pipeline de CI do Docker do Azure DevOps Services e exibir as instruções passo a passo, visite estes sites:
>
> - Como executar um agente do Visual Studio Team Services (agora, Azure DevOps Services) como um contêiner do Docker: \
>   <https://hub.docker.com/_/microsoft-azure-pipelines-vsts-agent>
>
> - Como criar imagens Linux Docker do .NET Core com o Azure DevOps Services: \
>   <https://docs.microsoft.com/archive/blogs/stevelasker/building-net-core-linux-docker-images-with-visual-studio-team-services>
>
> - Como criar um computador de build do Visual Studio Team Services baseada em Linux com suporte para Docker: \
>   <http://donovanbrown.com/post/2016/06/03/Building-a-Linux-Based-Visual-Studio-Team-Service-Build-Machine-with-Docker-Support>

### <a name="integrate-test-and-validate-multi-container-docker-applications"></a>Integrar, testar e validar aplicativos do Docker com vários contêineres

Normalmente, a maioria dos aplicativos do Docker são compostos por vários contêineres em vez de apenas um. Um bom exemplo é um aplicativo orientado a microsserviços para o qual você teria um contêiner por microsserviço. Entretanto, mesmo sem seguir estritamente os padrões da abordagem de microsserviços, é provável que seu aplicativo do Docker seja composto por vários contêineres ou serviços.

Portanto, após criar os contêineres de aplicativo no pipeline de CI, você também precisará implantar, integrar e testar o aplicativo como um todo, com todos os seus contêineres dentro de um host do Docker de integração ou até mesmo em um cluster de teste para o qual seus contêineres são distribuídos.

Se estiver usando um único host, você poderá usar comandos do Docker, como docker-compose, para compilar e implantar contêineres relacionados ao testar e à validação do ambiente do Docker em uma única VM. No entanto, se estiver trabalhando com um cluster de orquestrador, como Docker Swarm, Kubernetes ou DC/OS, você precisará implantar seus contêineres por meio de um mecanismo ou orquestrador diferente, dependendo do cluster/agendador selecionado.

Estes são vários tipos de testes que podem ser executados em contêineres do Docker:

- Testes de unidade para contêineres do Docker

- Grupos de testes de aplicativos ou microsserviços inter-relacionados

- Teste em versões de produção e "canário"

O ponto importante é que, ao executar testes de integração e funcionais, você precisa executar esses testes de fora dos contêineres. Testes não são contidos nem executados nos contêineres que você está implantando, pois os contêineres se baseiam em imagens estáticas que devem ser exatamente iguais às que você implantará para produção.

Uma opção prática ao testar cenários mais avançados, como incluir vários clusters (cluster de produção, de preparo e de teste), é publicar as imagens em um registro, para que possa ser testada em vários clusters.

### <a name="push-the-custom-application-docker-image-into-your-global-docker-registry"></a>Efetue push da imagem do Docker do aplicativo personalizado para seu Registro do Docker global

Após as imagens do Docker serem testadas e validadas, convém marcar e publicá-las em seu registro do Docker. O registro do Docker é uma peça fundamental no ciclo de vida do aplicativo do Docker, pois é o local central no qual você armazena seu teste personalizado (também conhecido como "imagens privilegiadas") para implantação em ambientes de produção e de garantia de qualidade.

De modo semelhante a como o código do aplicativo armazenado em seu repositório de SCC (Git etc.) é sua "fonte da verdade", o registro do Docker é a "fonte da verdade" de seus bits ou aplicativo binário a ser implantado nos ambientes de produção ou garantia de qualidade.

Normalmente, você pode querer ter repositórios privados para suas imagens personalizadas em um repositório privado no Registro de Contêiner do Azure ou em um registro local, como o Docker Trusted Registry, ou em um registro de nuvem pública com acesso restrito (como o Docker Hub). Neste último caso, se o código não for software livre, você precisará confiar na segurança do fornecedor. De qualquer forma, o método usado é semelhante e é baseado no comando `docker push`, conforme mostrado na Figura 5-4.

![Diagrama mostrando o envio de imagens personalizadas para um registro de contêiner.](./media/docker-application-outer-loop-devops-workflow/docker-push-custom-images.png)

**Figura 5-4**. Publicando imagens personalizadas no Registro do Docker

Na etapa 3, para integração e teste (CI) do build, você pode publicar as imagens do Docker resultantes em um registro público ou privado. Há várias ofertas de registros do Docker de fornecedores de nuvem, como o Registro de Contêiner do Azure, o Registro de Contêiner do Amazon Web Services, o Registro de Contêiner do Google, o Registro Quay e assim por diante.

Usando as tarefas do Docker, você pode efetuar push de um conjunto de imagens de serviço definidas por um arquivo `docker-compose.yml`, com várias marcas, para um registro do Docker autenticado (como o Registro de Contêiner do Azure), conforme mostrado na Figura 5-5.

![Captura de tela mostrando a etapa para publicar imagens em um registro.](./media/docker-application-outer-loop-devops-workflow/publish-custom-image-to-docker-registry.png)

**Figura 5-5**. Usando o Azure DevOps Services para publicar imagens personalizadas em um Registro do Docker

> [!INFORMAÇÕES] Para obter mais informações sobre o Registro de Contêiner do Azure, consulte <https://aka.ms/azurecontainerregistry>.

## <a name="step-4-cd-deploy"></a>Etapa 4: CD, implantar

A imutabilidade das imagens do Docker garante uma implantação repetível com relação ao que é desenvolvido, testado por meio de CI e executado em produção. Depois de ter as imagens de Docker do aplicativo publicadas em seu registro do Docker (público ou privado), você pode implantá-las em vários ambientes (produção, garantia de qualidade, preparo etc.) de seu pipeline de CD usando tarefas de pipeline do Azure DevOps Services ou o Release Management do Azure DevOps Services.

No entanto, neste ponto, depende do tipo de aplicativo do Docker você está usando. Implantar um aplicativo simples (do ponto de vista da composição e implantação), como um aplicativo monolítico composto por alguns contêineres ou serviços e implantado em alguns servidores ou VMs, é diferente de implantar um aplicativo mais complexo, como um aplicativo orientado a microsserviços com recursos em hiperescala. Esses dois cenários são explicados nas seções a seguir.

### <a name="deploying-composed-docker-applications-to-multiple-docker-environments"></a>Implantando aplicativos de Docker compostos em vários ambientes do Docker

Primeiro, vamos ver o cenário menos complexo: implantar em hosts de Docker simples (VMs ou servidores) em um único ambiente ou em vários ambientes (garantia de qualidade, preparo e produção). Neste cenário, internamente, seu pipeline de CD pode usar docker-compose (das tarefas de implantação do Azure DevOps Services) para implantar os aplicativos do Docker com o conjunto relacionado de contêineres ou serviços, conforme ilustrado na Figura 5-6.

![Diagrama que mostra a etapa de implantação do CD implantando em três ambientes.](./media/docker-application-outer-loop-devops-workflow/deploy-app-containers-to-docker-host-environments.png)

**Figura 5-6**. Implantando contêineres de aplicativo no registro de ambientes de host de Docker simples

A Figura 5-7 destaca como você pode conectar sua CI de build a ambientes de garantia de qualidade/teste por meio do Azure DevOps Services clicando em Docker Compose na caixa de diálogo Adicionar Tarefa. No entanto, ao implantar em ambientes de preparo ou produção, normalmente você usaria recursos do Release Management que lidam com vários ambientes (como garantia de qualidade, preparo e produção). Se você está implantando em hosts do Docker únicos, ele está usando a tarefa "Docker Compose" do Azure DevOps Services (que está invocando o comando `docker-compose up` nos bastidores). Se você está implantando no AKS (Serviço de Kubernetes do Azure), ele usa a tarefa Implantação do Docker, conforme explicado na seção a seguir.

![Captura de tela mostrando a caixa de diálogo adicionar tarefas da tarefa Docker Compose.](./media/docker-application-outer-loop-devops-workflow/add-tasks-docker-compose.png)

**Figura 5-7**. Adicionando uma tarefa Docker Compose em um pipeline do Azure DevOps Services

Quando você cria uma versão no Azure DevOps Services, ele usa um conjunto de artefatos de entrada. Esses artefatos devem ser imutáveis durante o tempo de vida da versão, em todos os ambientes. Quando você introduz contêineres, os artefatos de entrada identificam imagens em um registro para implantação. Dependendo de como essas imagens estão identificadas, não há garantia de que permaneçam as mesmas pela duração da versão, sendo o caso mais óbvio quando você referencia `myimage:latest` de um arquivo `docker-compose`.

Os modelos do Azure DevOps Services permitem gerar artefatos de build que contêm resumos de imagens específicas do registro que, certamente, identificarão exclusivamente o mesmo binário da imagem. Eles devem ser usados como entrada para uma versão.

### <a name="managing-releases-to-docker-environments-by-using-azure-devops-services-release-management"></a>Gerenciando versões para ambientes do Docker usando o Release Management do Azure DevOps Services

Por meio dos modelos do Azure DevOps Services, você pode criar uma nova imagem, publicá-la em um registro do Docker, executá-la em hosts Linux ou Windows e usar comandos como `docker-compose` para implantar vários contêineres como um aplicativo inteiro, tudo isso por meio das funcionalidades de Release Management do Azure DevOps Services destinadas a vários ambientes, conforme mostrado na Figura 5-8.

![Captura de tela mostrando a configuração das versões do Docker Compose.](./media/docker-application-outer-loop-devops-workflow/configure-docker-compose-release.png)

**Figura 5-8**. Configurando tarefas de Docker Compose do Azure DevOps Services do Release Management do Azure DevOps Services

No entanto, tenha em mente que o cenário mostrado na Figura 5-6 e implementado na Figura 5-8 é simples (ele está implantando VMs e hosts únicos do Docker e haverá um único contêiner ou instância por imagem) e provavelmente deveria ser usada apenas para cenários de desenvolvimento ou teste. Na maioria dos cenários de produção corporativos, você gostaria de ter HA (alta disponibilidade) e escalabilidade de fácil gerenciamento por meio do balanceamento de carga entre vários nós, servidores e VMs, além de "failovers inteligentes", de forma que, se um servidor ou nó falhar, seus serviços e contêineres serão movidos para outro servidor de host ou VM. Nesse caso, você precisa de tecnologias mais avançadas, como clusters de contêiner, orquestradores e agendadores. Assim, a maneira de implantar esses clusters é manipulando os cenários avançados explicados na próxima seção.

### <a name="deploying-docker-applications-to-docker-clusters"></a>Implantando aplicativos do Docker em clusters do Docker

A natureza dos aplicativos distribuídos exige recursos de computação que também são distribuídos. Para ter funcionalidades em escala de produção, você precisa ter funcionalidades de clustering que fornecem alta escalabilidade e alta disponibilidade com base em recursos em pool.

Você poderia implantar contêineres manualmente nesses clusters de uma ferramenta de CLI ou de uma interface do usuário da Web, mas você deveria reservar esse tipo de trabalho manual para finalidades de teste e gerenciamento de implantação especial, como expansão ou monitoramento.

Do ponto de vista da CD e especificamente do Azure DevOps Services, você pode executar tarefas de implantação feitas especialmente de seus ambientes de Release Management do Azure DevOps Services que implantarão seus aplicativos em contêineres em clusters distribuídos no Serviço de Contêiner, conforme ilustrado na Figura 5-9.

![Diagrama que mostra a etapa de implantação do CD implantando em orquestradores.](./media/docker-application-outer-loop-devops-workflow/cd-deploy-to-orchestrators.png)

**Figura 5-9**. Implantando aplicativos distribuídos no Serviço de Contêiner

Inicialmente, ao implantar em determinados clusters ou orquestradores, tradicionalmente você usaria scripts de implantação e mecanismos específicos a cada orquestrador (ou seja, o Kubernetes e o Service Fabric têm mecanismos de implantação diferentes) em vez da ferramenta `docker-compose`, mais simples e mais fácil de usar, no arquivo de definição `docker-compose.yml`. No entanto, graças à tarefa de implantação do Docker Azure DevOps Services, mostrada na Figura 5-10, agora você também pode implantar nos orquestradores com suporte usando apenas seu arquivo familiar, `docker-compose.yml` pois a ferramenta executa essa "tradução" para você (de seu `docker-compose.yml` arquivo para o formato necessário para o orquestrador).

![Captura de tela mostrando a tarefa implantar no kubernetes.](./media/docker-application-outer-loop-devops-workflow/add-deploy-to-kubernetes-task.png)

**Figura 5-10**. Adicionando a tarefa Implantar no Kubernetes ao seu ambiente

A Figura 5-11 demonstra como você pode editar o a tarefa Implantar no Kubernetes com as seções disponíveis para configuração. Esta é a tarefa que recuperará as imagens do Docker personalizadas prontas para uso para implantação como contêineres no cluster.

![Captura de tela mostrando a configuração de tarefa implantar em kubernetes.](./media/docker-application-outer-loop-devops-workflow/edit-deploy-to-kubernetes-task.png)

**Figura 5-11**. Definição da tarefa Implantação do Docker em CD/SO do ACS

> [!INFORMAÇÕES] Para ler mais sobre o pipeline de CD com o Azure DevOps Services e o Docker, visite <https://azure.microsoft.com/services/devops/pipelines>

## <a name="step-5-run-and-manage"></a>Etapa 5: executar e gerenciar

Uma vez que executar e gerenciar aplicativos no nível de produção empresarial é um tema maior e devido ao tipo de operações e de pessoas que trabalham nesse nível (operações de TI), bem como ao escopo grande desta área, o próximo capítulo inteiro é dedicado a explicá-lo.

## <a name="step-6-monitor-and-diagnose"></a>Etapa 6: monitorar e diagnosticar

Este tópico também é abordado no próximo capítulo como parte das tarefas que a TI executa em sistemas de produção. No entanto, é importante destacar que os insights obtidos nesta etapa devem ser informados à equipe de desenvolvimento para que o aplicativo seja aprimorado constantemente. Desse ponto de vista, ele também faz parte de DevOps, embora as operações e tarefas normalmente sejam executadas pela TI.

Somente quando o monitoramento e o diagnóstico estão 100% dentro do realm do DevOps, os processos de monitoramento e análise são realizados pela equipe de desenvolvimento em ambientes de teste ou beta. Isso é feito executando testes de carga ou monitorando ambientes de garantia de qualidade ou beta, em que testadores beta testam as novas versões.

>[!div class="step-by-step"]
>[Anterior](index.md) 
> [Avançar](create-ci-cd-pipelines-azure-devops-services-aspnetcore-kubernetes.md)
