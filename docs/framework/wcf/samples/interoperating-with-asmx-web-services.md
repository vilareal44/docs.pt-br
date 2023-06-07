---
title: Interoperabilidade com serviços Web ASMX
ms.date: 03/30/2017
ms.assetid: a7c11f0a-9e68-4f03-a6b1-39cf478d1a89
ms.openlocfilehash: 3f99ba7571c6d84f245b69c5b8f626128ce18627
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596618"
---
# <a name="interoperating-with-asmx-web-services"></a>Interoperabilidade com serviços Web ASMX
Este exemplo demonstra como integrar um aplicativo cliente do Windows Communication Foundation (WCF) com um serviço Web ASMX existente.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 Este exemplo consiste em um programa de console do cliente (. exe) e uma biblioteca de serviços (. dll) hospedados pelo Serviços de Informações da Internet (IIS). O serviço é um serviço Web ASMX que implementa um contrato que define um padrão de comunicação de solicitação-resposta. O serviço expõe operações matemáticas ( `Add` ,, `Subtract` `Multiply` e `Divide` ). O cliente faz solicitações síncronas para uma operação matemática e o serviço responde com o resultado. A atividade do cliente fica visível na janela do console.  
  
 A implementação do serviço Web ASMX mostrada no código de exemplo a seguir calcula e retorna o resultado apropriado.  
  
```csharp  
[WebService(Namespace="http://Microsoft.ServiceModel.Samples")]  
public class CalculatorService : System.Web.Services.WebService  
    {  
        [WebMethod]  
        public double Add(double n1, double n2)  
        {  
            return n1 + n2;  
        }  
        [WebMethod]  
        public double Subtract(double n1, double n2)  
        {  
            return n1 - n2;  
        }  
        [WebMethod]  
        public double Multiply(double n1, double n2)  
        {  
            return n1 * n2;  
        }  
        [WebMethod]  
        public double Divide(double n1, double n2)  
        {  
            return n1 / n2;  
        }  
    }  
```  
  
 Conforme configurado, o serviço pode ser acessado `http://localhost/servicemodelsamples/service.asmx` por um cliente no mesmo computador. Para clientes em computadores remotos acessarem o serviço, um nome de domínio qualificado deve ser especificado em vez de localhost.  
  
 A comunicação é feita por meio de um cliente gerado pela [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md). O cliente está contido no arquivo generatedClient.cs. O serviço ASMX deve estar disponível para gerar o código de proxy, pois ele é usado para recuperar os metadados atualizados. Execute o comando a seguir em um prompt de comando no diretório do cliente para gerar o proxy de tipo.  
  
```console  
svcutil.exe /n:http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples http://localhost/servicemodelsamples/service.svc?wsdl /out:generatedClient.cs  
```  
  
 Usando o cliente gerado, você pode acessar um ponto de extremidade de serviço Configurando o endereço e a associação apropriados. Assim como o serviço, o cliente usa um arquivo de configuração (App. config) para especificar o ponto de extremidade com o qual se comunicar. A configuração de ponto de extremidade do cliente consiste em um endereço absoluto para o ponto de extremidade de serviço, a associação e o contrato, conforme mostrado na seguinte configuração de exemplo.  
  
```xml  
<client>  
   <endpoint
      address="http://localhost/ServiceModelSamples/service.asmx"
      binding="basicHttpBinding"
      contract="Microsoft.ServiceModel.Samples.CalculatorServiceSoap" />  
</client>  
```  
  
 A implementação do cliente constrói uma instância do cliente gerado. O cliente gerado pode então ser usado para se comunicar com o serviço.  
  
```csharp  
// Create a client.  
CalculatorServiceSoapClient client = new CalculatorServiceSoapClient();  
  
// Call the Add service operation.  
double value1 = 100.00D;  
double value2 = 15.99D;  
double result = client.Add(value1, value2);  
Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);  
  
// Call the Subtract service operation.  
value1 = 145.00D;  
value2 = 76.54D;  
result = client.Subtract(value1, value2);  
Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);  
  
// Call the Multiply service operation.  
value1 = 9.00D;  
value2 = 81.25D;  
result = client.Multiply(value1, value2);  
Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);  
  
// Call the Divide service operation.  
value1 = 22.00D;  
value2 = 7.00D;  
result = client.Divide(value1, value2);  
Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);  
  
//Closing the client gracefully closes the connection and cleans up resources.  
client.Close();  
  
Console.WriteLine();  
Console.WriteLine("Press <ENTER> to terminate client.");  
Console.ReadLine();  
```  
  
 Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.  
  
```console
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
  
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
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Client\Interop\ASMX`  
