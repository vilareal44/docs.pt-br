---
title: Fazer solicitações assíncronas
description: Saiba como as classes System.Net usam o modelo de programação assíncrona padrão .NET Framework para acesso assíncrono aos recursos da Internet.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Internet, asynchronous access
- Networking
- asynchronous requests, Internet resources
- Network Resources
- WebRequest class, asynchronous access
ms.assetid: 735d3fce-f80c-437f-b02c-5c47f5739674
ms.openlocfilehash: 0af143b723c90b146dc5de8447d4a7e1866e7105
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502295"
---
# <a name="making-asynchronous-requests"></a>Fazer solicitações assíncronas
As classes <xref:System.Net> usam o modelo de programação assíncrona padrão do .NET Framework para acesso assíncrono aos recursos de Internet. Os métodos <xref:System.Net.WebRequest.BeginGetResponse%2A> e <xref:System.Net.WebRequest.EndGetResponse%2A> da classe <xref:System.Net.WebRequest> iniciam e concluem solicitações assíncronas para um recurso de Internet.  
  
> [!NOTE]
> Usar chamadas síncronas em métodos de retorno de chamada assíncronos pode resultar em severas penalidades. Solicitações de Internet feitas com **WebRequest** e seus descendentes devem usar <xref:System.IO.Stream.BeginRead%2A?displayProperty=nameWithType> para ler o fluxo retornado pelo método <xref:System.Net.WebResponse.GetResponseStream%2A?displayProperty=nameWithType>.  
  
 O código de exemplo a seguir demonstra como usar chamadas assíncronas com a classe **WebRequest**. O exemplo é um programa de console que usa um URI por meio da linha de comando, solicita o recurso no URI e imprime os dados para o console conforme eles são recebidos da Internet.  
  
 O programa define duas classes para seu próprio uso, a classe **RequestState**, que transmite dados entre chamadas assíncronas e a classe **ClientGetAsync**, que implementa a solicitação assíncrona para um recurso de Internet.  
  
 A classe **RequestState** preserva o estado da solicitação entre chamadas para os métodos assíncronos que atendem a solicitação. Ela contém instâncias de **WebRequest** e de <xref:System.IO.Stream> que contêm a solicitação atual para o recurso e o fluxo recebido em resposta, um buffer que contém os dados recebidos atualmente do recurso de Internet e um <xref:System.Text.StringBuilder> que contém a resposta completa. Um **RequestState** é passado como o parâmetro *state* quando o método <xref:System.AsyncCallback> é registrado com **WebRequest.BeginGetResponse**.  
  
 A classe **ClientGetAsync** implementa uma solicitação assíncrona para um recurso de Internet e grava a resposta resultante no console. Ela contém os métodos e as propriedades descritas na lista a seguir.  
  
- A propriedade `allDone` contém uma instância da classe <xref:System.Threading.ManualResetEvent> que indica a conclusão da solicitação.  
  
- O método `Main()` lê a linha de comando e inicia a solicitação para o recurso de Internet especificado. Ele cria o **WebRequest** `wreq` e **RequestState** `rs`, chama **BeginGetResponse** para começar a processar a solicitação e, em seguida, chama o método `allDone.WaitOne()` para que o aplicativo não saia até que o retorno de chamada seja concluído. Depois que a resposta é lida do recurso da Internet, `Main()` grava-o no console e o aplicativo é encerrado.  
  
- O método `showusage()` grava uma linha de comando de exemplo no console. Ele é chamado por `Main()` quando nenhum URI é fornecido na linha de comando.  
  
- O método `RespCallBack()` implementa o método de retorno de chamada assíncrono para a solicitação de Internet. Ele cria a instância de **WebResponse** que contém a resposta do recurso da Internet, obtém o fluxo de resposta e, em seguida, começa a ler os dados do fluxo de forma assíncrona.  
  
- O método `ReadCallBack()` implementa o método de retorno de chamada assíncrono para ler o fluxo de resposta. Ele transfere os dados recebidos do recurso da Internet para a propriedade **ResponseData** da instância **RequestState**, em seguida, inicia outra leitura assíncrona do fluxo de resposta até que mais nenhum dado seja retornado. Depois que todos os dados são lidos, `ReadCallBack()` fecha o fluxo de resposta e chama o método `allDone.Set()` para indicar que a resposta inteira está presente em **ResponseData**.  
  
    > [!NOTE]
    > É fundamental que todos os fluxos de rede estejam fechados. Se você não fechar cada fluxo de solicitação e resposta, o seu aplicativo ficará sem conexões para o servidor e não poderá processar solicitações adicionais.  
  
