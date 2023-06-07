---
title: Como criar um serviço que aceita dados arbitrários usando o modelo de programação WCF REST
ms.date: 03/30/2017
ms.assetid: e566c15a-b600-4e4a-be3a-4af43e767dae
ms.openlocfilehash: d908651f7815c102b45ea106f5bec4c07d869950
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601329"
---
# <a name="how-to-create-a-service-that-accepts-arbitrary-data-using-the-wcf-rest-programming-model"></a>Como criar um serviço que aceita dados arbitrários usando o modelo de programação WCF REST
Às vezes, os desenvolvedores devem ter controle total de como os dados são retornados de uma operação de serviço. Esse é o caso quando uma operação de serviço deve retornar dados em um formato sem suporte byWCF. Este tópico discute o uso do modelo de programação REST do WCF para criar um serviço que recebe dados arbitrários.  
  
### <a name="to-implement-the-service-contract"></a>Para implementar o contrato de serviço  
  
1. Defina o contrato de serviço. A operação que recebe os dados arbitrários deve ter um parâmetro do tipo <xref:System.IO.Stream> . Além disso, esse parâmetro deve ser o único parâmetro passado no corpo da solicitação. A operação descrita neste exemplo também usa um parâmetro filename. Esse parâmetro é passado dentro da URL da solicitação. Você pode especificar que um parâmetro seja passado dentro da URL especificando um <xref:System.UriTemplate> no <xref:System.ServiceModel.Web.WebInvokeAttribute> . Nesse caso, o URI usado para chamar esse método termina em "UploadFile/some-filename". A parte "{filename}" do modelo de URI especifica que o parâmetro filename para a operação é passado dentro do URI usado para chamar a operação.  
  
    ```csharp  
     [ServiceContract]  
    public interface IReceiveData  
    {  
        [WebInvoke(UriTemplate = "UploadFile/{fileName}")]  
        void UploadFile(string fileName, Stream fileContents);  
    }  
    ```  
  
2. Implemente o contrato de serviço. O contrato tem apenas um método, `UploadFile` que recebe um arquivo de dados arbitrários em um fluxo. A operação lê o fluxo contando o número de bytes lidos e, em seguida, exibe o nome do arquivo e o número de bytes lidos.  
  
    ```csharp  
    public class RawDataService : IReceiveData  
    {  
        public void UploadFile(string fileName, Stream fileContents)  
        {  
            byte[] buffer = new byte[10000];  
            int bytesRead, totalBytesRead = 0;  
            do  
            {  
                bytesRead = fileContents.Read(buffer, 0, buffer.Length);  
                totalBytesRead += bytesRead;  
            } while (bytesRead > 0);  
            Console.WriteLine("Service: Received file {0} with {1} bytes", fileName, totalBytesRead);  
        }  
    }  
    ```  
  
### <a name="to-host-the-service"></a>Para hospedar o serviço  
  
1. Crie um aplicativo de console para hospedar o serviço.  
  
    ```csharp  
    class Program  
    {  
       static void Main(string[] args)  
       {  
       }  
    }  
    ```  
  
2. Crie uma variável para conter o endereço base para o serviço dentro do `Main` método.  
  
    ```csharp  
    string baseAddress = "http://" + Environment.MachineName + ":8000/Service";  
    ```  
  
3. Crie uma <xref:System.ServiceModel.ServiceHost> instância para o serviço que especifica a classe de serviço e o endereço base.  
  
    ```csharp  
    ServiceHost host = new ServiceHost(typeof(RawDataService), new Uri(baseAddress));  
    ```  
  
4. Adicione um ponto de extremidade que especifique o contrato, <xref:System.ServiceModel.WebHttpBinding> e <xref:System.ServiceModel.Description.WebHttpBehavior> .  
  
    ```csharp  
    host.AddServiceEndpoint(typeof(IReceiveData), new WebHttpBinding(), "").Behaviors.Add(new WebHttpBehavior());  
    ```  
  
5. Abra o host do serviço. O serviço agora está pronto para receber solicitações.  
  
    ```csharp  
    host.Open();  
    Console.WriteLine("Host opened");  
    ```  
  
### <a name="to-call-the-service-programmatically"></a>Para chamar o serviço programaticamente  
  
1. Crie um <xref:System.Net.HttpWebRequest> com o URI usado para chamar o serviço. Nesse código, o endereço base é combinado com `"/UploadFile/Text"` . A `"UploadFile"` parte do URI especifica a operação a ser chamada. A `"Test.txt"` parte do URI especifica o parâmetro filename a ser passado para a `UploadFile` operação. Ambos os itens são mapeados para o <xref:System.UriTemplate> contrato de operação aplicado.  
  
    ```csharp  
    HttpWebRequest req = (HttpWebRequest)HttpWebRequest.Create(baseAddress + "/UploadFile/Test.txt");  
    ```  
  
