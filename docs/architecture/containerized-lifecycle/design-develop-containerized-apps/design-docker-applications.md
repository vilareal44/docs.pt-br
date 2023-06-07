---
title: Criar aplicativos de Docker
description: Encontre aqui uma referência a um guia detalhado sobre a arquitetura de microsserviços, porque esse é um tópico que não é detalhado neste guia.
ms.date: 08/06/2020
ms.openlocfilehash: b63d4344abb358a393587bdacf91f6091b531af0
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87915982"
---
# <a name="design-docker-applications"></a>Criar aplicativos de Docker

O Capítulo 1 apresentou os conceitos fundamentais sobre contêineres e Docker. Essa informação é o nível básico de informações que você precisa para começar. Entretanto, os aplicativos empresariais podem ser complexos e são compostos de vários serviços e não de um único serviço ou contêiner. Para esses casos de uso opcionais, você precisa saber outras abordagens para design, como a SOA (Arquitetura Orientada a Serviços) e os conceitos de microsserviços mais avançados, bem como os conceitos de orquestração de contêiner. O escopo deste documento não está limitado aos microserviços, mas a qualquer ciclo de vida do aplicativo Docker, portanto, ele não explora a arquitetura de microserviços em detalhes, pois você também pode usar contêineres e Docker com tarefas SOA, em segundo plano ou trabalhos regulares, ou mesmo com abordagens de implantação de aplicativo monolítico.

**Mais informações** Para saber mais sobre as arquiteturas de aplicativos empresariais e de microserviço em detalhes, leia o guia [microserviços: arquitetura para aplicativos .net em contêineres](../../microservices/index.md) dos quais você também pode baixar <https://aka.ms/MicroservicesEbook> .

No entanto, antes de entrar no ciclo de vida do aplicativo e DevOps, é importante saber como você pretende criar e construir seu aplicativo e quais são suas opções de design.

>[!div class="step-by-step"]
>[Anterior](index.md) 
> [Avançar](common-container-design-principles.md)
