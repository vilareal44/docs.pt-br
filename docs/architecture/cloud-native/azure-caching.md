---
title: Cache em um aplicativo nativo de nuvem
description: Saiba mais sobre estratégias de cache em um aplicativo nativo de nuvem.
author: robvet
ms.date: 05/17/2020
ms.openlocfilehash: a33f143499b5f9545493bc4bc757cc3d152f7aa9
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88557510"
---
# <a name="caching-in-a-cloud-native-app"></a>Caching em um aplicativo nativo de nuvem

Os benefícios do cache são bem compreendidos. A técnica funciona copiando temporariamente os dados acessados com frequência de um armazenamento de dados de back-end para o *armazenamento rápido* localizado mais próximo do aplicativo. O cache é geralmente implementado onde...

- Os dados permanecem relativamente estáticos.
- O acesso a dados é lento, especialmente em comparação com a velocidade do cache.
- Os dados estão sujeitos a altos níveis de contenção.

## <a name="why"></a>Por quê?

Conforme discutido nas [diretrizes de cache da Microsoft](https://docs.microsoft.com/azure/architecture/best-practices/caching), o Caching pode aumentar o desempenho, a escalabilidade e a disponibilidade de microserviços individuais e do sistema como um todo. Ele reduz a latência e a contenção de lidar com grandes volumes de solicitações simultâneas para um armazenamento de dados. À medida que o volume de dados e o número de usuários aumentam, quanto mais os benefícios do cache se tornam.

O Caching é mais eficaz quando um cliente lê repetidamente os dados que são imutáveis ou que são alterados com pouca frequência. Os exemplos incluem informações de referência, como informações sobre produtos e preços, ou recursos estáticos compartilhados que são caros de construir.

Enquanto os microserviços devem ser sem monitoração de estado, um cache distribuído pode dar suporte ao acesso simultâneo aos dados de estado da sessão quando absolutamente necessário.

Também considere o cache para evitar cálculos repetitivos. Se uma operação transformar dados ou executar um cálculo complicado, armazene em cache o resultado para solicitações subsequentes.

## <a name="caching-architecture"></a>Arquitetura de cache

Aplicativos nativos de nuvem normalmente implementam uma arquitetura de cache distribuído. O cache é hospedado como um [serviço de backup](./definition.md#backing-services)baseado em nuvem, separado dos microservices. A Figura 5-15 mostra a arquitetura.

![Caching em um aplicativo nativo de nuvem](media/caching-in-a-cloud-native-app.png)

**Figura 5-15**: Caching em um aplicativo nativo de nuvem

Na figura anterior, observe como o cache é independente e compartilhado pelos microservices. Nesse cenário, o cache é invocado pelo [Gateway de API](./front-end-communication.md). Conforme discutido no capítulo 4, o gateway serve como um front-end para todas as solicitações de entrada. O cache distribuído aumenta a capacidade de resposta do sistema retornando dados armazenados em cache sempre que possível. Além disso, separar o cache dos serviços permite que o cache seja escalado verticalmente ou horizontalmente de forma independente para atender às demandas de tráfego aumentadas.

A figura anterior apresenta um padrão de cache comum conhecido como [padrão de reserva de cache](https://docs.microsoft.com/azure/architecture/patterns/cache-aside). Para uma solicitação de entrada, primeiro você consulta o cache (etapa \# 1) para obter uma resposta. Se for encontrado, os dados serão retornados imediatamente. Se os dados não existirem no cache (conhecido como um [erro de cache](https://www.techopedia.com/definition/6308/cache-miss)), eles são recuperados de um banco de dados local em um serviço downstream (etapa \# 2). Em seguida, ele é gravado no cache para solicitações futuras (etapa \# 3) e retornado ao chamador. Deve-se ter cuidado para remover periodicamente os dados armazenados em cache para que o sistema permaneça em tempo hábil e consistente.

À medida que um cache compartilhado cresce, pode-se provar benéfico para particionar seus dados em vários nós. Isso pode ajudar a minimizar a contenção e melhorar a escalabilidade. Muitos serviços de cache oferecem suporte à capacidade de adicionar e remover nós dinamicamente e reequilibrar dados entre partições. Essa abordagem geralmente envolve clustering. O clustering expõe uma coleção de nós federados como um cache único e contínuo. Internamente, no entanto, os dados são dispersos entre os nós após uma estratégia de distribuição predefinida que equilibra a carga uniformemente.

## <a name="azure-cache-for-redis"></a>Cache Redis do Azure

O [cache do Azure para Redis](https://azure.microsoft.com/services/cache/) é um serviço de agente de mensagens e cache de dados seguro, totalmente gerenciado pela Microsoft. Consumido como uma oferta de PaaS (plataforma como serviço), ele fornece alta taxa de transferência e acesso de baixa latência aos dados. O serviço pode ser acessado por qualquer aplicativo dentro ou fora do Azure.

O cache do Azure para o serviço Redis gerencia o acesso a servidores Redis de software livre hospedados em data centers do Azure. O serviço atua como uma fachada que fornece gerenciamento, controle de acesso e segurança. O serviço nativamente dá suporte a um conjunto avançado de estruturas de dados, incluindo cadeias de caracteres, hashes, listas e conjuntos. Se seu aplicativo já usa Redis, ele funcionará no estado em que se encontra com o cache do Azure para Redis.

O cache do Azure para Redis é mais do que um servidor de cache simples. Ele pode dar suporte a vários cenários para aprimorar uma arquitetura de microserviços:

- Um armazenamento de dados na memória
- Um banco de dados não relacional distribuído
- Um agente de mensagem
- Um servidor de configuração ou descoberta
  
Para cenários avançados, uma cópia dos dados armazenados em cache pode ser [persistida no disco](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-persistence). Se um evento catastrófico desabilitar os caches primário e de réplica, o cache será reconstruído a partir do instantâneo mais recente.

O cache Redis do Azure está disponível em várias configurações predefinidas e tipos de preço. A [camada Premium](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-overview#service-tiers) apresenta muitos recursos de nível empresarial, como clustering, persistência de dados, replicação geográfica e isolamento de rede virtual.

>[!div class="step-by-step"]
>[Anterior](relational-vs-nosql-data.md) 
> [Avançar](elastic-search-in-azure.md)
