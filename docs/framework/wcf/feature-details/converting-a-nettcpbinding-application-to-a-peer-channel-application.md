---
title: Convertendo um aplicativo NetTcpBinding em um aplicativo de canal par
ms.date: 03/30/2017
ms.assetid: d4137292-a923-4b8f-8594-42276f2d3ce2
ms.openlocfilehash: 42266d8c7c04e2d8f3f1e4734d9a05181c3f1ea3
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595551"
---
# <a name="converting-a-nettcpbinding-application-to-a-peer-channel-application"></a>Convertendo um aplicativo NetTcpBinding em um aplicativo de canal par
Você pode criar conexões entre clientes usando o WinFX usando associações que descrevem os parâmetros da conexão. Converter um aplicativo .NET Framework para usar conexões ponto a ponto requer uma associação que dá suporte a essa tecnologia ao fazer conexões de cliente. O canal par fornece uma associação chamada <xref:System.ServiceModel.NetPeerTcpBinding> , que pode ser usada de forma semelhante ao <xref:System.ServiceModel.NetTcpBinding> . As principais diferenças incluem a especificação de um serviço de resolvedor e a definição de configurações de segurança.  
  
 Se um aplicativo estiver usando o resolvedor padrão e as configurações de segurança, converter um aplicativo normal baseado em cliente/servidor para usar o canal de pares envolve alterar o nome da Associação de "NetTcpbinding" para "NetPeerTcpBinding" no arquivo de configuração do aplicativo — você não precisa alterar a base de código do aplicativo.  
  
## <a name="see-also"></a>Consulte também

- [Compilando um aplicativo de canal par](building-a-peer-channel-application.md)
- [Associações fornecidas pelo sistema](../system-provided-bindings.md)
