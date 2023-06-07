---
title: Como migrar serviços habilitados para AJAX ASP.NET para o WCF
ms.date: 03/30/2017
ms.assetid: 1428df4d-b18f-4e6d-bd4d-79ab3dd5147c
ms.openlocfilehash: 6f356f47922945218e02271371d9ddea36ecc5a2
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597001"
---
# <a name="how-to-migrate-ajax-enabled-aspnet-web-services-to-wcf"></a>Como migrar serviços habilitados para AJAX ASP.NET para o WCF
Este tópico descreve os procedimentos para migrar um serviço AJAX ASP.NET básico para um serviço equivalente habilitado para AJAX (Windows Communication Foundation). Ele mostra como criar uma versão WCF equivalente funcional de um serviço ASP.NET AJAX. Os dois serviços podem ser usados lado a lado, ou o serviço WCF pode ser usado para substituir o serviço ASP.NET AJAX.

 A migração de um serviço ASP.NET AJAX existente para um serviço WCF AJAX oferece os seguintes benefícios:

- Você pode expor seu serviço AJAX como um serviço SOAP com configuração extra mínima.

- Você pode se beneficiar de recursos do WCF, como rastreamento e assim por diante.

 Os procedimentos a seguir pressupõem que você esteja usando o Visual Studio 2012.

 O código que resulta dos procedimentos descritos neste tópico é fornecido no exemplo após os procedimentos.

 Para obter mais informações sobre como expor um serviço WCF por meio de um ponto de extremidade habilitado para AJAX, consulte o tópico [como: usar a configuração para adicionar um ponto de extremidade do ASP.NET AJAX](how-to-use-configuration-to-add-an-aspnet-ajax-endpoint.md) .

### <a name="to-create-and-test-the-aspnet-web-service-application"></a>Para criar e testar o aplicativo de serviço Web ASP.NET

1. Abra o Visual Studio 2012.

2. No menu **arquivo** , selecione **novo**, **projeto**, **Web**e, em seguida, selecione **aplicativo de serviço Web ASP.net**.

3. Nomeie o projeto `ASPHello` e clique em **OK**.

4. Remova a marca de comentário da linha no arquivo Service1.asmx.cs que contém `System.Web.Script.Services.ScriptService]` para habilitar o Ajax para este serviço.

5. No menu **Compilar** , selecione **Compilar solução**.

6. No menu **Depurar**, selecione **Iniciar sem Depurar**.

7. Na página da Web gerada, selecione a `HelloWorld` operação.

