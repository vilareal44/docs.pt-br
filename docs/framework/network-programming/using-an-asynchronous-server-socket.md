---
title: Usando um soquete de servidor assíncrono
description: Este exemplo mostra um soquete de servidor assíncrono. A classe Socket usa .NET Framework programação assíncrona para processar solicitações de serviço de rede.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- application protocols, sockets
- sending data, sockets
- Socket class, asynchronous server sockets
- data requests, sockets
- sockets, asynchronous server sockets
- requesting data from Internet, sockets
- server sockets
- receiving data, sockets
- asynchronous server sockets
- protocols, sockets
- Internet, sockets
ms.assetid: 813489a9-3efd-41b6-a33f-371d55397676
ms.openlocfilehash: 8b85afb3ffdf69973eff37ccbb067b470ed44e3a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502022"
---
# <a name="using-an-asynchronous-server-socket"></a>Usando um soquete de servidor assíncrono
Os soquetes de servidor assíncrono usam o modelo de programação assíncrono do .NET Framework para processar solicitações de serviço da rede. A classe <xref:System.Net.Sockets.Socket> segue o padrão de nomenclatura assíncrona do .NET Framework; por exemplo, o método <xref:System.Net.Sockets.Socket.Accept%2A> síncrono corresponde aos métodos <xref:System.Net.Sockets.Socket.BeginAccept%2A> e <xref:System.Net.Sockets.Socket.EndAccept%2A> assíncronos.  
  
 Um soquete de servidor assíncrono exige um método para começar a aceitar solicitações de conexão da rede, um método de retorno de chamada para manipular as solicitações de conexão e começar a receber dados da rede e um método de retorno de chamada para encerrar o recebimento dos dados. Todos esses métodos são abordados mais diante nesta seção.  
  
 No exemplo a seguir, para começar a aceitar solicitações de conexão da rede, o método `StartListening` inicializa o **Socket** e, em seguida, usa o método **BeginAccept** para começar a aceitar novas conexões. O método de retorno de chamada de aceitação é chamado quando uma nova solicitação de conexão é recebida no soquete. Ele é responsável por obter a instância **Socket** que manipulará a conexão e por entregar esse **Socket** ao thread que processará a solicitação. O método de retorno de chamada de aceitação implementa o representante <xref:System.AsyncCallback>; ele retorna um nulo e usa um único parâmetro do tipo <xref:System.IAsyncResult>. O exemplo a seguir é o shell de um método de retorno de chamada de aceitação.  
  
```vb  
Sub AcceptCallback(ar As IAsyncResult)  
    ' Add the callback code here.  
End Sub 'AcceptCallback  
```  
  
```csharp  
void AcceptCallback(IAsyncResult ar)
{  
    // Add the callback code here.  
}  
```  
  
 O método **BeginAccept** usa dois parâmetros, um representante **AsyncCallback**, que aponta para o método de retorno de chamada de aceitação e um objeto, que é usado para passar informações de estado para o método de retorno de chamada. No exemplo a seguir, o **Socket** de escuta é passado para o método de retorno de chamada por meio do parâmetro *state*. Esse exemplo cria um representante **AsyncCallback** e começa a aceitar conexões da rede.  
  
```vb  
listener.BeginAccept( _  
    New AsyncCallback(SocketListener.AcceptCallback),_  
    listener)  
```  
  
```csharp  
listener.BeginAccept(new AsyncCallback(SocketListener.AcceptCallback), listener);  
```  
  
 Os soquetes assíncronos usam threads do pool de threads do sistema para processar conexões de entrada. Um thread é responsável por aceitar conexões, outro thread é usado para manipular cada conexão de entrada e outro thread é responsável por receber dados da conexão. Eles podem ser o mesmo thread, dependendo de qual thread é atribuído pelo pool de threads. No exemplo a seguir, a classe <xref:System.Threading.ManualResetEvent?displayProperty=nameWithType> suspende a execução do thread principal e sinaliza quando a execução pode continuar.  
  
 O exemplo a seguir mostra um método assíncrono que cria um soquete TCP/IP assíncrono no computador local e começa a aceitar conexões. Ele supõe que haja um **ManualResetEvent** global chamado `allDone`, que o método seja um membro de uma classe chamada `SocketListener` e que um método de retorno de chamada chamado `AcceptCallback` esteja definido.  
  
