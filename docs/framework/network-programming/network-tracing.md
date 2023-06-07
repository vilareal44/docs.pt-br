---
title: Rastreamento de rede no .NET Framework
description: Saiba mais sobre o rastreamento de rede no .NET Framework, que fornece informações sobre invocações de método e tráfego de rede para um aplicativo gerenciado.
ms.date: 03/30/2017
helpviewer_keywords:
- debugging [.NET Framework], network tracing
- methods, network tracing
- Networking
- trace enabled debugging
- network tracing, about network tracing
- network tracing
- network, network tracing
- traffic tracing
- tracing [.NET Framework], network
- deploying applications [.NET Framework], network traffic
- capturing network traffic
- Internet, network tracing
- Network Resources
- output, network tracing
- method invocations
ms.assetid: e993b7c3-087f-45d8-9c02-9dded936d804
ms.openlocfilehash: 172f8ce4a50fe9294ee34cf65c0a39eb2f29badc
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502243"
---
# <a name="network-tracing-in-the-net-framework"></a>Rastreamento de rede no .NET Framework
O rastreamento de rede no .NET Framework fornece acesso a informações sobre invocações de método e tráfego de rede geradas por um aplicativo gerenciado. Esse recurso é útil para a depuração de aplicativos em desenvolvimento e para analisar aplicativos implantados. A saída fornecida pelo rastreamento de rede é personalizável para oferecer suporte a diferentes cenários de uso em tempo de desenvolvimento e em um ambiente de produção.  
  
 Para habilitar o rastreamento de rede no .NET Framework, você deve selecionar um destino para rastrear a saída e adicionar parâmetros de configuração de rastreamento de rede ao aplicativo ou ao arquivo de configuração do computador. Para obter descrições dos arquivos de configuração e como eles são usados, consulte [Arquivos de configuração](../configure-apps/index.md). Para obter informações sobre como habilitar o rastreamento de rede, consulte [Habilitar o rastreamento de rede](enabling-network-tracing.md). Para obter informações sobre as configurações que você precisa adicionar ao arquivo de configuração, consulte [Como configurar o rastreamento de rede](how-to-configure-network-tracing.md).  
  
 Quando o rastreamento for habilitado, você poderá coletar informações de rastreamento emitidas pelas classes **System.Net**. Os membros da classe de rede que gerenciam as informações de rastreamento incluem esta nota na seção Observações da documentação da biblioteca de classes do NET Framework:  
  
> [!NOTE]
> Esse membro emite o rastreamento de informações quando você ativa o rastreamento de rede em seu aplicativo. Para obter mais informações, consulte Rastreamento de rede.  
  
## <a name="see-also"></a>Confira também

- [Habilitando o rastreamento de rede](enabling-network-tracing.md)
- [Como configurar o rastreamento de rede](how-to-configure-network-tracing.md)
- [Interpretando o rastreamento de rede](interpreting-network-tracing.md)
- [Rastreamento e instrumentação de aplicativos](../debug-trace-profile/tracing-and-instrumenting-applications.md)
