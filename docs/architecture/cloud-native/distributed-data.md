---
title: Dados distribuídos
description: Contraste o armazenamento de dados em aplicativos monolíticos e nativos de nuvem.
author: robvet
ms.date: 05/13/2020
ms.openlocfilehash: 28513f8691c06cf58ed14d57bf7830bb35d94852
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84144390"
---
# <a name="distributed-data"></a>Dados distribuídos

Como vimos em todo este livro, uma abordagem nativa de nuvem altera a maneira como você projeta, implanta e gerencia aplicativos. Ele também altera a maneira como você gerencia e armazena dados.

A Figura 5-1 contrasta as diferenças.

![Armazenamento de dados em aplicativos nativos de nuvem](./media/distributed-data.png)

**Figura 5-1**. Gerenciamento de dados em aplicativos nativos de nuvem

Os desenvolvedores experientes reconhecerão facilmente a arquitetura no lado esquerdo da Figura 5-1. Nesse *aplicativo monolítico*, os componentes do serviço de negócios se posicionam juntos em uma camada de serviços compartilhados, compartilhando dados de um único banco de dado relacional.

De várias maneiras, um banco de dados individual mantém o gerenciamento de dados simples. Consultar dados em várias tabelas é simples. Alterações na atualização de dados juntas ou todas elas são revertidas. [As transações ACID](https://docs.microsoft.com/windows/desktop/cossdk/acid-properties) garantem uma consistência forte e imediata.

Design para a nuvem nativa, adotamos uma abordagem diferente. No lado direito da Figura 5-1, observe como a funcionalidade de negócios é segregada em microserviços pequenos e independentes. Cada microserviço encapsula um recurso comercial específico e seus próprios dados. O banco de dados monolítico é decomposto em um modelo de dado distribuído com muitos bancos de dados menores, cada um alinhando com um microserviço. Quando a fumaça é limpa, surgemos um design que expõe um *banco de dados por microserviço*.

## <a name="database-per-microservice-why"></a>Banco de dados por microserviço, por quê?

Esse banco de dados por microserviço fornece muitos benefícios, especialmente para sistemas que devem evoluir rapidamente e dar suporte a escala maciça. Com este modelo...

- Os dados de domínio são encapsulados dentro do serviço
- O esquema de dados pode evoluir sem afetar diretamente outros serviços
- Cada repositório de dados pode ser dimensionado de forma independente
- Uma falha no armazenamento de dados em um serviço não afetará diretamente outros serviços

Separar dados também permite que cada microserviço implemente o tipo de armazenamento de dados que é melhor otimizado para sua carga de trabalho, necessidades de armazenamento e padrões de leitura/gravação. As opções incluem relacional, documento, chave-valor e até mesmo armazenamentos de dados baseados em grafo.

A Figura 5-2 apresenta o princípio da persistência de poliglota em um sistema nativo de nuvem.

![Persistência de dados poliglota](./media/polyglot-data-persistence.png)

**Figura 5-2**. Persistência de dados poliglota

Observe na figura anterior como cada microserviço dá suporte a um tipo diferente de armazenamento de dados.

- O microserviço de catálogo de produtos consome um banco de dados relacional para acomodar a estrutura relacional avançada de seus próprios dados subjacentes.
- O microserviço de carrinho de compras consome um cache distribuído que dá suporte a seu armazenamento de dados de valor-chave simples.
- O microserviço de ordenação consome um banco de dados de documentos NoSql para operações de gravação, juntamente com um repositório de chave/valor altamente desnormalizado para acomodar altos volumes de operações de leitura.
  
Embora os bancos de dados relacionais permaneçam relevantes para os microserviços com os complexos, eles ganharam uma popularidade considerável. Eles fornecem grande escala e alta disponibilidade. Sua natureza sem esquema permite que os desenvolvedores se afastem de uma arquitetura de classes de dados tipadas e ORMs que tornam a alteração cara e demorada. Abordamos os bancos de dados NoSQL posteriormente neste capítulo.

 Embora o encapsulamento de dados em microserviços separados possa aumentar a agilidade, o desempenho e a escalabilidade, ele também apresenta muitos desafios. Na próxima seção, discutiremos esses desafios junto com padrões e práticas para ajudar a superá-los.  

## <a name="cross-service-queries"></a>Consultas entre serviços

Embora os microserviço sejam independentes e se concentrem em recursos funcionais específicos, como inventário, envio ou ordenação, eles frequentemente exigem integração com outros microserviços. Geralmente, a integração envolve um microserviço *consultando* outro para dados. A Figura 5-3 mostra o cenário.

![Consultando em microserviços](./media/cross-service-query.png)

**Figura 5-3**. Consultando em microserviços

Na figura anterior, vemos um microserviço de cesta de compras que adiciona um item à cesta de compras de um usuário. Embora o armazenamento de dados para este microserviço contenha dados de item de linha e cesta, ele não mantém os dados de produtos ou preços. Em vez disso, esses itens de dados pertencem aos microserviços de catálogo e de preços. Isso apresenta um problema. Como o microserviço da cesta de compras pode adicionar um produto à cesta de compras do usuário quando ele não tem dados de produto nem de preço em seu banco de dado?

Uma opção discutida no capítulo 4 é uma [chamada http direta](service-to-service-communication.md#queries) da cesta de compras para os microserviços de catálogo e preço. No entanto, no capítulo 4, dissemos que *chamadas http síncronas agrupam* os microserviços, reduzindo sua autonomia e diminuindo seus benefícios arquitetônicos.

Também poderíamos implementar um padrão de solicitação-resposta com filas de entrada e saída separadas para cada serviço. No entanto, esse padrão é complicado e requer a inserção para correlacionar mensagens de solicitação e resposta.
Embora ele desassocie as chamadas de microserviço de back-end, o serviço de chamada ainda deve esperar de forma síncrona a chamada seja concluída. Congestionamento de rede, falhas transitórias ou um microserviço sobrecarregado e pode resultar em operações de execução longa e até mesmo com falha.

Em vez disso, um padrão amplamente aceito para remover dependências entre serviços é o [padrão de exibição materializado](https://docs.microsoft.com/azure/architecture/patterns/materialized-view), mostrado na Figura 5-4.

![Padrão de exibição materializada](./media/materialized-view-pattern.png)

**Figura 5-4**. Padrão de exibição materializada

Com esse padrão, você coloca uma tabela de dados local (conhecida como *modelo de leitura*) no serviço de cesta de compras. Esta tabela contém uma cópia desnormalizada dos dados necessários dos microserviços de produto e preço. Copiar os dados diretamente no microserviço da cesta de compras elimina a necessidade de chamadas caras entre serviços. Com os dados locais para o serviço, você melhora o tempo de resposta e a confiabilidade do serviço. Além disso, ter sua própria cópia dos dados torna o serviço de cesta de compras mais resiliente. Se o serviço de catálogo ficar indisponível, ele não afetaria diretamente o serviço de cesta de compras. A cesta de compras pode continuar operando com os dados de seu próprio armazenamento.

O problema dessa abordagem é que agora você tem dados duplicados em seu sistema. No entanto, a duplicação *estratégica* de dados em sistemas nativos de nuvem é uma prática estabelecida e não é considerada um antipadrão ou uma prática inadequada. Tenha em mente que *um e apenas um serviço* podem possuir um conjunto de dados e ter autoridade sobre ele. Você precisará sincronizar os modelos de leitura quando o sistema de registro for atualizado. Normalmente, a sincronização é implementada por meio de mensagens assíncronas com um [padrão de publicação/assinatura](service-to-service-communication.md#events), como mostra a Figura 5,4.

## <a name="distributed-transactions"></a>Transações distribuídas

Embora seja difícil consultar dados em microserviços, a implementação de uma transação em vários microserviços é ainda mais complexa. O desafio inerente de manter a consistência de dados entre fontes de dados independentes em diferentes microservices não pode ser subestado. A falta de transações distribuídas em aplicativos nativos de nuvem significa que você deve gerenciar transações distribuídas programaticamente. Você passa de um mundo de *consistência imediata* para a *consistência eventual*.

A Figura 5-5 mostra o problema.

![Transação no padrão saga](./media/saga-transaction-operation.png)

**Figura 5-5**. Implementando uma transação em microserviços

Na figura anterior, cinco microserviços independentes participam de uma transação distribuída que cria um pedido. Cada microserviço mantém seu próprio armazenamento de dados e implementa uma transação local para seu armazenamento. Para criar o pedido, a transação local para *cada* Microservice individual deve ter sucesso ou *todos* devem abortar e reverter a operação. Embora o suporte interno transacional esteja disponível dentro de cada um dos microserviços, não há suporte para uma transação distribuída que se englobe em todos os cinco serviços para manter os dados consistentes.

Em vez disso, você deve construir essa transação distribuída *programaticamente*.

Um padrão popular para adicionar suporte transacional distribuído é o padrão saga. Ele é implementado agrupando transações locais em conjunto de forma programática e invocando sequencialmente cada uma. Se qualquer uma das transações locais falhar, o saga anulará a operação e invocará um conjunto de [Transações de compensação](https://docs.microsoft.com/azure/architecture/patterns/compensating-transaction). As transações de compensação desfazem as alterações feitas pelas transações locais anteriores e restauram a consistência de dados. A Figura 5-6 mostra uma transação com falha com o padrão saga.

![Reverter no padrão saga](./media/saga-rollback-operation.png)

**Figura 5-6**. Revertendo uma transação

Na figura anterior, a operação *Atualizar inventário* falhou no microserviço de inventário. O saga invoca um conjunto de transações de compensação (em vermelho) para ajustar as contagens de inventário, cancelar o pagamento e a ordem e retornar os dados para cada microserviço de volta para um estado consistente.

Os padrões de Saga geralmente são coreografado como uma série de eventos relacionados ou orquestrados como um conjunto de comandos relacionados. No capítulo 4, discutimos o padrão agregador de serviço que seria a base para uma implementação de Saga orquestrada. Também discutimos eventos em conjunto com os tópicos do barramento de serviço do Azure e da grade de eventos do Azure que seriam uma base para uma implementação coreografado saga.

## <a name="high-volume-data"></a>Dados de alto volume

Grandes aplicativos nativos de nuvem geralmente dão suporte a requisitos de dados de alto volume. Nesses cenários, as técnicas tradicionais de armazenamento de dados podem causar gargalos. Para sistemas complexos que implantam em grande escala, o Separação das Operações de Comando e de Consulta (CQRS) e o fornecimento de eventos podem melhorar o desempenho do aplicativo.  

### <a name="cqrs"></a>CQRS

O [CQRS](https://docs.microsoft.com/azure/architecture/patterns/cqrs), é um padrão de arquitetura que pode ajudar a maximizar o desempenho, a escalabilidade e a segurança. O padrão separa as operações que lêem dados dessas operações que gravam dados.

Para cenários normais, o mesmo modelo de entidade e objeto de repositório de dados são *usados para operações* de leitura e gravação.

No entanto, um cenário de dados de alto volume pode se beneficiar de modelos e tabelas de dados separados para leituras e gravações. Para melhorar o desempenho, a operação de leitura pode consultar uma representação altamente desnormalizada dos dados para evitar junções de tabelas e bloqueios de tabelas repetitivas e decaradas. A operação de *gravação* , conhecida como um *comando*, seria atualizada em uma representação totalmente normalizada dos dados que garantiria a consistência. Em seguida, você precisa implementar um mecanismo para manter ambas as representações em sincronia. Normalmente, sempre que a tabela de gravação é modificada, ela publica um evento que Replica a modificação para a tabela de leitura.

A Figura 5-7 mostra uma implementação do padrão CQRS.

![Separação das Operações de Comando e de Consulta](./media/cqrs-implementation.png)

**Figura 5-7**. Implementação de CQRS

Na figura anterior, são implementados os modelos de comando e consulta separados. Cada operação de gravação de dados é salva no repositório de gravação e, em seguida, propagada para o repositório de leitura. Preste muita atenção à forma como o processo de propagação de dados opera no princípio de [consistência eventual](https://www.cloudcomputingpatterns.org/eventual_consistency/). O modelo de leitura eventualmente sincroniza com o modelo de gravação, mas pode haver algum atraso no processo. Discutiremos a consistência eventual na próxima seção.

Essa separação permite que leituras e gravações sejam dimensionadas de forma independente. As operações de leitura usam um esquema otimizado para consultas, enquanto as gravações usam um esquema otimizado para atualizações. As consultas de leitura vão contra dados desnormalizados, enquanto a lógica de negócios complexa pode ser aplicada ao modelo de gravação. Além de isso, você pode impor uma segurança mais rígida em operações de gravação do que aquelas que expõem leituras.

A implementação do CQRS pode melhorar o desempenho do aplicativo para serviços nativos de nuvem. No entanto, isso resulta em um design mais complexo. Aplique esse princípio com cuidado e estrategicamente a essas seções de seu aplicativo nativo de nuvem que se beneficiarão dela. Para saber mais sobre o CQRS, confira os [microserviços do Microsoft book .net: arquitetura para aplicativos .net em contêineres](https://docs.microsoft.com/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/apply-simplified-microservice-cqrs-ddd-patterns).

### <a name="event-sourcing"></a>Fornecimento de eventos

Outra abordagem para otimizar cenários de dados de alto volume envolve o [fornecimento de eventos](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing).

Normalmente, um sistema armazena o estado atual de uma entidade de dados. Se um usuário alterar seu número de telefone, por exemplo, o registro do cliente será atualizado com o novo número. Sempre sabemos o estado atual de uma entidade de dados, mas cada atualização substitui o estado anterior.

Na maioria dos casos, esse modelo funciona bem. No entanto, em sistemas de alto volume, a sobrecarga do bloqueio transacional e das operações de atualização frequentes pode afetar o desempenho, a capacidade de resposta e a escalabilidade do banco de dados.

O fornecimento de eventos usa uma abordagem diferente para capturar dados. Cada operação que afeta os dados é persistida em um repositório de eventos. Em vez de atualizar o estado de um registro de dados, acrescentamos cada alteração a uma lista sequencial de eventos passados, semelhante ao razão de um contador. O repositório de eventos torna-se o sistema de registro dos dados. Ele é usado para propagar várias exibições materializadas dentro do contexto limitado de um microserviço. A Figura 5,8 mostra o padrão.

![Fornecimento do evento](./media/event-sourcing.png)

**Figura 5-8**. Fornecimento do evento

Na figura anterior, observe como cada entrada (em azul) para o carrinho de compras de um usuário é anexada a um repositório de eventos subjacente. Na exibição materializada adjacente, o sistema projeta o estado atual reproduzindo todos os eventos associados a cada carrinho de compras. Esse modo de exibição ou leitura de modelo é então exposto de volta para a interface do usuário. Os eventos também podem ser integrados a sistemas e aplicativos externos ou consultados para determinar o estado atual de uma entidade. Com essa abordagem, você mantém o histórico. Você sabe não apenas o estado atual de uma entidade, mas também como chegou a esse estado.

Falando mecânico, o fornecimento de eventos simplifica o modelo de gravação. Não há atualizações ou exclusões. Acrescentar cada entrada de dados como um evento imutável minimiza a contenção, o bloqueio e os conflitos de simultaneidade associados a bancos de dados relacionais. A criação de modelos de leitura com o padrão de exibição materializada permite desacoplar o modo de exibição do modelo de gravação e escolher o melhor armazenamento de dados para otimizar as necessidades da interface do usuário do aplicativo.

Para esse padrão, considere um armazenamento de dados que dá suporte diretamente a fornecimento de eventos. Azure Cosmos DB, MongoDB, Cassandra, CouchDB e RavenDB são bons candidatos.

Assim como acontece com todos os padrões e tecnologias, implemente estrategicamente e quando necessário. Embora o fornecimento de eventos possa fornecer maior desempenho e escalabilidade, ele traz a despesa de complexidade e de uma curva de aprendizado.

>[!div class="step-by-step"]
>[Anterior](service-mesh-communication-infrastructure.md) 
> [Avançar](relational-vs-nosql-data.md)
