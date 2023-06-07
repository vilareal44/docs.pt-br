---
title: Codificação de MTOM
ms.date: 03/30/2017
ms.assetid: 820e316f-4ee1-4eb5-ae38-b6a536e8a14f
ms.openlocfilehash: cf048e1e6b2e2785accc1bde0336f07e3d84ae5e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602525"
---
# <a name="mtom-encoding"></a>Codificação de MTOM
Este exemplo demonstra o uso da codificação de mensagens do mecanismo de otimização de transmissão de mensagens (MTOM) com uma WSHttpBinding. MTOM é um mecanismo para transmitir anexos binários grandes com mensagens SOAP como bytes brutos, permitindo mensagens menores.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\WS\MTOM`  
  
 Por padrão, o WSHttpBinding envia e recebe mensagens como XML de texto normal. Para habilitar o envio e o recebimento de mensagens MTOM, defina o `messageEncoding` atributo na configuração da Associação (como no código de exemplo a seguir) ou diretamente na associação usando a `MessageEncoding` propriedade. O serviço ou cliente agora pode enviar e receber mensagens MTOM.  
  
```xml  
<wsHttpBinding>  
  <binding name="WSHttpBinding_IUpload" messageEncoding="Mtom" />  
</wsHttpBinding>  
```  
  
 O codificador MTOM pode otimizar matrizes de bytes e fluxos. Neste exemplo, a operação usa um `Stream` parâmetro e, portanto, pode ser otimizada.  

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
  public interface IUpload  
  {  
      [OperationContract]  
      int Upload(Stream data);  
  }  
```
  
 O contrato escolhido para este exemplo transmite dados binários para o serviço e recebe o número de bytes carregados como o valor de retorno. Quando o serviço é instalado e o cliente é executado, ele imprime o número 1000, que indica que todos os 1000 bytes foram recebidos. O restante da saída lista os tamanhos de mensagem otimizados e não otimizados para várias cargas.  
  
```console
Output:  
1000  
  
Text encoding with a 100 byte payload: 433  
MTOM encoding with a 100 byte payload: 912  
  
Text encoding with a 1000 byte payload: 1633  
MTOM encoding with a 1000 byte payload: 2080  
  
Text encoding with a 10000 byte payload: 13633  
MTOM encoding with a 10000 byte payload: 11080  
  
Text encoding with a 100000 byte payload: 133633  
MTOM encoding with a 100000 byte payload: 101080  
  
Text encoding with a 1000000 byte payload: 1333633  
MTOM encoding with a 1000000 byte payload: 1001080  
  
Press <ENTER> to terminate client.  
```  
  
 A finalidade de usar o MTOM é otimizar a transmissão de grandes cargas binárias. O envio de uma mensagem SOAP usando MTOM tem uma sobrecarga perceptível para pequenos conteúdos binários, mas se torna uma grande economia quando eles crescem em poucos milhares de bytes. O motivo disso é que o XML de texto normal codifica dados binários usando base64, que requer quatro caracteres para cada três bytes e aumenta o tamanho dos dados em um terço. O MTOM é capaz de transmitir dados binários como bytes brutos, salvar o tempo de codificação/decodificação e resultar em mensagens menores. O limite de alguns mil bytes é pequeno quando comparado aos documentos de negócios atuais e às fotografias digitais.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Instale o ASP.NET 4,0 usando o comando a seguir.  
  
    ```console
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
    ```  
  
2. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
3. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
4. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
