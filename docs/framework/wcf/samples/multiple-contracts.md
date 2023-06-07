---
title: Vários contratos
ms.date: 03/30/2017
ms.assetid: 2bef319b-fe9c-4d49-ac6c-dfb23eb35099
ms.openlocfilehash: e8451c49395a1dad55c5afca419f47a8e856b61f
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602499"
---
# <a name="multiple-contracts"></a>Vários contratos
O exemplo de vários contratos demonstra como implementar mais de um contrato em um serviço e como configurar pontos de extremidade para se comunicar com cada um dos contratos implementados. Este exemplo é baseado na [introdução](getting-started-sample.md). O serviço foi modificado para definir dois contratos, o `ICalculator` contrato e o `ICalculatorSession` contrato.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 A classe de serviço implementa os `ICalculator` `ICalculatorSession` contratos e. Como um dos contratos requer uma sessão, o serviço usa o <xref:System.ServiceModel.InstanceContextMode.PerSession> modo de instância para manter o estado durante o tempo de vida da sessão.  
  
 A configuração de serviço foi modificada para definir dois pontos de extremidade para expor cada contrato. O `ICalculator` ponto de extremidade é exposto no endereço base usando um `basicHttpBinding` . O `ICalculatorSession` ponto de extremidade é exposto no BaseAddress/Session usando um `wsHttpBinding` com o `bindingConfiguration` atributo definido como `BindingWithSession` , conforme mostrado na seguinte configuração de exemplo.  
  
```xml  
<service
    name="Microsoft.ServiceModel.Samples.CalculatorService"  
    behaviorConfiguration="CalculatorServiceBehavior">  
  <!-- ICalculator endpoint is exposed using BasicBinding at the base  
       address provided by host:   
       http://localhost/servicemodelsamples/service.svc  -->  
  <endpoint address=""  
            binding="basicHttpBinding"  
            contract="Microsoft.ServiceModel.Samples.ICalculator" />  
  <!-- ICalculatorSession endpoint is exposed using BindingWithSession  
       at {baseaddress}/session:  
       http://localhost/servicemodelsamples/service.svc/session -->  
  <endpoint address="session"  
            binding="wsHttpBinding"  
            bindingConfiguration="BindingWithSession"
           contract="Microsoft.ServiceModel.Samples.ICalculatorSession" />  
  ...  
</service>  
```  
  
 O código do cliente gerado agora inclui uma classe de cliente para o `ICalculator` contrato original e para o novo `ICalculatorSession` contrato. A configuração do cliente e o código foram modificados para se comunicar com cada contrato no ponto de extremidade de serviço apropriado.  
  
 O cliente é um aplicativo do Windows do console (. exe). O serviço é hospedado pelo Serviços de Informações da Internet (IIS).  
  
 A janela do console do cliente exibe as operações enviadas para cada um dos pontos de extremidade, primeiro o ponto final básico, seguido pelo ponto de extremidade seguro.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\MultipleContracts`  
