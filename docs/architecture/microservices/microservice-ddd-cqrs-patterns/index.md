---
title: Lidando com a complexidade de negócios em um microsserviço com padrões DDD e CQRS
description: Arquitetura de microsserviços do .NET para aplicativos .NET em contêineres | Entenda como lidar com cenários de negócios complexos aplicando padrões DDD e CQRS
ms.date: 10/08/2018
ms.openlocfilehash: 852073548a66fbe568fc5c2531342db944d5a8b0
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84144637"
---
# <a name="tackle-business-complexity-in-a-microservice-with-ddd-and-cqrs-patterns"></a>Lidar com a complexidade dos negócios em um microsserviço com padrões DDD e CQRS

*Crie um modelo de domínio para cada microsserviço ou contexto limitado que reflita o entendimento do domínio da empresa.*

Esta seção concentra-se em microsserviços mais avançados que você implementa quando precisa lidar com subsistemas complexos ou com microsserviços derivados do conhecimento de especialistas no domínio com mudanças constantes nas regras de negócio. Os padrões de arquitetura usados nesta seção são baseados nas abordagens DDD (design controlado por domínio) e CQRS (Segregação de Responsabilidade de Comando e Consulta), conforme é ilustrado na Figura 7-1.

:::image type="complex" source="./media/index/internal-versus-external-architecture.png" alt-text="Diagrama comparando padrões de arquitetura externa e interna.":::
Diferença entre arquitetura externa: padrões de microsserviço, gateways de API, comunicação resilientes, pub/sub etc. e a arquitetura interna: controlado por dados/CRUD, padrões de DDD, injeção de dependência, várias bibliotecas etc.
:::image-end:::

**Figura 7-1**. Arquitetura externa de microsserviço versus padrões de arquitetura interna para cada microsserviço

No entanto, a maioria das técnicas de microsserviços controlados por dados, incluindo como implementar um serviço de API Web do ASP.NET Core ou como expor metadados com Swashbuckle ou NSwag, também se aplica aos microsserviços mais avançados implementados internamente com padrões DDD. Esta seção é uma extensão das seções anteriores, pois a maioria das práticas de explicadas anteriormente também se aplicam aqui ou para qualquer tipo de microsserviço.

Esta seção primeiro fornece detalhes sobre os padrões CQRS simplificados usados no aplicativo de referência eShopOnContainers. Posteriormente, você obterá uma visão geral das técnicas de DDD que permitem encontrar padrões comuns que podem ser reutilizados em seus aplicativos.

O DDD é um tópico grande com um conjunto avançado de recursos de aprendizagem. Você pode iniciar com guias como [Domain-Driven Design](https://domainlanguage.com/ddd/) (Design controlado por domínio) do Eric Evans e os materiais adicionais dos autores Vernon, Jimmy Nilsson, Greg Young, Udi Dahan, Jimmy Bogard e muitos outros especialistas em DDD/CQRS. Mas, sobretudo, você precisa tentar aprender como aplicar as técnicas de DDD das sessões de conversa, de quadro de comunicações e de modelagem com os especialistas em seu domínio de negócio concreto.

#### <a name="additional-resources"></a>Recursos adicionais

##### <a name="ddd-domain-driven-design"></a>DDD (design controlado por domínio)

- **Eric Evans. Idioma do domínio** \
  <https://domainlanguage.com/>

- **Martin Fowler. Design controlado por domínio** \
  <https://martinfowler.com/tags/domain%20driven%20design.html>

- **Jimmy Bogard. Fortalecendo seu domínio: um primer** \
  <https://lostechies.com/jimmybogard/2010/02/04/strengthening-your-domain-a-primer/>

##### <a name="ddd-books"></a>Guias sobre DDD

- **Eric Evans. Design controlado por domínio: solução de complexidade no coração do software** \
  <https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/>

- **Eric Evans. Referência de design controlada por domínio: definições e resumos de padrões** \
  <https://www.amazon.com/Domain-Driven-Design-Reference-Definitions-2014-09-22/dp/B01N8YB4ZO/>

- **Vaughn Vernon. Implementando o design controlado por domínio** \
  <https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577/>

- **Vaughn Vernon. Design controlado por domínio difixado** \
  <https://www.amazon.com/Domain-Driven-Design-Distilled-Vaughn-Vernon/dp/0134434420/>

- **Jimmy Nilsson. Aplicando padrões e design orientados a domínio** \
  <https://www.amazon.com/Applying-Domain-Driven-Design-Patterns-Examples/dp/0321268202/>

- **Cesar de la Torre. Guia de arquitetura orientada a domínio N camadas com o .NET** \
  <https://www.amazon.com/N-Layered-Domain-Oriented-Architecture-Guide-NET/dp/8493903612/>

- **Abel Avram e algoritmo Floyd Marinescu. Design controlado por domínio rapidamente** \
  <https://www.amazon.com/Domain-Driven-Design-Quickly-Abel-Avram/dp/1411609255/>

- **Scott Millett, ajuste de Nick-padrões, princípios e práticas de design controlado por domínio** \
  <https://www.wiley.com/Patterns%2C+Principles%2C+and+Practices+of+Domain+Driven+Design-p-9781118714706>

##### <a name="ddd-training"></a>Treinamento em DDD

- **Julie Lerman e Steve Smith. Conceitos básicos de design controlado por domínio** \
  <https://bit.ly/PS-DDD>

>[!div class="step-by-step"]
>[Anterior](../multi-container-microservice-net-applications/implement-api-gateways-with-ocelot.md) 
> [Avançar](apply-simplified-microservice-cqrs-ddd-patterns.md)
