---
title: 'Como: enviar dados usando a classe WebRequest'
description: Saiba como enviar dados para um servidor usando a classe WebRequest no .NET Framework. Esse procedimento geralmente é usado para postar dados em uma página da Web.
ms.date: 03/25/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WebRequest class, sending data to a host
- Sending data to a host, using WebRequest class
ms.assetid: 66686878-38ac-4aa6-bf42-ffb568ffc459
ms.openlocfilehash: 4b353250fec778ee8b352f13de6d7faaf15c13ee
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502438"
---
# <a name="how-to-send-data-by-using-the-webrequest-class"></a>Como: enviar dados usando a classe WebRequest

O procedimento a seguir descreve as etapas usadas para enviar dados para um servidor. Esse procedimento normalmente é usado para enviar dados para uma página da Web.

## <a name="to-send-data-to-a-host-server"></a>Para enviar dados para um servidor de host

1. Crie uma instância de <xref:System.Net.WebRequest> chamando <xref:System.Net.WebRequest.Create%2A?displayProperty=nameWithType> com o URI do recurso, como um script ou uma página ASP.NET, que aceite dados. Por exemplo:

    ```csharp
    WebRequest request = WebRequest.Create("http://www.contoso.com/PostAccepter.aspx");
    ```

    ```vb
    Dim request as WebRequest = WebRequest.Create("http://www.contoso.com/PostAccepter.aspx")
    ```

    > [!NOTE]
    > O .NET Framework fornece classes específicas de protocolo derivadas das classes <xref:System.Net.WebRequest> e <xref:System.Net.WebResponse> para URIs que começam com *http:*, *https:*, *ftp:* e *file:*.

    Caso precise definir ou ler propriedades específica de protocolo, converta o objeto <xref:System.Net.WebRequest> ou <xref:System.Net.WebResponse> em um tipo de objeto específico de protocolo. Para obter mais informações, confira [Programando protocolos conectáveis](programming-pluggable-protocols.md).

2. Defina os valores de propriedade necessários no objeto `WebRequest`. Por exemplo, para habilitar a autenticação, defina a propriedade <xref:System.Net.WebRequest.Credentials%2A?displayProperty=nameWithType> como uma instância da classe <xref:System.Net.NetworkCredential>:

    ```csharp
    request.Credentials = CredentialCache.DefaultCredentials;
    ```

    ```vb
    request.Credentials = CredentialCache.DefaultCredentials
    ```

3. Especifique um método de protocolo que permite que os dados sejam enviados com uma solicitação, como o método `POST` HTTP:

    ```csharp
    request.Method = "POST";
    ```

    ```vb
    request.Method = "POST"
    ```

4. Defina a propriedade <xref:System.Web.HttpRequest.ContentLength> com o número de bytes incluídos com a solicitação. Por exemplo:

    ```csharp
    request.ContentLength = byteArray.Length;
    ```

    ```vb
    request.ContentLength = byteArray.Length
    ```

5. Defina a propriedade <xref:System.Web.HttpRequest.ContentType> com um valor apropriado. Por exemplo:

    ```csharp
    request.ContentType = "application/x-www-form-urlencoded";
    ```

    ```vb
    request.ContentType = "application/x-www-form-urlencoded"
    ```

6. Obtenha o fluxo que mantém dados de solicitação chamando o método <xref:System.Net.WebRequest.GetRequestStream%2A>. Por exemplo:

    ```csharp
    Stream dataStream = request.GetRequestStream();
    ```

    ```vb
    Dim dataStream As Stream = request.GetRequestStream()
    ```

7. Grave os dados no objeto <xref:System.IO.Stream> retornado pelo método `GetRequestStream`. Por exemplo:

    ```csharp
    dataStream.Write(byteArray, 0, byteArray.Length);
    ```

    ```vb
    dataStream.Write(byteArray, 0, byteArray.Length)
    ```

8. Feche o fluxo de solicitação chamando o método <xref:System.IO.Stream.Close%2A?displayProperty=nameWithType>. Por exemplo:

    ```csharp
    dataStream.Close();
    ```

    ```vb
    dataStream.Close()
    ```

9. Envie a solicitação para o servidor chamando <xref:System.Net.WebRequest.GetResponse%2A?displayProperty=nameWithType>. Esse método retorna um objeto que contém a resposta do servidor. O tipo do objeto `WebResponse` retornado é determinado pelo esquema do URI da solicitação. Por exemplo:

    ```csharp
    WebResponse response = request.GetResponse();
    ```

    ```vb
    Dim response As WebResponse = request.GetResponse()
    ```

10. Você pode acessar as propriedades do objeto `WebResponse` ou convertê-lo em uma instância específica de protocolo para ler propriedades específicas de protocolo.

    Por exemplo, para acessar as propriedades específicas de HTTP de <xref:System.Net.HttpWebResponse>, converta o objeto `WebResponse` em uma referência <xref:System.Net.HttpWebResponse>. O seguinte exemplo de código mostra como exibir a propriedade <xref:System.Net.HttpWebResponse.StatusDescription%2A?displayProperty=nameWithType> específica de HTTP enviada com uma resposta:

    ```csharp
    Console.WriteLine(((HttpWebResponse)response).StatusDescription);
    ```

    ```vb
    Console.WriteLine(CType(response, HttpWebResponse).StatusDescription)
    ```

11. Para obter o fluxo que contém os dados de resposta enviados pelo servidor, chame o método <xref:System.Net.WebResponse.GetResponseStream%2A?displayProperty=nameWithType> do objeto `WebResponse`. Por exemplo:

    ```csharp
    Stream dataStream = response.GetResponseStream();
    ```

    ```vb
    Dim dataStream As Stream = response.GetResponseStream()
    ```

12. Depois de ler os dados do objeto de resposta, feche-o com o método <xref:System.Net.WebResponse.Close%2A?displayProperty=nameWithType> ou feche o fluxo de resposta com o método <xref:System.IO.Stream.Close%2A?displayProperty=nameWithType>. Se você não fechar a resposta ou o fluxo, seu aplicativo poderá ficar sem conexões com o servidor e sem a capacidade de processar solicitações adicionais. Como o método `WebResponse.Close` chama `Stream.Close` quando ele fecha a resposta, não é necessário chamar `Close` nos objetos de resposta e de fluxo, embora fazer isso não seja prejudicial. Por exemplo:

    ```csharp
    response.Close();
    ```

    ```vb
    response.Close()
    ```

## <a name="example"></a>Exemplo

O seguinte exemplo mostra como enviar dados para um servidor Web e ler os dados na resposta:

[!code-csharp[SendDataUsingWebRequest](../../../samples/snippets/csharp/VS_Snippets_Network/SendDataUsingWebRequest/cs/WebRequestPostExample.cs)]
[!code-vb[SendDataUsingWebRequest](../../../samples/snippets/visualbasic/VS_Snippets_Network/SendDataUsingWebRequest/vb/WebRequestPostExample.vb)]

## <a name="see-also"></a>Confira também

- [Criando solicitações da Internet](creating-internet-requests.md)
- [Usando fluxos na rede](using-streams-on-the-network.md)
- [Acessando a Internet por meio de um proxy](accessing-the-internet-through-a-proxy.md)
- [Solicitando dados](requesting-data.md)
- [Como: solicitar dados usando a classe WebRequest](how-to-request-data-using-the-webrequest-class.md)
