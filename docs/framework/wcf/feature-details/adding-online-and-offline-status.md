---
title: Adicionando status online e offline
ms.date: 03/30/2017
ms.assetid: 05e5f51d-81b6-4c17-b364-9dda447a5fce
ms.openlocfilehash: da170bbc22d04dcbf5f7cd4ac084a004bb4b026e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597677"
---
# <a name="adding-online-and-offline-status"></a>Adicionando status online e offline
Em muitos casos, é importante que um aplicativo monitore detalhes específicos sobre o status de uma conexão de canal de mesmo nível. Você pode obter essas informações chamando o `GetProperty` método em uma implementação da <xref:System.ServiceModel.IOnlineStatus> interface. Um objeto com uma implementação dessa interface pode monitorar o status da conexão ou registrar-se para manipuladores de eventos, como `OnOnline` e e `OnOffline` reagir imediatamente conforme as alterações no status online ocorrem.  
  
 Na infraestrutura de canal par, um cliente será considerado online se estiver conectado a pelo menos um outro par e offline do contrário. Isso pode ser particularmente útil na depuração de aplicativos de desenvolvimento ou na exibição de informações detalhadas para o usuário final.  
  
> [!NOTE]
> Um manipulador de eventos online deve primeiro garantir que o nó esteja aberto antes de enviar qualquer mensagem.  
  
## <a name="see-also"></a>Consulte também

- [Compilando um aplicativo de canal par](building-a-peer-channel-application.md)
