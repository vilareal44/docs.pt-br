---
title: Padrão de gateway de API versus comunicação direta de cliente com microsserviço
description: Entenda as diferenças e os usos do padrão de gateway de API e da comunicação direta de cliente com microsserviço.
ms.date: 01/07/2019
ms.openlocfilehash: 089b6302132437e4bb733653b3edb401ff81a164
ms.sourcegitcommit: 5280b2aef60a1ed99002dba44e4b9e7f6c830604
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84306949"
---
# <a name="the-api-gateway-pattern-versus-the-direct-client-to-microservice-communication"></a>Padrão de gateway de API versus comunicação direta de cliente com microsserviço

Em uma arquitetura de microsserviços, cada microsserviço expõe um conjunto de pontos de extremidade (tipicamente) refinados. Esse fato pode afetar a comunicação de cliente com microsserviço, conforme explicado nesta seção.

## <a name="direct-client-to-microservice-communication"></a>Comunicação direta de cliente com microsserviço

Uma abordagem possível é usar uma arquitetura de comunicação direta de cliente com microsserviço. Nessa abordagem, um aplicativo cliente pode fazer solicitações diretamente para alguns dos microsserviços, conforme mostra a Figura 4-12.

![Diagrama mostrando a arquitetura de comunicação de cliente para microserviço.](./media/direct-client-to-microservice-communication.png)

**Figura 4-12**. Usando uma arquitetura de comunicação direta de cliente com microsserviço

Nessa abordagem, cada microsserviço tem um ponto de extremidade público, às vezes, com uma porta TCP diferente para cada microsserviço. Um exemplo de uma URL para um serviço específico pode ser a URL a seguir no Azure:

`http://eshoponcontainers.westus.cloudapp.azure.com:88/`

Em um ambiente de produção com base em um cluster, essa URL mapearia para o balanceador de carga usado no cluster, que, por sua vez, distribui as solicitações entre os microsserviços. Em ambientes de produção, você pode ter um ADC (Controlador de Entrega de Aplicativo) como o [Gateway de Aplicativo do Azure](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) entre os microsserviços e a Internet. Isso funciona como uma camada transparente que não executa balanceamento de carga, mas protege seus serviços ao oferecer terminação SSL. Isso aumenta a carga de seus hosts descarregando terminação SSL com uso intensivo de CPU e outras tarefas de roteamento para o Gateway de Aplicativo do Azure. Em qualquer caso, um balanceador de carga e ADC são transparentes de um ponto de vista da arquitetura do aplicativo lógico.

Uma arquitetura de comunicação direta de cliente com microsserviço pode ser boa o bastante para um aplicativo baseado em microsserviço pequeno, especialmente se o aplicativo cliente for um aplicativo Web do lado do servidor como um aplicativo ASP.NET MVC. No entanto, quando você cria aplicativos grandes e complexos baseados em microsserviço (por exemplo, ao lidar com dezenas de tipos de microsserviço), e especialmente quando os aplicativos cliente são aplicativos móveis remotos ou aplicativos Web do SPA, essa abordagem enfrenta alguns problemas.

Considere as seguintes questões ao desenvolver um aplicativo grande com base em microsserviços:

- *Como os aplicativos cliente podem minimizar o número de solicitações para o back-end e reduzir a comunicação excessiva com vários microsserviços?*

Interagir com vários microsserviços para criar uma única tela de interface do usuário aumenta o número de viagens de ida e volta pela Internet. Isso aumenta a latência e a complexidade do lado da interface do usuário. O ideal é que as respostas sejam agregadas com eficiência no lado do servidor. Isso reduz a latência, já que várias partes de dados voltam em paralelo e algumas interfaces do usuário podem mostrar dados assim que eles estão prontos.

- *Como você pode lidar com preocupações abrangentes, como autorização, transformações de dados e despacho de solicitação dinâmica?*