```csharp  
using System;  
using System.Net;  
using System.Threading;  
using System.Text;  
using System.IO;  
  
// The RequestState class passes data across async calls.  
public class RequestState  
{  
   const int BufferSize = 1024;  
   public StringBuilder RequestData;  
   public byte[] BufferRead;  
   public WebRequest Request;  
   public Stream ResponseStream;  
   // Create Decoder for appropriate enconding type.  
   public Decoder StreamDecode = Encoding.UTF8.GetDecoder();  
  
   public RequestState()  
   {  
      BufferRead = new byte[BufferSize];  
      RequestData = new StringBuilder(String.Empty);  
      Request = null;  
      ResponseStream = null;  
   }
}  
  
// ClientGetAsync issues the async request.  
class ClientGetAsync
{  
   public static ManualResetEvent allDone = new ManualResetEvent(false);  
   const int BUFFER_SIZE = 1024;  
  
   public static void Main(string[] args)
   {  
      if (args.Length < 1)
      {  
         showusage();  
         return;  
      }  
  
      // Get the URI from the command line.  
      Uri httpSite = new Uri(args[0]);  
  
      // Create the request object.  
      WebRequest wreq = WebRequest.Create(httpSite);  
  
      // Create the state object.  
      RequestState rs = new RequestState();  
  
      // Put the request into the state object so it can be passed around.  
      rs.Request = wreq;  
  
      // Issue the async request.  
      IAsyncResult r = (IAsyncResult) wreq.BeginGetResponse(  
         new AsyncCallback(RespCallback), rs);  
  
      // Wait until the ManualResetEvent is set so that the application
      // does not exit until after the callback is called.  
      allDone.WaitOne();  
  
      Console.WriteLine(rs.RequestData.ToString());  
   }  
  
   public static void showusage() {  
      Console.WriteLine("Attempts to GET a URL");  
      Console.WriteLine("\r\nUsage:");  
      Console.WriteLine("   ClientGetAsync URL");  
      Console.WriteLine("   Example:");  
      Console.WriteLine("      ClientGetAsync http://www.contoso.com/");  
   }  
  
   private static void RespCallback(IAsyncResult ar)  
   {  
      // Get the RequestState object from the async result.  
      RequestState rs = (RequestState) ar.AsyncState;  
  
      // Get the WebRequest from RequestState.  
      WebRequest req = rs.Request;  
  
      // Call EndGetResponse, which produces the WebResponse object  
      //  that came from the request issued above.  
      WebResponse resp = req.EndGetResponse(ar);
  
      //  Start reading data from the response stream.  
      Stream ResponseStream = resp.GetResponseStream();  
  
      // Store the response stream in RequestState to read
      // the stream asynchronously.  
      rs.ResponseStream = ResponseStream;  
  
      //  Pass rs.BufferRead to BeginRead. Read data into rs.BufferRead  
      IAsyncResult iarRead = ResponseStream.BeginRead(rs.BufferRead, 0,
         BUFFER_SIZE, new AsyncCallback(ReadCallBack), rs);
   }  
  
   private static void ReadCallBack(IAsyncResult asyncResult)  
   {  
      // Get the RequestState object from AsyncResult.  
      RequestState rs = (RequestState)asyncResult.AsyncState;  
  
      // Retrieve the ResponseStream that was set in RespCallback.
      Stream responseStream = rs.ResponseStream;  
  
      // Read rs.BufferRead to verify that it contains data.
      int read = responseStream.EndRead( asyncResult );  
      if (read > 0)  
      {  
         // Prepare a Char array buffer for converting to Unicode.  
         Char[] charBuffer = new Char[BUFFER_SIZE];  
  
         // Convert byte stream to Char array and then to String.  
         // len contains the number of characters converted to Unicode.  
      int len =
         rs.StreamDecode.GetChars(rs.BufferRead, 0, read, charBuffer, 0);  
  
         String str = new String(charBuffer, 0, len);  
  
         // Append the recently read data to the RequestData stringbuilder  
         // object contained in RequestState.  
         rs.RequestData.Append(  
            Encoding.ASCII.GetString(rs.BufferRead, 0, read));
  
         // Continue reading data until
         // responseStream.EndRead returns –1.  
         IAsyncResult ar = responseStream.BeginRead(
            rs.BufferRead, 0, BUFFER_SIZE,
            new AsyncCallback(ReadCallBack), rs);  
      }  
      else  
      {  
         if(rs.RequestData.Length>0)  
         {  
            //  Display data to the console.  
            string strContent;
            strContent = rs.RequestData.ToString();  
         }  
         // Close down the response stream.  
         responseStream.Close();
         // Set the ManualResetEvent so the main thread can exit.  
         allDone.Set();
      }  
      return;  
   }
}  
```  
  
