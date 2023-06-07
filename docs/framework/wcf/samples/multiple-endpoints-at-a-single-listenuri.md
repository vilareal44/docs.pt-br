---
title: Vários pontos de extremidade em único ListenUri
ms.date: 03/30/2017
ms.assetid: 911ffad4-4d47-4430-b7c2-79192ce6bcbd
ms.openlocfilehash: 91220c6631db2f283b6571fbc32af2211feeaa35
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602486"
---
# <a name="multiple-endpoints-at-a-single-listenuri"></a>Vários pontos de extremidade em único ListenUri
Este exemplo demonstra um serviço que hospeda vários pontos de extremidade em um único `ListenUri` . Este exemplo é baseado no [introdução](getting-started-sample.md) que implementa um serviço de calculadora.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 Conforme demonstrado no exemplo de [vários pontos de extremidade](multiple-endpoints.md) , um serviço pode hospedar vários pontos de extremidade, cada um com endereços diferentes e, possivelmente, também associações diferentes. Este exemplo mostra que é possível hospedar vários pontos de extremidade no mesmo endereço. Este exemplo também demonstra as diferenças entre os dois tipos de endereços que um ponto de extremidade de serviço tem: `EndpointAddress` e `ListenUri` .  
  
 O `EndpointAddress` é o endereço lógico de um serviço. É o endereço para o qual as mensagens SOAP são endereçadas. O `ListenUri` é o endereço físico do serviço. Ele tem as informações de porta e endereço em que o ponto de extremidade de serviço realmente escuta mensagens no computador atual. Na maioria dos casos, não é necessário que esses endereços sejam diferentes; Quando um `ListenUri` não é especificado explicitamente, ele usa como padrão o URI do `EndpointAddress` ponto de extremidade. Em alguns casos, é útil distingui-los, como ao configurar um roteador, que pode aceitar mensagens endereçadas a vários serviços diferentes.  
  
## <a name="service"></a>Serviço  
 O serviço neste exemplo tem dois contratos `ICalculator` e `IEcho` . Além do ponto de extremidade personalizado `IMetadataExchange` , há três pontos de extremidades de aplicativo, conforme mostrado no código a seguir.  
  
```xml  
<endpoint address="urn:Stuff"  
        binding="wsHttpBinding"  
        contract="Microsoft.ServiceModel.Samples.ICalculator"
        listenUri="http://localhost/servicemodelsamples/service.svc" />  
<endpoint address="urn:Stuff"  
        binding="wsHttpBinding"  
        contract="Microsoft.ServiceModel.Samples.IEcho"
        listenUri="http://localhost/servicemodelsamples/service.svc" />  
<endpoint address="urn:OtherEcho"  
        binding="wsHttpBinding"  
        contract="Microsoft.ServiceModel.Samples.IEcho"
        listenUri="http://localhost/servicemodelsamples/service.svc" />  
```  
  
 Todos os três pontos de extremidade são hospedados ao mesmo tempo `ListenUri` e usam os mesmos `binding` pontos de extremidade, no mesmo `ListenUri` , devem ter a mesma ligação, pois estão compartilhando uma pilha de canal única que escuta mensagens nesse endereço físico no computador. O `address` de cada ponto de extremidade é um urn; embora normalmente endereços representem locais físicos, na verdade, o endereço pode ser qualquer tipo de URI, pois o endereço é usado para fins de correspondência e filtragem, como é demonstrado neste exemplo.  
  
 Como todos os três pontos de extremidade compartilham os mesmos `ListenUri` , quando uma mensagem chega lá, Windows Communication Foundation (WCF) deve decidir para qual ponto de extremidade a mensagem é destinada. Cada ponto de extremidade tem um filtro de mensagem composto por duas partes: o filtro de endereço e o filtro de contrato. O filtro de endereço corresponde ao `To` da mensagem SOAP para o endereço do ponto de extremidade de serviço. Por exemplo, somente as mensagens tratadas `To "Urn:OtherEcho"` são candidatas ao terceiro ponto de extremidade deste serviço. O filtro de contrato corresponde às ações associadas às operações de um contrato específico. Por exemplo, mensagens com a ação de `IEcho` . `Echo`corresponde aos filtros de contrato do segundo e terceiro pontos de extremidade desse serviço, pois ambos os pontos de extremidade hospedam o `IEcho` contrato.  
  
 Assim, a combinação de filtro de endereço e filtro de contrato torna possível rotear cada mensagem que chega a esse serviço `ListenUri` para o ponto de extremidade correto. O terceiro ponto de extremidade é diferenciado dos outros dois porque ele aceita mensagens enviadas a um endereço diferente dos outros pontos de extremidade. O primeiro e o segundo pontos de extremidade são diferenciados uns dos outros com base em seus contratos (a ação da mensagem de entrada).  
  
## <a name="client"></a>Cliente  
 Assim como os pontos de extremidade no servidor têm dois endereços diferentes, os pontos de extremidade do cliente também têm dois endereços. No servidor e no cliente, o endereço lógico é chamado de `EndpointAddress` . Mas, enquanto o endereço físico é chamado de `ListenUri` no servidor, no cliente, o endereço físico é chamado de `Via` .  
  
 Como no servidor, por padrão, esses dois endereços são os mesmos. Para especificar um `Via` no cliente que seja diferente do endereço do ponto de extremidade, `ClientViaBehavior` é usado:  
  
```csharp  
Uri via = new Uri("http://localhost/ServiceModelSamples/service.svc");  
CalculatorClient calcClient = new CalculatorClient();  
calcClient.ChannelFactory.Endpoint.Behaviors.Add(  
        new ClientViaBehavior(via));  
```  
  
 Como de costume, o endereço vem do arquivo de configuração do cliente, que foi gerado pelo svcutil. exe. O `Via` (que corresponde ao `ListenUri` do serviço) não aparece nos metadados do serviço e, portanto, essas informações devem ser comunicadas ao cliente fora de banda (assim como o endereço de metadados do serviço).  
  
 O cliente neste exemplo envia mensagens para cada um dos três pontos de extremidade do aplicativo do servidor, para demonstrar que ele pode se comunicar com (e diferenciar) todos os três pontos de extremidade, mesmo que todos tenham o mesmo `Via` .  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
    > [!NOTE]
    > Para computadores cruzados, você deve substituir localhost no arquivo Client.cs pelo nome do computador de serviço.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\MultipleEndpointsSingleUri`  