8. Clique no botão **invocar** na `HelloWorld` página de teste. Você deve receber a resposta XML a seguir.

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <string xmlns="http://tempuri.org/">Hello World</string>
    ```

9. Essa resposta confirma que agora você tem um serviço ASP.NET AJAX em funcionamento e, em particular, que o serviço agora expôs um ponto de extremidade em Service1. asmx/HelloWorld que responde às solicitações HTTP POST e retorna XML.

     Agora você está pronto para converter este serviço para usar um serviço WCF AJAX.

### <a name="to-create-an-equivalent-wcf-ajax-service-application"></a>Para criar um aplicativo de serviço WCF AJAX equivalente

1. Clique com o botão direito do mouse no projeto **ASPHello** e selecione **Adicionar**, **novo item**e, em seguida, **serviço WCF habilitado para AJAX**.

2. Nomeie o serviço `WCFHello` e clique em **Adicionar**.

3. Abra o arquivo WCFHello.svc.cs.

4. Em Service1.asmx.cs, copie a seguinte implementação da `HelloWorld` operação.

    ```csharp
    public string HelloWorld()
    {
        return "Hello World";
    }
    ```

5. Cole na implementação copiada da `HelloWorld` operação no arquivo WCFHello.svc.cs no lugar do código a seguir.

    ```csharp
    public void DoWork()
    {
          // Add your operation implementation here
          return;
    }
    ```

6. Especifique o `Namespace` atributo para <xref:System.ServiceModel.ServiceContractAttribute> como `WCFHello` .

    ```csharp
    [ServiceContract(Namespace="WCFHello")]
    [AspNetCompatibilityRequirements(RequirementsMode=AspNetCompatibilityRequirementsMode.Required)]
    public class WCFHello
    { … }
    ```

7. Adicione o <xref:System.ServiceModel.Web.WebInvokeAttribute> à `HelloWorld` operação e defina a <xref:System.ServiceModel.Web.WebInvokeAttribute.ResponseFormat%2A> Propriedade a ser retornada <xref:System.ServiceModel.Web.WebMessageFormat.Xml> . Observe que, se não estiver definido, o tipo de retorno padrão será <xref:System.ServiceModel.Web.WebMessageFormat.Json> .

    ```csharp
    [OperationContract]
    [WebInvoke(ResponseFormat=WebMessageFormat.Xml)]
    public string HelloWorld()
    {
        return "Hello World";
    }
    ```

8. No menu **Compilar** , selecione **Compilar solução**.

9. Abra o arquivo WCFHello. svc e, no menu **depurar** , selecione **Iniciar sem depuração**.

10. O serviço agora expõe um ponto de extremidade em `WCFHello.svc/HelloWorld` , que responde às solicitações HTTP post. As solicitações HTTP POST não podem ser testadas no navegador, mas o ponto de extremidade retorna XML após XML.

    ```xml
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Hello World</string>
    ```

11. O `WCFHello.svc/HelloWorld` e os `Service1.aspx/HelloWorld` pontos de extremidade agora são funcionalmente equivalentes.

## <a name="example"></a>Exemplo
 O código que resulta dos procedimentos descritos neste tópico é fornecido no exemplo a seguir.

```csharp
//This is the ASP.NET code in the Service1.asmx.cs file.

using System;
using System.Collections;
using System.ComponentModel;
using System.Data;
using System.Linq;
using System.Web;
using System.Web.Services;
using System.Web.Services.Protocols;
using System.Xml.Linq;
using System.Web.Script.Services;

namespace ASPHello
{
    /// <summary>
    /// Summary description for Service1.
    /// </summary>
    [WebService(Namespace = "http://tempuri.org/")]
    [WebServiceBinding(ConformsTo = WsiProfiles.BasicProfile1_1)]
    [ToolboxItem(false)]
    // To allow this Web Service to be called from script, using ASP.NET AJAX, uncomment the following line.
    [System.Web.Script.Services.ScriptService]
    public class Service1 : System.Web.Services.WebService
    {

        [WebMethod]
        public string HelloWorld()
        {
            return "Hello World";
        }
    }
}

//This is the WCF code in the WCFHello.svc.cs file.
using System;
using System.Linq;
using System.Runtime.Serialization;
using System.ServiceModel;
using System.ServiceModel.Activation;
using System.ServiceModel.Web;

namespace ASPHello
{
    [ServiceContract(Namespace = "WCFHello")]
    [AspNetCompatibilityRequirements(RequirementsMode = AspNetCompatibilityRequirementsMode.Allowed)]
    public class WCFHello
    {
        // Add [WebInvoke] attribute to use HTTP GET.
        [OperationContract]
        [WebInvoke(ResponseFormat=WebMessageFormat.Xml)]
        public string HelloWorld()
        {
            return "Hello World";
        }