```vb  
Public Sub StartListening()  
    Dim ipHostInfo As IPHostEntry = Dns.Resolve(Dns.GetHostName())  
    Dim localEP = New IPEndPoint(ipHostInfo.AddressList(0), 11000)  
  
    Console.WriteLine($"Local address and port : {localEP.ToString()}")  
  
    Dim listener As New Socket(localEP.Address.AddressFamily, _  
       SocketType.Stream, ProtocolType.Tcp)  
  
    Try  
        listener.Bind(localEP)  
        listener.Listen(10)  
  
        While True  
            allDone.Reset()  
  
            Console.WriteLine("Waiting for a connection...")  
            listener.BeginAccept(New _  
                AsyncCallback(SocketListener.AcceptCallback), _  
                listener)  
  
            allDone.WaitOne()  
        End While  
    Catch e As Exception  
        Console.WriteLine(e.ToString())  
    End Try  
    Console.WriteLine("Closing the listener...")  
End Sub 'StartListening  
```  
  
```csharp  
public void StartListening()
{  
    IPHostEntry ipHostInfo = Dns.Resolve(Dns.GetHostName());  
    IPEndPoint localEP = new IPEndPoint(ipHostInfo.AddressList[0], 11000);  
  
    Console.WriteLine($"Local address and port : {localEP.ToString()}");  
  
    Socket listener = new Socket(localEP.Address.AddressFamily, SocketType.Stream, ProtocolType.Tcp);  
  
    try
    {  
        listener.Bind(localEP);  
        listener.Listen(10);  
  
        while (true)
        {  
            allDone.Reset();  
  
            Console.WriteLine("Waiting for a connection...");  
            listener.BeginAccept(new AsyncCallback(SocketListener.AcceptCallback), listener);  
  
            allDone.WaitOne();  
        }  
    }
    catch (Exception e)
    {  
        Console.WriteLine(e.ToString());  
    }  
  
    Console.WriteLine("Closing the listener...");  
}  
```  
  
 O método de retorno de chamada de aceitação (`AcceptCallback` no exemplo anterior) é responsável por sinalizar o thread principal do aplicativo para continuar o processamento, estabelecendo a conexão com o cliente e por iniciar a leitura assíncrona de dados do cliente. O exemplo a seguir é a primeira parte de uma implementação do método `AcceptCallback`. Esta seção do método sinaliza o thread principal do aplicativo para continuar o processamento e estabelece a conexão com o cliente. Ele supõe que haja um **ManualResetEvent** global chamado `allDone`.  
  
```vb  
Public Sub AcceptCallback(ar As IAsyncResult)  
    allDone.Set()  
  
    Dim listener As Socket = CType(ar.AsyncState, Socket)  
    Dim handler As Socket = listener.EndAccept(ar)  
  
    ' Additional code to read data goes here.  
End Sub 'AcceptCallback  
```  
  
```csharp  
public void AcceptCallback(IAsyncResult ar)
{  
    allDone.Set();  
  
    Socket listener = (Socket) ar.AsyncState;  
    Socket handler = listener.EndAccept(ar);  
  
    // Additional code to read data goes here.
}  
```  
  
 A leitura de dados de um soquete de cliente exige um objeto de estado que passa valores entre chamadas assíncronas. O exemplo a seguir implementa um objeto de estado para o recebimento de uma cadeia de caracteres do cliente remoto. Ele contém campos para o soquete de cliente, um buffer de dados para o recebimento de dados e um <xref:System.Text.StringBuilder> para a criação da cadeia de caracteres de dados enviada pelo cliente. A colocação desses campos no objeto de estado permite que seus valores sejam preservados em várias chamadas para a leitura de dados do soquete de cliente.  
  
```vb  
Public Class StateObject  
    Public workSocket As Socket = Nothing  
    Public BufferSize As Integer = 1024  
    Public buffer(BufferSize) As Byte  
    Public sb As New StringBuilder()  
End Class 'StateObject  
```  
  
```csharp  
public class StateObject
{  
    public Socket workSocket = null;  
    public const int BufferSize = 1024;  
    public byte[] buffer = new byte[BufferSize];  
    public StringBuilder sb = new StringBuilder();  
}  
```  
  
 A seção do método `AcceptCallback` que começa a receber os dados do soquete de cliente primeiro inicializa uma instância da classe `StateObject` e, em seguida, chama o método <xref:System.Net.Sockets.Socket.BeginReceive%2A> para começar a ler os dados do soquete de cliente de forma assíncrona.  
  
 O exemplo a seguir mostra todo o método `AcceptCallback`. Ele supõe que haja um **ManualResetEvent** global chamado `allDone,`, que a classe `StateObject` esteja definida e que o método `ReadCallback` esteja definido em uma classe chamada `SocketListener`.  
  
