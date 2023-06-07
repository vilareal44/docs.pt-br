---
title: Recomendações de hospedagem do Azure para aplicativos Web ASP.NET Core
description: Projetar aplicativos Web modernos com o ASP.NET Core e o Azure | Recomendações de hospedagem do Azure para aplicativos Web ASP.NET Core
author: ardalis
ms.author: wiwagn
ms.date: 06/06/2019
ms.openlocfilehash: 547654e77812481daffc9a03ccd28d3d2f6b5f09
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164429"
---
# <a name="azure-hosting-recommendations-for-aspnet-core-web-apps"></a>Recomendações de hospedagem do Azure para aplicativos Web ASP.NET Core

> "Líderes de linha de negócios do mundo todo estão passando por cima dos departamentos de TI para obter aplicativos da nuvem (também conhecidos como SaaS) e pagando por eles como se fossem uma assinatura de revista. E quando o serviço não é mais necessário, eles podem cancelar a assinatura sem nenhum equipamento deixado inutilizado no canto."  
> _\-Daryl Plummer, analista da Gartner_

Quaisquer que sejam as necessidades e a arquitetura de seu aplicativo, o Microsoft Azure pode dar suporte a elas. Suas necessidades de hospedagem podem variar de um simples site estático até aplicativos extremamente sofisticados compostos por dezenas de serviços. Para aplicativos Web ASP.NET Core monolíticos e os serviços de suporte, há várias configurações bem conhecidas que são recomendadas. As recomendações neste artigo são agrupadas com base no tipo de recurso a ser hospedado, seja para aplicativos completos, processos individuais ou dados.

## <a name="web-applications"></a>Aplicativos Web

Os aplicativos Web podem ser hospedados com:

- Aplicativos Web do Serviço de Aplicativo

- Contêineres (várias opções)

- Máquinas Virtuais (VMs)

Entre essas opções, os Aplicativos Web do Serviço de Aplicativo é a abordagem recomendada para a maioria dos cenários, incluindo aplicativos simples baseados em contêiner. Para arquiteturas de microsserviço, considere uma abordagem baseada em contêiner. Caso precise ter mais controle sobre os computadores que executam o aplicativo, considere a possibilidade de usar as Máquinas Virtuais do Azure.

### <a name="app-service-web-apps"></a>Aplicativos Web do Serviço de Aplicativo

Os Aplicativos Web do Serviço de Aplicativo oferecem uma plataforma totalmente gerenciada, otimizada para a hospedagem de aplicativos Web. É uma oferta de PaaS (plataforma como serviço) que permite que você se concentre na lógica de negócios, enquanto o Azure se encarrega da infraestrutura necessária para executar e dimensionar o aplicativo. Alguns dos principais recursos dos Aplicativos Web do Serviço de Aplicativo:

- Otimização de DevOps (integração e entrega contínuas, múltiplos ambientes, testes A/B, suporte a script).

- Escala global e alta disponibilidade.

- Conexões com plataformas de SaaS e seus dados locais.

- Segurança e conformidade.

- Integração com o Visual Studio.

O Serviço de Aplicativo do Azure é a melhor opção para a maioria dos aplicativos Web. A implantação e o gerenciamento estão integrados na plataforma, os sites podem ser dimensionados rapidamente para suportar altas cargas de tráfego e o gerenciador de balanceamento de carga e tráfego integrado oferece alta disponibilidade. Você pode mover os sites existentes para o Serviço de Aplicativo do Azure com facilidade usando uma ferramenta de migração online, usar um aplicativo de software livre por meio da Galeria de Aplicativos Web ou criar um novo site usando a estrutura e as ferramentas de sua escolha. O recurso WebJobs facilita a adição de um processamento de trabalho em segundo plano ao aplicativo Web do Serviço de Aplicativo. Se você tem um aplicativo ASP.NET hospedado no local usando um banco de dados, há um caminho claro de migração para um Aplicativo Web do Serviço de Aplicativo com um Banco de Dados SQL do Azure (ou acesso seguro ao servidor de banco de dados local, se preferir).

![Estratégia de migração recomendada para aplicativos .NET locais no Serviço de Aplicativo do Azure](./media/image1-6.png)