Implementar preocupações relativas a segurança e abrangentes, como segurança e autorização em cada microsserviço, pode exigir um esforço significativo de desenvolvimento. Uma abordagem possível é ter esses serviços no host do Docker ou no cluster interno para restringir o acesso direto a eles do lado de fora e implementar esses interesses paralelos em um local centralizado, como um Gateway de API.

- *Como os aplicativos cliente podem se comunicar com serviços que usam protocolos não amigáveis para a Internet?*

Protocolos usados no lado do servidor (como AMQP ou protocolos binários) geralmente não são compatíveis com aplicativos cliente. Portanto, as solicitações devem ser executadas por meio de protocolos como HTTP/HTTPS e convertidas em outros protocolos posteriormente. Uma abordagem *man-in-the-middle* pode ajudar nessa situação.

- *Como é possível moldar uma fachada feita especialmente para aplicativos móveis?*

A API de vários microsserviços podem não ser bem projetadas para as necessidades de diferentes aplicativos cliente. Por exemplo, as necessidades de um aplicativo móvel podem ser diferentes das necessidades de um aplicativo Web. Para aplicativos móveis, convém otimizar ainda mais para que as respostas de dados possam ser mais eficientes. Você pode fazer isso agregando dados de vários microsserviços e retornando um único conjunto de dados e, às vezes, eliminando os dados na resposta que não são necessários para o aplicativo móvel. E, claro, você pode compactar dados. Novamente, uma fachada ou uma API entre o aplicativo móvel e os microsserviços pode ser conveniente para esse cenário.

## <a name="why-consider-api-gateways-instead-of-direct-client-to-microservice-communication"></a>Por que considerar o uso de Gateways de API em vez de comunicação direta de cliente com microsserviço

Em uma arquitetura de microsserviços, os aplicativos cliente normalmente precisam consumir a funcionalidade de mais de um microsserviço. Se esse consumo for executado diretamente, o cliente precisará manipular várias chamadas para terminais de microsserviço. O que acontece quando o aplicativo evolui e novos microsserviços são introduzidos ou os microsserviços existentes são atualizados? Se o seu aplicativo tiver muitos microsserviços, lidar com tantos pontos de extremidade dos aplicativos cliente pode ser um pesadelo. Como o aplicativo cliente seria acoplado a esses pontos de extremidade internos, a evolução dos microsserviços no futuro pode causar alto impacto aos aplicativos cliente.

Portanto, ter um nível intermediário ou indireto (Gateway) pode ser muito conveniente para aplicativos baseados em microsserviço. Se você não tiver Gateways de API, os aplicativos do cliente deverão enviar solicitações diretamente aos microsserviços, o que causará problemas como os seguintes:

- **Acoplamento**: sem o padrão de Gateway de API, os aplicativos cliente são acoplados aos microsserviços internos. Os aplicativos clientes precisam saber como as várias áreas do aplicativo são decompostas em microsserviços. Ao evoluir e refatorar os microserviços internos, essas ações impactam a manutenção porque causam alterações significativas nos aplicativos cliente devido à referência direta aos microserviços internos dos aplicativos cliente. Os aplicativos clientes precisam ser atualizados com frequência,o que dificulta a evolução da solução.

- **Muitas viagens de ida e volta**: uma única página/tela no aplicativo cliente pode exigir diversas chamadas para vários serviços. Isso pode resultar em várias viagem de ida e volta na rede entre o cliente e o servidor, adicionando latência significativa. A agregação manipulada em um nível intermediário pode melhorar o desempenho e a experiência do usuário para o aplicativo cliente.

- **Problemas de segurança**: sem um gateway, todos os microsserviços precisam ser expostos ao "mundo externo", tornando a superfície de ataque maior do que se você ocultar microsserviços internos que não são usados ​​diretamente pelos aplicativos cliente. Quanto menor a superfície de ataque, mais segura sua aplicação pode ser.

- **Preocupações**abrangentes: cada microserviço publicado publicamente deve lidar com questões como autorização e SSL. Em muitas situações, esses interessas podem ser tratados em uma única camada para que os microsserviços internos sejam simplificados.

