---
title: SRMP
ms.date: 03/30/2017
ms.assetid: cf37078c-dcb4-45e0-acaf-2f196521b226
ms.openlocfilehash: f3b0e57f05ccb77eef25c97e7d5d028183e7b13e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600926"
---
# <a name="srmp"></a>SRMP
Este exemplo demonstra como executar a comunicação em fila transacionada usando o MSMQ (enfileiramento de mensagens) via HTTP.  
  
 Na comunicação em fila, o cliente se comunica com o serviço usando uma fila. Mais precisamente, o cliente envia mensagens para uma fila. O serviço recebe mensagens da fila. O serviço e o cliente, portanto, não precisam estar em execução ao mesmo tempo para se comunicarem usando uma fila.  
  
 O MSMQ permite o uso de HTTP (incluindo o uso de HTTPS) para enviar mensagens a uma fila. Neste exemplo, demonstramos o uso da comunicação em fila do Windows Communication Foundation (WCF) e como enviar mensagens por HTTP. O MSMQ usa um protocolo chamado SRMP, que é um protocolo baseado em SOAP para comunicação via HTTP.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
4. Antes de executar o exemplo em **Adicionar/remover componentes do Windows**, verifique se o MSMQ está instalado com o suporte a http. A instalação do suporte a HTTP instala automaticamente Serviços de Informações da Internet (IIS) e adiciona o suporte de protocolo no IIS para MSMQ.  
  
5. Se você quiser ter certeza de que o HTTP é usado para comunicação, você pode habilitar o MSMQ para ser executado no modo protegido. Isso garante que nenhuma mensagem para qualquer fila hospedada no computador possa chegar usando qualquer transporte não HTTP.  
  
6. Depois de selecionar o MSMQ para execução no modo protegido, o computador requer uma reinicialização no Windows Server 2003.  
  
7. Executar o serviço.  
  
8. Execute o cliente. Certifique-se de alterar o endereço do ponto de extremidade para apontar para o nome do computador ou endereço IP em vez de localhost. O cliente envia uma mensagem e sai.  
  
## <a name="requirements"></a>Requisitos  
 Para executar este exemplo, o IIS deve ser instalado no serviço e nos computadores cliente, além do MSMQ.  
  
## <a name="demonstrates"></a>Demonstra  
 O exemplo demonstra o envio de mensagens em fila do WCF usando o MSMQ sobre HTTP. Isso também é chamado de sistema de mensagens SRMP. Quando uma mensagem em fila é enviada, o MSMQ no computador de envio transfere as mensagens para o Gerenciador de filas de recebimento por transporte TCP ou HTTP. Escolhendo SRMP, o usuário indica a opção de HTTP como um transporte para transferência de fila. SRMP Secure permite o uso de HTTPS.  
  
## <a name="example"></a>Exemplo  
 O código de exemplo é baseado no exemplo transacionado. Como você envia uma mensagem para a fila e recebe uma mensagem da fila usando SRMP é o mesmo que enviar e receber mensagens usando um protocolo nativo.  
  
 A configuração do cliente é alterada para indicar a escolha do protocolo de transferência de fila. O protocolo de transferência de fila pode ser nativo, SRMP ou SrmpSecure. Por padrão, o protocolo de transferência é nativo. O cliente e o serviço especificam na configuração para usar SRMP neste exemplo.  
  
 Há limitações para SRMP em relação à segurança de transporte. A segurança de transporte MSMQ padrão requer Active Directory que exija que o Gerenciador de filas de envio e o Gerenciador de filas de recebimento residam no mesmo domínio do Windows. Isso não é possível ao enviar mensagens por limite HTTP. Como tal, a segurança de transporte padrão não funciona. A segurança de transporte deverá ser definida como certificado se a segurança de transporte for desejada. A segurança da mensagem também pode ser usada para proteger a mensagem. Neste exemplo, a segurança de transporte e de mensagem é desativada para ilustrar mensagens SRMP.  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
  
  <system.serviceModel>  
  
    <client>  
      <!-- Define NetMsmqEndpoint -->  
      <endpoint name="OrderProcessorEndpoint"  
           address=  
          "net.msmq://localhost/private/ServiceModelSamplesSrmp"
           bindingConfiguration="srmpBinding"
           binding="netMsmqBinding"
           contract="IOrderProcessor" />  
    </client>  
    <bindings>  
      <netMsmqBinding>  
        <binding name="srmpBinding"  
                 queueTransferProtocol="Srmp">  
          <security mode="None" />  
        </binding>  
      </netMsmqBinding>  
    </bindings>  
  </system.serviceModel>  
  
</configuration>  
```  
  
 A execução da amostra produz a saída a seguir.  
  
```console  
Processing Purchase Order: 556b70be-31ee-4a3b-8df4-ed5e538015a4
Customer: somecustomer.com
OrderDetails
    Order LineItem: 54 of Blue Widget @unit price: $29.99
    Order LineItem: 890 of Red Widget @unit price: $45.89
    Total cost of this order: $42461.56
    Order status: Pending  
```  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Net\MSMQ\SRMP`  