```vb  
Imports System  
Imports System.Net  
Imports System.Threading  
Imports System.Text  
Imports System.IO  
  
' The RequestState class passes data across async calls.  
Public Class RequestState  
  
      Public RequestData As New StringBuilder("")  
      Public BufferRead(1024) As Byte  
      Public Request As HttpWebRequest  
      Public ResponseStream As Stream  
      ' Create Decoder for appropriate encoding type.  
      Public StreamDecode As Decoder = Encoding.UTF8.GetDecoder()  
  
      Public Sub New  
         Request = Nothing  
         ResponseStream = Nothing  
      End Sub  
End Class  
  
' ClientGetAsync issues the async request.  
Class ClientGetAsync  
   Shared allDone As New ManualResetEvent(False)  
      Const BUFFER_SIZE As Integer = 1024  
  
    Shared Sub Main()  
        Dim Args As String() = Environment.GetCommandLineArgs()  
  
        If Args.Length < 2 Then  
            ShowUsage()  
            Return  
        End If  
  
      ' Get the URI from the command line.  
        Dim HttpSite As Uri = New Uri(Args(1))  
  
      ' Create the request object.  
        Dim wreq As HttpWebRequest = _  
           CType(WebRequest.Create(HttpSite), HttpWebRequest)  
  
      ' Create the state object.  
      Dim rs As RequestState = New RequestState()  
  
      ' Put the request into the state so it can be passed around.  
      rs.Request = wreq  
  
      ' Issue the async request.  
        Dim r As IAsyncResult = _  
           CType(wreq.BeginGetResponse( _  
           New AsyncCallback(AddressOf RespCallback), rs), IAsyncResult)  
  
      ' Wait until the ManualResetEvent is set so that the application  
      ' does not exit until after the callback is called.  
        allDone.WaitOne()  
    End Sub  
  
    Shared Sub ShowUsage()  
        Console.WriteLine("Attempts to GET a URI")  
        Console.WriteLine()  
        Console.WriteLine("Usage:")  
        Console.WriteLine("ClientGetAsync URI")  
        Console.WriteLine("Examples:")  
        Console.WriteLine("ClientGetAsync http://www.contoso.com/")  
    End Sub  
  
    Shared Sub RespCallback(ar As IAsyncResult)  
       ' Get the RequestState object from the async result.  
       Dim rs As RequestState = CType(ar.AsyncState, RequestState)  
  
       ' Get the HttpWebRequest from RequestState.  
       Dim req As HttpWebRequest= rs.Request  
  
       ' Call EndGetResponse, which returns the HttpWebResponse object  
       ' that came from the request issued above.  
       Dim resp As HttpWebResponse = _  
           CType(req.EndGetResponse(ar), HttpWebResponse)  
  
       ' Start reading data from the response stream.  
       Dim ResponseStream As Stream = resp.GetResponseStream()  
  
       ' Store the reponse stream in RequestState to read  
       ' the stream asynchronously.  
       rs.ResponseStream = ResponseStream  
  
       ' Pass rs.BufferRead to BeginRead. Read data into rs.BufferRead.  
       Dim iarRead As IAsyncResult = _  
          ResponseStream.BeginRead(rs.BufferRead, 0, BUFFER_SIZE, _  
          New AsyncCallback(AddressOf ReadCallBack), rs)  
    End Sub  
  
   Shared Sub ReadCallBack(asyncResult As IAsyncResult)  
      ' Get the RequestState object from the AsyncResult.  
      Dim rs As RequestState = CType(asyncResult.AsyncState, RequestState)  
  
      ' Retrieve the ResponseStream that was set in RespCallback.  
      Dim responseStream As Stream = rs.ResponseStream  
  
      ' Read rs.BufferRead to verify that it contains data.  
      Dim read As Integer = responseStream.EndRead( asyncResult )  
      If read > 0 Then  
         ' Prepare a Char array buffer for converting to Unicode.  
         Dim charBuffer(1024) As Char  
  
         ' Convert byte stream to Char array and then String.  
         ' len contains the number of characters converted to Unicode.  
         Dim len As Integer = _  
           rs.StreamDecode.GetChars(rs.BufferRead, 0, read, charBuffer, 0)  
         Dim str As String = new String(charBuffer, 0, len)
  
         ' Append the recently read data to the RequestData stringbuilder
         ' object contained in RequestState.  
         rs.RequestData.Append( _  
            Encoding.ASCII.GetString(rs.BufferRead, 0, read))  
  
         ' Continue reading data until responseStream.EndRead  
         ' returns –1.  
         Dim ar As IAsyncResult = _  
            responseStream.BeginRead(rs.BufferRead, 0, BUFFER_SIZE, _  
            New AsyncCallback(AddressOf ReadCallBack), rs)  
      Else  
          If rs.RequestData.Length > 1 Then  
            ' Display data to the console.  
            Dim strContent As String  
            strContent = rs.RequestData.ToString()  
            Console.WriteLine(strContent)  
         End If  
  
         ' Close down the response stream.  
         responseStream.Close()  
  
         ' Set the ManualResetEvent so the main thread can exit.  
         allDone.Set()  
      End If  
  
      Return  
   End Sub  
End Class  
```  
  
## <a name="see-also"></a>Confira também

- [Solicitando dados](requesting-data.md)
