---
title: Como definir o que é nativo de nuvem
description: Saiba mais sobre os pilares básicos que fornecem o Fundação para sistemas nativos de nuvem
author: robvet
ms.date: 05/13/2020
ms.openlocfilehash: f50c144d99fae0c4702965342fd76ec22e8bd8c8
ms.sourcegitcommit: 32f0d6f4c01ddc6ca78767c3a30e3305f8cd032c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87427028"
---
# <a name="defining-cloud-native"></a>Definindo a nuvem nativa

Pare o que está fazendo e o texto dez de seus colegas. Peça que eles definam o termo "nuvem nativa". Boa chance de você obter dez respostas diferentes.

A nuvem nativa está prestes a mudar a maneira como você imagina a construção de sistemas comerciais críticos.

Os sistemas nativos de nuvem são projetados para adotar alteração rápida, grande escala e resiliência.

A base de computação nativa da nuvem fornece uma [definição oficial](https://github.com/cncf/foundation/blob/master/charter.md):

> *As tecnologias nativas de nuvem capacitam as organizações a criar e executar aplicativos escalonáveis em ambientes modernos e dinâmicos, como nuvens públicas, privadas e híbridas. Contêineres, malhas de serviço, microservices, infraestrutura imutável e APIs declarativas exemplificam essa abordagem.*

> *Essas técnicas permitem sistemas menos rígidos resilientes, gerenciáveis e observáveis. Combinada com a automação robusta, eles permitem que os engenheiros façam alterações de alto impacto com frequência e previsíveis com o mínimo de toil.*

Os aplicativos se tornaram cada vez mais complexos com os usuários exigindo mais e mais. Os usuários esperam uma rápida capacidade de resposta, recursos inovadores e zero tempo de inatividade. Problemas de desempenho, erros recorrentes e a incapacidade de mover rapidamente não são mais aceitáveis. Eles mudarão facilmente para o seu concorrente.

A nuvem nativa é muito sobre *velocidade* e *agilidade*. Os sistemas de negócios estão evoluindo da habilitação de recursos de negócios a armas de transformação estratégica, acelerando a velocidade e o crescimento dos negócios. É imperativo obter ideias para o mercado imediatamente.

Aqui estão algumas empresas que implementaram essas técnicas. Pense na velocidade, na agilidade e na escalabilidade que eles atingiram.

| Empresa | Experiência |
| :-------- | :-------- |
| [Netflix](https://www.infoq.com/news/2013/06/netflix/) | Tem mais de 600 serviços em produção. Implanta centenas de vezes por dia. |
| [Uber](https://eng.uber.com/micro-deploy/) | Tem mais de 1.000 serviços em produção. Implanta vários milhares de vezes por semana. |
| [WeChat](https://www.cs.columbia.edu/~ruigu/papers/socc18-final100.pdf) | Tem mais de 3.000 serviços em produção. Implanta 1.000 vezes por dia. |

Como você pode ver, Netflix, Uber e WeChat expõem sistemas que consistem em centenas de microservices independentes. Esse estilo de arquitetura permite que eles respondam rapidamente às condições do mercado. Eles podem atualizar de forma instantânea áreas pequenas de um aplicativo dinâmico e complexo e dimensionar individualmente essas áreas conforme necessário.

A velocidade e a agilidade da nuvem nativa vêm de vários fatores. A maioria é a infraestrutura de nuvem. Cinco pilares básicos adicionais mostrados na Figura 1-3 também fornecem o Fundação para sistemas nativos de nuvem.

![Pilares básicos nativos da nuvem](./media/cloud-native-foundational-pillars.png)

**Figura 1-3**. Pilares básicos nativos da nuvem

Vamos dar algum tempo para entender melhor o significado de cada pilar.

## <a name="the-cloud"></a>A nuvem...

Os sistemas nativos de nuvem aproveitam ao máximo o modelo de serviço de nuvem.

Projetado para prosperar em um ambiente de nuvem dinâmico e virtualizado, esses sistemas fazem uso extensivo da infraestrutura de computação e dos serviços gerenciados de [PaaS (plataforma como serviço)](https://azure.microsoft.com/overview/what-is-paas/) . Eles tratam a infraestrutura subjacente como *desdescartável* -provisionado em minutos e redimensionados, dimensionados, movidos ou destruídos sob demanda – via automação.

Considere o conceito de DevOps amplamente aceito de [pets versus burro](https://medium.com/@Joachim8675309/devops-concepts-pets-vs-cattle-2380b5aab313). Em um data center tradicional, os servidores são tratados como *animais de estimação*: um computador físico, dado um nome significativo e cuidados para. Você dimensiona adicionando mais recursos ao mesmo computador (expandindo verticalmente). Se o servidor se tornar doente, você o enfermaria de volta para a integridade. Se o servidor ficar indisponível, todos os avisos serão percebidos.

O modelo de serviço *burro* é diferente. Você provisiona cada instância como uma máquina virtual ou contêiner. Eles são idênticos e atribuídos a um identificador de sistema, como Service-01, Service-02 e assim por diante. Você dimensiona criando mais deles (expandindo). Quando um se torna indisponível, ninguém percebe.

O modelo burro adota a *infraestrutura imutável*. Os servidores não são reparados ou modificados. Se uma falha ou requer atualização, ela é destruída e uma nova é provisionada – tudo feito por meio da automação.

Os sistemas nativos de nuvem adotam o modelo de serviço burro. Eles continuam a ser executados conforme a infraestrutura é dimensionada para dentro ou para fora, sem considerar as máquinas nas quais estão em execução.

A plataforma de nuvem do Azure dá suporte a esse tipo de infraestrutura altamente elástica com recursos de dimensionamento automático, auto-recuperação e monitoramento.

## <a name="modern-design"></a>Design moderno

Como você criaria um aplicativo nativo de nuvem? Como seria a aparência de sua arquitetura? Para quais princípios, padrões e práticas recomendadas você deve aderir? Quais questões operacionais e de infraestrutura seriam importantes?

### <a name="the-twelve-factor-application"></a>O aplicativo de doze fatores

Uma metodologia amplamente aceita para construir aplicativos baseados em nuvem é o [aplicativo de doze fatores](https://12factor.net/). Ele descreve um conjunto de princípios e práticas que os desenvolvedores seguem para construir aplicativos otimizados para ambientes de nuvem modernos. A atenção especial é dada à portabilidade entre ambientes e automação declarativa.

Embora seja aplicável a qualquer aplicativo baseado na Web, muitos profissionais consideram doze fatores como uma base sólida para a criação de aplicativos nativos de nuvem. Os sistemas criados com base nesses princípios podem implantar e dimensionar rapidamente e adicionar recursos para reagir rapidamente às mudanças no mercado.

A tabela a seguir destaca a metodologia de doze fatores:

|    |  Fator | Explicação  |
| :-------- | :-------- | :-------- |
| 1 | Base de código | Uma única base de código para cada microserviço, armazenada em seu próprio repositório. Acompanhado com o controle de versão, ele pode ser implantado em vários ambientes (QA, preparo, produção). |
| 2 | Dependências | Cada microserviço isola e empacota suas próprias dependências, adotando as alterações sem afetar o sistema inteiro. |
| 3 | Configurações  | As informações de configuração são movidas do microserviço e externas por meio de uma ferramenta de gerenciamento de configuração fora do código. A mesma implantação pode ser propagada entre ambientes com a configuração correta aplicada.  |
| 4 | Serviços de backup | Os recursos auxiliares (armazenamentos de dados, caches, agentes de mensagens) devem ser expostos por meio de uma URL endereçável. Isso desacopla o recurso do aplicativo, permitindo que ele seja intercambiável.  |
| 5 | Compilar, liberar, executar | Cada versão deve impor uma separação estrita nos estágios de Build, versão e execução. Cada um deve ser marcado com uma ID exclusiva e dar suporte à capacidade de reverter. Os sistemas de CI/CD modernos ajudam a atender a esse princípio. |
| 6 | Processos | Cada microserviço deve ser executado em seu próprio processo, isolado de outros serviços em execução. Externaize o estado necessário para um serviço de backup, como um cache distribuído ou armazenamento de dados. |
| 7 | Associação de porta | Cada microserviço deve ser independente com suas interfaces e funcionalidade exposta em sua própria porta. Isso fornece isolamento de outros microserviços. |
| 8 | Simultaneidade | Os serviços são expandidos em um grande número de pequenos processos idênticos (cópias), em oposição à expansão de uma única instância grande no computador mais potente disponível. |
| 9 | Disposability | As instâncias de serviço devem ser descartáveis, favorecer as inicializações rápidas para aumentar as oportunidades de escalabilidade e os desligamentos normais para deixar o sistema em um estado correto. Os contêineres do Docker junto com um orquestrador atendem inerentemente a esse requisito. |
| 10 | Paridade de desenvolvimento/prod | Mantenha os ambientes em todo o ciclo de vida do aplicativo o mais semelhante possível, evitando atalhos dispendiosos. Aqui, a adoção de contêineres pode contribuir muito promovendo o mesmo ambiente de execução. |
| 11 | Registrando em log | Tratar logs gerados por microservices como fluxos de eventos. Processe-os com um agregador de eventos e propague os dados para ferramentas de gerenciamento de log/mineração de dados como Azure Monitor ou Splunk e eventualmente arquivamento de longo prazo. |
| 12 | Processos de administração | Execute tarefas administrativas/de gerenciamento como processos únicos. As tarefas podem incluir a limpeza de dados e a obtenção de análises para um relatório. As ferramentas que executam essas tarefas devem ser invocadas no ambiente de produção, mas separadamente do aplicativo. |

No livro, [além do aplicativo de doze fatores](https://content.pivotal.io/blog/beyond-the-twelve-factor-app), o autor Kevin Hoffman detalha cada um dos 12 fatores originais (escritos em 2011). Além disso, ele aborda três fatores adicionais que refletem o design de aplicativos de nuvem moderno de hoje.

|    |  Novo fator | Explicação  |
| :-------- | :-------- | :-------- |
| 13 | API em primeiro lugar | Tornar tudo um serviço. Suponha que seu código será consumido por um cliente front-end, um gateway ou outro serviço. |
| 14 | Telemetria | Em uma estação de trabalho, você tem visibilidade profunda do seu aplicativo e seu comportamento. Na nuvem, você não tem. Verifique se o design inclui a coleção de dados de monitoramento, específicos do domínio e de integridade/sistema. |
| 15 | Autenticação/autorização  | Implemente a identidade desde o início. Considere os recursos de [RBAC (controle de acesso baseado em função)](https://docs.microsoft.com/azure/role-based-access-control/overview) disponíveis em nuvens públicas.  |

Vamos nos referir a muitos dos mais de 12 fatores neste capítulo e em todo o livro.

### <a name="critical-design-considerations"></a>Considerações de design críticas

Além das diretrizes fornecidas pela metodologia de doze fatores, há várias decisões críticas de design que você deve tomar ao construir sistemas distribuídos.

*Comunicação*

Como os aplicativos cliente front-end se comunicarão com os serviços principais de back-end? Você permitirá a comunicação direta? Ou, você pode abstrair os serviços de back-end com uma fachada de gateway que fornece flexibilidade, controle e segurança?

Como os serviços principais de back-end se comunicam entre si? Você permitirá chamadas HTTP diretas que levam ao acoplamento e impactam o desempenho e a agilidade? Ou você pode considerar as mensagens desacopladas com as tecnologias de fila e de tópico?

A comunicação é abordada em detalhes do capítulo 4, *padrões de comunicação nativos de nuvem*.

*Resiliência*

Uma arquitetura de microserviços move o sistema de dentro do processo para comunicação de rede fora do processo. Em uma arquitetura distribuída, o que acontece quando o serviço B não está respondendo a uma chamada de rede do serviço A? Ou, o que acontece quando o serviço C fica temporariamente indisponível e outros serviços que o chamam são bloqueados?

A resiliência é abordada no capítulo 6 de detalhes, *resiliência nativa na nuvem*.

*Dados distribuídos*

Por design, cada microserviço encapsula seus próprios dados, expondo as operações por meio de sua interface pública. Em caso afirmativo, como você consulta dados ou implementa uma transação em vários serviços?

Os dados distribuídos são abordados em detalhes no capítulo 5, *padrões de dados nativos de nuvem*.

*Identidade*

Como seu serviço identificará quem está acessando e quais permissões eles têm?

A identidade é abordada no capítulo 8 de detalhes, *identidade*.

## <a name="microservices"></a>Microsserviços

Os sistemas nativos de nuvem adotam os microserviços, um estilo de arquitetura popular para construir aplicativos modernos.

Criado como um conjunto distribuído de serviços pequenos e independentes que interagem por meio de uma malha compartilhada, os microserviços compartilham as seguintes características:

- Cada um implementa um recurso comercial específico dentro de um contexto de domínio maior.

- Cada um é desenvolvido de forma autônoma e pode ser implantado independentemente.

- Cada um é um encapsulamento independente de sua própria tecnologia de armazenamento de dados (SQL, NoSQL) e plataforma de programação.

- Cada é executado em seu próprio processo e se comunica com outras pessoas usando protocolos de comunicação padrão, como HTTP/HTTPS, WebSockets ou [AMQP](https://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol).

- Eles compõem juntos para formar um aplicativo.

A Figura 1-4 contrasta uma abordagem de aplicativo monolítico com uma abordagem de microserviços. Observe como o monolítico é composto de uma arquitetura em camadas, que é executada em um único processo. Normalmente, ele consome um banco de dados relacional. No entanto, a abordagem de microserviço separa a funcionalidade em serviços independentes que incluem a lógica e os dados. Cada Microservice hospeda seu próprio repositório de armazenamento.

![Implantação monolítica versus microserviços](./media/monolithic-vs-microservices.png)

**Figura 1-4.** Implantação monolítica versus microserviços

Observe como os microserviços promovem o princípio "uma base de código, um aplicativo" do [aplicativo de doze fatores](https://12factor.net/), discutido anteriormente no capítulo.

> *\#O fator 1 especifica "uma única base de código para cada microserviço, armazenado em seu próprio repositório. Acompanhado com o controle de versão, ele pode ser implantado em vários ambientes. "*

### <a name="why-microservices"></a>Por que os microsserviços?

Os microserviços fornecem agilidade.

No início do capítulo, comparamos um aplicativo de comércio eletrônico criado como um monolítico para isso com os microservices. No exemplo, vimos alguns benefícios claros:

- Cada Microservice tem um ciclo de vida autônomo e pode evoluir de forma independente e implantar com frequência. Você não precisa esperar por uma versão trimestral para implantar novos recursos ou atualizar. Você pode atualizar uma pequena área de um aplicativo complexo com menos risco de interromper todo o sistema.

- Cada microserviço pode ser dimensionado de forma independente. Em vez de dimensionar todo o aplicativo como uma única unidade, você dimensiona apenas os serviços que exigem mais capacidade de processamento ou largura de banda de rede. Essa abordagem refinada para o dimensionamento oferece maior controle do seu sistema e ajuda a reduzir os custos gerais à medida que você dimensiona partes do seu sistema, não tudo.

Um excelente guia de referência para entender os microserviços é o [.net microservices: arquitetura para aplicativos .net em contêineres](https://dotnet.microsoft.com/download/thank-you/microservices-architecture-ebook). O livro se aprofunda no design e na arquitetura de microserviços. É um complemento para uma [arquitetura de referência de microatendimento de pilha completa](https://github.com/dotnet-architecture/eShopOnContainers) disponível como um download gratuito da Microsoft.

### <a name="developing-microservices"></a>Desenvolvendo microserviços

Os microserviços podem ser criados com qualquer plataforma de desenvolvimento moderna.

A plataforma Microsoft .NET Core é uma excelente opção. Gratuito e de código aberto, ele tem muitos recursos internos para simplificar o desenvolvimento de microserviço. O .NET Core é uma plataforma cruzada. Os aplicativos podem ser criados e executados no Windows, no macOS e na maioria dos tipos de Linux.

O .NET Core é altamente funcional e tem um bom desempenho em comparação com Node.js e outras plataformas concorrentes. Curiosamente, a [TechEmpower](https://www.techempower.com/) realizou um amplo conjunto de [benchmarks de desempenho](https://www.techempower.com/benchmarks/#section=data-r17&hw=ph&test=plaintext) em várias plataformas e estruturas de aplicativos Web. O .NET Core foi pontuado nos 10 principais, bem acima Node.js e em outras plataformas concorrentes.

O .NET Core é mantido pela Microsoft e pela Comunidade do .NET no GitHub.

## <a name="containers"></a>Contêineres

Hoje em dia, é natural ouvir o *contêiner* de termos mencionado em qualquer conversa relacionada à *nuvem nativa*. No livro, os [padrões nativos de nuvem](https://www.manning.com/books/cloud-native-patterns), o autor Cornelia Davis observa que, "os contêineres são um ótimo capacitador de software nativo de nuvem". A computação nativa de nuvem Foundation coloca a contêinerização de microserviço como a primeira etapa em seu [mapa de rastros de nuvem nativa](https://raw.githubusercontent.com/cncf/trailmap/master/CNCF_TrailMap_latest.png) -orientação para empresas que começam sua jornada nativa de nuvem.

O contêiner de um microserviço é simples e direto. O código, suas dependências e tempo de execução são empacotados em um binário chamado [imagem de contêiner](https://docs.docker.com/glossary/?term=image). As imagens são armazenadas em um [registro de contêiner](https://caylent.com/container-registries/), que atua como um repositório ou biblioteca para imagens. Um registro pode estar localizado em seu computador de desenvolvimento, em seu data center ou em uma nuvem pública. O Docker em si mantém um registro público por meio do [Hub do Docker](https://hub.docker.com/). A nuvem do Azure apresenta um [registro de contêiner](https://azure.microsoft.com/services/container-registry/) para armazenar imagens de contêiner perto dos aplicativos de nuvem que os executarão.

Quando necessário, você transforma a imagem em uma instância de contêiner em execução. A instância é executada em qualquer computador que tenha um mecanismo de [tempo de execução de contêiner](https://kubernetes.io/docs/setup/production-environment/container-runtimes/) instalado. Você pode ter tantas instâncias do serviço em contêineres, conforme necessário.

A Figura 1-5 mostra três microserviços diferentes, cada um em seu próprio contêiner, em execução em um único host.

![Vários contêineres em execução em um host de contêiner](./media/hosting-mulitple-containers.png)

**Figura 1-5**. Vários contêineres em execução em um host de contêiner

Observe como cada contêiner mantém seu próprio conjunto de dependências e tempo de execução, que pode ser diferente. Aqui, vemos versões diferentes do microserviço de produto em execução no mesmo host. Cada contêiner compartilha uma fatia do sistema operacional do host subjacente, da memória e do processador, mas é isolado uns dos outros.

Observe como o modelo de contêiner adota o princípio de "dependências" do [aplicativo de doze fatores](https://12factor.net/).

> *O fator \# 2 especifica que "cada microserviço isola e empacota suas próprias dependências, adotando as alterações sem afetar o sistema inteiro".*

Contêineres dão suporte a cargas de trabalho do Linux e do Windows. A nuvem do Azure adota as duas opções. Curiosamente, é Linux, não Windows Server, que se tornou o sistema operacional mais popular no Azure.

Embora existam vários fornecedores de contêineres, o Docker capturou a participação de Lion do mercado. A empresa está conduzindo o movimento do contêiner de software. Ele se tornou o padrão de fato para empacotamento, implantação e execução de aplicativos nativos de nuvem.

### <a name="why-containers"></a>Por que contêineres?

Os contêineres fornecem portabilidade e garantem a consistência entre ambientes. Ao encapsular tudo em um único pacote, você *isola* o microserviço e suas dependências da infraestrutura subjacente.

Você pode implantar esse mesmo contêiner em qualquer ambiente que tenha o mecanismo de tempo de execução do Docker. As cargas de trabalho em contêineres também eliminam a despesa de pré-configurar cada ambiente com estruturas, bibliotecas de software e mecanismos de tempo de execução.

Ao compartilhar o sistema operacional subjacente e os recursos do host, os contêineres têm uma superfície muito menor do que uma máquina virtual completa. O tamanho menor aumenta a *densidade*, ou o número de microserviços, que um determinado host pode executar ao mesmo tempo.

### <a name="container-orchestration"></a>Orquestração de contêiner

Embora ferramentas como o Docker criem imagens e executem contêineres, você também precisará de ferramentas para gerenciá-las. O gerenciamento de contêineres é feito com um programa de software especial chamado orquestrador de contêiner. Ao operar em escala, a orquestração de contêiner é essencial.

A Figura 1-6 mostra as tarefas de gerenciamento que os orquestradores de contêiner fornecem.

![O que os orquestradores de contêiner fazem](./media/what-container-orchestrators-do.png)

**Figura 1-6**. O que os orquestradores de contêiner fazem

A tabela a seguir descreve as tarefas de orquestração comuns.

|  Tarefas | Explicação  |
| :-------- | :-------- |
| Agendamento | Provisione automaticamente instâncias de contêiner.|
| Afinidade/antiafinidade | Provisione contêineres próximos ou distantes uns dos outros, ajudando a disponibilidade e o desempenho. |
| Monitoramento da integridade | Detectar e corrigir automaticamente as falhas.|
| Failover | Reprovisionar automaticamente a instância com falha para computadores íntegros.|
| Scaling | Adicione ou remova automaticamente a instância de contêiner para atender à demanda.|
| Rede | Gerenciar uma sobreposição de rede para comunicação de contêiner.|
| Descoberta de Serviços | Habilite os contêineres para localizar um ao outro.|
| Atualizações sem interrupção | Coordene atualizações incrementais com uma implantação sem tempo de inatividade. Reverter alterações problemáticas automaticamente.|

Observe como os orquestradores adotam os princípios de Disposability e simultaneidade do [aplicativo de doze fatores](https://12factor.net/), discutido anteriormente no capítulo.

> *O fator \# 9 especifica que "as instâncias de serviço devem ser descartáveis, favorecendo as inicializações rápidas para aumentar as oportunidades de escalabilidade e os desligamentos normais para deixar o sistema em um estado correto. Os contêineres do Docker junto com um orquestrador atendem inerentemente a esse requisito. "*

> *\#O fator 8 especifica que "os serviços se expandem por um grande número de pequenos processos idênticos (cópias), em oposição à expansão de uma única instância grande no computador mais potente disponível".*

Embora existam vários orquestradores de contêiner, [kubernetes](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/) se tornou o padrão de fato para o mundo nativo da nuvem. É uma plataforma portátil, extensível e de software livre para gerenciar cargas de trabalho em contêineres.

Você pode hospedar sua própria instância do kubernetes, mas seria responsável por provisionar e gerenciar seus recursos, o que pode ser complexo. Os recursos de nuvem do Azure kubernetes como um serviço gerenciado, o [AKs (serviço kubernetes do Azure)](https://azure.microsoft.com/services/kubernetes-service/). Um serviço gerenciado permite que você aproveite totalmente seus recursos, sem precisar instalá-lo e mantê-lo.

Os serviços Kubernetess do Azure são abordados no capítulo 2 de detalhes, *Dimensionando aplicativos nativos de nuvem*.

## <a name="backing-services"></a>Serviços de backup

Os sistemas nativos de nuvem dependem de vários recursos auxiliares diferentes, como armazenamentos de dados, agentes de mensagens, monitoramento e serviços de identidade. Esses serviços são conhecidos como [serviços de backup](https://12factor.net/backing-services).

 A Figura 1-7 mostra muitos serviços de apoio comuns que os sistemas nativos de nuvem consomem.

![Serviços de suporte comuns](./media/common-backing-services.png)

**Figura 1-7**. Serviços de suporte comuns

Os serviços de backup promovem o princípio "sem estado" do [aplicativo de doze fatores](https://12factor.net/), discutido anteriormente no capítulo.

>O *fator \# 6* especifica que, "cada microserviço deve ser executado em seu próprio processo, isolado de outros serviços em execução. Externaize o estado necessário para um serviço de backup, como um cache distribuído ou armazenamento de dados. "

Você pode hospedar seus próprios serviços de backup, mas, em seguida, você será responsável por licenciar, provisionar e gerenciar esses recursos.

Os provedores de nuvem oferecem uma vasta variedade de *serviços de backup gerenciados.* Em vez de possuir o serviço, basta consumi-lo. O provedor opera o recurso em escala e tem a responsabilidade por desempenho, segurança e manutenção. O monitoramento, a redundância e a disponibilidade são incorporados ao serviço. Os provedores dão suporte total aos serviços gerenciados – Abra um tíquete e eles corrigem seu problema.

Os sistemas nativos de nuvem favorecem serviços de backup gerenciados de fornecedores de nuvem. A economia em tempo e trabalho é excelente. O risco operacional de hospedar seu próprio e enfrentar problemas pode ser mais caro.

Uma prática recomendada é tratar um serviço de backup como um *recurso anexado*, vinculado dinamicamente a um Microservice com informações (uma URL e credenciais) armazenadas em uma configuração externa. Este guia é escrito no aplicativo de [doze fatores](https://12factor.net/), discutido anteriormente no capítulo.

>O *fator \# 4* especifica que os serviços de backup "devem ser expostos por meio de uma URL endereçável. Isso desacopla o recurso do aplicativo, permitindo que ele seja intercambiável. "

>O *fator \# 3* especifica que "as informações de configuração são movidas do microserviço e externas por meio de uma ferramenta de gerenciamento de configuração fora do código".

Com esse padrão, um serviço de backup pode ser anexado e desanexado sem alterações de código. Você pode promover um microserviço de QA para um ambiente de preparo. Você atualiza a configuração de microserviço para apontar para os serviços de backup em preparo e injetar as configurações em seu contêiner por meio de uma variável de ambiente.

Os fornecedores de nuvem fornecem APIs para que você se comunique com seus serviços de apoio proprietários. Essas bibliotecas encapsulam o encanamento e a complexidade. A comunicação direta com essas APIs irá acoplar rigidamente seu código ao serviço de backup. É uma prática melhor isolar os detalhes de implementação da API do fornecedor. Introduza uma camada de intermediação ou uma API intermediária, expondo operações genéricas ao seu código de serviço. Esse acoplamento flexível permite que você troque um serviço de backup por outro ou mova seu código para uma nuvem pública diferente sem precisar fazer alterações no código de serviço principal.

Os serviços de backup são discutidos em detalhes capítulo 5, *padrões de dados nativos de nuvem*e capítulo 4, *padrões de comunicação nativas de nuvem*.

## <a name="automation"></a>Automação

Como você viu, os sistemas nativos de nuvem adotam microserviços, contêineres e design de sistema moderno para atingir velocidade e agilidade. Mas isso é apenas parte da história. Como provisionar os ambientes de nuvem nos quais esses sistemas são executados? Como você implanta rapidamente recursos e atualizações do aplicativo? Como você arredondar o panorama completo?

Insira a prática amplamente aceita da [Infraestrutura como código](https://docs.microsoft.com/azure/devops/learn/what-is-infrastructure-as-code)ou IaC.

Com o IaC, você automatiza o provisionamento de plataforma e a implantação de aplicativos. Essencialmente, você aplica práticas de engenharia de software, como teste e controle de versão, às suas práticas de DevOps. Sua infraestrutura e implantações são automatizadas, consistentes e reproduzíveis.

### <a name="automating-infrastructure"></a>Automatizando a infraestrutura

Ferramentas como [Azure Resource Manager](https://azure.microsoft.com/documentation/articles/resource-group-overview/), Terraform e o [CLI do Azure](https://docs.microsoft.com/cli/azure/), permitem que você declarate o script de infraestrutura de nuvem de forma declarativa. Nomes de recursos, locais, capacidades e segredos são parametrizados e dinâmicos. O script tem versão e fez check-in do controle do código-fonte como um artefato do seu projeto. Você invoca o script para provisionar uma infraestrutura consistente e reproduzível entre ambientes de sistema, como QA, preparo e produção.

Nos bastidores, IaC é idempotente, o que significa que você pode executar o mesmo script repetidamente sem efeitos colaterais. Se a equipe precisar fazer uma alteração, ela Editará e executará novamente o script. Somente os recursos atualizados são afetados.

No artigo, [o que é infraestrutura como código](https://docs.microsoft.com/azure/devops/learn/what-is-infrastructure-as-code), autor Sam Guckenheimer descreve como "as equipes que implementam o IaC podem fornecer ambientes estáveis rapidamente e em escala. As equipes evitam a configuração manual de ambientes e impõem a consistência, representando o estado desejado de seus ambientes por meio de código. Implantações de infraestrutura com IaC são repetíveis e evitam problemas de tempo de execução causados por descompasso de configuração ou dependências ausentes. As equipes do DevOps podem trabalhar junto com um conjunto unificado de práticas e ferramentas para fornecer aplicativos e sua infraestrutura de suporte de forma rápida, confiável e em escala. "

### <a name="automating-deployments"></a>Automatizando implantações

O [aplicativo de doze fatores](https://12factor.net/), discutido anteriormente, chama para etapas separadas ao transformar o código concluído em um aplicativo em execução.

> O *fator \# 5* especifica que "cada versão deve impor uma separação estrita nos estágios de Build, versão e execução. Cada um deve ser marcado com uma ID exclusiva e dar suporte à capacidade de reverter. "

Os sistemas de CI/CD modernos ajudam a atender a esse princípio. Eles fornecem etapas de implantação separadas e ajudam a garantir o código consistente e de qualidade que está prontamente disponível para os usuários.

A Figura 1-8 mostra a separação entre o processo de implantação.

![Etapas de implantações no pipeline de CI/CD](./media/build-release-run-pipeline.png)

**Figura 1-8**. Etapas de implantação em um pipeline de CI/CD

Na figura anterior, preste atenção especial à separação de tarefas.

O desenvolvedor constrói um recurso em seu ambiente de desenvolvimento, Iterando por meio do que é chamado de "loop interno" de código, execução e depuração. Ao concluir, esse código é *enviado por push* para um repositório de código, como GitHub, Azure DevOps ou BitBucket.

O push dispara um estágio de compilação que transforma o código em um artefato binário. O trabalho é implementado com um pipeline de [CI (integração contínua)](https://martinfowler.com/articles/continuousIntegration.html) . Ele cria, testa e empacota automaticamente o aplicativo.

O estágio de lançamento pega o artefato binário, aplica informações de configuração de ambiente e aplicativo externo e produz uma versão imutável. A versão é implantada em um ambiente especificado. O trabalho é implementado com um pipeline de [entrega contínua (CD)](https://martinfowler.com/bliki/ContinuousDelivery.html) . Cada versão deve ser identificável. Você pode dizer "esta implantação está executando a versão 2.1.1 do aplicativo".

Por fim, o recurso liberado é executado no ambiente de execução de destino. As versões são imutáveis, o que significa que qualquer alteração deve criar uma nova versão.

Aplicando essas práticas, as organizações evoluíram radicalmente como elas enviam software. Muitos foram movidos de versões trimestrais para atualizações sob demanda. O objetivo é capturar problemas no início do ciclo de desenvolvimento quando eles são menos caros de serem corrigidos. Quanto maior a duração entre as integrações, os problemas mais caros se tornarão resolvidos.  Com a consistência no processo de integração, as equipes podem confirmar as alterações de código com mais frequência, levando a uma melhor colaboração e qualidade de software.

### <a name="azure-pipelines"></a>Azure Pipelines

A nuvem do Azure inclui um novo serviço de CI/CD intitulado [Azure pipelines](https://azure.microsoft.com/services/devops/pipelines/), que faz parte da oferta do [Azure DevOps](https://azure.microsoft.com/services/devops/) mostrada na Figura 1-9.

![Azure Pipelines em DevOps](./media/devops-components.png)

**Figura 1-9**. Ofertas do Azure DevOps

Azure Pipelines é um serviço de nuvem que combina CI (integração contínua) e CD (entrega contínua). Você pode testar, compilar e enviar seu código automaticamente para qualquer destino.

Você define seu pipeline no código em um arquivo YAML junto com o restante do código para seu aplicativo.

- O pipeline tem controle de versão com seu código e segue a mesma estrutura de ramificação.
- Você obtém a validação de suas alterações por meio de revisões de código em solicitações de pull e políticas de build de ramificações.
- Cada ramificação que você usa pode personalizar a política de compilação modificando o arquivo Azure-pipelines. yml.
- O arquivo de pipeline é verificado no controle de versão e pode ser investigado se houver um problema.

O serviço de Azure Pipelines dá suporte à maioria dos provedores git e pode gerar pipelines de implantação para aplicativos escritos nas plataformas Linux, macOS ou Windows. Ele inclui suporte para Java, .NET, JavaScript, Python, PHP, go, XCode e C++.

>[!div class="step-by-step"]
>[Anterior](introduction.md) 
> [Avançar](candidate-apps.md)