Na maioria dos casos, o processo de migração de um aplicativo ASP.NET hospedado localmente para um Aplicativo Web do Serviço de Aplicativo é simples. Pouca ou nenhuma modificação deve ser necessária no próprio aplicativo, e ele pode ser iniciado rapidamente para aproveitar os muitos recursos oferecidos pelos Aplicativos Web do Serviço de Aplicativo do Azure.

Além dos aplicativos não otimizados para a nuvem, os Aplicativos Web do Serviço de Aplicativo do Azure são uma excelente solução para muitos aplicativos monolíticos simples (não distribuídos), como muitos aplicativos ASP.NET Core. Nessa abordagem, a arquitetura é básica e simples de entender e gerenciar:

![Arquitetura básica do Azure](./media/image1-5.png)

Um pequeno número de recursos de um único grupo geralmente é suficiente para gerenciar esse aplicativo. Aplicativos que normalmente são implantados como uma única unidade, em vez serem compostos de muitos processos separados, são bons candidatos para essa [abordagem arquitetural básica](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/basic-web-app). Embora tenha uma arquitetura simples, essa abordagem ainda permite que o aplicativo hospedado seja expandido verticalmente (mais recursos por nó) e horizontalmente (mais nós hospedados), para atender a qualquer aumento de demanda. Com dimensionamento automático, o aplicativo pode ser configurado para ajustar automaticamente o número de nós que hospedam o aplicativo com base na demanda e na carga média nos nós.

### <a name="app-service-web-apps-for-containers"></a>Aplicativos Web do Serviço de Aplicativo para Contêineres

