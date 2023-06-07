---
title: Tratando erros
description: Saiba mais sobre as exceções específicas da Web e do sistema lançadas por WebRequest e WebResponse. Use a propriedade status para entender e resolver o problema.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Internet, WebRequest and WebResponse classes exceptions
- Status property
- WebExceptions class, about WebExceptions class
- Timeout enumeration member
- ConnectFailure enumeration member
- TrustFailure enumeration member
- WebRequest class, exceptions
- requesting data from Internet, error handling
- Success enumeration member
- receiving data, errors
- ProtocolError enumeration member
- downloading Internet resources, error handling
- WebResponse class, exceptions
- SendFailure enumeration member
- errors [.NET Framework], WebRequest and WebResponse classes exceptions
- sending data, errors
- response to Internet request, error handling
- NameResolutionFailure enumeration member
- KeepAliveFailure enumeration member
- network resources, WebRequest and WebResponse classes exceptions
- RequestCanceled enumeration member
- ReceiveFailure enumeration member
- ServerProtocolViolation enumeration member
- ConnectionClosed enumeration member
- SecureChannelFailure enumeration member
ms.assetid: 657141cd-5cf5-4fdb-a4b2-4c040eba84b5
ms.openlocfilehash: 786b2bd8bc4d1b394bcfe920053b2f4f55d1cdea
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502568"
---
# <a name="handling-errors"></a>Tratando erros

As classes <xref:System.Net.WebRequest> e <xref:System.Net.WebResponse> geram exceções do sistema (como <xref:System.ArgumentException>) e exceções específicas à Web (que são <xref:System.Net.WebException> geradas pelo método <xref:System.Net.WebRequest.GetResponse%2A>).  
  
Cada **WebException** inclui uma propriedade <xref:System.Net.WebException.Status%2A> que contém um valor da enumeração <xref:System.Net.WebExceptionStatus>. Examine a propriedade **Status** para determinar o erro ocorrido e execute as etapas apropriadas para resolver o erro.  
  
A tabela a seguir descreve os possíveis valores para a propriedade **Status**.  
  
|Status|Descrição|  
|------------|-----------------|  
|ConnectFailure|O serviço remoto não pôde ser contatado no nível do transporte.|  
|ConnectionClosed|A conexão foi fechada prematuramente.|  
|KeepAliveFailure|O servidor fechou uma conexão estabelecida com o conjunto de cabeçalhos Keep-alive.|  
|NameResolutionFailure|O serviço de nomes não pôde resolver o nome do host.|  
|ProtocolError|A resposta recebida do servidor estava completa, mas indicava um erro no nível do protocolo.|  
|ReceiveFailure|Não foi recebida uma resposta completa do servidor remoto.|  
|RequestCanceled|A solicitação foi cancelada.|  
|SecureChannelFailure|Ocorreu um erro em um link de canal seguro.|  
|SendFailure|Não foi possível enviar uma solicitação completa para o servidor remoto.|  
|ServerProtocolViolation|A resposta do servidor não era uma resposta HTTP válida.|  
|Sucesso|Nenhum erro foi encontrado.|  
|Tempo limite|Nenhuma resposta foi recebida no tempo limite definido para a solicitação.|  
|TrustFailure|Não foi possível validar um certificado do servidor.|  
|MessageLengthLimitExceeded|Foi recebida uma mensagem que ultrapassa o limite especificado ao enviar uma solicitação ou receber uma resposta do servidor.|  
|Pendente|Uma solicitação assíncrona interna está pendente.|  
|PipelineFailure|Esse valor dá suporte à infraestrutura .NET Framework e não se destina a ser usado diretamente no código.|  
|ProxyNameResolutionFailure|O serviço de resolvedor de nome não pôde resolver o nome de host do proxy.|  
|UnknownError|Ocorreu uma exceção de tipo desconhecido.|  
  
Quando a propriedade **Status** é **WebExceptionStatus.ProtocolError**, uma **WebResponse** que contém a resposta do servidor está disponível. Examine essa resposta para determinar a origem real do erro de protocolo.  
  
O exemplo a seguir mostra como capturar uma **WebException**.  
  
