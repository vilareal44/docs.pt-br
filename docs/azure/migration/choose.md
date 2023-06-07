---
title: Escolher a opção de hospedagem certa do Azure
description: Saiba qual caminho de migração do Azure é correto para seu aplicativo Web ASP.NET.
author: CESARDELATORRE
ms.author: cesardl
ms.date: 03/01/2020
ms.openlocfilehash: 162dc8eb87dfd78d050b93b1c24ac573d7092126
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174290"
---
# <a name="choose-the-right-azure-hosting-option"></a>Escolher a opção de hospedagem certa do Azure

Este artigo fornece considerações e comparações entre as várias opções que você tem no Azure ao migrar seus aplicativos .NET Framework existentes do local para o Azure.

As áreas fundamentais a considerar ao migrar os aplicativos .NET existentes para o Azure são:

1. Opções de computação
1. Opções de banco de dados
1. Considerações de rede e segurança
1. Considerações de autenticação e autorização

## <a name="compute-choices"></a>Opções de computação

Ao migrar os aplicativos .NET Framework existentes para o Azure, você tem várias opções. No entanto, como o .NET Framework depende do Windows, as seguintes opções são limitadas aos serviços de computação baseados no Windows.

A tabela a seguir mostra várias comparações e recomendações para ajudá-lo a escolher o caminho de migração da computação correto para seu aplicativo .NET existente.