## <a name="what-is-the-api-gateway-pattern"></a>Por que o padrão de Gateway de API?

Quando você projeta e cria aplicativos grandes ou complexos baseado em microsserviço com vários aplicativos cliente, uma boa abordagem a considerar pode ser um [Gateway de API](https://microservices.io/patterns/apigateway.html). Esse é um serviço que fornece um único ponto de entrada para determinados grupos de microsserviços. É semelhante ao [padrão fachada](https://en.wikipedia.org/wiki/Facade_pattern) do design orientado a objeto, mas, nesse caso, faz parte de um sistema distribuído. O padrão de Gateway de API às vezes também é conhecido como [BFF](https://samnewman.io/patterns/architectural/bff/) ("back-end para front-end"), porque ele é criado pensando nas necessidades do aplicativo cliente.

Portanto, o gateway da API fica entre os aplicativos cliente e os microsserviços. Ele atua como um proxy reverso, encaminhando as solicitações de clientes para serviços. Ele também pode fornecer outros recursos paralelos, como autenticação, terminação SSL e cache.

A Figura 4-13 mostra como um Gateway de API personalizado pode se encaixar em uma arquitetura simplificada baseada em microsserviço com apenas alguns microsserviços.

![Diagrama que mostra um gateway de API implementado como um serviço personalizado.](./media/direct-client-to-microservice-communication-versus-the-API-Gateway-pattern/custom-service-api-gateway.png)

**Figura 4-13**. Uso de um Gateway de API implementado como um serviço personalizado

Os aplicativos se conectam a um único ponto de extremidade, o gateway de API, que é configurado para encaminhar solicitações a microserviços individuais. Neste exemplo, o Gateway de API deve ser implementado como um serviço de WebHost ASP.NET Core personalizado em execução como um contêiner.

É importante destacar que, neste diagrama, você usa um único serviço personalizado de Gateway de API, voltado para vários aplicativos cliente diferentes. Esse fato pode ser um risco importante, pois seu serviço de Gateway de API estará crescendo e em evolução com base em vários requisitos diferentes dos aplicativos cliente. Por fim, ele será inflado por causa dessas diferentes necessidades e efetivamente pode ser semelhante a um aplicativo monolítico ou serviço monolítico. É por isso que é muito recomendável dividir o Gateway de API em vários serviços ou vários Gateways de API menores, um por tipo de fator forma de aplicativo cliente, por exemplo.

É necessário ter cuidado ao implementar o padrão de Gateway de API. Geralmente, não é recomendável ter um único Gateway de API para agregar todos os microsserviços internos do seu aplicativo. Em caso afirmativo, ele atua como um agregador ou orquestrador monolítico e viola a autonomia de microsserviço acoplando todos os microsserviços.

Portanto, os Gateways de API devem ser segregados com base nos limites de negócios e nos aplicativos cliente, e não agir como um agregador único para todos os microsserviços internos.

Ao dividir a camada do Gateway de API em vários Gateways de API, se o aplicativo tiver vários aplicativos cliente, isso poderá ser um fator fundamental ao identificar os vários tipos de Gateways de API, de modo que você possa ter uma fachada distinta para as necessidades de cada aplicativo cliente. Esse caso é um padrão chamado "back-end para front-end" ([BFF](https://samnewman.io/patterns/architectural/bff/)), em que cada gateway de API pode fornecer uma API diferente adaptada para cada tipo de aplicativo cliente, possivelmente mesmo com base no fator forma de cliente implementando um código de adaptador específico que, sob a chamada de vários microserviços internos, conforme mostrado na imagem a seguir:

![Diagrama mostrando vários gateways de API personalizados.](./media/direct-client-to-microservice-communication-versus-the-API-Gateway-pattern/multiple-custom-api-gateways.png)

**Figura 4-13.1**. Uso de mostrando vários Gateways de API personalizados

Figura 4-13.1 mostra os gateways de API que são separados por tipo de cliente; um para clientes móveis e outro para clientes Web. Um aplicativo Web tradicional se conecta a um microsserviço MVC que usa o Gateway de API Web. O exemplo ilustra uma arquitetura simplificada com vários gateways de API refinados. Nesse caso, os limites identificados para cada Gateway de API são baseados puramente no padrão [BFF](https://samnewman.io/patterns/architectural/bff/) ("Back-end para Front-end"), portanto, baseados apenas na API necessária por aplicativo cliente. Porém, em aplicativos maiores, você deve ir além e criar Gateways de API adicionais com base nos limites de negócios como um segundo fator de design.

## <a name="main-features-in-the-api-gateway-pattern"></a>Principais recursos do padrão de Gateway de API

Um Gateway de API pode oferecer vários recursos. Dependendo do produto, ele pode oferecer recursos mais avançados ou mais simples, no entanto, os recursos mais importantes e fundamentais para qualquer Gateway de API são os seguintes padrões de design:

**Proxy reverso ou roteamento de gateway.** O Gateway de API oferece um proxy reverso para redirecionar ou encaminhar solicitações (roteamento da camada 7, geralmente solicitações HTTP) para os pontos de extremidade dos microsserviços internos. O gateway fornece um único ponto de extremidade ou URL para os aplicativos clientes e, em seguida, mapeia internamente as solicitações para um grupo de microsserviços internos. Esse recurso de roteamento ajuda a desacoplar os aplicativos cliente dos microserviços, mas também é conveniente ao modernizar uma API monolítica posicionando o gateway de API entre a API monolítica e os aplicativos cliente, então você pode adicionar novas APIs como novos microserviços enquanto ainda usa a API monolítica herdada até que ela seja dividida em muitos microserviços no futuro. Devido ao Gateway de API, os aplicativos cliente não notarão se as APIs em uso forem implementadas como microsserviços internos ou uma API monolítica. O mais importante é o fato que, ao evoluir e refatorar a API monolítica em microsserviços, graças ao roteamento do Gateway de API, os aplicativos cliente não serão afetados por nenhuma alteração de URI.

Para saber mais, confira [Padrão de roteamento do gateway](https://docs.microsoft.com/azure/architecture/patterns/gateway-routing).

**Solicitações de agregação.** Como parte do padrão de gateway, é possível agregar várias solicitações de cliente (geralmente solicitações HTTP) direcionadas a vários microsserviços internos em uma única solicitação de cliente. Esse padrão é especialmente conveniente quando uma página/tela do cliente precisa de informações de vários microsserviços. Com essa abordagem, o aplicativo cliente envia uma única solicitação ao Gateway da API, que envia várias solicitações aos microsserviços internos e, em seguida, agrega os resultados e envia tudo de volta ao aplicativo cliente. O principal benefício e o objetivo desse padrão de design é reduzir a informação entre os aplicativos cliente e a API de back-end, que é especialmente importante para aplicativos remotos do datacenter em que os microserviços residem, como aplicativos móveis ou solicitações provenientes de aplicativos de SPA provenientes do JavaScript em navegadores remotos do cliente. Para aplicativos Web comuns que executam as solicitações no ambiente do servidor (como um aplicativo Web ASP.NET Core MVC), esse padrão não é tão importante, já que a latência é muito menor do que em aplicativos cliente remotos.

Dependendo do produto do Gateway de API usado, ele poderá executar essa agregação. No entanto, em muitos casos, é mais flexível criar microsserviços de agregação no escopo do Gateway de API, de modo que você defina a agregação no código (ou seja, o código C#):

Para saber mais, confira o [Padrão de agregação de Gateway](https://docs.microsoft.com/azure/architecture/patterns/gateway-aggregation).

**Interesses paralelos ou descarregamento de gateway.** Dependendo dos recursos oferecidos por cada produto do Gateway de API, você pode descarregar a funcionalidade de microsserviços individuais para o gateway, o que simplifica a implementação de cada microsserviço ao consolidar interesses paralelos em uma camada. Isso é especialmente conveniente para recursos especializados que podem ser complexos de implementar adequadamente em cada microsserviço interno, como as seguintes funcionalidades:

- Autenticação e autorização
- Integração de serviços de descoberta
- Cache de resposta
- Políticas de repetição, disjuntor e QoS
- Limitação de taxa
- Balanceamento de carga
- Registrando em log, rastreamento, correlação
- Cabeçalhos, cadeias de caracteres de consulta e transformação de declarações
- Lista de permissões de IP

Para saber mais, confira o [Padrão de descarregamento de Gateway](https://docs.microsoft.com/azure/architecture/patterns/gateway-offloading).

## <a name="using-products-with-api-gateway-features"></a>Uso de produtos com recursos do Gateway de API

Pode haver muitos outros interesses paralelos oferecidos pelos produtos de Gateways de API, dependendo de cada implementação. Exploraremos aqui:

- [Gerenciamento de API do Azure](https://azure.microsoft.com/services/api-management/)
- [Ocelot](https://github.com/ThreeMammals/Ocelot)

### <a name="azure-api-management"></a>Gerenciamento de API do Azure

O [Gerenciamento de API do Azure](https://azure.microsoft.com/services/api-management/) (conforme mostrado na Figura 4-14) não apenas soluciona as necessidades do Gateway de API, mas também fornece funcionalidades como coleta de insights das APIs. Se você estiver usando uma solução de gerenciamento de API, um Gateway de API será apenas um componente nessa solução completa de gerenciamento de API.

![Diagrama mostrando como usar o gerenciamento de API do Azure como seu gateway de API.](./media/direct-client-to-microservice-communication-versus-the-API-Gateway-pattern/api-gateway-azure-api-management.png)

**Figura 4-14**. Usando o Gerenciamento de API do Azure para o Gateway de API

O gerenciamento de API do Azure resolve o gateway de API e as necessidades de gerenciamento, como log, segurança, medição, etc. Nesse caso, ao usar um produto como o gerenciamento de API do Azure, o fato de que você pode ter um único gateway de API não é tão arriscado, pois esses tipos de gateways de API são "mais finos", o que significa que você não implementa código C# personalizado que poderia evoluir em direção a um componente monolítico.

Os produtos de Gateway de API costumam atuar como um proxy reverso para comunicação de entrada, em que você também pode filtrar as APIs dos microsserviços internos e aplicar a autorização para as APIs publicadas nessa camada única.

As informações disponíveis do sistema Gerenciamento de API ajudam a entender como as suas APIs estão sendo usadas e como elas são executadas. Isso ocorre porque você pode exibir relatórios de análise quase em tempo real e identificar tendências que podem afetar seus negócios. Além disso, você pode ter logs sobre a atividade de solicitação e resposta para mais análises online e offline.

Com o Gerenciamento de API do Azure, você pode proteger suas APIs usando uma chave, um token e filtragem de IP. Esses recursos permitem que você imponha cotas flexíveis e refinadas e limites de taxa, modifique a forma e o comportamento de suas APIs usando políticas e melhore o desempenho com o cache de resposta.

Neste guia e no aplicativo de exemplo de referência (eShopOnContainers), a arquitetura é limitada a uma arquitetura em contêiner mais simples e personalizada para focar em contêineres simples sem o uso de produtos PaaS como Gerenciamento de API do Azure. Mas, para grandes aplicativos baseados em microsserviço implantados no Microsoft Azure, recomendamos que você avaliar o Gerenciamento de API do Azure como base para seus Gateways de API em produção.

### <a name="ocelot"></a>Ocelot

O [Ocelot](https://github.com/ThreeMammals/Ocelot) é um Gateway de API leve, recomendado para abordagens mais simples. O Ocelot é um gateway de API baseado em .NET Core de software livre, especialmente feito para arquiteturas de microserviços que precisam de pontos unificados de entrada em seus sistemas. É leve, rápido e escalonável e fornece roteamento e autenticação entre muitos outros recursos.

O principal motivo para escolher Ocelot para o [aplicativo de referência eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) é porque o Ocelot é um gateway de API leve do .NET Core que pode ser implantado no mesmo ambiente de implantação de aplicativos em que você está implantando seus microserviços/contêineres, como um host do Docker, kubernetes, etc. E, como ele é baseado no .NET Core, ele é multiplataforma que permite que você implante no Linux ou no Windows.

Os diagramas anteriores que mostram os Gateways de API personalizados em execução nos contêineres correspondem precisamente ao modo como você também pode executar o Ocelot em um contêiner e em um aplicativo baseado em microsserviço.

Além disso, existem muitos outros produtos no mercado que oferecem recursos de Gateways de API, como Apigee, Kong, MuleSoft, WSO2 e outros produtos como Linkerd e Istio para recursos de controlador de entrada de malha de serviço.

Após as seções iniciais de explicação sobre arquitetura e padrões, as próximas seções explicam como implementar Gateways de API com [Ocelot](https://github.com/ThreeMammals/Ocelot).

## <a name="drawbacks-of-the-api-gateway-pattern"></a>Desvantagens do padrão de Gateway de API

- A desvantagem mais importante é que, quando você implementa um Gateway de API, está acoplando essa camada aos microsserviços internos. Um acoplamento como este pode introduzir problemas sérios no seu aplicativo. Clemens Vaster, arquiteto na equipe do Barramento de Serviço do Azure, refere-se a essa possível dificuldade como "o novo ESB" na sessão "[Mensagens e microsserviços](https://www.youtube.com/watch?v=rXi5CLjIQ9k)" na GOTO 2016.

- Usar um Gateway de API de microsserviços cria um possível ponto único de falha adicional.

- Um Gateway de API pode apresentar um maior tempo de resposta devido à chamada de rede adicional. No entanto, essa chamada extra normalmente causa um menor impacto do que ter uma interface do cliente com excesso de chamadas diretas aos microsserviços internos.

- Se não for dimensionado corretamente, o Gateway de API poderá se tornar um gargalo.

- Um Gateway de API requer custos adicionais de desenvolvimento e manutenção futura se incluir lógica personalizada e agregação de dados. Os desenvolvedores precisam atualizar o Gateway de API para expor os pontos de extremidade de cada microsserviço. Além disso, as alterações de implementação nos microsserviços internos podem causar alterações de código no nível do Gateway da API. No entanto, se o Gateway de API estiver apenas aplicando segurança, logon e controle de versão (como ao usar o Gerenciamento de API do Azure), esse custo de desenvolvimento adicional poderá não se aplicar.

- Se o Gateway de API for desenvolvido por uma única equipe, poderá haver um gargalo de desenvolvimento. Esse é outro motivo pelo qual uma abordagem melhor é ter vários Gateways de API refinados que respondam às diferentes necessidades do cliente. Você também pode separar o Gateway de API internamente em várias áreas ou camadas de propriedade de diferentes equipes trabalhando em microsserviços internos.

## <a name="additional-resources"></a>Recursos adicionais

- **Chris Richardson. Padrão: gateway de API/backend para front-end** \
  <https://microservices.io/patterns/apigateway.html>

- **Padrão de gateway de API** \
  <https://docs.microsoft.com/azure/architecture/microservices/gateway>

- **Padrão de agregação e composição** \
  <https://microservices.io/patterns/data/api-composition.html>

- **Gerenciamento de API do Azure** \
  <https://azure.microsoft.com/services/api-management/>

- **Udi Dahan. Composição orientada por serviço** \
  <https://udidahan.com/2014/07/30/service-oriented-composition-with-video/>

- **Grandes Clemenss. Mensagens e microserviços em GOTO 2016 (vídeo)** \
  <https://www.youtube.com/watch?v=rXi5CLjIQ9k>

- **Gateway de API em resumo** (ASP.NET Core série de tutoriais de gateway de API) \
  <https://www.pogsdotnet.com/2018/08/api-gateway-in-nutshell.html>

>[!div class="step-by-step"]
>[Anterior](identify-microservice-domain-model-boundaries.md) 
> [Avançar](communication-in-microservice-architecture.md)
