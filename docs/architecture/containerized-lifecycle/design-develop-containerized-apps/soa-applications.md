---
title: Aplicativos de SOA
description: Tenha em mente que os contêineres também podem ser uma opção de implantação útil para aplicativos de SOA.
ms.date: 08/06/2020
ms.openlocfilehash: 5cc616eaf3be31ae704320df6ec54deed9529989
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87915261"
---
# <a name="service-oriented-applications"></a>Aplicativos orientados a serviços

A SOA (Arquitetura Orientada a Serviços) era um termo usado de maneira exagerada que significava diferentes coisas para diferentes pessoas. Mas, como um denominador comum, a SOA significa que você estrutura a arquitetura de seu aplicativo o decompondo em vários serviços (mais comumente, como serviços HTTP) que podem ser classificados em diferentes tipos, como subsistemas ou, em outros casos, como camadas.

Atualmente, é possível implantar esses serviços como contêineres do Docker, que resolve problemas de implantação, porque todas as dependências estão incluídas na imagem de contêiner. No entanto, quando precisar expandir SOAs, você poderá encontrar desafios se estiver implantando com base em instâncias únicas. Esse desafio pode ser solucionado usando softwares de clustering ou um orquestrador do Docker. Você poderá examinar os orquestradores mais detalhadamente na próxima seção, ao explorar as abordagens de microserviço.

Os contêineres do Docker são úteis (mas não necessários) para arquiteturas tradicionais orientadas a serviço e para as arquiteturas de microsserviços mais avançadas.

Ao considerar tudo, as soluções de clustering de contêiner são úteis para uma arquitetura de SOA tradicional e para uma arquitetura de microsserviços mais avançada na qual cada microsserviço tem seu próprio modelo de dados. E graças a vários bancos de dados, você também pode escalar horizontalmente a camada data, em vez de trabalhar com bancos de dados monolíticos compartilhados pelos serviços SOA. No entanto, a discussão sobre divisão dos dados é puramente sobre arquitetura e design.

>[!div class="step-by-step"]
>[Anterior](state-and-data-in-docker-applications.md) 
> [Avançar](orchestrate-high-scalability-availability.md)
