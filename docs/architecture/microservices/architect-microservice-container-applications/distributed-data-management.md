---
title: Desafios e soluções do gerenciamento de dados distribuídos
description: Saiba quais são os desafios e soluções de gerenciamento de dados distribuídos no mundo de microsserviços.
ms.date: 09/20/2018
ms.openlocfilehash: 8b91879e879db293ed61bd5f3c49dc391b9d8f5a
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84144312"
---
# <a name="challenges-and-solutions-for-distributed-data-management"></a>Desafios e soluções do gerenciamento de dados distribuídos

## <a name="challenge-1-how-to-define-the-boundaries-of-each-microservice"></a>Desafio \#1: Como definir os limites de cada microsserviço

Definir os limites dos microsserviços provavelmente é o primeiro desafio que qualquer pessoa encontra. Cada microsserviço deve ser uma parte do aplicativo e cada microsserviço deve ser autônomo com todos os benefícios e desafios que ele abrange. Mas como identificar esses limites?

Primeiro, você precisa se concentrar nos modelos de domínio lógicos do aplicativo e nos dados relacionados. Tente identificar ilhas de dados desacopladas e contextos diferentes dentro do mesmo aplicativo. Cada contexto pode ter uma linguagem de negócios diferente (termos de negócios diferentes). Os contextos devem ser definidos e gerenciados de forma independente. Os termos e as entidades usados nesses contextos diferentes podem parecer semelhantes, mas você descobrirá que, em um contexto específico, um conceito empresarial é usado para uma finalidade e em outro contexto para outra finalidade, podendo até mesmo ter um nome diferente. Por exemplo, um usuário pode ser mencionado como um usuário no contexto de identidade ou de associação, como um cliente em um contexto de CRM, como um comprador em um contexto de pedidos e assim por diante.

Sua forma de identificar os limites entre vários contextos de aplicativo com um domínio diferente para cada contexto é exatamente a forma que você pode usar para identificar os limites de cada microsserviço de negócios, além de seu modelo de domínio e seus dados relacionados. Você sempre deve tentar minimizar o acoplamento entre esses microsserviços. Mais adiante, este guia apresentará mais detalhes sobre essa identificação e esse design de modelo de domínio na seção [Identificando limites de modelo de domínio para cada microsserviço](identify-microservice-domain-model-boundaries.md).

## <a name="challenge-2-how-to-create-queries-that-retrieve-data-from-several-microservices"></a>Desafio \#2: Como criar consultas que recuperam dados de vários microsserviços

Um segundo desafio é como implementar consultas que recuperam dados de vários microsserviços, evitando a comunicação intensa dos aplicativos clientes remotos com os microsserviços. Um exemplo pode ser uma única tela de um aplicativo móvel que precisa mostrar microsserviços de informações do usuário que pertencem ao carrinho de compras, de catálogo e de identidade do usuário. Outro exemplo seria um relatório complexo envolvendo diversas tabelas localizadas em vários microsserviços. A solução certa depende da complexidade das consultas. Mas em qualquer caso, será necessária uma maneira de agregar informações se você desejar melhorar a eficiência nas comunicações do sistema. As soluções mais comuns são as seguintes.