|                 | VMs do Azure | Serviço de Aplicativo do Azure | Contêineres do Windows |
|-----------------|-----------|-------------------|--------------------|
|Quando usar      |<ul><li>O aplicativo depende muito do servidor e das instalações .msi locais.</li><li>Você deseja o caminho de migração do aplicativo mais fácil</li></ul>|O aplicativo não tem nenhuma dependência do servidor, ele é apenas um aplicativo Web ASP.NET limpo (MVC, WebForm) ou um aplicativo de N Camadas (API da Web, WCf) acessando um servidor do banco de dados. |<ul><li>O aplicativo tem dependências no servidor original, mas essas dependências podem ser incluídas na imagem do Docker Windows.</li><li>Deseja modernizar o aplicativo para que ele esteja [pronto para DevOps de nuvem](../../architecture/modernize-with-azure-containers/modernize-existing-apps-to-cloud-optimized/reasons-to-modernize-existing-net-apps-to-cloud-optimized-applications.md)</li></ul>|
|Vantagens e benefícios  |<ul><li>Caminho de migração mais fácil</li><li>Ambiente familiar. O ambiente de implantação é uma VM, portanto, é semelhante aos servidores locais.</li></ul> |Manutenção PaaS contínua, um modo mais simples de gerenciar e dimensionar os aplicativos no Azure. |<ul><li>Preparado para o futuro, DevOps em nuvem com dependências incluídas nos contêineres do aplicativo.</li><li>Quase não há necessidade de Refatorar o código .NET/C #.</li></ul> |
|Contras             |É IaaS. A manutenção é cara. Você precisa gerenciar a infraestrutura da VM sobre rede, balanceador de carga, expansão, gerenciamento do IIS e assim por diante. |<ul><li>Nem todos os aplicativos têm [suporte](https://appmigration.microsoft.com/assessment)</li><li>Alguns aplicativos podem precisar ser refatorado e, até mesmo, rearquitetados, para que ofereçam suporte a Azure App serviço.</li></ul> |<ul><li>Curva de aprendizado de habilidades do Docker</li><li>Algumas alterações nas definições de configuração do aplicativo e do código</li></ul>|
|Requisitos |VM do Windows Server com os mesmos requisitos do aplicativo local | Azure App requisitos de serviço especificados em [verificações de preparação](https://github.com/Azure/App-Service-Migration-Assistant/wiki/Readiness-Checks). |<ul><li>[Docker Engine-Enterprise para Windows Server 2019](https://azuremarketplace.microsoft.com/marketplace/apps/cloud-infrastructure-services.docker-windows-2019)<br />ou</li><li>[Serviço de Contêiner do Azure (AKS)](https://azure.microsoft.com/services/container-service/) (que é o orquestrador Kubernetes)<br />ou<li>Orquestrador [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/)</li></ul> |
|Como migrar |Confira [Migrar para Máquinas Virtuais do Azure](vm.md) | Confira [Migrar o Serviço de Aplicativo do Azure](app-service.md) | Siga as considerações, os cenários e as orientações explicadas no [livro modernizando aplicativos .net existentes com o Azure e contêineres do Windows](https://aka.ms/liftandshiftwithcontainersebook) |

O diagrama de fluxograma a seguir mostra uma árvore de decisão ao planejar uma migração para o Azure para seus aplicativos .NET Framework existentes. Se for viável, tente a opção A primeiro, mas a opção B é o caminho mais fácil de executar.

![Fluxograma mostrando a árvore de decisão de hospedagem](../media/migration/choose/decision-tree.png)

## <a name="database-choices"></a>Opções de banco de dados

Ao migrar os bancos de dados relacionais para o Azure, você tem várias opções. Confira [Migrar seu banco de dados SQL Server para o Azure](sql.md) para ajudá-lo a escolher o caminho de migração do banco de dados correto para seu aplicativo .NET existente.

## <a name="networking-and-security-considerations"></a>Considerações de rede e segurança

Ao implantar aplicativos em uma nuvem pública, como o Microsoft Azure, convém isolar e proteger certas redes [criando redes de perímetro de rede](/azure/architecture/reference-architectures/dmz/), como um [rede de perímetro entre o Azure e o local](/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) ou um [rede de perímetro entre o Azure e a Internet](/azure/architecture/reference-architectures/dmz/secure-vnet-dmz). As redes de perímetro podem ser implementados com a [Rede Virtual do Azure](/azure/virtual-network/virtual-networks-overview).

As redes virtuais do Azure permitem:

- compilar uma infraestrutura híbrida que você controla
- trazer seus próprios endereços IP e servidores DNS
- proteger suas conexões com uma VPN IPsec ou ExpressRoute
- ter um controle granular do tráfego entre as sub-redes
- criar topologias de rede sofisticadas usando soluções de virtualização
- Obtenha um ambiente isolado e altamente seguro para seus aplicativos

Para começar a criar sua própria rede virtual, confira a [Documentação da Rede Virtual do Azure](/azure/virtual-network/).

## <a name="authentication-and-authorization-considerations-when-migrating-to-azure"></a>Considerações de autenticação e autorização ao migrar para o Azure

Uma preocupação principal de qualquer organização ao mover para a nuvem é a segurança. A maioria das empresas investiu uma quantidade significativa de tempo, dinheiro e engenharia na criação e no desenvolvimento de um modelo de segurança, e é importante que eles possam aproveitar os investimentos existentes, como armazenamentos de identidades e soluções de logon único.

Muitos aplicativos .NET B2E existentes de empresas em execução no local usam o Active Directory para a autenticação e o gerenciamento de identidades. O Azure AD Connect permite integrar seus diretórios locais no Azure Active Directory. Para começar, confira [Integrar seus diretórios locais no Azure Active Directory](/azure/active-directory/connect/active-directory-aadconnect).

Confira [Requisitos de identidade para sua solução de identidade híbrida](/azure/active-directory/active-directory-hybrid-identity-design-considerations-business-needs) para ter um planejamento adicional relacionado ao Azure Active Directory.

As outras opções de protocolo de autenticação são [OAuth](https://en.wikipedia.org/wiki/OAuth) e [OpenID](https://en.wikipedia.org/wiki/OpenID), comuns nos aplicativos voltados para o consumidor. Ao usar bancos de dados de identidade autônomos, como um banco de dados SQL de Identidade do ASP.NET encapsulado pelo IdentityServer4 que usa o OAuth, geralmente não é necessária nenhuma conectividade com diretórios ou bancos de dados locais.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Migrar um aplicativo Web ASP.NET para o serviço Azure App](app-service.md)
