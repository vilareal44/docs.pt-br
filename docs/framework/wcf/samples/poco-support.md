---
title: Suporte para POCO
ms.date: 03/30/2017
ms.assetid: 3846ca73-2819-4ca2-8367-dc739dde5a5b
ms.openlocfilehash: a9f8d185c58b22e68f7a8c11954e0e534c4bd48f
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600458"
---
# <a name="poco-support"></a>Suporte para POCO
Este exemplo demonstra o suporte de serialização para tipos desmarcados; ou seja, tipos para os quais os atributos de serialização não foram aplicados, às vezes chamados de tipos POCO (objeto CLR antigo). O <xref:System.Runtime.Serialization.DataContractSerializer> infere um contrato de dados para todos os tipos públicos não marcados que têm um construtor sem parâmetros. Os contratos de dados permitem que você transmita dados estruturados de e para serviços. Para obter mais informações sobre tipos desmarcados, consulte [tipos serializáveis](../feature-details/serializable-types.md).  
  
 Este exemplo é baseado na [introdução](getting-started-sample.md), mas usa números complexos em vez de tipos numéricos primitivos. Ele também é semelhante ao exemplo de [contrato de dados básico](basic-data-contract.md) , exceto que <xref:System.Runtime.Serialization.DataContractAttribute> os <xref:System.Runtime.Serialization.DataMemberAttribute> atributos e não são usados.  
  
 O serviço é hospedado pelo Serviços de Informações da Internet (IIS) e o cliente é um aplicativo de console (. exe).  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 A `ComplexNumber` classe é usada no `ServiceContract` . O `ComplexNumber` tipo não tem os <xref:System.Runtime.Serialization.DataContractAttribute> atributos e <xref:System.Runtime.Serialization.DataMemberAttribute> , conforme mostrado no código de exemplo a seguir. Por padrão, todas as propriedades públicas e campos são serializados.  
  
```csharp
public class ComplexNumber  
{  
    public double Real;  
    public double Imaginary;  
    public ComplexNumber()  
    {  
        Real = double.MinValue;  
        Imaginary = double.MinValue;  
    }  
    public ComplexNumber(double real, double imaginary)  
    {  
        this.Real = real;  
        this.Imaginary = imaginary;  
    }  
}  
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
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Data\POCO`  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.Serialization.IgnoreDataMemberAttribute>
- [Tipos serializáveis](../feature-details/serializable-types.md)
