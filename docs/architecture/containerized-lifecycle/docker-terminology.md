---
title: Terminologia do Docker
description: Aprenda a terminologia básica usada todos os dias ao trabalhar com o Docker.
ms.date: 08/06/2020
ms.openlocfilehash: b47639a2995c3a0a30ea7111c16bbea21f1048ba
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87915195"
---
# <a name="docker-terminology"></a>Terminologia do Docker

Esta seção lista os termos e definições que você deve conhecer antes de se aprofundar no Docker. Para obter mais definições, consulte o amplo [glossário](https://docs.docker.com/glossary/) fornecido pelo Docker.

**Imagem de contêiner**: um pacote com todas as dependências e informações necessárias para criar um contêiner. Uma imagem inclui todas as dependências (como estruturas), além da configuração de implantação e execução a ser usada por um runtime de contêiner. Geralmente, uma imagem deriva de várias imagens base que são camadas empilhadas umas sobre as outras para formar o sistema de arquivos do contêiner. Uma imagem é imutável depois de ser criada.

**Dockerfile**: um arquivo de texto que contém instruções para criar uma imagem do Docker. É como um script em lotes, a primeira linha declara a imagem base com a qual começar e, em seguida, siga as instruções para instalar os programas necessários, copiar os arquivos e assim por diante, até obter o ambiente de trabalho que precisa.

**Build**: a ação de criar de uma imagem de contêiner com base nas informações e no contexto fornecido pelo Dockerfile, além de arquivos adicionais na pasta em que a imagem é criada. Você pode criar imagens com o seguinte comando do Docker:

```bash
docker build
```

**Contêiner**: uma instância de uma imagem do Docker. Um contêiner representa a execução de um único aplicativo, processo ou serviço. Consiste no conteúdo de uma imagem do Docker, um ambiente de execução e um conjunto padrão de instruções. Ao dimensionar um serviço, você cria várias instâncias de um contêiner da mesma imagem. Ou um trabalho em lotes pode criar vários contêineres da mesma imagem, passando parâmetros diferentes para cada instância.

**Volumes**: oferecem um sistema de arquivos gravável que o contêiner pode usar. Uma vez que as imagens são somente leitura, mas a maioria dos programas precisam gravar para o sistema de arquivos, os volumes adicionam uma camada gravável sobre a imagem de contêiner, de modo que os programas têm acesso a um sistema de arquivos gravável. O programa não sabe que está acessando um sistema de arquivos em camadas, é apenas o sistema de arquivos como de costume. Os volumes ficam no sistema de host e são gerenciados pelo Docker.

**Marcação**: uma marca ou um rótulo pode ser aplicado a imagens para que imagens ou versões diferentes da mesma imagem (dependendo do número de versão ou do ambiente de destino) possam ser identificadas.

**Build de vários estágios**: é um recurso disponível no Docker 17.05 e posteriores que ajuda a reduzir o tamanho das imagens finais. Por exemplo, uma imagem de base grande, contendo o SDK, pode ser usada para compilar e publicar e, em seguida, uma pequena imagem base somente de tempo de execução pode ser usada para hospedar o aplicativo.

**Repositório**: uma coleção de imagens do Docker relacionadas, rotulada com uma marcação que indica a versão da imagem. Alguns repositórios contêm várias variantes de uma imagem específica, como uma imagem que contém SDKs (mais pesado), uma imagem contendo apenas tempos de execução (mais leves), etc. Essas variantes podem ser marcadas com marcas. Um único repositório pode conter variantes de plataforma, como uma imagem do Linux e uma imagem do Windows.

**Registro**: um serviço que dá acesso aos repositórios. O registro padrão para as imagens mais públicas é o [Docker Hub](https://hub.docker.com/) (propriedade da Docker como uma organização). Um registro geralmente contém repositórios de várias equipes. As empresas geralmente têm registros privados para armazenar e gerenciar as imagens que criaram. O Registro de Contêiner do Azure é outro exemplo.

**Imagem de vários arcos**: para várias arquiteturas, trata-se de um recurso que simplifica a seleção da imagem apropriada, de acordo com a plataforma em que o Docker está em execução. Por exemplo, quando um Dockerfile solicita uma imagem base **do MCR.Microsoft.com/dotnet/Core/SDK:3.1** a partir do registro, ele realmente Obtém **3,1-SDK-** los-1909, **3,1-sdk-los Server-1809** ou **3,1-SDK-Buster-Slim**, dependendo do sistema operacional e da versão em que o Docker está em execução.

**Docker Hub**: um registro público para carregar imagens e trabalhar com elas. O Docker Hub hospeda imagens do Docker, registros públicos ou privados, cria gatilhos e ganchos da Web e integra-se com o GitHub e o Bitbucket.

**Registro de Contêiner do Azure**: um recurso público para trabalhar com imagens do Docker e seus componentes no Azure. Fornece um registro que está perto de suas implantações no Azure e que permite controlar o acesso, tornando possível usar as permissões e os grupos do Azure Active Directory.

**Docker Trusted Registry (DTR)**: serviço de Registro do Docker que pode ser instalado localmente para funcionar no datacenter e na rede da organização. É conveniente para imagens privadas que devem ser gerenciadas dentro da empresa. O Docker Trusted Registry é parte do produto Docker Datacenter. Para saber mais, consulte [Docker Trusted Registry (DTR)](https://www.docker.com/sites/default/files/Docker%20Trusted%20Registry.pdf).

**Docker Community Edition (CE)**: ferramentas de desenvolvimento para Windows e macOS para build, execução e teste de contêineres localmente. O Docker CE para Windows fornece os ambientes de desenvolvimento para Linux e contêineres do Windows. O host do Docker do Linux no Windows é baseado em uma máquina virtual [Hyper-V](https://www.microsoft.com/cloud-platform/server-virtualization). O host para contêineres do Windows se baseia diretamente no Windows. Docker CE para Mac baseia-se na estrutura do Apple Hypervisor e o [xhyve hypervisor](https://github.com/mist64/xhyve), que fornece uma máquina virtual do host Linux Docker no Mac OS X. O Docker CE para Windows e Mac substitui o Docker Toolbox, que foi baseado no Oracle VirtualBox.

**Docker Enterprise Edition (EE)**: uma versão empresarial das ferramentas do Docker para desenvolvimento em Linux e Windows.

**Compose**: uma ferramenta de linha de comando e formato de arquivo YAML com metadados para definir e executar aplicativos de vários contêineres. Você define um único aplicativo com base em várias imagens com um ou mais arquivos .yml que podem substituir valores dependendo do ambiente. Depois de criar as definições, você pode implantar todo o aplicativo de vários contêineres com um único comando (docker-compose up), que cria um contêiner por imagem no host do Docker.

**Cluster**: uma coleção de hosts do Docker expostos como um único host virtual, para que o aplicativo possa ser dimensionado para várias instâncias dos serviços distribuídos em vários hosts do cluster. Os clusters do Docker podem ser criados com o Kubernetes, o Azure Service Fabric, o Docker Swarm e o Mesosphere DC/OS.

**Orchestrator**: uma ferramenta que simplifica o gerenciamento de clusters e hosts do Docker. Os orquestradores permitem gerenciar imagens, contêineres e hosts por meio de uma CLI (interface de linha de comando) ou uma interface do usuário gráfica. É possível gerenciar a rede de contêiner, configurações, balanceamento de carga, descoberta de serviço, alta disponibilidade, configuração de host do Docker e muito mais. Um orquestrador é responsável por executar, distribuir, dimensionar e reparar de cargas de trabalho em uma coleção de nós. Normalmente, produtos de orquestrador são os mesmos que fornecem infraestrutura de cluster, como Kubernetes e Azure Service Fabric, além de outras ofertas no mercado.

>[!div class="step-by-step"]
>[Anterior](what-is-docker.md) 
> [Avançar](docker-containers-images-and-registries.md)