2. Defina a <xref:System.Net.HttpWebRequest.Method%2A> propriedade de <xref:System.Net.HttpWebRequest> para `POST` e a <xref:System.Net.HttpWebRequest.ContentType%2A> propriedade como `"text/plain"` . Isso informa ao serviço que o código está enviando dados e que os dados estão em texto sem formatação.  
  
    ```csharp  
    req.Method = "POST";  
    req.ContentType = "text/plain";  
    ```  
  
3. Chame <xref:System.Net.HttpWebRequest.GetRequestStream%2A> para obter o fluxo de solicitação, crie os dados a serem enviados, grave esses dados no fluxo de solicitação e feche o fluxo.  
  
    ```csharp  
    Stream reqStream = req.GetRequestStream();  
    byte[] fileToSend = new byte[12345];  
    for (int i = 0; i < fileToSend.Length; i++)  
       {  
           fileToSend[i] = (byte)('a' + (i % 26));  
       }  
    reqStream.Write(fileToSend, 0, fileToSend.Length);  
    reqStream.Close();  
    ```  
  
4. Obtenha a resposta do serviço chamando <xref:System.Net.HttpWebRequest.GetResponse%2A> e exiba os dados de resposta para o console.  
  
    ```csharp  
    HttpWebResponse resp = (HttpWebResponse)req.GetResponse();  
    Console.WriteLine("Client: Receive Response HTTP/{0} {1} {2}", resp.ProtocolVersion, (int)resp.StatusCode, resp.StatusDescription);  
    ```  
  
5. Feche o host de serviço.  
  
    ```csharp  
    host.Close();  
    ```  
  
## <a name="example"></a>Exemplo  
 A seguir está uma lista completa do código para este exemplo.  
  
```csharp  
using System;  
using System.Collections.Generic;  
using System.Text;  
using System.ServiceModel;  
using System.ServiceModel.Web;  
using System.ServiceModel.Description;  
using System.IO;  
using System.Net;  
  
namespace ReceiveRawData  
{  
    [ServiceContract]  
    public interface IReceiveData  
    {  
        [WebInvoke(UriTemplate = "UploadFile/{fileName}")]  
        void UploadFile(string fileName, Stream fileContents);  
    }  
    public class RawDataService : IReceiveData  
    {  
        public void UploadFile(string fileName, Stream fileContents)  
        {  
            byte[] buffer = new byte[10000];  
            int bytesRead, totalBytesRead = 0;  
            do  
            {  
                bytesRead = fileContents.Read(buffer, 0, buffer.Length);  
                totalBytesRead += bytesRead;  
            } while (bytesRead > 0);  
            Console.WriteLine("Service: Received file {0} with {1} bytes", fileName, totalBytesRead);  
        }  
    }  
  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            string baseAddress = "http://" + Environment.MachineName + ":8000/Service";  
            ServiceHost host = new ServiceHost(typeof(RawDataService), new Uri(baseAddress));  
            host.AddServiceEndpoint(typeof(IReceiveData), new WebHttpBinding(), "").Behaviors.Add(new WebHttpBehavior());  
            host.Open();  
            Console.WriteLine("Host opened");  
  
            HttpWebRequest req = (HttpWebRequest)HttpWebRequest.Create(baseAddress + "/UploadFile/Test.txt");  
            req.Method = "POST";  
            req.ContentType = "text/plain";  
            Stream reqStream = req.GetRequestStream();  
            byte[] fileToSend = new byte[12345];  
            for (int i = 0; i < fileToSend.Length; i++)  
            {  
                fileToSend[i] = (byte)('a' + (i % 26));  
            }  
            reqStream.Write(fileToSend, 0, fileToSend.Length);  
            reqStream.Close();  
            HttpWebResponse resp = (HttpWebResponse)req.GetResponse();  
            Console.WriteLine("Client: Receive Response HTTP/{0} {1} {2}", resp.ProtocolVersion, (int)resp.StatusCode, resp.StatusDescription);  
            host.Close();  
  
        }  
    }  
}  
```  
  
## <a name="compiling-the-code"></a>Compilando o código  
  
- Ao compilar a referência de código System. ServiceModel. dll e System. ServiceModel. Web. dll  
  
## <a name="see-also"></a>Consulte também

- [UriTemplate and UriTemplateTable](uritemplate-and-uritemplatetable.md)
- [Modelo de programação WCF Web HTTP](wcf-web-http-programming-model.md)
- [Visão geral do modelo de programação HTTP Web do WCF](wcf-web-http-programming-model-overview.md)