```csharp  
try
{  
    // Create a request instance.  
    WebRequest myRequest =
    WebRequest.Create("http://www.contoso.com");  
    // Get the response.  
    WebResponse myResponse = myRequest.GetResponse();  
    //Get a readable stream from the server.
    Stream sr = myResponse.GetResponseStream();  
  
    //Read from the stream and write any data to the console.  
    bytesread = sr.Read( myBuffer, 0, length);  
    while( bytesread > 0 )
    {  
        for (int i=0; i<bytesread; i++) {  
            Console.Write( "{0}", myBuffer[i]);  
        }  
        Console.WriteLine();  
        bytesread = sr.Read( myBuffer, 0, length);  
    }  
    sr.Close();  
    myResponse.Close();  
}  
catch (WebException webExcp)
{  
    // If you reach this point, an exception has been caught.  
    Console.WriteLine("A WebException has been caught.");  
    // Write out the WebException message.  
    Console.WriteLine(webExcp.ToString());  
    // Get the WebException status code.  
    WebExceptionStatus status =  webExcp.Status;  
    // If status is WebExceptionStatus.ProtocolError,
    //   there has been a protocol error and a WebResponse
    //   should exist. Display the protocol error.  
    if (status == WebExceptionStatus.ProtocolError) {  
        Console.Write("The server returned protocol error ");  
        // Get HttpWebResponse so that you can check the HTTP status code.  
        HttpWebResponse httpResponse = (HttpWebResponse)webExcp.Response;  
        Console.WriteLine((int)httpResponse.StatusCode + " - "  
           + httpResponse.StatusCode);  
    }  
}  
catch (Exception e)
{  
    // Code to catch other exceptions goes here.  
}  
```  
  
```vb  
Try  
    ' Create a request instance.  
    Dim myRequest As WebRequest = WebRequest.Create("http://www.contoso.com")  
    ' Get the response.  
    Dim myResponse As WebResponse = myRequest.GetResponse()  
    'Get a readable stream from the server.
    Dim sr As Stream = myResponse.GetResponseStream()  
  
    Dim i As Integer
    'Read from the stream and write any data to the console.  
    bytesread = sr.Read(myBuffer, 0, length)  
    While bytesread > 0  
        For i = 0 To bytesread - 1  
            Console.Write("{0}", myBuffer(i))  
        Next i  
        Console.WriteLine()  
        bytesread = sr.Read(myBuffer, 0, length)  
    End While  
    sr.Close()  
    myResponse.Close()  
Catch webExcp As WebException  
    ' If you reach this point, an exception has been caught.  
    Console.WriteLine("A WebException has been caught.")  
    ' Write out the WebException message.  
    Console.WriteLine(webExcp.ToString())  
    ' Get the WebException status code.  
    Dim status As WebExceptionStatus = webExcp.Status  
    ' If status is WebExceptionStatus.ProtocolError,
    '   there has been a protocol error and a WebResponse
    '   should exist. Display the protocol error.  
    If status = WebExceptionStatus.ProtocolError Then  
        Console.Write("The server returned protocol error ")  
        ' Get HttpWebResponse so that you can check the HTTP status code.  
        Dim httpResponse As HttpWebResponse = _  
           CType(webExcp.Response, HttpWebResponse)  
        Console.WriteLine(CInt(httpResponse.StatusCode).ToString() & _  
           " - " & httpResponse.StatusCode.ToString())  
    End If  
Catch e As Exception  
    ' Code to catch other exceptions goes here.  
End Try  
```  
  
Os aplicativos que usam a classe <xref:System.Net.Sockets.Socket> geram <xref:System.Net.Sockets.SocketException> quando ocorrem erros no soquete do Windows. As classes <xref:System.Net.Sockets.TcpClient>, <xref:System.Net.Sockets.TcpListener> e <xref:System.Net.Sockets.UdpClient> são criadas com base na classe **Socket** e geram **SocketExceptions** também.  
  
Quando uma **SocketException** é gerada, a classe **SocketException** define a propriedade <xref:System.Net.Sockets.SocketException.ErrorCode%2A> com o último erro de soquete do sistema operacional ocorrido. Para obter mais informações sobre códigos de erro de soquete, consulte a documentação de códigos de erro da API do Winsock 2.0 no MSDN.  
  
## <a name="see-also"></a>Confira também

- [Tratando e gerando exceções no .NET](../../standard/exceptions/index.md)
- [Solicitando dados](requesting-data.md)
