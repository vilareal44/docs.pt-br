---
title: 'Padrões de design: publicação-assinatura baseada em lista'
ms.date: 03/30/2017
ms.assetid: f4257abc-12df-4736-a03b-0731becf0fd4
ms.openlocfilehash: 5aaea5b0def544f8b3e963d71e269592b9b40784
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84592340"
---
# <a name="design-patterns-list-based-publish-subscribe"></a>Padrões de design: publicação-assinatura baseada em lista
Este exemplo ilustra o padrão de publicação-assinatura baseado em lista implementado como um programa Windows Communication Foundation (WCF).  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 O padrão de design de publicação-assinatura com base em lista é descrito na publicação de práticas de & de padrões da Microsoft, [padrões de integração](https://docs.microsoft.com/previous-versions/msp-n-p/ff647309(v=pandp.10)). O padrão de publicação/assinatura passa informações para uma coleção de destinatários que assinaram um tópico de informações. Publicação com base em lista-assinatura mantém uma lista de assinantes. Quando há informações a serem compartilhadas, uma cópia é enviada para cada Assinante na lista. Este exemplo demonstra um padrão de publicação-assinatura com base em lista dinâmica, em que os clientes podem assinar ou cancelar a assinatura com a frequência necessária.  
  
 O exemplo de publicação-assinatura com base em lista consiste em um cliente, um serviço e um programa de fonte de dados. Pode haver mais de um cliente e mais de um programa de fonte de dados em execução. Os clientes assinam o serviço, recebem notificações e cancelam a assinatura. Os programas de fonte de dados enviam informações para o serviço a serem compartilhados com todos os assinantes atuais.  
  
 Neste exemplo, o cliente e a fonte de dados são programas de console (arquivos. exe) e o serviço é uma biblioteca (. dll) hospedada no Serviços de Informações da Internet (IIS). A atividade cliente e fonte de dados fica visível na área de trabalho.  
  
 O serviço usa a comunicação duplex. O `ISampleContract` contrato de serviço é emparelhado com um `ISampleClientCallback` contrato de retorno de chamada. O serviço implementa as operações de inscrição e cancelamento de assinatura do serviço, que os clientes usam para ingressar ou sair da lista de assinantes. O serviço também implementa a `PublishPriceChange` operação de serviço, que o programa de fonte de dados chama para fornecer o serviço com novas informações. O programa cliente implementa a `PriceChange` operação de serviço, que o serviço chama para notificar todos os assinantes de uma alteração de preço.  
  
```csharp  
// Create a service contract and define the service operations.  
// NOTE: The service operations must be declared explicitly.  
[ServiceContract(SessionMode=SessionMode.Required,  
      CallbackContract=typeof(ISampleClientContract))]  
public interface ISampleContract  
{  
    [OperationContract(IsOneWay = false, IsInitiating=true)]  
    void Subscribe();  
    [OperationContract(IsOneWay = false, IsTerminating=true)]  
    void Unsubscribe();  
    [OperationContract(IsOneWay = true)]  
    void PublishPriceChange(string item, double price,
                                     double change);  
}  
  
public interface ISampleClientContract  
{  
    [OperationContract(IsOneWay = true)]  
    void PriceChange(string item, double price, double change);  
}  
```  
  
 O serviço usa um evento de .NET Framework como o mecanismo para informar todos os assinantes sobre novas informações. Quando um cliente ingressa no serviço chamando Subscribe, ele fornece um manipulador de eventos. Quando um cliente sai, ele cancela a assinatura de seu manipulador de eventos do evento. Quando uma fonte de dados chama o serviço para relatar uma alteração de preço, o serviço gera o evento. Isso chama cada instância do serviço, uma para cada cliente que assinou e faz com que seus manipuladores de eventos sejam executados. Cada manipulador de eventos passa as informações para seu cliente por meio de sua função de retorno de chamada.  
  
```csharp
public class PriceChangeEventArgs : EventArgs  
    {  
        public string Item;  
        public double Price;  
        public double Change;  
    }  
  
    // The Service implementation implements your service contract.  
    [ServiceBehavior(InstanceContextMode=InstanceContextMode.PerSession)]  
    public class SampleService : ISampleContract  
    {  
        public static event PriceChangeEventHandler PriceChangeEvent;  
        public delegate void PriceChangeEventHandler(object sender, PriceChangeEventArgs e);  
  
        ISampleClientContract callback = null;  
  
        PriceChangeEventHandler priceChangeHandler = null;  
  
        //Clients call this service operation to subscribe.  
        //A price change event handler is registered for this client instance.  
  
        public void Subscribe()  
        {  
            callback = OperationContext.Current.GetCallbackChannel<ISampleClientContract>();  
            priceChangeHandler = new PriceChangeEventHandler(PriceChangeHandler);  
            PriceChangeEvent += priceChangeHandler;  
        }  
  
        //Clients call this service operation to unsubscribe.  
        //The previous price change event handler is unregistered.  
  
        public void Unsubscribe()  
        {  
            PriceChangeEvent -= priceChangeHandler;  
        }  
  
        //Information source clients call this service operation to report a price change.  
        //A price change event is raised. The price change event handlers for each subscriber will execute.  
  
        public void PublishPriceChange(string item, double price, double change)  
        {  
            PriceChangeEventArgs e = new PriceChangeEventArgs();  
            e.Item = item;  
            e.Price = price;  
            e.Change = change;  
            PriceChangeEvent(this, e);  
        }  
  
        //This event handler runs when a PriceChange event is raised.  
        //The client's PriceChange service operation is invoked to provide notification about the price change.  
  
        public void PriceChangeHandler(object sender, PriceChangeEventArgs e)  
        {  
            callback.PriceChange(e.Item, e.Price, e.Change);  
        }  
  
    }  
```  
  
 Ao executar o exemplo, inicie vários clientes. Os clientes assinam o serviço. Em seguida, execute o programa de fonte de dados, que envia informações para o serviço. O serviço passa as informações para todos os assinantes. Você pode ver a atividade em cada console do cliente confirmando que as informações foram recebidas. Pressione ENTER na janela do cliente para desligar o cliente.  
  
### <a name="to-set-up-and-build-the-sample"></a>Para configurar e compilar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
### <a name="to-run-the-sample-on-the-same-machine"></a>Para executar o exemplo no mesmo computador  
  
1. Teste se você pode acessar o serviço usando um navegador digitando o seguinte endereço: `http://localhost/servicemodelsamples/service.svc` . Uma página de confirmação deve ser exibida em resposta.  
  
2. Execute Client. exe de \client\bin \\ , de dentro da pasta específica do idioma. A atividade do cliente é exibida na janela do console do cliente. Inicie vários clientes.  
  
3. Execute DataSource. exe de \datasource\bin \\ , de dentro da pasta específica do idioma. A atividade de fonte de dados é exibida na janela do console. Depois que a fonte de dados envia informações para o serviço, elas devem ser passadas para cada cliente.  
  
4. Se o cliente, a fonte de dados e os programas de serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
### <a name="to-run-the-sample-across-machines"></a>Para executar o exemplo entre computadores  
  
1. Configurar a máquina de serviço:  
  
    1. Na máquina de serviço, crie um diretório virtual chamado ServiceModelSamples. O arquivo em lotes Setupvroot. bat do [procedimento de instalação única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md) pode ser usado para criar o diretório de disco e o diretório virtual.  
  
    2. Copie os arquivos de programa do serviço de%SystemDrive%\Inetpub\wwwroot\servicemodelsamples para o diretório virtual ServiceModelSamples no computador de serviço. Certifique-se de incluir os arquivos no diretório \bin.  
  
    3. Teste se você pode acessar o serviço do computador cliente usando um navegador.  
  
2. Configurar os computadores cliente:  
  
    1. Copie os arquivos de programas do cliente da pasta \client\bin\, na pasta específica do idioma, para os computadores cliente.  
  
    2. Em cada arquivo de configuração de cliente, altere o valor de endereço da definição do ponto de extremidade para corresponder ao novo endereço do serviço. Substitua todas as referências a "localhost" por um nome de domínio totalmente qualificado no endereço.  
  
3. Configurar o computador de fonte de dados:  
  
    1. Copie os arquivos de programa da fonte de dados da pasta \datasource\bin\, na pasta específica do idioma, para a máquina de fonte de dados.  
  
    2. No arquivo de configuração da fonte de dados, altere o valor do endereço da definição do ponto de extremidade para corresponder ao novo endereço do serviço. Substitua todas as referências a "localhost" por um nome de domínio totalmente qualificado no endereço.  
  
4. Nos computadores cliente, inicie o Client. exe em um prompt de comando.  
  
5. Na máquina de fonte de dados, inicie o DataSource. exe em um prompt de comando.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Scenario\DesignPatterns/ListBasedPublishSubscribe`  
