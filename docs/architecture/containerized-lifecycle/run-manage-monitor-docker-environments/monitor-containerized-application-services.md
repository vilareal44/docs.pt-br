---
title: Monitorar serviços de aplicativos em contêineres
description: Saiba alguns aspectos fundamentais do monitoramento de arquiteturas de contêiner
ms.date: 08/06/2020
ms.openlocfilehash: 5fcb5065d02018ab3c2d3ad71cf507bd43b53446
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87914942"
---
# <a name="monitor-containerized-application-services"></a>Monitorar serviços de aplicativos em contêineres

É essencial para aplicativos divididos em vários contêineres e microsserviços ter uma maneira de monitorar e analisar o comportamento do aplicativo como um todo.

## <a name="azure-monitor"></a>Azure Monitor

O [Azure Monitor](https://azure.microsoft.com/services/monitor/) é um serviço de análise extensível que monitora seu aplicativo online. Com ele, você pode detectar e diagnosticar problemas de desempenho e entender o que os usuários realmente fazem com seu aplicativo. É projetado para desenvolvedores com a intenção de ajudar você a melhorar continuamente o desempenho e a usabilidade de seus aplicativos ou serviços. O Azure Monitor funciona com serviços e aplicativos Web autônomos em uma ampla variedade de plataforma, como .NET, Java, Node.js e diversas outras hospedadas localmente ou na nuvem.

### <a name="additional-resources"></a>Recursos adicionais

- **Visão geral do Azure Monitor** \
  <https://docs.microsoft.com/azure/azure-monitor/overview>

- **O que é o Application Insights?** \
  <https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview>

- **O que são as Métricas do Azure Monitor?** \
  <https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics>

- **Solução de monitoramento de contêiner no Azure Monitor** \
  <https://docs.microsoft.com/azure/azure-monitor/insights/containers>

## <a name="security-and-backup-services"></a>Serviços de backup e segurança

Há muitas tarefas de suporte com vários detalhes que você precisa abordar para garantir que os aplicativos e a infraestrutura estejam nas melhores condições para apoiar as necessidades de negócios. A situação fica mais complicada no caso dos microsserviços, portanto, você precisa de uma forma de obter exibições detalhadas e de alto nível quando precisar tomar medidas.

O Azure tem as ferramentas para gerenciar e oferecer uma exibição unificada de quatro aspectos fundamentais dos recursos locais e em nuvem:

- **Segurança**. Com a [Central de Segurança do Azure](https://azure.microsoft.com/services/security-center/).
  - Tenha visibilidade e controle da segurança das suas máquinas virtuais, seus aplicativos e suas cargas de trabalho.
  - Centralize o gerenciamento das suas políticas de segurança e integre os processos e as ferramentas existentes.
  - Detecte ameaças reais com análise avançada.

- **Backup**. Com o [Backup do Azure](https://azure.microsoft.com/services/backup/).
  - Evite interrupções onerosas nos negócios, cumpra as metas de conformidade e proteja seus dados contra ransomware e erros humanos.
  - Mantenha seus dados de backup criptografados em trânsito e em repouso.
  - Garanta acesso com base na autenticação multifator para evitar o uso não autorizado.

- **Recursos locais**. Com [soluções de nuvem híbrida](https://azure.microsoft.com/solutions/hybrid-cloud-app/).

>[!div class="step-by-step"]
>[Anterior](manage-production-docker-environments.md) 
> [Avançar](../key-takeaways/index.md)
