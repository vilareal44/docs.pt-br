---
title: Como usar um Moniker de serviço com contratos de intercâmbio de metadados
ms.date: 03/30/2017
ms.assetid: c41a07e5-cb9d-45d6-9ea4-34511e227faf
ms.openlocfilehash: 04a940a6e8f010e5cd851684c5fc62bab2a1a034
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601160"
---
# <a name="how-to-use-a-service-moniker-with-metadata-exchange-contracts"></a>Como usar um Moniker de serviço com contratos de intercâmbio de metadados
Depois de desenvolver alguns novos serviços WCF, você pode decidir que deseja ser capaz de chamar esses serviços de um script ou de um aplicativo Visual Basic 6,0. Um método seria gerar um assembly de cliente WCF, registrar o assembly com COM, instalar o assembly no GAC e, em seguida, fazer referência aos tipos COM do seu código Visual Basic. Ao distribuir o aplicativo, você também precisará distribuir o assembly do cliente WCF. O usuário precisará registrar o assembly do cliente WCF com com e colocá-lo no GAC. A interoperabilidade COM do WCF também permite que você faça as mesmas chamadas de serviço sem depender de um assembly de cliente WCF. O moniker do WCF permite chamar qualquer serviço WCF de qualquer linguagem compatível COM COM (Visual Basic, VBScript, Visual Basic for Applications (VBA) e assim por diante), especificando um URI de ponto de extremidade de intercâmbio de metadados (MEX) que o moniker do serviço usa para extrair informações de tipo sobre o serviço. Este tópico descreve como chamar o Introdução exemplo do WCF usando um moniker do WCF que especifica um ponto de extremidade MEX.  
  
> [!NOTE]
> Os tipos definidos pelo assembly do cliente WCF nunca são instanciados na verdade. O assembly é usado somente para metadados.  
  
### <a name="using-the-service-moniker-with-a-mex-address"></a>Usando o moniker do serviço com um endereço MEX  
  
1. Crie o Introdução exemplo e use o Internet Explorer para navegar até sua URL ( `http://localhost/ServiceModelSamples/Service.svc` ) para garantir que o serviço esteja funcionando.  
  
2. Crie um script de Visual Basic ou Visual Basic aplicativo que contenha o seguinte código:  
  
    ```vb
    monString = "service:mexaddress=http://localhost/ServiceModelSamples/Service.svc/MEX"  
    monString = monString + ", address=http://localhost/ServiceModelSamples/Service.svc"  
    monString = monString + ", contract=ICalculator, contractNamespace=http://Microsoft.ServiceModel.Samples"  
    monString = monString + ", binding=WSHttpBinding_ICalculator, bindingNamespace=http://Microsoft.ServiceModel.Samples"  
  
    Set calc = GetObject(monString)  
    MsgBox calc.Add(3, 4)  
    ```  
  
3. Execute o aplicativo Visual Basic ou o script.  
  
    > [!NOTE]
    > O serviço que você está chamando deve expor um ponto de extremidade MEX para que o moniker possa ler os metadados do serviço. Para obter mais informações, consulte [como: publicar metadados para um serviço usando um arquivo de configuração](how-to-publish-metadata-for-a-service-using-a-configuration-file.md).  
  
    > [!NOTE]
    > Se o moniker estiver malformado ou se o serviço estiver indisponível, a chamada para retornará `GetObject` um erro dizendo "sintaxe inválida".  Se você receber esse erro, verifique se o moniker que você está usando está correto e se o serviço está disponível.  
  
## <a name="see-also"></a>Consulte também

- [Como usar o Moniker de serviço do Windows Communication Foundation sem registro](use-the-wcf-service-moniker-without-registration.md)
- [Como usar um Moniker de serviço com contratos WSDL](how-to-use-a-service-moniker-with-wsdl-contracts.md)
