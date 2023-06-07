---
title: Orquestrar microsserviços e aplicativos de vários contêineres para alta escalabilidade e disponibilidade
description: Saiba como implantar um aplicativo usando o Serviço de Kubernetes do Azure.
ms.date: 08/06/2020
ms.openlocfilehash: b4deb9906e0fece7fb611b6988df576e8b07fe46
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916109"
---
# <a name="deploy-to-azure-kubernetes-service-aks"></a>Implantar no AKS (Serviço de Kubernetes do Azure)

Você pode interagir com o AKS usando seu sistema operacional cliente preferencial (Windows, macOS ou Linux) com a interface de linha de comando do Azure (CLI do Azure) instalada. Para obter mais detalhes, consulte [CLI do Azure documentação](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) e [Guia de instalação](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) para os ambientes disponíveis.

## <a name="create-the-aks-environment-in-azure"></a>Criar o ambiente do AKS no Azure

Existem várias maneiras de criar o Ambiente do AKS. Isso pode ser feito usando comandos CLI do Azure ou o portal do Azure.

Aqui você pode explorar alguns exemplos usando o CLI do Azure para criar o cluster e o portal do Azure para examinar os resultados. Você também precisará ter o Kubectl e o Docker em seu computador de desenvolvimento.

## <a name="create-the-aks-cluster"></a>Criar o cluster do AKS

Crie o cluster AKS usando este comando (o grupo de recursos deve existir):

```console
az aks create --resource-group explore-docker-aks-rg --name explore-docker-aks --node-vm-size Standard_B2s --node-count 1 --generate-ssh-keys --location westeurope
```

> [!NOTE]
> Os `--node-vm-size` `--node-count` valores de parâmetro e são bons o suficiente para um aplicativo de exemplo/dev.

Após a conclusão do trabalho de criação, você poderá ver:

- O cluster AKS criado no grupo de recursos inicial
- Um novo grupo de recursos relacionado, que contém os recursos relacionados ao cluster AKS, conforme mostrado nas imagens a seguir.

O grupo de recursos inicial, com o cluster AKS:

![Exibição de navegador do grupo de recursos do AKS.](media/deploy-azure-kubernetes-service/aks-cluster-view.png)

**Figura 4-17**. Exibição do Grupo de Recursos do AKS do Azure.

O grupo de recursos de cluster AKS:

![Exibição de navegador do grupo de recursos do AKS do Azure.](media/deploy-azure-kubernetes-service/aks-resource-group-view.png)

**Figura 4-18**. Exibição do AKS do Azure.

> [!IMPORTANT]
> Em geral, você não deve precisar modificar os recursos no grupo de recursos de cluster AKS. Por exemplo, o grupo de recursos é excluído quando você exclui o cluster AKS.

Você também pode exibir o nó criado usando `Azure CLI` e `Kubectl`.

Em primeiro lugar, obtendo as credenciais:

```console
az aks get-credentials --resource-group explore-docker-aks-rg --name explore-docker-aks
```

![Saída do console do comando acima: "Explore-Docker-AKS" mesclado como o contexto atual em/Home/Miguel/.Kube/config.](media/deploy-azure-kubernetes-service/get-credentials-command-result.png)

**Figura 4-19**. Resultado do comando `aks get-credentials`.

E, em seguida, obtendo nós do Kubectl:

```console
kubectl get nodes
```

![Saída do console do comando acima: lista de nós com status, idade (tempo em execução) e versão](media/deploy-azure-kubernetes-service/kubectl-get-nodes-command-result.png)

**Figura 4-20**. Resultado do comando `kubectl get nodes`.

> [!div class="step-by-step"]
> [Anterior](orchestrate-high-scalability-availability.md) 
>  [Avançar](docker-apps-development-environment.md)
