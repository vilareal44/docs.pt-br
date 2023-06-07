---
title: Exemplo de XmlReader
ms.date: 03/30/2017
helpviewer_keywords:
- XML Reader
ms.assetid: 60e5848d-7d9c-4ea5-bed9-22758c9ac16c
ms.openlocfilehash: 470a3ad0d3241e51676928b77dd93e5d31249515
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594810"
---
# <a name="xmlreader-sample"></a>Exemplo de XmlReader

O exemplo de XmlReader demonstra o processamento de um corpo de mensagem usando um <xref:System.Xml.XmlReader> . O exemplo se baseia na [introdução](getting-started-sample.md), que implementa um serviço de calculadora. Uma operação de serviço adicional, `Sum` , foi adicionada que aceita uma mensagem que contém uma matriz de valores para adicionar juntos. O serviço lê a mensagem usando um <xref:System.Xml.XmlReader> .

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

A interface da calculadora inclui uma operação de serviço chamada `Sum` que aceita um <xref:System.ServiceModel.Channels.Message> parâmetro, conforme mostrado no código de exemplo a seguir.

```csharp
public interface ICalculator
{
    [OperationContract]
    double Add(double n1, double n2);
    [OperationContract]
    double Subtract(double n1, double n2);
    [OperationContract]
    double Multiply(double n1, double n2);
    [OperationContract]
    double Divide(double n1, double n2);
    [OperationContract]
    Message Sum(Message message);
}
```

O cliente acessa `Sum` primeiramente criando uma matriz de valores inteiros, criando uma mensagem da matriz e, em seguida, chamando o `Sum` método usando a mensagem criada, conforme mostrado no código de exemplo a seguir.

```csharp
CalculatorClient client = new CalculatorClient();
//...

// Call the Sum service operation.
int[] values = { 1, 2, 3, 4, 5 };
using (new OperationContextScope(client.InnerChannel))
{
    Message request = Message.CreateMessage(OperationContext.Current.OutgoingMessageHeaders.MessageVersion, "http://Microsoft.ServiceModel.Samples/ICalculator/Sum", values);
    Message reply = client.Sum(request);
    int sum = reply.GetBody<int>();

    Console.WriteLine("Sum(1,2,3,4,5) = {0}", sum);
}
```

No serviço, a implementação da operação de serviço `Sum` acessa o corpo da mensagem usando um <xref:System.Xml.XmlReader> objeto para iterar pelos valores a serem somados. O <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents%2A> método é chamado para acessar o corpo da mensagem, conforme mostrado no código de exemplo a seguir.

```csharp
public int Sum(Message message)
{
    int sum = 0;
    string text = "";

    //The body of the message contains a list of numbers that are read
    //directly using an XmlReader.
    XmlReader body = message.GetReaderAtBodyContents ();
    while (body.Read())
    {
        text = body.ReadString().Trim();
        if (text.Length>0)
        {
            sum += Convert.ToInt32(text);
        }
    }
    body.Close();
    Message response = Message.CreateMessage(
       "http://Microsoft.ServiceModel.Samples/ICalculator/SumResponse",
       sum);
    return response;
}
```

Quando você executa o exemplo, as solicitações e respostas da operação são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.

```console
Add(100,15.99) = 115.99
Subtract(145,76.54) = 68.46
Multiply(9,81.25) = 731.25
Divide(22,7) = 3.14285714285714
Sum(1,2,3,4,5) = 15

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
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Message\XmlReader`
