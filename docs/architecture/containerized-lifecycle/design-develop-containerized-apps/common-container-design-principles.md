---
title: Noções básicas de criação de contêiner comum
description: 'Aprenda um princípio fundamental do bom design de contêineres: um contêiner deve hospedar apenas um processo.'
ms.date: 08/06/2020
ms.openlocfilehash: 7dcf4b4af3385a2a500c5bc16a889b56fa2c25d5
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916002"
---
# <a name="common-container-design-principles"></a>Noções básicas de criação de contêiner comum

Antes de entrarmos no processo de desenvolvimento, há alguns conceitos básicos que vale a pena mencionar com relação a como você pode usar contêineres.

## <a name="container-equals-a-process"></a>Um contêiner é igual a um processo

No modelo de contêiner, um contêiner representa um único processo. Definindo um contêiner como um limite de processo, você começa a criar os primitivos usados para dimensionar ou agrupar os processos em lotes. Quando executar um contêiner do Docker, você verá uma definição [ENTRYPOINT](https://docs.docker.com/engine/reference/builder/#entrypoint). Ela define o processo e o tempo de vida do contêiner. Quando o processo for concluído, o ciclo de vida do contêiner será encerrado. Há processos de longa execução, como servidores Web, e processos de curta duração, como trabalhos em lotes, que podem ter sido implementados como Microsoft Azure [WebJobs](https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/). Se o processo falhar, o contêiner é encerrado e o orquestrador assume. Se o orquestrador foi instruído a manter cinco instâncias em execução e uma falhar, ele criará outro contêiner para substituir o processo com falha. Em um trabalho em lotes, o processo é iniciado com parâmetros. Quando o processo é concluído, o trabalho é concluído.

Você pode encontrar um cenário em que deseje vários processos em execução em um único contêiner. Em qualquer documento de arquitetura, nunca há um "nunca" e nem sempre há um "sempre". Para cenários que exigem vários processos, um padrão comum é usar o [Supervisor](http://supervisord.org/).

>[!div class="step-by-step"]
>[Anterior](design-docker-applications.md) 
> [Avançar](monolithic-applications.md)
