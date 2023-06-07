---
title: Crie serviços resilientes prontos para a nuvem. adote as falhas transitórias na nuvem
description: Modernizar aplicativos .NET existentes com contêineres de nuvem e Windows do Azure | Crie serviços resilientes prontos para a nuvem. adote as falhas transitórias na nuvem
ms.date: 04/30/2018
ms.openlocfilehash: 899084ac00d9be0df47ef88c026f4e8c19722bb6
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84144246"
---
# <a name="build-resilient-services-ready-for-the-cloud-embrace-transient-failures-in-the-cloud"></a>Compilar serviços resilientes prontos para a nuvem: adotar falhas transitórias na nuvem 

A resiliência é a capacidade de se recuperar de falhas e continuar funcionando. A resiliência não está prestes a evitar falhas, mas aceitando o fato de que as falhas ocorrerão e, em seguida, respondendo a elas de uma forma que evite tempo de inatividade ou perda de dados. A meta de resiliência é retornar o aplicativo para um estado totalmente funcional após uma falha.

Seu aplicativo está pronto para a nuvem quando, no mínimo, implementa um modelo de resiliência baseado em software, em vez de um modelo baseado em hardware. Seu aplicativo de nuvem deve adotar as falhas parciais que certamente ocorrerão. Projetar ou refatorar parcialmente seu aplicativo para obter resiliência com falhas parciais esperadas. Ele deve ser projetado para lidar com falhas parciais, como interrupções de rede transitórias e nós, ou VMs falhando na nuvem. Até mesmo os contêineres sendo movidos para um nó diferente dentro de um cluster Orchestrator podem causar falhas curtas intermitentes dentro do aplicativo.

## <a name="handling-partial-failure"></a>Tratando falha parcial

Em um aplicativo baseado em nuvem, há um risco de falha parcial em qualquer momento. Por exemplo, uma única instância de site ou um contêiner pode falhar ou pode estar indisponível ou sem resposta por um curto período de tempo. Ou, uma única VM ou servidor pode falhar.

Como clientes e serviços são processos separados, um serviço pode não ser capaz de responder oportunamente à solicitação de um cliente. O serviço pode estar sobrecarregado e responder lentamente a solicitações ou pode não estar acessível por um curto período devido a problemas de rede.

Por exemplo, considere um aplicativo .NET monolítico que acessa um banco de dados no banco de dados SQL do Azure. Se o banco de dados SQL do Azure ou qualquer outro serviço de terceiros não estiver respondendo por um breve tempo (um banco de dados SQL do Azure pode ser movido para um nó ou servidor diferente e não responder por alguns segundos), quando o usuário tentar realizar qualquer ação, o aplicativo poderá falhar e mostrar uma exceção ao mesmo tempo.

Um cenário semelhante pode ocorrer em um aplicativo que consome serviços HTTP. A rede ou o serviço em si pode não estar disponível na nuvem durante uma falha curta e transitória.

Um aplicativo resiliente como o mostrado na Figura 4-9 deve implementar técnicas como "repetições com retirada exponencial" para dar ao aplicativo uma oportunidade de lidar com falhas transitórias nos recursos. Você também deve usar "disjuntores de circuito" em seus aplicativos. Um disjuntor impede que um aplicativo tente acessar um recurso quando é realmente uma falha de longo prazo. Usando um disjuntor, o aplicativo evita provocativa uma negação de serviço a si mesmo.

![Diagrama de falhas parciais tratadas por repetições com retirada exponencial.](./media/retry-partial-failures.png)

**Figura 4-9.** Falhas parciais tratadas por repetições com retirada exponencial

Você pode usar essas técnicas tanto em recursos HTTP quanto em recursos de banco de dados. Na Figura 4-9, o aplicativo é baseado em uma arquitetura de três camadas, portanto, você precisa dessas técnicas no nível de serviços (HTTP) e no nível de camada de dados (TCP). Em um aplicativo monolítico que usa uma única camada de aplicativo além do banco de dados (sem serviços ou microserviços adicionais), lidar com falhas transitórias no nível de conexão do banco de dados pode ser suficiente. Nesse cenário, apenas uma configuração específica da conexão de banco de dados é necessária.

Ao implementar comunicações resilientes que acessam o banco de dados, dependendo da versão do .NET que você está usando, pode ser simples (por exemplo, [com o Entity Framework 6 ou posterior](/ef/ef6/fundamentals/connection-resiliency/retry-logic). É apenas uma questão de configurar a conexão de banco de dados. Ou talvez você precise usar bibliotecas adicionais, como o [bloco de aplicativo de tratamento de falhas transitórias](https://docs.microsoft.com/previous-versions/msp-n-p/hh680934(v=pandp.50)) (para versões anteriores do .net), ou até mesmo implementar sua própria biblioteca.

Ao implementar repetições de HTTP e disparadores de circuito, a recomendação para o .NET é usar a biblioteca [Polly](https://github.com/App-vNext/Polly) , que tem como alvo .NET Framework 4,0, .NET Framework 4,5 e .net Standard 1,1, que inclui o suporte do .NET Core.

Para saber como implementar estratégias para lidar com falhas parciais na nuvem, consulte as referências a seguir.

### <a name="additional-resources"></a>Recursos adicionais

- **Implementando a comunicação resiliente para lidar com falhas parciais**

    [https://docs.microsoft.com/dotnet/architecture/microservices/implement-resilient-applications/partial-failure-strategies](../../microservices/implement-resilient-applications/partial-failure-strategies.md)

- **Entity Framework a resiliência da conexão e a lógica de repetição (versão 6 e posterior)**

    [https://docs.microsoft.com/ef/ef6/fundamentals/connection-resiliency/retry-logic](/ef/ef6/fundamentals/connection-resiliency/retry-logic)

- **Bloco de aplicativos de tratamento de falhas transitórias**

- <https://docs.microsoft.com/previous-versions/msp-n-p/hh680934(v=pandp.50)>

- **Biblioteca Polly para comunicação HTTP resiliente**

    <https://github.com/App-vNext/Polly>

>[!div class="step-by-step"]
>[Anterior](when-to-deploy-windows-containers-to-azure-container-service-kubernetes.md) 
> [Avançar](modernize-your-apps-with-monitoring-and-telemetry.md)
