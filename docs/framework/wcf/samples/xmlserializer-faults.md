---
title: Falhas de XmlSerializer
ms.date: 03/30/2017
ms.assetid: c6b80f14-64f4-4162-ae76-71664cf42fd3
ms.openlocfilehash: ce5aa6d2c579d4776f9505ae694768203e48eecf
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84584058"
---
# <a name="xmlserializer-faults"></a>Falhas de XmlSerializer
O <xref:System.Xml.Serialization.XmlSerializer> exemplo de contrato de falha demonstra como comunicar informações de erro de um serviço para um cliente usando o <xref:System.Xml.Serialization.XmlSerializer> . O exemplo é baseado na [introdução](getting-started-sample.md), com algum código adicional adicionado ao serviço para converter uma exceção interna em uma falha. O cliente tenta executar a divisão por zero para forçar uma condição de erro no serviço.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 O contrato da calculadora foi modificado para incluir um <xref:System.ServiceModel.FaultContractAttribute> , conforme mostrado no código de exemplo a seguir. Além disso, o <xref:System.ServiceModel.XmlSerializerFormatAttribute> é usado para habilitar a serialização usando o <xref:System.Xml.Serialization.XmlSerializer> . A <xref:System.ServiceModel.XmlSerializerFormatAttribute.SupportFaults%2A> propriedade é definida como `true` neste atributo, que instrui o serializador a usar o <xref:System.Xml.Serialization.XmlSerializer> para leitura e gravação de falhas.  
  
```csharp
[XmlSerializerFormat(SupportFaults=true)]  
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface ICalculator  
{  
    [OperationContract]  
    int Add(int n1, int n2);  
  
    [OperationContract]  
    int Subtract(int n1, int n2);  
  
    [OperationContract]  
    int Multiply(int n1, int n2);  
  
    [OperationContract]  
    [FaultContract(typeof(MathFault))]  
    int Divide(int n1, int n2);  
}  
```  
  
 Ao gerar o código para o proxy do cliente, você deve aplicar o sinalizador **/UseSerializerForFaults** à [ferramenta do utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md).  
  
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
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Service\XmlSerializerFaults`  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.XmlSerializerFormatAttribute>
- <xref:System.ServiceModel.XmlSerializerFormatAttribute.SupportFaults%2A>
