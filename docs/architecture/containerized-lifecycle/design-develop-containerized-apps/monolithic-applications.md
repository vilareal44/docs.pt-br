---
title: Aplicativos monolíticos
description: Entenda os principais conceitos da implantação de aplicativos monolíticos em contêineres.
ms.date: 08/06/2020
ms.openlocfilehash: f188a2ff576436d9378030e0a858ffb8110dad17
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87915439"
---
# <a name="monolithic-applications"></a>Aplicativos monolíticos

Neste cenário, você está criando um aplicativo ou serviço Web único e monolítico e implantando-o como um contêiner. A estrutura do aplicativo pode não ser monolítica, mas pode compreender várias bibliotecas, componentes ou até mesmo camadas (camada de aplicativo, de domínio, de acesso a dados etc.). Externamente, ele é um único contêiner, como um único processo, um único aplicativo Web ou um único serviço.

Para gerenciar esse modelo, implante um contêiner único para representar o aplicativo. Para dimensionar, adicione mais cópias com o balanceador de carga na frente. A simplicidade está em gerenciar uma implantação única em um contêiner ou VM (máquina virtual) único.

Seguindo o princípio de que um contêiner executa uma ação e faz isso em um processo, o padrão monolítico pode gerar um conflito. É possível incluir vários componentes/bibliotecas ou camadas internas em cada contêiner, conforme ilustrado na Figura 4-1.

![Diagrama mostrando um aplicativo monolítico que se expande clonando o aplicativo.](./media/monolithic-applications/monolithic-application-architecture-example.png)

**Figura 4-1.** Um exemplo de arquitetura de aplicativo monolítico

Um aplicativo monolítico tem todas ou a maioria das funcionalidades em um único processo ou contêiner, além dividir os componentes em camadas ou bibliotecas internas. O ponto negativo dessa abordagem ficará evidente se ou quando o aplicativo crescer e for necessário dimensioná-lo. Se o aplicativo inteiro for dimensionado, isso não será realmente um problema. Entretanto, na maioria dos casos, algumas partes do aplicativo são os pontos de redução que exigem dimensionamento, enquanto outros componentes são menos utilizados.

Usando o exemplo típico de comércio eletrônico, o que você provavelmente precisa é dimensionar o componente de informações do produto. Uma quantidade muito maior de clientes procura produtos em vez de comprá-los. Mais clientes usam o carrinho em vez do pipeline de pagamento. Menos clientes fazem comentários ou exibem o histórico de compras. E você provavelmente tem apenas um grupo de funcionários, em uma única região, que precisa gerenciar o conteúdo e as campanhas de marketing. Ao dimensionar o design monolítico, todo o código é implantado várias vezes.

Além do problema de “ter que dimensionar tudo”, a alteração de um único componente exige um novo teste completo de todo o aplicativo e uma reimplantação completa de todas as instâncias.

A abordagem monolítica é comum e muitas organizações estão desenvolvendo com essa abordagem de arquitetura. Várias têm resultados bons o suficiente, enquanto outras encontram obstáculos. Muitas criaram seus aplicativos nesse modelo porque as ferramentas e a infraestrutura eram muito difíceis para criar SOAs e elas não viam a necessidade até que o aplicativo crescesse.

De uma perspectiva de infraestrutura, cada servidor pode executar vários aplicativos no mesmo host e ter um índice de eficiência razoável de uso de recursos, conforme mostrado na Figura 4-2.

![Um diagrama que mostra um host com vários aplicativos em contêineres separados.](./media/monolithic-applications/host-with-multiple-apps-containers.png)

**Figura 4-2.** Um host executando vários aplicativos/contêineres

Por fim, de uma perspectiva de disponibilidade, os aplicativos monolíticos devem ser implantados como um todo. Isso significa que se você precisar *parar e iniciar*, todas as funcionalidades e todos os usuários serão afetados durante a janela de implantação. Em certos casos, o uso do Azure e dos contêineres pode minimizar essas situações e reduzir a probabilidade de tempo de inatividade do aplicativo, como você pode ver na Figura 4-3.