```vb  
Public Shared Sub AcceptCallback(ar As IAsyncResult)  
    ' Get the socket that handles the client request.  
    Dim listener As Socket = CType(ar.AsyncState, Socket)  
    Dim handler As Socket = listener.EndAccept(ar)  
  
    ' Signal the main thread to continue.  
    allDone.Set()  
  
    ' Create the state object.  
    Dim state As New StateObject()  
    state.workSocket = handler  
    handler.BeginReceive(state.buffer, 0, state.BufferSize, 0, _  
        AddressOf AsynchronousSocketListener.ReadCallback, state)  
End Sub 'AcceptCallback  
```  
  
```csharp  
public static void AcceptCallback(IAsyncResult ar)
{  
    // Get the socket that handles the client request.  
    Socket listener = (Socket) ar.AsyncState;  
    Socket handler = listener.EndAccept(ar);  
  
    // Signal the main thread to continue.  
    allDone.Set();  
  
    // Create the state object.  
    StateObject state = new StateObject();  
    state.workSocket = handler;  
    handler.BeginReceive(state.buffer, 0, StateObject.BufferSize, 0,  
        new AsyncCallback(AsynchronousSocketListener.ReadCallback), state);  
}  
```  
  
 O método final que precisa ser implementado para o servidor de soquete assíncrono é o método de retorno de chamada de leitura que retorna os dados enviados pelo cliente. Assim como o método de retorno de chamada de aceitação, o método de retorno de chamada de leitura é um representante **AsyncCallback**. Esse método lê um ou mais bytes do soquete do cliente no buffer de dados e, em seguida, chama o método **BeginReceive** novamente até que os dados enviados pelo cliente estejam completos. Depois que a mensagem inteira for lida do cliente, a cadeia de caracteres será exibida no console e o soquete de servidor que manipula a conexão com o cliente será fechado.  
  
 A amostra a seguir implementa o método `ReadCallback`. Ele supõe que a classe `StateObject` esteja definida.  
  
```vb  
Public Shared Sub ReadCallback(ar As IAsyncResult)  
    Dim state As StateObject = CType(ar.AsyncState, StateObject)  
    Dim handler As Socket = state.workSocket  
  
    ' Read data from the client socket.
    Dim read As Integer = handler.EndReceive(ar)  
  
    ' Data was read from the client socket.  
    If read > 0 Then  
        state.sb.Append(Encoding.ASCII.GetString(state.buffer, 0, read))  
        handler.BeginReceive(state.buffer, 0, state.BufferSize, 0, _  
            AddressOf ReadCallback, state)  
    Else  
        If state.sb.Length > 1 Then  
            ' All the data has been read from the client;  
            ' display it on the console.  
            Dim content As String = state.sb.ToString()  
            Console.WriteLine($"Read {content.Length} bytes from socket. {ControlChars.Cr} Data : {content}")  
        End If  
    End If  
End Sub 'ReadCallback  
```  
  
```csharp  
public static void ReadCallback(IAsyncResult ar)
{  
    StateObject state = (StateObject) ar.AsyncState;  
    Socket handler = state.WorkSocket;  
  
    // Read data from the client socket.  
    int read = handler.EndReceive(ar);  
  
    // Data was read from the client socket.  
    if (read > 0)
    {  
        state.sb.Append(Encoding.ASCII.GetString(state.buffer,0,read));  
        handler.BeginReceive(state.buffer, 0, StateObject.BufferSize, 0,  
            new AsyncCallback(ReadCallback), state);  
    }
    else
    {  
        if (state.sb.Length > 1)
        {  
            // All the data has been read from the client;  
            // display it on the console.  
            string content = state.sb.ToString();  
            Console.WriteLine($"Read {content.Length} bytes from socket.\n Data : {content}");
        }  
        handler.Close();  
    }  
}  
```  
  
## <a name="see-also"></a>Confira também

- [Usando um soquete de servidor síncrono](using-a-synchronous-server-socket.md)
- [Exemplo de soquete de servidor assíncrono](asynchronous-server-socket-example.md)
- [Threading](../../standard/threading/index.md)
- [Escutando com soquetes](listening-with-sockets.md)
