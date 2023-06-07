---
title: Contêineres de base para colaboração de DevOps
description: Entenda o papel fundamental dos contêineres para simplificar DevOps.
ms.date: 08/06/2020
ms.openlocfilehash: af28c1add8b2e6befbd2f3e6ae9fe707ccc5b106
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916013"
---
# <a name="containers-as-the-foundation-for-devops-collaboration"></a>Contêineres de base para colaboração de DevOps

Pela própria natureza dos contêineres e da tecnologia do Docker, os desenvolvedores podem compartilhar o software e as dependências facilmente com as operações de TI e os ambientes de produção e, ao mesmo tempo, eliminar a desculpa comum de que "funciona no meu computador". Os contêineres resolvem conflitos de aplicativo entre ambientes diferentes. Indiretamente, os contêineres e o Docker estreitam a relação entre os desenvolvedores e as operações de TI, facilitando uma colaboração eficiente. Adotar o fluxo de trabalho de contêiner oferece a vários clientes a continuidade de DevOps que eles buscam, mas tiveram que implantar anteriormente por meio de uma configuração mais complexa para versão e pipelines de build. Os contêineres simplificam os pipelines de build/teste/implantação no DevOps.

![Diagrama que mostra a propriedade do ciclo de vida de um aplicativo do Docker.](./media/containers-foundation-for-devops-collaboration/persona-workloads-docker-container-lifecycle.png)

**Figura 2-1.** Principais cargas de trabalho por "persona" no ciclo de vida dos aplicativos do Docker em contêineres

Com os contêineres do Docker, os desenvolvedores são os proprietários dos elementos do contêiner (aplicativo, serviço, dependências, estruturas e componentes) e são responsáveis pela maneira como os contêineres e os serviços se comportam juntos como um aplicativo composto por uma coleção de serviços. As interdependências de vários contêineres são definidas em um arquivo `docker-compose.yml` ou o que poderia ser chamado de *manifesto de implantação*. Enquanto isso, as equipes de operações de TI (profissionais e gerenciamento de TI) podem se concentrar no gerenciamento dos ambientes de produção, na infraestrutura, na escalabilidade, no monitoramento. Por fim, podem garantir que os aplicativos estão sendo entregues corretamente para os usuários finais sem precisar saber o conteúdo dos vários contêineres. Por isso o nome "contêiner", o que lembra a analogia com os contêineres de transporte da vida real. Dessa maneira, os proprietários do conteúdo do contêiner não precisam se preocupar com o modo de envio dele, e a transportadora conduz o contêiner do ponto de origem ao destino sem saber ou se preocupar com o conteúdo. De maneira semelhante, os desenvolvedores podem criar e serem proprietários do conteúdo de um contêiner do Docker sem precisar se preocupar com os mecanismos de "transporte".

No pilar do lado esquerdo da Figura 2-1, os desenvolvedores escrevem e executam código localmente nos contêineres do Docker for Windows ou Mac. Eles definem o ambiente operacional do código usando um Dockerfile que configura o sistema operacional de base para executar, bem como as etapas de build para criar o código em uma imagem do Docker. Os desenvolvedores definem como uma ou mais imagens interoperarão usando o arquivo `docker-compose.yml` do manifesto de implantação já mencionado. Conforme o desenvolvimento local é concluído, eles efetuam push do código do aplicativo e dos arquivos de configuração do Docker para o repositório de código escolhido (ou seja, o repositório do Git).

O pilar de DevOps define o build – pipelines de CI (integração contínua) usando o Dockerfile fornecido no repositório de código. O sistema de CI efetua pull das imagens de contêiner de base do Registro do Docker selecionado e cria as imagens personalizadas do Docker para o aplicativo. Em seguida, as imagens são validadas e efetua-se o push delas para o Registro do Docker usado para as implantações em vários ambientes.

