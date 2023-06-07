---
title: Exemplo de DataContractSerializer
description: Este exemplo demonstra o DataContractSerializer no WCF, que executa os serviços de serialização e desserialização gerais para as classes de contrato de dados.
ms.date: 03/30/2017
helpviewer_keywords:
- XML Formatter
ms.assetid: e0a2fe89-3534-48c8-aa3c-819862224571
ms.openlocfilehash: c2f62c8926f09e2d4cdea1941909e7d8f59c43a0
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244394"
---
# <a name="datacontractserializer-sample"></a>Exemplo de DataContractSerializer
O exemplo DataContractSerializer demonstra o <xref:System.Runtime.Serialization.DataContractSerializer> , que executa os serviços de serialização e desserialização gerais para as classes de contrato de dados. O exemplo cria um `Record` objeto, serializa-o para um fluxo de memória e desserializa o fluxo de memória de volta para outro `Record` objeto para demonstrar o uso do <xref:System.Runtime.Serialization.DataContractSerializer> . Em seguida, o exemplo serializa o `Record` objeto usando um gravador binário para demonstrar como o gravador afeta a serialização.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 O contrato de dados do `Record` é mostrado no código de exemplo a seguir.  
  
```csharp  
[DataContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
internal class Record  
{  
    private double n1;  
    private double n2;  
    private string operation;  
    private double result;  
  
    internal Record(double n1, double n2, string operation, double result)  
    {  
        this.n1 = n1;  
        this.n2 = n2;  
        this.operation = operation;  
        this.result = result;  
    }  
  
    [DataMember]  
    internal double OperandNumberOne  
    {  
        get { return n1; }  
        set { n1 = value; }  
    }  
  
    [DataMember]  
    internal double OperandNumberTwo  
    {  
        get { return n2; }  
        set { n2 = value; }  
    }  
  
    [DataMember]  
    internal string Operation  
    {  
        get { return operation; }  
        set { operation = value; }  
    }  
  
    [DataMember]  
    internal double Result  
    {  
        get { return result; }  
        set { result = value; }  
    }  
  
    public override string ToString()  
    {  
        return $"Record: {n1} {operation} {n2} = {result}";
    }  
}  
```  
  
 O código de exemplo cria um `Record` objeto chamado `record1` e, em seguida, exibe o objeto.  
  
```csharp
Record record1 = new Record(1, 2, "+", 3);  
Console.WriteLine("Original record: {0}", record1.ToString());  
```  
  
 Em seguida, o exemplo usa o <xref:System.Runtime.Serialization.DataContractSerializer> para serializar `record1` em um fluxo de memória.  
  
```csharp  
MemoryStream stream1 = new MemoryStream();  
  
//Serialize the Record object to a memory stream using DataContractSerializer.  
DataContractSerializer serializer = new DataContractSerializer(typeof(Record));  
serializer.WriteObject(stream1, record1);  
```  
  
 Em seguida, o exemplo usa o <xref:System.Runtime.Serialization.DataContractSerializer> para desserializar o fluxo de memória de volta para um novo `Record` objeto e exibi-lo.  
  
```csharp  
stream1.Position = 0;  
  
//Deserialize the Record object back into a new record object.  
Record record2 = (Record)serializer.ReadObject(stream1);  
  
Console.WriteLine("Deserialized record: {0}", record2.ToString());  
```  
  
 Por padrão, o `DataContractSerializer` codifica objetos em um fluxo usando uma representação textual de XML. No entanto, você pode influenciar a codificação do XML passando um gravador diferente. O exemplo cria um gravador binário chamando <xref:System.Xml.XmlDictionaryWriter.CreateBinaryWriter%2A> . Em seguida, ele passa o gravador e o objeto de registro para o serializador quando ele chama <xref:System.Runtime.Serialization.DataContractSerializer.WriteObjectContent%2A> . Por fim, a amostra libera o gravador e os relatórios sobre o comprimento dos fluxos.  
  
```csharp  
MemoryStream stream2 = new MemoryStream();  
  
XmlDictionaryWriter binaryDictionaryWriter = XmlDictionaryWriter.CreateBinaryWriter(stream2);  
serializer.WriteObject(binaryDictionaryWriter, record1);  
binaryDictionaryWriter.Flush();  
  
//report the length of the streams  
Console.WriteLine("Text Stream is {0} bytes long", stream1.Length);  
Console.WriteLine("Binary Stream is {0} bytes long", stream2.Length);  
```  
  
 Quando você executa o exemplo, o registro original e o registro desserializado são exibidos, seguidos pela comparação entre o comprimento da codificação de texto e a codificação binária. Pressione ENTER na janela do cliente para desligar o cliente.  
  
```console  
Original record: Record: 1 + 2 = 3  
Deserialized record: Record: 1 + 2 = 3  
Text Stream is 233 bytes long  
Binary Stream is 156 bytes long  
  
Press <ENTER> to terminate client.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo, inicie o cliente no prompt de comando digitando client\bin\client.exe.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Data\DataContractSerializer`  
