---
title: Descrição do Serviço
ms.date: 03/30/2017
ms.assetid: 7034b5d6-d608-45f3-b57d-ec135f83ff24
ms.openlocfilehash: 467d8437ec6b3383974b1faf2a96aacb1524771a
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596572"
---
# <a name="service-description"></a>Descrição do Serviço
O exemplo de descrição de serviço demonstra como um serviço pode recuperar suas informações de descrição de serviço em tempo de execução. O exemplo é baseado na [introdução](getting-started-sample.md), com uma operação de serviço adicional definida para retornar informações descritivas sobre o serviço. As informações retornadas listam os endereços base e os pontos de extremidade do serviço. O serviço fornece essas informações usando as <xref:System.ServiceModel.OperationContext> <xref:System.ServiceModel.ServiceHost> classes, e <xref:System.ServiceModel.Description.ServiceDescription> .  
  
 Neste exemplo, o cliente é um aplicativo de console (. exe) e o serviço é hospedado pelo Serviços de Informações da Internet (IIS).  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 Este exemplo tem uma versão modificada do contrato de calculadora chamado `IServiceDescriptionCalculator` . O contrato define uma operação de serviço adicional denominada `GetServiceDescriptionInfo` que retorna uma cadeia de caracteres de várias linhas para o cliente que descreve o endereço base ou endereços e pontos de extremidade de serviço ou de acesso para o serviço.  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface IServiceDescriptionCalculator  
{  
    [OperationContract]  
    int Add(int n1, int n2);  
    [OperationContract]  
    int Subtract(int n1, int n2);  
    [OperationContract]  
    int Multiply(int n1, int n2);  
    [OperationContract]  
    int Divide(int n1, int n2);  
    [OperationContract]  
    string GetServiceDescriptionInfo();  
}  
```  
  
 O código de implementação para o `GetServiceDescriptionInfo` usa o <xref:System.ServiceModel.Description.ServiceDescription> para listar os pontos de extremidade de serviço. Como os pontos de extremidade de serviço podem ter endereços relativos, ele primeiro lista os endereços base para o serviço. Para obter todas essas informações, o código obtém seu contexto de operação usando <xref:System.ServiceModel.OperationContext.Current%2A> . O <xref:System.ServiceModel.ServiceHost> e seu <xref:System.ServiceModel.Description.ServiceDescription> objeto são recuperados do contexto de operação. Para listar os pontos de extremidade base para o serviço, o código é iterado por meio da coleção do host de serviço <xref:System.ServiceModel.ServiceHostBase.BaseAddresses%2A> . Para listar os pontos de extremidade de serviço para o serviço, o código é iterado pela coleção de pontos de extremidade da descrição de serviço.  
  
```csharp
public string GetServiceDescriptionInfo()  
{  
    string info = "";  
    OperationContext operationContext = OperationContext.Current;  
    ServiceHost host = (ServiceHost)operationContext.Host;  
    ServiceDescription desc = host.Description;  
    // Enumerate the base addresses in the service host.  
    info += "Base addresses:\n";  
    foreach (Uri uri in host.BaseAddresses)  
    {  
        info += "    " + uri + "\n";  
    }  
    // Enumerate the service endpoints in the service description.  
    info += "Service endpoints:\n";  
    foreach (ServiceEndpoint endpoint in desc.Endpoints)  
    {  
        info += "    Address:  " + endpoint.Address + "\n";  
        info += "    Binding:  " + endpoint.Binding.Name + "\n";  
        info += "    Contract: " + endpoint.Contract.Name + "\n";  
    }  
     return info;  
}  
```  
  
 Ao executar o exemplo, você vê as operações de calculadora e as informações de serviço retornadas pela `GetServiceDescriptionInfo` operação. Pressione ENTER na janela do cliente para desligar o cliente.  
  
```console  
Add(15,3) = 18  
Subtract(145,76) = 69  
Multiply(9,81) = 729  
Divide(22,7) = 3  
GetServiceDescriptionInfo  
Base addresses:  
    http://<machine-name>/ServiceModelSamples/service.svc  
    https://<machine-name>/ServiceModelSamples/service.svc  
Service endpoints:  
    Address:  http://<machine-name>/ServiceModelSamples/service.svc  
    Binding:  WSHttpBinding  
    Contract: IServiceDescriptionCalculator  
    Address:  http://<machine-name>/ServiceModelSamples/service.svc/mex  
    Binding:  MetadataExchangeHttpBinding  
    Contract: IMetadataExchange  
  
Press <ENTER> to terminate client.  
```  
  
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
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\ServiceDescription`  
