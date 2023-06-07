---
title: Configuração simplificada para serviços do WCF
description: Saiba como implementar e configurar um serviço e cliente típicos usando o WCF. O serviço se comunica usando um ponto de extremidade especificado em um arquivo de configuração.
ms.date: 03/30/2017
ms.assetid: 1e39ec25-18a3-4fdc-b6a3-9dfafbd60112
ms.openlocfilehash: 46a0c878b29de34219413a508799ddaddf507dd8
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246208"
---
# <a name="simplified-configuration-for-wcf-services"></a>Configuração simplificada para serviços do WCF
Este exemplo demonstra como implementar e configurar um serviço e cliente típicos usando o Windows Communication Foundation (WCF). Este exemplo é a base para todos os outros exemplos de tecnologia básica.  
  
 Esse serviço, que expõe um ponto de extremidade para se comunicar com o serviço, usa a configuração simplificada no .NET Framework 4. Antes do .NET Framework 4, o ponto de extremidade normalmente é definido em um arquivo de configuração (Web.config), conforme mostrado no código de configuração de exemplo a seguir.  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<!-- Copyright ©) Microsoft Corporation.  All Rights Reserved. -->  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="CalculatorServiceBehavior">  
          <serviceMetadata httpGetEnabled="True"/>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    <services>  
      <service name="Microsoft.Samples.GettingStarted.CalculatorService"  
               behaviorConfiguration="CalculatorServiceBehavior">  
        <endpoint address="" binding="basicHttpBinding" contract="ICalculator"/>  
        <endpoint address="mex" binding="mexHttpBinding" contract="IMetadataExchange"/>  
      </service>  
    </services>  
  </system.serviceModel>  
</configuration>  
```  
  
 No .NET Framework 4, o `<service>` elemento é opcional. Quando um serviço não define nenhum ponto de extremidade, um ponto de extremidades para cada endereço base e contrato implementado é adicionado ao serviço. O endereço base é acrescentado ao nome do contrato para determinar o ponto de extremidade e a associação é determinada pelo esquema de endereço. O exemplo de código a seguir demonstra um arquivo de configuração simplificado. Conforme configurado, o serviço pode ser acessado pelo `http://localhost/servicemodelsamples/service.svc` cliente do no mesmo computador. Para clientes em computadores remotos acessarem o serviço, um nome de domínio totalmente qualificado deve ser especificado em vez de localhost. O serviço não expõe metadados por padrão. Como tal, o serviço ativa o <xref:System.ServiceModel.Description.ServiceMetadataBehavior> comportamento.  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<!-- Copyright © Microsoft Corporation.  All Rights Reserved. -->  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="">  
          <serviceMetadata httpGetEnabled="True"/>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
### <a name="to-use-this-sample"></a>Para usar este exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Execute o exemplo seguindo estas etapas:  
  
    1. Clique com o botão direito do mouse no projeto de **serviço** e selecione **definir como projeto de inicialização**e pressione **Ctrl + F5**.  
  
    2. Aguarde a saída do console confirmando se o serviço está em execução.  
  
    3. Clique com o botão direito do mouse no projeto **cliente** e selecione **definir como projeto de inicialização**e pressione **Ctrl + F5**.  
  
> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\ConfigSimplificationIn40`  
  
## <a name="see-also"></a>Veja também

- [Exemplos de gerenciamento do AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ff383405(v=azure.10))
- [Configuração simplificada](../simplified-configuration.md)