No pilar à direita, as equipes de operações gerenciam os aplicativos e a infraestrutura implantados na produção e monitoram o ambiente e os aplicativos. Desse modo, é possível oferecer feedback e insights à equipe de desenvolvimento sobre como o aplicativo pode ser aprimorado. Os aplicativos de contêiner são normalmente executados em produção usando orquestradores de contêiner como [kubernetes](https://kubernetes.io/), em que geralmente os [gráficos Helm](https://helm.sh/) são usados para configurar unidades de implantação, em vez de arquivos Docker-Compose.

Ambas as equipes colaboram por meio de uma plataforma fundamental (os contêineres do Docker) que oferece a separação de preocupações como um contrato, além de melhorar muito a colaboração entre as duas equipes no ciclo de vida do aplicativo. Os desenvolvedores são proprietários do conteúdo do contêiner, do ambiente operacional e das interdependências do contêiner, ao passo que as equipes de operações usam as imagens criadas junto com o manifesto e as executam no sistema de orquestração.

## <a name="challenges-in-the-application-life-cycle-when-using-docker"></a>Desafios no ciclo de vida do aplicativo ao usar o Docker.

Há muitos motivos que aumentarão o número de aplicativos em contêiner nos próximos anos. Um deles é a criação de aplicativos baseados em microsserviços.

Durante os últimos 15 anos, o uso de Web Services tem sido a base de milhares de aplicativos e, provavelmente, após alguns anos, você encontrará a mesma situação com aplicativos baseados em microserviço em execução em contêineres do Docker.

Também vale a pena mencionar que é possível usar os contêineres do Docker para aplicativos monolíticos e ainda aproveitar todos os benefícios do Docker. Os contêineres não usam só os microsserviços.

O uso dos contêineres do Docker e dos microsserviços gera novos desafios no processo de desenvolvimento das organizações. Portanto, é necessária uma sólida estratégia para manter vários contêineres e microsserviços em execução nos sistemas de produção. Por fim, os aplicativos empresariais terão centenas ou milhares de contêineres/instâncias em execução na produção.

Esses desafios criam novas demandas ao usar ferramentas de DevOps, portanto, você precisará definir novos processos em suas atividades de DevOps e encontrar respostas para o seguinte tipo de perguntas:

- Quais ferramentas posso usar para o desenvolvimento, CI/CD, gerenciamento e operações??

- Como minha empresa pode gerenciar erros nos contêineres ao executar na produção?

- Como alterar partes do software na produção com um tempo de inatividade mínimo?

- Como é possível dimensionar e monitorar nosso sistema de produção?

- Como podemos incluir o teste e a implantação de contêineres em nosso pipeline de lançamento?

- Como usar ferramentas/plataformas open-source para contêineres no Microsoft Azure?

Se você conseguir responder a todas essas perguntas, estará mais bem preparado para migrar seus aplicativos (existentes ou novos) para os contêineres do Docker.

## <a name="introduction-to-a-generic-end-to-end-docker-application-life-cycle-workflow"></a>Introdução a um fluxo de trabalho genérico de ponta a ponta do ciclo de vida do aplicativo do Docker

A Figura 2-2 apresenta um fluxo de trabalho mais detalhado do ciclo de vida de um aplicativo do Docker, concentrando-se nessa instância em atividades e ativos específicos de DevOps.

![Diagrama mostrando o ciclo de vida de ponta a ponta genérico de um aplicativo do Docker.](./media/containers-foundation-for-devops-collaboration/generic-end-to-enddpcker-app-life-cycle.png)

**Figura 2-2.** Fluxo de trabalho de alto nível para o ciclo de vida do aplicativo em contêineres do Docker

Tudo começa com o desenvolvedor, que inicia a escrita do código no fluxo de trabalho do loop interno. É no loop interno que os desenvolvedores definem tudo o que acontece antes de efetuar push do código para o repositório (por exemplo, um sistema de controle do código-fonte, como o Git). Após a confirmação, o repositório dispara a CI (integração contínua) e o restante do fluxo de trabalho.

O loop interno consiste em etapas típicas, como "código", "executar", "testar" e "depurar", além das etapas adicionais necessárias imediatamente antes de executar o aplicativo localmente. Esse é o processo do desenvolvedor para executar e testar o aplicativo como um contêiner do Docker. O fluxo de trabalho do loop interno será explicado nas seções a seguir.

Voltando um pouco para examinar o fluxo de trabalho de ponta a ponta, o fluxo de trabalho do DevOps é mais do que uma tecnologia ou um conjunto de ferramentas, é uma mentalidade que exige uma evolução cultural. São as pessoas, os processos e as ferramentas adequadas para acelerar e tornar mais previsível o ciclo de vida do aplicativo. As empresas que adotam um fluxo de trabalho em contêineres normalmente reestruturam suas organizações para representar as pessoas e os processos que combinam com ele.

Praticar DevOps pode ajudar as equipes a responder mais rápido às pressões competitivas ao substituir processos manuais suscetíveis a erro pela automação, o que resulta em rastreabilidade melhor e fluxos de trabalho reproduzíveis. As organizações também podem gerenciar ambientes com mais eficiência e economizar custos com uma combinação de recursos locais e em nuvem, além de ferramentas totalmente integradas.

Ao implantar o fluxo de trabalho DevOps para aplicativos do Docker, você verá que as tecnologias do Docker estão presentes na maioria das etapas do fluxo de trabalho, do desenvolvimento ao trabalhar no loop interno (código, execução, depuração), na fase de build-teste-CI e, por fim, na implantação dos contêineres nos ambientes de preparo e produção.

A melhoria das práticas de qualidade ajuda a identificar defeitos no início do ciclo de desenvolvimento, o que reduz o custo de corrigi-los. Ao incluir o ambiente e as dependências na imagem e adotar uma filosofia de implantação da mesma imagem em vários ambientes, você promove uma disciplina de extrair as configurações específicas do ambiente, o que torna as implantações mais confiáveis.

Os dados avançados obtidos por meio de uma instrumentação eficiente (monitoramento e diagnóstico) oferecem insights sobre os problemas de desempenho e o comportamento do usuário para orientar as futuras prioridades e investimentos.

O DevOps precisa ser considerado uma jornada, não um destino. Ele deve ser implementado incrementalmente por meio dos projetos de escopo adequado, em que você pode demonstrar sucesso, aprendizado e evolução.

## <a name="benefits-of-devops-for-containerized-applications"></a>Benefícios do DevOps para aplicativos em contêineres

Confira alguns dos mais importantes benefícios oferecidos por um fluxo de trabalho sólido de DevOps:

- Proporcionar um software de melhor qualidade, mais rápido e com conformidade melhor.

- Promover melhoria e ajustes contínuos mais cedo e de maneira mais econômica.

- Aumentar a transparência e a colaboração entre os stakeholders envolvidos na entrega e na operação do software.

- Controlar os custos, usar recursos provisionados de modo mais eficiente e minimizar os riscos de segurança.

- Bom plug and play com vários investimentos existentes de DevOps, incluindo os investimentos open-source.

>[!div class="step-by-step"]
>[Anterior](index.md) 
> [Avançar](../Microsoft-platform-tools-containerized-apps/index.md)