**Gateway de API.** Para a agregação de dados simples de vários microsserviços que têm bancos de dados diferentes, a abordagem recomendada é um microsserviço de agregação, conhecido como Gateway de API. No entanto, você precisa ter cuidado ao implementar esse padrão, porque ele pode ser um ponto de redução no sistema e pode violar o princípio de autonomia dos microsserviços. Para atenuar essas possibilidades, você pode ter vários Gateways de API refinados, cada um concentrado em uma "fatia" vertical ou em uma área de negócios do sistema. O padrão do Gateway de API será explicado mais detalhadamente na [seção Gateway de API](direct-client-to-microservice-communication-versus-the-api-gateway-pattern.md#why-consider-api-gateways-instead-of-direct-client-to-microservice-communication) mais adiante.

**CQRS com tabelas de consulta/leituras.** Outra solução para agregar dados de vários microsserviços é o [Padrão de exibição materializada](https://docs.microsoft.com/azure/architecture/patterns/materialized-view). Nessa abordagem, você gera com antecedência (prepara os dados desnormalizados antes que as consultas reais ocorram) uma tabela somente leitura com os dados que pertencem a vários microsserviços. A tabela tem um formato adequado às necessidades do aplicativo cliente.

Considere algo como tela de um aplicativo móvel. Se houver um banco de dados individual, você poderá reunir os dados para essa tela usando uma consulta SQL que execute uma junção complexa envolvendo várias tabelas. No entanto, quando houver vários bancos de dados, e cada banco de dados pertencer a um microsserviço diferente, você não poderá consultar esses bancos de dados e criar uma junção SQL. Sua consulta complexa se tornará um desafio. Você pode atender ao requisito usando uma abordagem de CQRS, ou seja, criar uma tabela desnormalizada em outro banco de dados que é usado apenas para consultas. A tabela pode ser desenvolvida especificamente para os dados necessários para essa consulta complexa, com uma relação um-para-um entre os campos necessários para a tela do aplicativo e as colunas na tabela de consulta. Ela também pode funcionar para a geração de relatórios.

Além de resolver o problema original (como fazer a consulta e a junção entre os microsserviços), essa abordagem também melhora o desempenho consideravelmente em comparação com uma junção complexa, porque os dados que o aplicativo precisa já estão na tabela de consulta. É claro que, o uso de CQRS (segregação de responsabilidade de comando e consulta) com tabelas de consulta/leituras significa um trabalho de desenvolvimento adicional e a necessidade de abranger uma coerência eventual. No entanto, o CQRS com vários bancos de dados deve ser aplicado para atender requisitos de desempenho e de alta escalabilidade em [cenários de colaboração](http://udidahan.com/2011/10/02/why-you-should-be-using-cqrs-almost-everywhere/) (ou cenários de concorrência, dependendo do ponto de vista).

**"Dados frios" em bancos de dados centrais.** Para relatórios e consultas complexos que talvez não exijam dados em tempo real, uma abordagem comum é exportar os "dados quentes" (dados transacionais dos microsserviços) como "dados frios" para bancos de dados grandes usados somente para relatórios. Esse sistema de banco de dados central pode ser um sistema baseado em Big Data, como o Hadoop, um data warehouse como um baseado no SQL Data Warehouse do Azure ou até mesmo um Banco de Dados SQL individual usado apenas para relatórios (se o tamanho não for problema).

Tenha em mente que esse banco de dados centralizado deve ser usado somente para consultas e relatórios que não precisam de dados em tempo real. As atualizações e transações originais, ou seja, a fonte confiável, precisam estar nos dados dos microsserviços. A maneira de sincronizar os dados seria usar a comunicação controlada por evento (abordada nas próximas seções) ou usar outras ferramentas de importação/exportação da infraestrutura do banco de dados. Se você usar a comunicação controlada por evento, esse processo de integração será semelhante à maneira de propagar dados já descrita para tabelas de consulta de CQRS.

No entanto, se o design do aplicativo envolver a agregação constante de informações de vários microsserviços para consultas complexas, esse poderá ser um sintoma de design ruim, pois um microsserviço deve estar o mais isolado possível dos outros microsserviços. (Isso exclui relatórios/análises que sempre devem usar bancos de dados do Cold Data Central.) Ter esse problema muitas vezes pode ser um motivo para mesclar os microserviços. Você precisa equilibrar a autonomia de evolução e a implantação de cada microsserviço com dependências fortes, coesão e agregação de dados.

## <a name="challenge-3-how-to-achieve-consistency-across-multiple-microservices"></a>Desafio \#3: Como obter consistência entre vários microsserviços

Como mencionado anteriormente, os dados pertencentes a cada microsserviço são privados desse microsserviço e só podem ser acessados usando a API desse microsserviço. Portanto, um desafio apresentado é como implementar processos de negócios de ponta a ponta, mantendo a consistência entre vários microsserviços.

Para analisar o problema, vamos examinar um exemplo do [Aplicativo de referência eShopOnContainers](https://aka.ms/eshoponcontainers). O microsserviço de catálogo mantém informações sobre todos os produtos, incluindo o preço do produto. O microsserviço Carrinho de compras gerencia dados temporais sobre itens de produto que os usuários estão adicionando aos seus carrinhos de compras, o que inclui o preço dos itens no momento em que eles foram adicionados ao carrinho de compras. Quando o preço de um produto é atualizado no catálogo, esse preço também deve ser atualizado nos carrinhos de compras ativos que contêm o mesmo produto e, além disso, o sistema provavelmente deve avisar o usuário, informando que o preço de um determinado item foi alterado desde que o usuário o adicionou ao seu carrinho de compras.

Em uma versão monolítica hipotética deste aplicativo, quando o preço é alterado na tabela de produtos, o subsistema de catálogo pode simplesmente usar uma transação ACID para atualizar o preço atual na tabela de Carrinho de compras.

No entanto, em um aplicativo baseado em microsserviços, as tabelas de Produto e Carrinho de compras pertencem aos seus respectivos microsserviços. Um microsserviço nunca deve incluir tabelas/armazenamento pertencentes a outro microsserviço em suas próprias transações, nem mesmo em consultas diretas, conforme é mostrado na Figura 4-9.

![Diagrama mostrando que não é possível compartilhar os dados do banco de dado dos microservices.](./media/distributed-data-management/indepentent-microservice-databases.png)

**Figura 4-9**. Um microsserviço não pode acessar diretamente uma tabela em outro microsserviço

O microsserviço de Catálogo não deve atualizar a tabela de Carrinho de compras diretamente, porque a tabela de Carrinho de compras é de propriedade do microsserviço de Carrinho de compras. Para fazer uma atualização no microsserviço de Carrinho de compras, o microsserviço de Catálogo deve usar a consistência eventual provavelmente com base comunicação assíncrona, assim como em eventos de integração (comunicação baseada em mensagem e em evento). É assim que o aplicativo de referência [eShopOnContainers](https://aka.ms/eshoponcontainers) executa esse tipo de consistência entre microsserviços.

Conforme indicado pelo [Teorema CAP](https://en.wikipedia.org/wiki/CAP_theorem), é necessário escolher entre a disponibilidade e a coerência forte de ACID. A maioria dos cenários baseados em microsserviço exigem disponibilidade e alta escalabilidade em vez de coerência forte. Os aplicativos críticos precisam permanecer em funcionamento e os desenvolvedores podem contornar a coerência forte usando técnicas para trabalhar com consistência eventual ou fraca. Essa é a abordagem usada pela maioria das arquiteturas baseadas em microsserviço.

Além de o estilo ACID ou as transações de confirmação de duas fases serem contra os princípios dos microsserviços, a maioria dos bancos de dados NoSQL (como o Azure Cosmos DB, o MongoDB, etc.) não dão suporte às transações de confirmação de duas fases, típicas em cenários de bancos de dados distribuídos. No entanto, manter a consistência dos dados entre os serviços e os bancos de dados é fundamental. Esse desafio também está relacionado à questão de como propagar alterações entre vários microsserviços quando determinados dados precisam ser redundante. Por exemplo, quando o nome ou a descrição do produto precisa estar no microsserviço de catálogo e também no microsserviço de Carrinho de compras.

Uma boa solução para esse problema é usar a consistência eventual entre os microsserviços, articulada pela comunicação controlada por evento e por um sistema de publicação e assinatura. Esses tópicos serão abordados na seção [Comunicação controlada por evento assíncrono](asynchronous-message-based-communication.md#asynchronous-event-driven-communication) mais adiante neste guia.

## <a name="challenge-4-how-to-design-communication-across-microservice-boundaries"></a>Desafio \#4: Como projetar a comunicação entre os limites dos microsserviços

A comunicação entre os limites dos microsserviços é realmente um grande desafio. Nesse contexto, a comunicação não se refere a qual protocolo você deve usar (HTTP e REST, AMQP, mensagens e assim por diante). Nesse caso, ela aborda qual estilo de comunicação você deve usar e, principalmente, que nível de acoplamento os microsserviços devem ter. Dependendo do nível de acoplamento, quando ocorrer uma falha, o impacto dessa falha no sistema poderá variar significativamente.

Em um sistema distribuído, como um aplicativo baseado em microsserviços, com um número muito grande de artefatos em movimentação, com serviços distribuídos em vários servidores ou hosts, eventualmente, os componentes falharão. Falhas parciais e interrupções ainda maiores certamente ocorrerão, portanto, você precisa projetar seus microsserviços e a comunicação entre eles levando em conta os riscos comuns nesse tipo de sistema distribuído.

Uma abordagem comum é implementar microsserviços baseados em HTTP (REST), devido à simplicidade. Uma abordagem baseada em HTTP é perfeitamente aceitável e, nesse caso, o problema está relacionado à maneira de usá-la. Se você usar solicitações e respostas HTTP apenas para que os aplicativos cliente ou os gateways de API interajam com os microsserviços, tudo bem. Mas, se você criar longas cadeias de chamadas HTTP síncronas entre os microsserviços, comunicando-se entre seus limites como se os microsserviços fossem objetos em um aplicativo monolítico, seu aplicativo acabará tendo problemas.

Por exemplo, imagine que o aplicativo cliente faça uma chamada HTTP à API para um microsserviço individual, como o microsserviço de pedidos. Se o microsserviço de pedidos, por sua vez, chamar microsserviços adicionais usando HTTP no mesmo ciclo de solicitação/resposta, você estará criando uma cadeia de chamadas HTTP. Parece razoável em um primeiro momento. No entanto, existem pontos importantes a serem considerados ao adotar esse caminho:

- Bloqueio e baixo desempenho. Devido à natureza síncrona de HTTP, a solicitação original não obtém uma resposta até que todas as chamadas HTTP internas sejam concluídas. Imagine se o número dessas chamadas aumentar significativamente e ao mesmo tempo uma das chamadas HTTP intermediárias para um microsserviço for bloqueada. O resultado é que o desempenho será afetado e a escalabilidade geral será extremamente afetada com o aumento de solicitações HTTP adicionais.

- Acoplando microsserviços com HTTP. Os microsserviços empresariais não devem ser acoplados com outros microsserviços empresariais. O ideal é que eles não "saibam" da existência de outros microsserviços. Se seu aplicativo depender do acoplamento de microsserviços como no exemplo, será praticamente impossível alcançar a autonomia de cada microsserviço.

- Falha em um dos microsserviços. Se você implementar uma cadeia de microsserviços vinculados por chamadas HTTP, quando um dos microsserviços falhar (e, com certeza, isso vai ocorrer) toda a cadeia de microsserviços falhará. Um sistema baseado em microsserviço deve ser criado para continuar a trabalhar da melhor maneira possível durante falhas parciais. Mesmo se você implementar uma lógica do cliente que use novas tentativas com retirada exponencial ou com mecanismos de disjuntor, quanto mais complexas as cadeias de chamadas HTTP forem, mais complexa será a implementação de uma estratégia de falha baseada em HTTP.

Na verdade, se os microsserviços internos estiverem se comunicando por meio da criação de cadeias de solicitações HTTP, conforme descrito, será possível argumentar que você tem um aplicativo monolítico, mas baseado em HTTP entre processos e não em mecanismos de comunicação entre processos.

Portanto, para impor a autonomia do microsserviço e melhorar a resiliência, você deve minimizar o uso de cadeias de comunicação de solicitação/resposta entre os microsserviços. É recomendável usar somente a interação assíncrona para a comunicação entre os microsserviços, seja usando a comunicação assíncrona baseada em evento e em mensagem ou usando a sondagem de HTTP (assíncrona), independentemente do ciclo de solicitação/resposta HTTP original.

O uso da comunicação assíncrona será explicado em detalhes mais adiante neste guia nas seções [A integração assíncrona dos microsserviços impõe a autonomia do microsserviço](communication-in-microservice-architecture.md#asynchronous-microservice-integration-enforces-microservices-autonomy) e [Comunicação assíncrona baseada em mensagem](asynchronous-message-based-communication.md).

## <a name="additional-resources"></a>Recursos adicionais

- **Teorema CAP** \
  <https://en.wikipedia.org/wiki/CAP_theorem>

- **Consistência eventual** \
  <https://en.wikipedia.org/wiki/Eventual_consistency>

- **Primer de consistência de dados** \
  <https://docs.microsoft.com/previous-versions/msp-n-p/dn589800(v=pandp.10)>

- **Martin Fowler. CQRS (Separação das Operações de Comando e de Consulta)** \
  <https://martinfowler.com/bliki/CQRS.html>

- **Exibição materializada** \
  <https://docs.microsoft.com/azure/architecture/patterns/materialized-view>

- **Carlos linha. ACID versus BASE: a pH de mudança do processamento de transações de banco de dados** \
  <https://www.dataversity.net/acid-vs-base-the-shifting-ph-of-database-transaction-processing/>

- **Transação de compensação** \
  <https://docs.microsoft.com/azure/architecture/patterns/compensating-transaction>

- **Udi Dahan. Composição orientada por serviço** \
  <https://udidahan.com/2014/07/30/service-oriented-composition-with-video/>

>[!div class="step-by-step"]
>[Anterior](logical-versus-physical-architecture.md) 
> [Avançar](identify-microservice-domain-model-boundaries.md)