Os aplicativos monolíticos no Microsoft Azure podem ser implantados por meio de VMs dedicadas a cada instância. Com os [Conjuntos de Dimensionamento de VMs do Azure](https://docs.microsoft.com/azure/virtual-machine-scale-sets/), é possível dimensionar as VMs com facilidade.

Também é possível usar os [Serviços de Aplicativos do Azure](https://azure.microsoft.com/services/app-service/) para executar aplicativos monolíticos e dimensionar instâncias com facilidade, sem a necessidade de gerenciar as VMs. Os Serviços de Aplicativos do Azure também podem executar instâncias únicas de contêineres do Docker, simplificando a implantação.

É possível implantar várias VMs como hosts do Docker e executar qualquer quantidade de contêineres por VM. Em seguida, usando o Azure Load Balancer, conforme ilustrado na Figura 4-3, você pode gerenciar o dimensionamento.

![Um diagrama que mostra um aplicativo monolítico dimensionado para hosts diferentes.](./media/monolithic-applications/multiple-hosts-from-single-docker-container.png)

**Figura 4-3**. Vários hosts expandindo um único aplicativo Docker

É possível gerenciar a implantação dos próprios hosts por meio de técnicas de implantação tradicionais.

Você pode gerenciar os contêineres do Docker na linha de comando usando comandos como `docker run` e `docker-compose up`. Além disso, também é possível automatizá-los nos pipelines de CD (entrega contínua) e implantá-los nos hosts do Docker por meio do Azure DevOps Services, por exemplo.

## <a name="monolithic-application-deployed-as-a-container"></a>Aplicativo monolítico implantado como um contêiner

Há benefícios em usar contêineres para gerenciar implantações monolíticas. O dimensionamento das instâncias de contêineres é muito mais rápido e fácil do que a implantação de VMs adicionais.

Implantar atualizações como imagens do Docker é muito mais rápido e eficiente em termos de rede. Os contêineres do Docker costumam ser iniciados em segundos, o que agiliza as distribuições. Desmontar um contêiner do Docker é tão fácil quanto invocar o comando `docker stop` e geralmente leva menos de um segundo.

Como os contêineres são inerentemente imutáveis por design, você nunca precisa se preocupar com VMs corrompidas porque um script de atualização esqueceu de levar em conta alguma configuração específica ou um arquivo deixado no disco.

Embora aplicativos monolíticos possam se beneficiar do Docker, estamos mencionando apenas as dicas de benefícios. Outros benefícios de gerenciar contêineres são decorrentes da implantação com orquestradores de contêiner, que gerenciam as diversas instâncias e o ciclo de vida de cada instância de contêiner. Dividir o aplicativo monolítico em subsistemas que podem ser dimensionados, desenvolvidos e implantados individualmente é o ponto de entrada no universo dos microsserviços.

Para saber mais sobre como "migrar e deslocar" aplicativos monolíticos com contêineres e como você pode modernizar seus aplicativos, leia este guia adicional da Microsoft, [modernizando aplicativos .net existentes com contêineres de nuvem e Windows do Azure](../../modernize-with-azure-containers/index.md), que também pode ser baixado como PDF <https://aka.ms/LiftAndShiftWithContainersEbook> .

## <a name="publish-a-single-docker-container-app-to-azure-app-service"></a>Publicar um aplicativo de contêiner único do Docker no Serviço de Aplicativo do Azure

Seja para validar rapidamente um contêiner implantado no Azure ou simplesmente quando um aplicativo de contêiner único, os Serviços de Aplicativos do Azure é uma ótima maneira de oferecer serviços escalonáveis de contêiner único.

Usar o Serviço de Aplicativo do Azure é intuitivo e você pode começar a trabalhar rapidamente, porque ele oferece uma ótima integração com o Git para processar seu código, compilá-lo no Microsoft Visual Studio e implantá-lo diretamente no Azure. No entanto, tradicionalmente (sem o Docker), se você precisasse de outras funcionalidades, estruturas ou dependências não compatíveis com os Serviços de Aplicativos, deveria aguardar até a equipe do Azure atualizar essas dependências no Serviço de Aplicativo ou migrar para outros serviços como o Service Fabric, os Serviços de Nuvem ou até mesmo VMs simples, em que você tem mais controle e pode instalar um componente ou estrutura obrigatória para o aplicativo.

Agora, conforme mostrado na Figura 4-4, ao usar o Visual Studio 2017, o suporte para contêineres no Serviço de Aplicativo do Azure oferece a capacidade de incluir tudo o que você deseja no ambiente do aplicativo. Se você tiver adicionado uma dependência ao aplicativo porque ele está sendo executado em um contêiner, terá a capacidade de incluir essas dependências no Dockerfile ou na imagem do Docker.

![Captura de tela da caixa de diálogo Criar serviço de aplicativo mostrando um registro de contêiner.](./media/monolithic-applications/publish-azure-app-service-container.png)

**Figura 4-4**. Publicação de um contêiner no Serviço de Aplicativo do Azure nos aplicativos/contêineres do Visual Studio

A Figura 4-4 também mostra que o fluxo de publicação efetua push de uma imagem por meio de um Contêiner de Registro, que pode ser o Registro de Contêiner do Azure (um registro próximo às implantações no Azure e protegido por grupos e contas do Azure Active Directory) ou outros registros do Docker, como o Docker Hub ou um registro local.

>[!div class="step-by-step"]
>[Anterior](common-container-design-principles.md) 
> [Avançar](state-and-data-in-docker-applications.md)
