---
title: Integração com aplicativos COM
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Communication Foundation, COM integration
- COM [WCF], Windows Communication Foundation integration
- WCF, reusing code
- Windows Communication Foundation, reusing code
- COM [WCF]
- WCF, COM integration
ms.assetid: c98bda3e-6779-419e-8e6d-9aa94053026d
ms.openlocfilehash: dc3bbe0ee72ca5583b1e52a61c914ad866a22a05
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596804"
---
# <a name="integrating-with-com-applications"></a>Integração com aplicativos COM
Os serviços do Windows Communication Foundation (WCF) podem ser integrados diretamente ao seu código existente usando o moniker do serviço WCF. O moniker do serviço pode ser usado em uma ampla variedade de ambientes de desenvolvimento baseados em COM, como o Office VBA, Visual Basic 6,0 ou Visual C++ 6,0.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Visão geral da integração com aplicativos COM](integrating-with-com-applications-overview.md)  
 Fornece uma visão geral das principais partes do processo de integração.  
  
 [Como registrar e configurar um Moniker de serviço](how-to-register-and-configure-a-service-moniker.md)  
 Para usar o moniker do serviço WCF em um aplicativo COM, registre os tipos atribuídos necessários com com e configure o aplicativo COM e o moniker com a configuração de associação necessária.  
  
 [Como usar o Moniker de serviço do Windows Communication Foundation sem registro](use-the-wcf-service-moniker-without-registration.md)  
 Explica como obter a definição do contrato na forma de um documento WSDL (Web Services Definition Language) ou de um ponto de extremidade WS-MetadataExchange.  
  
 [Como usar um Moniker de serviço com contratos WSDL](how-to-use-a-service-moniker-with-wsdl-contracts.md)  
 Descreve como chamar um exemplo do WCF usando um moniker WSDL do WCF.  
  
 [Como usar um moniker de serviço com contratos de troca de metadados](how-to-use-a-service-moniker-with-metadata-exchange-contracts.md)  
 Descreve como chamar um exemplo do WCF usando um moniker do WCF que especifica um ponto de extremidade MEX.  
  
 [Como especificar credenciais de segurança de canal](how-to-specify-channel-security-credentials.md)  
 O moniker do serviço WCF dá suporte à `IChannelCredentials` interface que permite um intervalo de métodos alternativos para especificar credenciais de canal.  
  
## <a name="reference"></a>Referência  
 <xref:System.ServiceModel>  
  
## <a name="see-also"></a>Consulte também

- [Integração com aplicativos COM+](integrating-with-com-plus-applications.md)
