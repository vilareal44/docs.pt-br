---
title: Solicitação-resposta não tipada
ms.date: 03/30/2017
ms.assetid: 0bf0f9d9-7caf-4d3d-8c9e-2d468cca16a5
ms.openlocfilehash: 46047d1671fadb18052991451910b9056015edd2
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84591092"
---
# <a name="untyped-requestreply"></a>Solicitações/Respostas não digitadas
Este exemplo demonstra como definir contratos de operação que usam a classe Message.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 Este exemplo é baseado na [introdução](getting-started-sample.md). O contrato de serviço define uma operação que usa um tipo de mensagem como um argumento e retorna uma mensagem. A operação coleta todos os dados necessários para calcular a soma do corpo da mensagem e, em seguida, envia a soma como corpo na mensagem de retorno.  
  
```csharp
[OperationContract(Action = CalculatorService.RequestAction, ReplyAction = CalculatorService.ReplyAction)]  
Message ComputeSum(Message request);  
```  
  
 No serviço, a operação recupera a matriz de inteiros passada na mensagem de entrada e, em seguida, computa a soma. Para enviar uma mensagem de resposta, o exemplo cria uma nova mensagem com a versão e a ação da mensagem apropriada e adiciona a soma computada como seu corpo. O código de exemplo a seguir demonstra isso.  
  
```csharp
public Message ComputeSum(Message request)  
{  
    //The body of the message contains a list of numbers which will be
    //read as a int[] using GetBody<T>  
    int result = 0;  
  
    int[] inputs = request.GetBody<int[]>();  
    foreach (int i in inputs)  
    {  
        result += i;  
    }  
  
    Message response = Message.CreateMessage(request.Version,
                                      ReplyAction, result);  
    return response;  
}  
```  
  
 O cliente usa o código gerado pela [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) para criar um proxy para o serviço remoto. Para enviar uma mensagem de solicitação, o cliente deve ter a versão da mensagem, que depende do canal subjacente. Portanto, ele cria um novo <xref:System.ServiceModel.OperationContextScope> escopo para o canal de proxy que ele criou, que cria um <xref:System.ServiceModel.OperationContext> com a versão de mensagem correta populada em sua `OutgoingMessageHeaders.MessageVersion` propriedade. O cliente passa uma matriz de entrada como o corpo para a mensagem de solicitação e, em seguida, invoca o `ComputeSum` no proxy. Em seguida, o cliente recupera a soma das entradas passadas acessando o `GetBody<T>` método na mensagem de resposta. O código de exemplo a seguir demonstra isso.  
  
```csharp
using (new OperationContextScope(client.InnerChannel))  
{  
    // Call the Sum service operation.  
    int[] values = { 1, 2, 3, 4, 5 };  
    Message request = Message.CreateMessage(  
        OperationContext.Current.OutgoingMessageHeaders.MessageVersion,
        RequestAction, values);  
    Message reply = client.ComputeSum(request);  
    int response = reply.GetBody<int>();  
  
    Console.WriteLine("Sum of numbers passed (1,2,3,4,5) = {0}",
                                                       response);  
}  
```  
  
 Este exemplo é um exemplo hospedado na Web e, portanto, somente o executável do cliente deve ser executado. A seguir está a saída de exemplo no cliente.  
  
```console  
Prompt>Client.exe  
Sum of numbers passed (1,2,3,4,5) = 15  
  
Press <ENTER> to terminate client.  
```  
  
 Este exemplo é um exemplo hospedado na Web e, portanto, verifique o link fornecido na etapa 3 para ver como criar e executar o exemplo.  
  
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
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Message\Untyped`  