        // Add more operations here and mark them with [OperationContract].
    }
}
```

 <xref:System.Xml.XmlDocument>Não há suporte para o tipo no <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> porque ele não é serializável pelo <xref:System.Xml.Serialization.XmlSerializer> . Você pode usar um <xref:System.Xml.Linq.XDocument> tipo ou serializar o <xref:System.Xml.XmlDocument.DocumentElement%2A> em vez disso.

 Se os serviços Web ASMX estiverem sendo atualizados e migrados lado a lado para os serviços WCF, evite mapear dois tipos para o mesmo nome no cliente. Isso causará uma exceção em serializadores se o mesmo tipo for usado em um <xref:System.Web.Services.WebMethodAttribute> e um <xref:System.ServiceModel.ServiceContractAttribute> :

- Se o serviço WCF for adicionado primeiro, invocar o método no serviço Web ASMX causa uma exceção em <xref:System.Web.UI.ObjectConverter.ConvertValue%28System.Object%2CSystem.Type%2CSystem.String%29> porque a definição de estilo do WCF da ordem no proxy tem precedência.

- Se o serviço Web ASMX for adicionado primeiro, invocar o método no serviço WCF causará exceção em <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> porque a definição de estilo do serviço Web da ordem no proxy tem precedência.

 Há diferenças significativas no comportamento entre o <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> e o AJAX ASP.NET <xref:System.Web.Script.Serialization.JavaScriptSerializer> . Por exemplo, o <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> representa um dicionário como uma matriz de pares chave/valor, enquanto o AJAX ASP.NET <xref:System.Web.Script.Serialization.JavaScriptSerializer> representa um dicionário como objetos JSON reais. O seguinte é o dicionário representado no AJAX ASP.NET.

```csharp
Dictionary<string, int> d = new Dictionary<string, int>();
d.Add("one", 1);
d.Add("two", 2);
```

 Esse dicionário é representado em objetos JSON, conforme mostrado na lista a seguir:

- [{"Chave": "um", "valor": 1}, {"chave": "dois", "valor": 2}] pelo<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>

- {"One": 1, "Two": 2} pelo AJAX ASP.NET<xref:System.Web.Script.Serialization.JavaScriptSerializer>

 O <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> é mais potente no sentido de que ele pode manipular dicionários em que o tipo de chave não é uma cadeia de caracteres, enquanto o <xref:System.Web.Script.Serialization.JavaScriptSerializer> não pode. Mas o último é mais amigável para JSON.

 As diferenças significativas entre esses serializadores são resumidas na tabela a seguir.

|Categoria de diferenças|DataContractJsonSerializer|ASP.NET AJAX JavaScriptSerializer|
|-----------------------------|--------------------------------|---------------------------------------|
|Desserializar o buffer vazio (novo byte [0]) em <xref:System.Object> (ou <xref:System.Uri> ou em algumas outras classes).|SerializationException|null|
|Serialização de<xref:System.DBNull.Value>|{}(ou {"__type": "#System"})|Null|
|Serialização dos membros privados de tipos de [Serializable].|serializado|não serializado|
|Serialização das propriedades públicas de <xref:System.Runtime.Serialization.ISerializable> tipos.|não serializado|serializado|
|"Extensões" de JSON|Segue a especificação JSON, que requer aspas em nomes de membros de objetos ({"a": "Olá"}).|Dá suporte aos nomes de membros de objeto sem aspas ({a: "Olá"}).|
|<xref:System.DateTime>Tempo universal coordenado (UTC)|Não dá suporte ao formato " \\ /Date (123456789U) \\ /" ou " \\ /Date \\ (\d + (U&#124; ( \\ + \\ -[\d {4} ]))? \\ ) \\ \\ /)".|Dá suporte ao formato " \\ /Date (123456789U) \\ /" e " \\ /Date \\ (\d + (U&#124; ( \\ + \\ -[\d {4} ]))? \\ ) \\ \\ /) "como valores de DateTime.|
|Representação de dicionários|Uma matriz de KeyValuePair \<K,V> , lida com tipos de chave que não são cadeias de caracteres.|Como objetos JSON reais – mas só trata os tipos de chave que são cadeias de caracteres.|
|Caracteres de escape|Sempre com uma barra de escape (/); Nunca permite caracteres JSON inválidos sem escape, como "\n".|Com uma barra de escape (/) para valores DateTime.|

## <a name="see-also"></a>Consulte também

- [Como usar a configuração para adicionar um ponto de extremidade do ASP.NET AJAX](how-to-use-configuration-to-add-an-aspnet-ajax-endpoint.md)
