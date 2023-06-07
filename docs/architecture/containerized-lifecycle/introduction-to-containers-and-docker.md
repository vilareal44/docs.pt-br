---
title: Introdução aos contêineres e ao Docker
description: Obtenha uma visão geral de alto nível dos principais benefícios de usar o Docker.
ms.date: 04/15/2020
ms.openlocfilehash: 79b4752ef4d2f0aa6199414aea5cf4ef357c86d3
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87915939"
---
# <a name="introduction-to-containers-and-docker"></a>Introdução aos contêineres e ao Docker

*A Containerização é uma abordagem para o desenvolvimento de software no qual um aplicativo ou serviço, suas dependências e sua configuração (abstratas como arquivos de manifesto de implantação) são empacotados juntos como uma imagem de contêiner. Em seguida, você pode testar o aplicativo em contêiner como uma unidade e implantá-lo como uma instância de imagem de contêiner no sistema operacional do host (SO).*

Assim como os contêineres de remessa permitem que as mercadorias sejam transportadas por navio, trem ou caminhão, independentemente da carga que contenham, os contêineres de implantação de software agem como uma unidade de software padrão que pode conter diferentes dependências e códigos. Inserir o software em contêineres dessa maneira permite que desenvolvedores e profissionais de TI os implantem em diferentes ambientes com pouca ou sem nenhuma modificação.

Os contêineres também isolam os aplicativos uns dos outros em um sistema operacional compartilhado. Os aplicativos em contêineres são executados com base em um host do contêiner que por sua vez é executado no sistema operacional (Linux ou Windows). Os contêineres, portanto, ocupam um volume bem menor do que as imagens de VM (máquina virtual).

Cada contêiner pode executar um aplicativo Web ou um serviço inteiro, conforme mostrado na Figura 1-1. Neste exemplo, o host do Docker é um host de contêiner e App1, App2, Svc1 e Svc2 são aplicativos ou serviços em contêineres.

![Diagrama mostrando quatro contêineres em execução em uma VM ou um servidor.](./media/introduction-to-containers-and-docker/multiple-containers-single-host.png)

**Figura 1-1**. Vários contêineres em execução em um host de contêiner

Outra vantagem do uso de contêineres é a escalabilidade. Você pode aumentar rapidamente, criando novos contêineres para tarefas de curto prazo. Do ponto de vista do aplicativo, criar uma instância de uma imagem (criar um contêiner) é semelhante a criar uma instância de um processo, como um serviço ou aplicativo Web. No entanto, para assegurar a confiabilidade, ao executar várias instâncias da mesma imagem em vários servidores host, geralmente é melhor que cada contêiner (instância da imagem) seja executado em um servidor host ou em uma VM diferente em domínios de falha diferentes.

Resumindo, os contêineres oferecem os benefícios de isolamento, portabilidade, agilidade, escalabilidade e controle em todo o fluxo de trabalho do ciclo de vida do aplicativo. O benefício mais importante é o isolamento de ambiente fornecido entre Desenvolvimento e Operações.

>[!div class="step-by-step"]
>[Anterior](index.md) 
> [Avançar](what-is-docker.md)
