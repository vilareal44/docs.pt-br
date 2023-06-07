---
title: O que é o Docker?
description: Obtenha um pouco mais de conhecimento sobre o Docker, uma analogia simples aqui pode ajudá-lo.
ms.date: 08/06/2020
ms.openlocfilehash: 73b6032465583861169a8ac2bed81585027f42ec
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87915060"
---
# <a name="what-is-docker"></a>O que é o Docker?

O [Docker](https://www.docker.com/) é um [projeto de software livre](https://github.com/docker/docker) para automatizar a implantação de aplicativos como contêineres autossuficientes portáteis que podem ser executados na nuvem ou localmente. O Docker é também uma [empresa](https://www.docker.com/) que promove e aprimora essa tecnologia, trabalhando em colaboração com fornecedores de nuvem, do Linux e do Windows, incluindo a Microsoft.

![O diagrama que mostra os contêineres do Docker pode ser executado.](./media/what-is-docker/docker-containers-run-anywhere.png)

**Figura 1-2**. O Docker implanta contêineres em todas as camadas da nuvem híbrida

Conforme mostrado no diagrama acima, os contêineres do Docker podem ser executados em qualquer lugar, local no datacenter do cliente, em um provedor de serviços externo ou na nuvem, no Azure. Os contêineres de imagem do Docker também podem ser executados nativamente no Linux e no Windows. No entanto, imagens do Windows podem executar somente em hosts do Windows e imagens do Linux podem executar em hosts do Linux e hosts do Windows (usando uma VM do Linux do Hyper-V, até o momento), em que o host significa um servidor ou uma VM.

Os desenvolvedores podem usar ambientes de desenvolvimento no Windows, Linux ou macOS. No computador de desenvolvimento, o desenvolvedor executa um host Docker em que as imagens do Docker são implantadas, incluindo o aplicativo e suas dependências. Os desenvolvedores que trabalham no Linux ou no Mac usam um host do Docker que é baseado no Linux e eles podem criar imagens apenas para contêineres do Linux. (Os desenvolvedores que trabalham no Mac podem editar código ou executar a interface de linha de comando (CLI) do Docker a partir do macOS, mas no momento da redação deste artigo, os contêineres não são executados diretamente no macOS.) Os desenvolvedores que trabalham no Windows podem criar imagens para contêineres do Linux ou do Windows.

Para hospedar contêineres em ambientes de desenvolvimento e fornecer ferramentas para desenvolvedores adicionais, o Docker envia o [Docker Community Edition (CE)](https://www.docker.com/community-edition) para Windows ou para o macOS. Esses produtos instalam a VM necessária (o host do Docker) para hospedar os contêineres. O Docker também disponibiliza o [Docker Enterprise Edition (EE)](https://www.docker.com/enterprise-edition), que foi projetado para desenvolvimento empresarial e é usado por equipes de TI que criam, enviam e executam aplicativos de grande porte críticos para os negócios em produção.

Para executar [contêineres do Windows](/virtualization/windowscontainers/about/), há dois tipos de runtimes:

- Os **contêineres do Windows Server** fornecem isolamento de aplicativos por meio da tecnologia de isolamento de processo e de namespace. Um contêiner do Windows Server compartilha um kernel com o host do contêiner e todos os contêineres em execução no host.

- Os **contêineres do Hyper-V** expandem o isolamento fornecido pelos contêineres do Windows Server executando cada contêiner em uma máquina virtual altamente otimizada. Nessa configuração, o kernel do host do contêiner não é compartilhado com os contêineres do Hyper-V, fornecendo melhor isolamento.

As imagens desses contêineres são criadas e funcionam da mesma maneira. A diferença está em como o contêiner é criado da imagem – executar um contêiner do Hyper-V exige um parâmetro extra. Para obter detalhes, consulte [Contêineres do Hyper-V](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/hyperv-container).

## <a name="comparing-docker-containers-with-virtual-machines"></a>Comparando os contêineres do Docker com máquinas virtuais

A Figura 1-3 mostra uma comparação entre VMs e contêineres do Docker.

![Diagrama mostrando uma comparação de ambientes de VM e de contêiner.](./media/what-is-docker/comparison-vms-docker-conatiners.png)

**Figura 1-3**. Comparação de máquinas virtuais tradicionais com contêineres do Docker

Conforme mostrado no diagrama acima, para VMs, há três camadas de base no servidor host. De baixo para cima: infraestrutura, sistema operacional do host e um hipervisor. Além de tudo isso, cada VM tem seu próprio sistema operacional e todas as bibliotecas necessárias. Por outro lado, para o Docker, o servidor host tem apenas a infraestrutura e o sistema operacional. Além disso, o mecanismo de contêiner mantém os contêineres isolados, mas permite que eles compartilhem os serviços do so de base único.

Como os contêineres requerem muito menos recursos (por exemplo, eles não precisam de um sistema operacional completo), eles iniciam rapidamente e são fáceis de implantar. Isso permite que você tenha maior densidade, o que significa que permite a você executar mais serviços na mesma unidade de hardware, reduzindo os custos.

Como um efeito colateral da execução no mesmo kernel, você obtém menos isolamento do que em VMs.

A meta principal de uma imagem é garantir o mesmo ambiente (dependências) entre diferentes implantações. Isso significa que é possível depurá-la em seu computador e, em seguida, implantá-la em outro computador com o mesmo ambiente garantido.

Uma imagem de contêiner é uma maneira de empacotar um aplicativo ou serviço e implantá-lo de maneira confiável e reproduzível. Seria possível dizer que o Docker não é apenas uma tecnologia, mas também uma filosofia e um processo.

Ao usar o Docker, você não escutará os desenvolvedores dizerem "Funciona no meu computador, por que não em produção?" Eles podem simplesmente dizer "É executado no Docker", porque o aplicativo empacotado do Docker pode ser executado em qualquer ambiente do Docker compatível, sendo executado da maneira pretendida em todos os destinos de implantação (como desenvolvimento, garantia de qualidade, processo de preparo e produção).

## <a name="a-simple-analogy"></a>Uma analogia simples

Talvez uma analogia simples possa ajudar a obter o entendimento do conceito principal do Docker.

Vamos voltar no tempo à década de 1950 por um momento. Não havia nenhum processador de palavras e as fotocopiadoras eram usadas em toda parte (ou quase).

Imagine que você seja responsável por emitir lotes de cartas rapidamente conforme necessário, enviá-las aos clientes usando papel e envelopes de verdade, a serem entregues fisicamente ao endereço de cada cliente (não havia email naquela época).

Em algum momento, você percebe que as cartas são apenas uma composição de um grande conjunto de parágrafos, que são separados e organizados conforme necessário de acordo com a finalidade da carta, então você elabora um sistema para emitir cartas rapidamente, esperando receber um aumento substancial.

O sistema é simples:

1. Você começa com um maço de folhas transparentes, que contém um parágrafo cada.

2. Para emitir um conjunto de cartas, você escolhe as planilhas com os parágrafos que precisa e, em seguida, empilha-as e alinha-as para que elas possam ser lidas corretamente e fiquem aparência organizada.

3. Por fim, você coloca o conjunto na fotocopiadora e pressiona o botão de início para produzir tantas cartas quantas forem necessárias.

Portanto, simplificando, essa é a ideia central do Docker.

No Docker, cada camada é o conjunto resultante de alterações que ocorrem no sistema de arquivos depois de executar um comando, tal como instalar um programa.

Assim, quando você "olha" para o sistema de arquivos depois que a camada é copiada, você vê todos os arquivos, incluindo a camada quando o programa foi instalado.

Você pode pensar uma imagem como um auxiliar somente leitura disco rígido pronto para ser instalado em um "computador" em que o sistema operacional já está instalado.

Da mesma forma, você pode pensar em um contêiner como o "computador" com o disco rígido de imagem instalado. O contêiner, assim como um computador, pode ser ativado ou desativado.

>[!div class="step-by-step"]
>[Anterior](introduction-to-containers-and-docker.md) 
> [Avançar](docker-terminology.md)
