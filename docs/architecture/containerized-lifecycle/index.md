---
title: Containerized Docker Application Lifecycle with Microsoft Platform and Tools (Ciclo de vida de aplicativo do Docker em contêineres com a plataforma e as ferramentas da Microsoft)
description: Obtenha uma visão geral de alto nível do processo de desenvolvimento e implantação para desenvolver e implantar aplicativos em contêineres com o Docker e a plataforma e as ferramentas da Microsoft.
ms.date: 07/30/2020
ms.openlocfilehash: d8055315b25f73d7b0b355026ab6b2c4767f9d89
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87915160"
---
# <a name="containerized-docker-application-lifecycle-with-microsoft-platform-and-tools"></a>Containerized Docker Application Lifecycle with Microsoft Platform and Tools (Ciclo de vida de aplicativo do Docker em contêineres com a plataforma e as ferramentas da Microsoft)

![Capa do livro](./media/devops-book-cover-large-we.png)

**Edição v 3.1** -atualizado para ASP.NET Core 3,1

Este guia é uma visão geral para desenvolver e implantar aplicativos ASP.NET Core em contêineres com o Docker, usando a plataforma e as ferramentas da Microsoft. O guia inclui uma introdução de alto nível ao Azure DevOps, para implementar pipelines de CI/CD, bem como o ACR (registro de contêiner do Azure) e os serviços Kubernetess do Azure AKS para implantação.

Para obter detalhes relacionados ao desenvolvimento de baixo nível, você pode ver o guia [microservices do .net: arquitetura para aplicativos em contêineres .net](https://docs.microsoft.com/dotnet/architecture/microservices/) e o aplicativo de referência relacionado [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers).

## <a name="send-us-your-feedback"></a>Envie-nos seus comentários!

Escrevemos este guia para ajudá-lo a entender a arquitetura de aplicativos em contêineres e de microsserviços no .NET. O guia e o aplicativo de referência relacionado continuarão sendo desenvolvidos, portanto seus comentários são bem-vindos! Se você tiver comentários sobre como esse guia pode ser melhorado, envie comentários em <https://aka.ms/ebookfeedback> .

## <a name="credits"></a>Credits

Autor:

> **Cesar de la Torre**, Gerente sênior de produtos , equipe de produto do .NET, Microsoft Corp.

Editor de aquisições:

> **Janine Patrick**

Editor de desenvolvimento:

> **Bob Russell**, profissional de soluções na Microsoft
>
> [**Publicação octal, Inc.**](http://www.octalpub.com/)

Produção editorial:

> [Dianne Russell](http://www.octalpub.com/)
>
> **Publicação octal, Inc.**

Copyeditor:

> **Bob Russell**, profissional de soluções na Microsoft

Participantes e revisores:

> **Nish Anil**, Gerente de Programa Sênior, Equipe .NET, Microsoft
>
> **Miguel Veloso**, engenheiro de desenvolvimento de software em conceitos simples
>
> **Pedro Ghosh**, consultor principal da Neudesic

## <a name="copyright"></a>Direitos autorais

PUBLICADO POR

Divisão de Desenvolvedores Microsoft, equipes dos produtos .NET e Visual Studio

Uma divisão da Microsoft Corporation

One Microsoft Way

Redmond, Washington 98052-6399

Copyright &copy; 2020 da Microsoft Corporation

Todos os direitos reservados. Nenhuma parte do conteúdo deste guia pode ser reproduzida ou transmitida de nenhuma forma nem por nenhum meio sem a permissão por escrito do publicador.

Este livro é fornecido “no estado em que se encontra” e expressa os pontos de vista e as opiniões do autor. Os pontos de vista, as opiniões e as informações expressos neste guia, incluindo URLs e outras referências a sites da Internet, podem ser alteradas sem aviso prévio.

 Alguns exemplos aqui representados são fornecidos somente para fins de ilustração e são fictícios. Nenhuma associação ou conexão real é intencional ou deve ser inferida.

A Microsoft e as marcas listadas em <https://www.microsoft.com> na página da Web "Marcas" são marcas comerciais do grupo de empresas Microsoft.

Mac e macOS são marcas comerciais da Apple Inc.

O logotipo de redistribuição do Docker é uma marca registrada do Docker, Inc. usada pela permissão.

Todas as outras marcas e logotipos são propriedade de seus respectivos proprietários.

>[!div class="step-by-step"]
>[Próximo](introduction-to-containers-and-docker.md)
