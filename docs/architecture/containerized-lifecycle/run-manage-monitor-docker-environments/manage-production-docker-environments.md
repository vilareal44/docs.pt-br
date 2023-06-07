---
title: Gerenciar ambientes de produção do Docker
description: Saiba os principais pontos do gerenciamento de um ambiente de produção baseado em contêiner.
ms.date: 08/06/2020
ms.openlocfilehash: 11880a523d6ff79c9646fd1e174f380779d00dcc
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87914933"
---
# <a name="manage-production-docker-environments"></a>Gerenciar ambientes de produção do Docker

O gerenciamento e a orquestração de clusters é o processo de controlar um grupo de hosts. Isso pode envolver a adição e a remoção de hosts de um cluster, obter informações sobre o estado atual dos hosts e os contêineres e iniciar e interromper processos. O gerenciamento e a orquestração de clusters estão estreitamente vinculados ao agendamento, porque o agendador precisa ter acesso a cada host no cluster para agendar serviços. Por esse motivo, a mesma ferramenta é geralmente usada para ambas as finalidades.

## <a name="container-service-and-management-tools"></a>Serviço de Contêiner e ferramentas de gerenciamento

O Serviço de Contêiner fornece implantação rápida de soluções populares de orquestração e clustering de contêiner open-source. Ele utiliza imagens do Docker para garantir que os contêineres de aplicativo sejam totalmente portáteis. Com o Serviço de Contêiner, é possível implantar DC/OS (desenvolvido pela Mesosphere e o Apache Mesos) e clusters do Docker Swarm com modelos do Azure Resource Manager ou o portal do Azure para garantir que esses aplicativos possam ser dimensionados para milhares – até mesmo dezenas de milhares – de contêineres.

Você implanta esses clusters usando Conjuntos de Escala de Máquina Virtual do Azure e tiram proveito das ofertas de rede e armazenamento do Azure. Para acessar o Serviço de Contêiner, você precisa de uma assinatura do Azure. O Serviço de Contêiner permite aproveitar os recursos empresariais do Azure e manter a portabilidade do aplicativo, até mesmo nas camadas de orquestração.

A Tabela 6-1 lista as ferramentas de gerenciamento comuns relacionadas aos seus orquestradores, agendadores e à plataforma de clustering.

**Tabela 6-1**. Ferramentas de gerenciamento do Docker

| Ferramentas de gerenciamento | Descrição | Orquestradores relacionados |
|------------------|-------------|-----------------------|
| [Azure Monitor para Contêineres](https://docs.microsoft.com/azure/monitoring/monitoring-container-insights-overview) | Ferramenta de gerenciamento do Kubernetes dedicada ao Azure | AKS (Serviços do Kubernetes do Azure) |
| [Interface do usuário da Web do amKubernetes (Dashboard)](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) | A ferramenta de gerenciamento do Kubernetes pode monitorar e gerenciar o cluster local do Kubernetes | AKS (Serviço de Kubernetes do Azure)<br/>Kubernetes local |
| [Portal do Azure para o Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal)<br/>[Azure Service Fabric Explorer](https://docs.microsoft.com/azure/service-fabric/service-fabric-visualizing-your-cluster) | Versão online e para área de trabalho para gerenciar clusters do Service Fabric no Azure, localmente, no desenvolvimento local e em outras nuvens | Azure Service Fabric |
| [Monitoramento de contêiner (Azure Monitor)](https://docs.microsoft.com/azure/azure-monitor/insights/containers) | Gerenciamento geral de contêiner e solução de monitoramento. Pode gerenciar clusters do Kubernetes por meio do [Azure Monitor para Contêineres](https://docs.microsoft.com/azure/monitoring/monitoring-container-insights-overview). | Azure Service Fabric<br/>AKS (Serviço de Kubernetes do Azure)<br/>DC/OS do Mesosphere e outros. |

## <a name="azure-service-fabric"></a>Azure Service Fabric

Outra opção para gerenciamento e implantação de cluster é o Azure Service Fabric. O [Service Fabric](https://azure.microsoft.com/services/service-fabric/) é uma plataforma de microsserviços da Microsoft que conta com orquestração de contêineres e modelos de programação para desenvolvedores para criar aplicativos de microsserviço altamente escalonáveis. O Service Fabric é compatível com os contêineres do Docker no Linux e no Windows e pode ser executado em servidores de ambos os sistemas operacionais.

A seguir estão Service Fabric ferramentas de gerenciamento:

- [Portal do Azure para Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal) para operações relacionadas a clusters (criar/atualizar/excluir) ou configuração de infraestrutura (VMs, balanceador de carga, rede etc.)

- O [Azure Service Fabric Explorer](https://docs.microsoft.com/azure/service-fabric/service-fabric-visualizing-your-cluster) é uma ferramenta multiplataforma especializada para interface do usuário da Web e área de trabalho que fornece insights e determinadas operações no cluster do Service Fabric, do ponto de vista dos nós/VMs, do aplicativo e dos serviços.

>[!div class="step-by-step"]
>[Anterior](run-microservices-based-applications-in-production.md) 
> [Avançar](monitor-containerized-application-services.md)