Além do suporte à hospedagem de aplicativos Web diretamente, os [Aplicativos Web do Serviço de Aplicativo para Contêineres](https://azure.microsoft.com/services/app-service/containers/) podem ser usados para fazer execuções em contêiner no Windows e no Linux. Usando esse serviço, você pode implantar e executar facilmente aplicativos em contêiner que podem ser usados em grande escala na sua empresa. Os aplicativos têm todos os recursos dos Aplicativos Web do Serviço de Aplicativo listados acima. Além disso, os Aplicativos Web para Contêineres dão suporte a CI/CD simplificado com Docker Hub, Registro de Contêiner do Azure e GitHub. É possível usar o Azure DevOps para definir pipelines de criação e implantação que publicam as alterações em um registro. Essas alterações podem ser testadas em um ambiente de preparação e implantadas automaticamente na produção usando slots de implantação, o que possibilita atualizações sem tempo de inatividade. A reversão para versões anteriores pode ser feita com a mesma facilidade.

Há alguns cenários em que os Aplicativos Web para Contêineres fazem mais sentido. Se você tiver aplicativos que podem ser colocados em contêiner, seja em contêineres Windows ou Linux, poderá hospedá-los facilmente usando esse conjunto de ferramentas. Basta publicar o contêiner e configurar os Aplicativos Web para Contêineres para extrair a versão mais recente dessa imagem do registro de sua escolha. Essa é uma abordagem "lift and shift" para migrar de modelos clássicos de hospedagem de aplicativos para um modelo otimizado para nuvem.

![Migrar aplicativos .NET em contêiner locais para Aplicativos Web para Contêineres do Azure](./media/image1-8.png)

Essa abordagem também funcionará bem se sua equipe de desenvolvimento puder migrar para um processo de desenvolvimento baseado em contêiner. O "loop interno" de desenvolvimento de aplicativos com contêineres inclui a criação do aplicativo com contêineres. As alterações feitas no código, bem como na configuração do contêiner, são enviadas para o controle do código-fonte, e um build automatizado é responsável pela publicação de novas imagens de contêiner em um registro como o Docker Hub ou o Registro de Contêiner do Azure. Essas imagens são usadas como base para desenvolvimento adicional, bem como em implantações para produção, conforme mostrado no seguinte diagrama:

![Fluxo de trabalho completo do ciclo de vida do DevOps no Docker](./media/image1-7.png)

O desenvolvimento com contêineres oferece muitas vantagens, especialmente quando os contêineres são usados ​​na produção. A mesma configuração de contêiner é usada para hospedar o aplicativo em cada ambiente em que ele é executado, do computador de desenvolvimento local até a criação e teste de sistemas, incluindo a produção. Isso reduz muito a probabilidade de defeitos decorrentes de diferenças na configuração do computador ou nas versões do software. Os desenvolvedores também podem usar as ferramentas com as quais são mais produtivos, incluindo o sistema operacional, já que os contêineres podem ser executados em qualquer sistema. Em alguns casos, aplicativos distribuídos envolvendo muitos contêineres podem usar muitos recursos para serem executados em um único computador de desenvolvimento. Nesse cenário, pode fazer sentido atualizar para o uso do Kubernetes e do Azure Dev Spaces, abordados na próxima seção.

Como partes de aplicativos maiores são divididas em seus próprios *microsserviços* menores e independentes, padrões de design adicionais podem ser usados ​​para melhorar o comportamento do aplicativo. Em vez de trabalhar diretamente com serviços individuais, um *gateway de API* pode simplificar o acesso e separar o cliente de seu back-end. Ter back ends de serviços separados para front-ends diferentes também permite que os serviços evoluam em sintonia com seus consumidores. Os serviços comuns podem ser acessados por meio de um contêiner *sidecar* separado, que pode incluir bibliotecas comuns de conectividade do cliente usando o padrão *embaixador*.

![Exemplo de arquitetura de microsserviços com vários padrões de design comuns observados.](./media/image1-10.png)

[Saiba mais sobre os padrões de design a serem considerados ao criar sistemas baseados em microsserviço.](https://docs.microsoft.com/azure/architecture/microservices/design/patterns)

### <a name="azure-kubernetes-service"></a>Serviço de Kubernetes do Azure

O Serviço de Kubernetes do Azure (AKS) gerencia seu ambiente Kubernetes hospedado, tornando rápido e fácil implantar e gerenciar os aplicativos em contêiner sem conhecimento de orquestração do contêiner. Também elimina a sobrecarga das operações em andamento e a manutenção provisionando, atualizando e dimensionamento os recursos sob demanda, sem colocar seus aplicativos offline.

O AKS reduz a complexidade e a sobrecarga operacional de gerenciar um cluster Kubernetes deixando grande parte dessa responsabilidade para o Azure. Como um serviço Kubernetes hospedado, o Azure lida com as tarefas críticas para você, como o monitoramento da integridade e a manutenção. Além disso, você paga apenas pelos nós do agente dentro dos clusters, não pelos mestres. Como um serviço Kubernetes gerenciado, o AKS fornece:

- Atualizações de versão e aplicação de patch automatizadas para o Kubernetes.
- Dimensionamento fácil do cluster.
- Plano de controle hospedado de autorrecuperação (mestres).
- Economia de custos – pague somente pelos nós do pool de agentes em execução.

Com o Azure lidando com o gerenciamento de nós no cluster AKS, você não precisa mais executar muitas tarefas manualmente, como as atualizações do cluster. Como o Azure lida com essas tarefas de manutenção importantes, o AKS não fornece acesso direto (como com SSH) ao cluster.

As equipes que estão utilizando o AKS também podem aproveitar o Azure Dev Spaces. O Azure Dev Spaces ajuda as equipes a se concentrarem no desenvolvimento e na rápida iteração de seu aplicativo de microsserviço permitindo que trabalhem diretamente com toda a arquitetura ou aplicativo de microsserviço em execução no AKS. O Azure Dev Spaces também fornece uma maneira independente de atualizar partes da arquitetura de seus microsserviços de forma isolada, sem afetar o restante do cluster do AKS ou outros desenvolvedores.

![Exemplo de fluxo de trabalho do Azure Dev Spaces](./media/image1-9.gif)

Azure Dev Spaces:

- Minimizar requisitos de tempo e recursos de configuração do computador local
- Permitir que as equipes façam a iteração com mais rapidez
- Reduzir o número de ambientes de integração exigidos pela equipe
- Remover a necessidade de simular determinados serviços em um sistema distribuído durante o desenvolvimento/teste

[Saiba mais sobre o Azure Dev Spaces](https://docs.microsoft.com/azure/dev-spaces/about)

### <a name="azure-virtual-machines"></a>Máquinas Virtuais do Azure

Se você tiver um aplicativo existente que exija a execução de modificações substanciais no Serviço de Aplicativo, poderá escolher Máquinas Virtuais para simplificar a migração para a nuvem. No entanto, configurar, proteger e realizar a manutenção corretamente de VMs exige muito mais tempo e conhecimento de TI em comparação ao Serviço de Aplicativo do Azure. Se estiver considerando o uso das Máquinas Virtuais do Azure, leve em conta o esforço de manutenção contínuo necessário para aplicar patch, atualizar e gerenciar seu ambiente de VM. Máquinas Virtuais do Azure são uma IaaS (infraestrutura como serviço), enquanto o Serviço de Aplicativo é PaaS. Você também deve considerar se vai implantar seu aplicativo como um Contêiner do Windows para Aplicativo Web para Contêineres ser uma opção viável para seu cenário.

## <a name="logical-processes"></a>Processos lógicos

Processos lógicos individuais que podem ser desacoplados do restante do aplicativo podem ser implantados de forma independente no Azure Functions de modo "sem servidor". O Azure Functions permite simplesmente escrever o código necessário para um problema específico, sem a preocupação com o aplicativo ou a infraestrutura necessária para executá-lo. Escolha uma entre uma variedade de linguagens de programação, incluindo C\#, F\#, Node.js, Python e PHP, que permitem que você escolha a linguagem mais produtiva para a tarefa em mãos. Como a maioria das soluções baseadas em nuvem, você paga apenas pelo tempo que usar e pode confiar no Azure Functions para escalar verticalmente, conforme necessário.

## <a name="data"></a>Dados

O Azure oferece uma ampla variedade de opções de armazenamento de dados, de modo que seu aplicativo possa usar o provedor de dados apropriado para os dados em questão.

Para dados transacionais e relacionais, os Bancos de Dados SQL do Azure são a melhor opção. Para dados de alto desempenho em sua maior parte de leitura, um Cache Redis com o apoio de um Banco de Dados SQL do Azure é uma boa solução.

Os dados JSON não estruturados podem ser armazenados de várias maneiras, de colunas do banco do dados SQL a BLOBs ou tabelas no armazenamento do Azure, para Azure Cosmos DB. Desses, Azure Cosmos DB oferece a melhor funcionalidade de consulta e é a opção recomendada para grandes números de documentos baseados em JSON que devem dar suporte à consulta.

Dados transitórios baseados em comando ou evento usados para orquestrar o comportamento do aplicativo podem usar o Barramento de Serviço do Azure ou as Filas do Armazenamento do Azure. O barramento de serviço do Azure oferece mais flexibilidade e é o serviço recomendado para mensagens não triviais dentro e entre aplicativos.

## <a name="architecture-recommendations"></a>Recomendações de arquitetura

Os requisitos do aplicativo devem determinar sua arquitetura. Há muitos serviços do Azure diferentes disponíveis. Escolher o certo é uma decisão importante. A Microsoft oferece uma galeria de arquiteturas de referência para ajudar a identificar as arquiteturas típicas otimizadas para cenários comuns. Você pode encontrar uma arquitetura de referência que mapeie de perto as necessidades do seu aplicativo ou que, pelo menos, ofereça um ponto de partida.

A Figura 11-1 mostra uma arquitetura de referência de exemplo. Esse diagrama descreve uma abordagem de arquitetura recomendada para um site de sistema de gerenciamento de conteúdo do Sitecore otimizado para marketing.

![Figura 11-1](./media/image11-2.png)

**Figura 11-1.** Arquitetura de referência do site de marketing do Sitecore.

**Referências – recomendações de hospedagem do Azure**

- Arquiteturas de solução do Azure\
  <https://azure.microsoft.com/solutions/architecture/>

- Arquitetura do aplicativo Web básico do Azure\
  <https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/basic-web-app>

- Padrões de design para Microsserviços\
  <https://docs.microsoft.com/azure/architecture/microservices/design/patterns>

- Guia do Desenvolvedor do Azure\
  <https://azure.microsoft.com/campaigns/developer-guide/>

- Visão geral dos Aplicativos Web\
  <https://docs.microsoft.com/azure/app-service/app-service-web-overview>

- Aplicativo Web para Contêineres\
  <https://azure.microsoft.com/services/app-service/containers/>

- Introdução ao AKS (Serviço de Kubernetes do Azure)\
  <https://docs.microsoft.com/azure/aks/intro-kubernetes>

>[!div class="step-by-step"]
>[Anterior](development-process-for-azure.md)
