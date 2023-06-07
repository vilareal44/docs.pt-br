---
title: Etapas no fluxo de trabalho de DevOps loop externo para um aplicativo de Docker
description: Containerized Docker Application Lifecycle with Microsoft Platform and Tools (Ciclo de vida de aplicativo do Docker em contêineres com a plataforma e as ferramentas da Microsoft)
ms.date: 08/06/2020
ms.openlocfilehash: 1a973407d59484899f99fb6e326b8d7c8e97079b
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87915222"
---
# <a name="creating-cicd-pipelines-in-azure-devops-services-for-a-net-core-application-on-containers-and-deploying-to-a-kubernetes-cluster"></a>Criando pipelines de CI/CD no Azure DevOps Services para um aplicativo .NET Core em contêineres e implantando em um cluster kubernetes

Na Figura 5-12, você pode ver o cenário de DevOps de ponta a ponta, que abrange o gerenciamento de código, a compilação de código, o build de imagens do Docker, o push de imagens do Docker para um registro de Docker e, por fim, a implantação em um cluster do Kubernetes no Azure.

![Fluxo de trabalho: inicia no computador de desenvolvimento. O envio por push para um repositório inicia a tarefa de build/CI usando uma imagem personalizada que é enviada por push a um registro do Docker e, em seguida, é usada pela tarefa de CD/implantar para, por fim, enviar por push ao AKS.](media/docker-workflow-ci-cd-aks.png)

**Figura 5-12**. Cenário de CI/CD criando imagens do Docker e implantando em um cluster do Kubernetes no Azure

É importante destacar que os dois pipelines, build/CI e lançamento/CD, são conectados por meio do Registro do Docker (como o Docker Hub ou o Registro de Contêiner do Azure). O registro do Docker é uma das principais diferenças em comparação com um processo de CI/CD tradicional sem o Docker.

Conforme mostrado na Figura 5-13, a primeira fase é o pipeline de build/CI. No Azure DevOps Services, você pode criar pipelines de build/CI que compilarão o código, criar as imagens do Docker e efetuar push delas para um Registro do Docker, como o Docker Hub ou o Registro de Contêiner do Azure.

![Modo de exibição de navegador do Azure DevOps, com a definição da tarefa do processo de build.](media/build-ci-pipeline-azure-devops-push-to-docker-registry.png)

**Figura 5-13**. Pipeline de build/CI no Azure DevOps, compilando imagens do Docker e enviando as imagens por push a um registro do Docker

A segunda fase é criar um pipeline de implantação/lançamento. No Azure DevOps Services, você pode criar facilmente um pipeline de implantação direcionado a um cluster do Kubernetes usando as tarefas do Kubernetes para Azure DevOps Services, conforme mostrado na Figura 5-14.

![Modo de exibição de navegador do Azure DevOps, com a definição da tarefa de implantação no Kubernetes.](media/release-cd-pipeline-azure-devops-deploy-to-kubernetes.png)

**Figura 5-14**. Pipeline de lançamento/CD na implantação do Azure DevOps Services em um cluster do Kubernetes

> [!Instruções passo a passo] Implantando eShopModernized no Kubernetes:
>
> Para obter uma explicação detalhada dos pipelines de CI/CD do Azure DevOps implantados no Kubernetes, consulte esta postagem: \
><https://github.com/dotnet-architecture/eShopModernizing/wiki/04.-How-to-deploy-your-Windows-Containers-based-apps-into-Kubernetes-in-Azure-Container-Service-(Including-CI-CD)>

>[!div class="step-by-step"]
>[Anterior](docker-application-outer-loop-devops-workflow.md) 
> [Avançar](../run-manage-monitor-docker-environments/index.md)
