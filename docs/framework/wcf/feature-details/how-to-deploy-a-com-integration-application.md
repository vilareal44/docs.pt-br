---
title: Como implantar uma aplicação de integração + COM
ms.date: 03/30/2017
ms.assetid: 2e5a0510-db3c-4988-a09c-696285836650
ms.openlocfilehash: b4ae7f730296d54debc1cf2971b61e5700503430
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595421"
---
# <a name="how-to-deploy-a-com-integration-application"></a>Como implantar uma aplicação de integração + COM
Depois de escrever um aplicativo de integração COM+, talvez você queira implantá-lo em outro computador. Este tópico descreve como mover um aplicativo de integração COM+ de um computador para outro.  
  
### <a name="moving-a-com-hosted-integration-app"></a>Movendo um aplicativo de integração hospedado COM+  
  
1. Verifique se o WCF está instalado em ambos os computadores.  
  
2. Exporte o aplicativo do computador A.  
  
3. Importe o aplicativo no computador B.  
  
4. Defina o diretório raiz do aplicativo. Por convenção, é% PROGRAMfiles%/ComPlus Applications/{AppGUID}.  
  
5. Copie os arquivos Application. config e Application. manifest do diretório raiz do aplicativo no computador A para o diretório raiz do aplicativo no computador B.  
  
6. Edite os endereços de ponto de extremidade de serviço no arquivo Application. config no computador B para identificar o computador apropriado. Por exemplo, altere `http://machineA/MyService` para `http://machineB/MyService`.  
  
### <a name="moving-a-web-hosted-integration-application"></a>Movendo um aplicativo de integração hospedado na Web  
  
1. Verifique se o WCF está instalado em ambos os computadores.  
  
2. Exporte o aplicativo do computador A.  
  
3. Importe o aplicativo no computador B.  
  
4. Crie uma VRoot do IIS no computador B.  
  
5. Copie o arquivo. svc (ComponentName. svc) e o arquivo Web. config do vroot no computador a para o vroot recentemente criado no computador B.  
  
## <a name="see-also"></a>Consulte também

- [Integração com visão geral de aplicativos COM+](integrating-with-com-plus-applications-overview.md)
- [Como configurar configurações de serviço de COM+](how-to-configure-com-service-settings.md)
- [Como usar a ferramenta de configuração de modelo de serviço do COM+](how-to-use-the-com-service-model-configuration-tool.md)
