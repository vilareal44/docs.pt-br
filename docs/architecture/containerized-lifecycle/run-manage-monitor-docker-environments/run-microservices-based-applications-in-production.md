---
title: Executar aplicativos compostos e microsserviços em ambientes de produção
description: Conheça os principais componentes para executar aplicativos baseados em contêiner em produção
ms.date: 08/06/2020
ms.openlocfilehash: a045804e2e40dcf401a697d3e58b13f05ca61b94
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87915054"
---
# <a name="run-composed-and-microservices-based-applications-in-production-environments"></a>Executar aplicativos compostos e microsserviços em ambientes de produção

Aplicativos compostos por vários microsserviços precisam ser implantados em clusters do orquestrador para simplificar a complexidade da implantação e torná-la viável do ponto de vista de TI. Sem um cluster orquestrador, seria difícil implantar e expandir um aplicativo de microsserviço complexo.

## <a name="introduction-to-orchestrators-schedulers-and-container-clusters"></a>Introdução a clusters de contêiner, agendadores e orquestradores

Anteriormente neste livro eletrônico, *clusters* e *agendadores* foram introduzidos como parte da discussão sobre arquitetura e desenvolvimento de software. O Kubernetes e o Service Fabric são exemplos de clusters do Docker. Esses dois orquestradores podem ser executados como parte da infraestrutura fornecida pelo Serviço de Kubernetes do Microsoft Azure.

Quando aplicativos são expandidos para vários sistemas de host, a capacidade de gerenciar cada sistema de host e abstrair a complexidade da plataforma subjacente se torna atraente. É exatamente isso que orquestradores e agendadores oferecem. Vamos examiná-los brevemente:

- **Agendadores**."Agendamento" se refere à capacidade de um administrador carregar um arquivo de serviço em um sistema de host que estabelece como executar um contêiner específico. Iniciar contêineres em um cluster do Docker tende a ser conhecido como agendamento. Embora agendamento se refira ao ato específico de carregar a definição do serviço, em um sentido mais geral, agendadores são responsáveis por se vincular ao sistema de inicialização do host para gerenciar serviços de qualquer forma necessária.

   O agendador de um cluster tem vários objetivos: usar os recursos do cluster com eficiência, trabalhar com restrições de posicionamento fornecidas pelo usuário, agendar aplicativos rapidamente para não os deixar em estado pendente, ter um grau de "integridade", ser robusto a erros e sempre estar disponíveis.

- **Orquestradores**.As plataformas estendem as funcionalidades de gerenciamento de ciclo de vida para cargas de trabalho complexas, com vários contêineres, implantadas em um cluster de hosts. Abstraindo a infraestrutura do host, as ferramentas de orquestração fornecem aos usuários uma maneira de tratar todo o cluster como um único destino de implantação.

   O processo de orquestração envolve ferramentas e uma plataforma que possam automatizar todos os aspectos do gerenciamento de aplicativos, como posicionar ou implantar inicialmente por contêiner; mover contêineres para hosts diferentes dependendo do desempenho ou da integridade do host; controlar a versão e implantar atualizações sem interrupção e funções de monitoramento de integridade com suporte para dimensionamento e failover; entre muitos outros.

   Orquestração é um termo amplo que se refere ao agendamento do contêiner, ao gerenciamento de cluster e, possivelmente, ao provisionamento de hosts adicionais.

As funcionalidades fornecidas por orquestradores e agendadores são complexas para desenvolver e criar do zero, portanto, normalmente você usaria as soluções de orquestração oferecidas por fornecedores.

>[!div class="step-by-step"]
>[Anterior](index.md) 
> [Avançar](manage-production-docker-environments.md)
