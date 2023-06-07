---
title: Recuperar metadados
ms.date: 03/30/2017
ms.assetid: e8a6ef8c-a195-495a-a15e-7d92bdf0b28c
ms.openlocfilehash: 4763686485dfe97844fad78cf0bb279113c0ce08
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594602"
---
# <a name="retrieve-metadata"></a>Recuperar metadados
Este exemplo demonstra como implementar um cliente que recupera dinamicamente os metadados de um serviço para escolher um ponto de extremidade com o qual se comunicar. Este exemplo é baseado na [introdução](getting-started-sample.md). O serviço foi modificado para expor dois pontos de extremidade — um ponto de extremidades no endereço base usando a `basicHttpBinding` associação e um ponto de extremidade seguro em {*BaseAddress*}/Secure usando a `wsHttpBinding` associação. Em vez de configurar o cliente com os endereços e associações de ponto de extremidade, o cliente recupera dinamicamente os metadados para o serviço usando a <xref:System.ServiceModel.Description.MetadataExchangeClient> classe e, em seguida, importa os metadados como um <xref:System.ServiceModel.Description.ServiceEndpointCollection> usando a <xref:System.ServiceModel.Description.WsdlImporter> classe.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 O aplicativo cliente usa o importado <xref:System.ServiceModel.Description.ServiceEndpointCollection> para criar clientes para se comunicar com o serviço. O aplicativo cliente itera por meio de cada ponto de extremidade recuperado e se comunica com cada ponto de extremidade que implementa o `ICalculator` contrato. O endereço apropriado e a associação são fornecidos com o ponto de extremidade recuperado, para que o cliente seja configurado para se comunicar com cada ponto de extremidade, conforme mostrado no código de exemplo a seguir.  
  
```csharp
// Create a MetadataExchangeClient for retrieving metadata.  
EndpointAddress mexAddress = new EndpointAddress(ConfigurationManager.AppSettings["mexAddress"]);  
MetadataExchangeClient mexClient = new MetadataExchangeClient(mexAddress);  
  
// Retrieve the metadata for all endpoints using metadata exchange protocol (mex).  
MetadataSet metadataSet = mexClient.GetMetadata();  
  
//Convert the metadata into endpoints.  
WsdlImporter importer = new WsdlImporter(metadataSet);  
ServiceEndpointCollection endpoints = importer.ImportAllEndpoints();  
  
CalculatorClient client = null;  
ContractDescription contract = ContractDescription.GetContract(typeof(ICalculator));  
// Communicate with each endpoint that supports the ICalculator contract.  
foreach (ServiceEndpoint ep in endpoints)  
{  
    if (ep.Contract.Namespace.Equals(contract.Namespace) && ep.Contract.Name.Equals(contract.Name))  
    {  
        // Create a client using the endpoint address and binding.  
        client = new CalculatorClient(ep.Binding, new EndpointAddress(ep.Address.Uri));  
        Console.WriteLine("Communicate with endpoint: ");  
        Console.WriteLine("   AddressPath={0}", ep.Address.Uri.PathAndQuery);  
        Console.WriteLine("   Binding={0}", ep.Binding.Name);  
        // Call operations.  
        DoCalculations(client);  
  
        //Closing the client gracefully closes the connection and cleans up resources.  
        client.Close();  
    }  
}  
```  
  
 A janela do console do cliente exibe as operações enviadas para cada um dos pontos de extremidade, exibindo o caminho do endereço e o nome da associação.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C#, C++ ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Client\RetrieveMetadata`  
