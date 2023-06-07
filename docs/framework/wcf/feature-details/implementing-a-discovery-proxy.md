---
title: Implementando um proxy de descoberta
ms.date: 03/30/2017
ms.assetid: dda20e79-8df3-438e-a281-69d779d978ec
ms.openlocfilehash: 382df95fef2108d338e4ea327da9185c856eca5a
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84579235"
---
# <a name="implementing-a-discovery-proxy"></a>Implementando um proxy de descoberta
Esta seção descreve as etapas necessárias para implementar um proxy de descoberta. Um proxy de descoberta é um serviço autônomo que contém um repositório de serviços. Os clientes podem consultar um proxy de descoberta para localizar serviços detectáveis que o proxy esteja ciente. A forma como um proxy é populado com serviços é o implementador. Por exemplo, um proxy de descoberta pode se conectar a um repositório de serviço existente e tornar essas informações detectáveis, um administrador pode usar uma API de gerenciamento para adicionar serviços detectáveis a um proxy ou um proxy de descoberta pode usar a funcionalidade de anúncio para atualizar seu cache interno.  
  
 A implementação do WCF fornece classes base que permitem que você crie facilmente um proxy. Você pode utilizar essas APIs para criar um proxy de descoberta na parte superior do repositório existente.  
  
 O proxy de descoberta implementado aqui é como qualquer outro serviço do WCF, no que você também pode tornar o proxy de descoberta detectável e fazer com que os clientes localizem seus pontos de extremidade.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Como implementar um proxy de descoberta](how-to-implement-a-discovery-proxy.md)  
 Descreve como implementar um proxy de descoberta.  
  
 [Como implementar um serviço de descoberta que registra usando o proxy de descoberta](discoverable-service-that-registers-with-the-discovery-proxy.md)  
 Descreve como implementar um serviço WCF detectável que registra com o proxy de descoberta.  
  
 [Como implementar um aplicativo cliente que utiliza o proxy de descoberta para encontrar um serviço](client-app-discovery-proxy-to-find-a-service.md)  
 Descreve como implementar um aplicativo cliente WCF que usa o proxy de descoberta para pesquisar um serviço.  
  
 [Como testar o proxy de descoberta](how-to-test-the-discovery-proxy.md)  
 Descreve como testar o código escrito nos três tópicos anteriores.  
  
## <a name="see-also"></a>Consulte também

- [Descoberta de WCF](wcf-discovery.md)
- [Como adicionar programaticamente a capacidade de descoberta para um cliente e serviço do WCF](how-to-programmatically-add-discoverability-to-a-wcf-service-and-client.md)
