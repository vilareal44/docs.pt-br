---
title: Usando um soquete de cliente assíncrono
description: Este exemplo mostra um soquete de cliente assíncrono. .NET Framework programação assíncrona permite que um aplicativo continue a ser executado durante o processamento de uma conexão.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- application protocols, sockets
- sending data, sockets
- data requests, sockets
- asynchronous client sockets
- Socket class, asynchronous client sockets
- requesting data from Internet, sockets
- sockets, asynchronous client sockets
- receiving data, sockets
- protocols, sockets
- Internet, sockets
- client sockets
ms.assetid: fd85bc88-e06c-467d-a30d-9fd7cffcfca1
ms.openlocfilehash: 9cf46e9519bcecf4d7a20ff99b86fa5f66af2087
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502035"
---
# <a name="using-an-asynchronous-client-socket"></a>Usando um soquete de cliente assíncrono
Um soquete de cliente assíncrono não suspende o aplicativo enquanto aguarda a conclusão das operações de rede. Em vez disso, ele usa o modelo padrão de programação assíncrona do .NET Framework para processar a conexão de rede em um thread, enquanto o aplicativo continua em execução no thread original. Soquetes assíncronos são apropriados para aplicativos que fazem uso intenso da rede ou que não podem aguardar a conclusão das operações de rede antes de continuar.  
  
 A classe <xref:System.Net.Sockets.Socket> segue o padrão de nomenclatura do .NET Framework para métodos assíncronos; por exemplo, o método <xref:System.Net.Sockets.Socket.Receive%2A> síncrono corresponde aos métodos <xref:System.Net.Sockets.Socket.BeginReceive%2A> e <xref:System.Net.Sockets.Socket.EndReceive%2A> assíncronos.  
  
 As operações assíncronas exigem um método de retorno de chamada para retornar o resultado da operação. Se o aplicativo não precisar saber o resultado, nenhum método de retorno de chamada será necessário. O código de exemplo desta seção demonstra como usar um método para iniciar a conexão com um dispositivo de rede e um método de retorno de chamada para concluir a conexão, um método para iniciar o envio de dados e um método de retorno de chamada para concluir o envio, bem como um método para iniciar o recebimento de dados e um método de retorno de chamada para encerrar o recebimento de dados.  
  
 Os soquetes assíncronos usam vários threads do pool de threads do sistema para processar conexões de rede. Um thread é responsável por iniciar o envio ou recebimento de dados; outros threads concluem a conexão com o dispositivo de rede e enviam ou recebem dados. Nos exemplos a seguir, instâncias da classe <xref:System.Threading.ManualResetEvent?displayProperty=nameWithType> são usadas para suspender a execução do thread principal e sinalizar quando a execução pode continuar.  
  
 No exemplo a seguir, para conectar um soquete assíncrono com um dispositivo de rede, o método `Connect` inicializa um **Socket** e, em seguida, chama o método <xref:System.Net.Sockets.Socket.Connect%2A?displayProperty=nameWithType>, passando um ponto de extremidade remoto que representa o dispositivo de rede, o método de retorno de chamada de conexão e um objeto de estado, (o **Socket** de cliente), que é usado para passar informações de estado entre chamadas assíncronas. O exemplo implementa o método `Connect` para conectar o **Socket** especificado ao ponto de extremidade especificado. Ele supõe que haja um **ManualResetEvent** global chamado `connectDone`.  
  
```vb  
Public Shared Sub Connect(remoteEP As EndPoint, client As Socket)  
    client.BeginConnect(remoteEP, _  
       AddressOf ConnectCallback, client)  
  
    connectDone.WaitOne()  
End Sub 'Connect  
```  
  
```csharp  
public static void Connect(EndPoint remoteEP, Socket client) {  
    client.BeginConnect(remoteEP,
        new AsyncCallback(ConnectCallback), client );  
  
   connectDone.WaitOne();  
}  
```  
  
 O método de retorno de chamada de conexão `ConnectCallback` implementa o representante <xref:System.AsyncCallback>. Ele se conecta ao dispositivo remoto quando o dispositivo remoto está disponível e, em seguida, sinaliza ao thread de aplicativo que a conexão foi concluída definindo o **ManualResetEvent como ** `connectDone`. O código a seguir implementa o método `ConnectCallback`.  
  
```vb  
Private Shared Sub ConnectCallback(ar As IAsyncResult)  
    Try  
        ' Retrieve the socket from the state object.  
        Dim client As Socket = CType(ar.AsyncState, Socket)  
  
        ' Complete the connection.  
        client.EndConnect(ar)  
  
        Console.WriteLine("Socket connected to {0}", _  
            client.RemoteEndPoint.ToString())  
  
        ' Signal that the connection has been made.  
        connectDone.Set()  
    Catch e As Exception  
        Console.WriteLine(e.ToString())  
    End Try  
End Sub 'ConnectCallback  
```  
  
```csharp  
private static void ConnectCallback(IAsyncResult ar) {  
    try {  
        // Retrieve the socket from the state object.  
        Socket client = (Socket) ar.AsyncState;  
  
        // Complete the connection.  
        client.EndConnect(ar);  
  
        Console.WriteLine("Socket connected to {0}",  
            client.RemoteEndPoint.ToString());  
  
        // Signal that the connection has been made.  
        connectDone.Set();  
    } catch (Exception e) {  
        Console.WriteLine(e.ToString());  
    }  
}  
```  
  
 O método de exemplo `Send` codifica os dados de cadeia de caracteres especificados no formato ASCII e envia-os de forma assíncrona para o dispositivo de rede representado pelo soquete especificado. O exemplo a seguir implementa o método `Send`.  
  
```vb  
Private Shared Sub Send(client As Socket, data As [String])  
    ' Convert the string data to byte data using ASCII encoding.  
    Dim byteData As Byte() = Encoding.ASCII.GetBytes(data)  
  
    ' Begin sending the data to the remote device.  
    client.BeginSend(byteData, 0, byteData.Length, SocketFlags.None, _  
        AddressOf SendCallback, client)  
End Sub 'Send  
```  
  
```csharp  
private static void Send(Socket client, String data) {  
    // Convert the string data to byte data using ASCII encoding.  
    byte[] byteData = Encoding.ASCII.GetBytes(data);  
  
    // Begin sending the data to the remote device.  
    client.BeginSend(byteData, 0, byteData.Length, SocketFlags.None,  
        new AsyncCallback(SendCallback), client);  
}  
```  
  
 O método de retorno de chamada de envio `SendCallback` implementa o representante <xref:System.AsyncCallback>. Ele envia os dados quando o dispositivo de rede está pronto para recebê-los. O exemplo a seguir mostra a implementação do método `SendCallback`. Ele supõe que haja um **ManualResetEvent** global chamado `sendDone`.  
  
```vb  
Private Shared Sub SendCallback(ar As IAsyncResult)  
    Try  
        ' Retrieve the socket from the state object.  
        Dim client As Socket = CType(ar.AsyncState, Socket)  
  
        ' Complete sending the data to the remote device.  
        Dim bytesSent As Integer = client.EndSend(ar)  
        Console.WriteLine("Sent {0} bytes to server.", bytesSent)  
  
        ' Signal that all bytes have been sent.  
        sendDone.Set()  
    Catch e As Exception  
        Console.WriteLine(e.ToString())  
    End Try  
End Sub 'SendCallback  
```  
  
```csharp  
private static void SendCallback(IAsyncResult ar) {  
    try {  
        // Retrieve the socket from the state object.  
        Socket client = (Socket) ar.AsyncState;  
  
        // Complete sending the data to the remote device.  
        int bytesSent = client.EndSend(ar);  
        Console.WriteLine("Sent {0} bytes to server.", bytesSent);  
  
        // Signal that all bytes have been sent.  
        sendDone.Set();  
    } catch (Exception e) {  
        Console.WriteLine(e.ToString());  
    }  
}  
```  
  
 A leitura de dados de um soquete de cliente exige um objeto de estado que passa valores entre chamadas assíncronas. A classe a seguir é um objeto de estado de exemplo para recebimento dos dados de um soquete de cliente. Ele contém um campo para o soquete de cliente, um buffer para os dados recebidos e um <xref:System.Text.StringBuilder> para conter a cadeia de caracteres de dados de entrada. A colocação desses campos no objeto de estado permite que seus valores sejam preservados em várias chamadas para a leitura de dados do soquete de cliente.  
  
```vb  
Public Class StateObject  
    ' Client socket.  
    Public workSocket As Socket = Nothing
    ' Size of receive buffer.  
    Public BufferSize As Integer = 256  
    ' Receive buffer.  
    Public buffer(256) As Byte
    ' Received data string.  
    Public sb As New StringBuilder()  
End Class 'StateObject  
```  
  
```csharp  
public class StateObject {  
    // Client socket.  
    public Socket workSocket = null;  
    // Size of receive buffer.  
    public const int BufferSize = 256;  
    // Receive buffer.  
    public byte[] buffer = new byte[BufferSize];  
    // Received data string.  
    public StringBuilder sb = new StringBuilder();  
}  
```  
  
 O método `Receive` de exemplo configura o objeto de estado e, em seguida, chama o método **BeginReceive** para ler os dados do soquete de cliente de forma assíncrona. O exemplo a seguir implementa o método `Receive`.  
  
```vb  
Private Shared Sub Receive(client As Socket)  
    Try  
        ' Create the state object.  
        Dim state As New StateObject()  
        state.workSocket = client  
  
        ' Begin receiving the data from the remote device.  
        client.BeginReceive(state.buffer, 0, state.BufferSize, 0, _  
            AddressOf ReceiveCallback, state)  
    Catch e As Exception  
        Console.WriteLine(e.ToString())  
    End Try  
End Sub 'Receive  
```  
  
```csharp  
private static void Receive(Socket client) {  
    try {  
        // Create the state object.  
        StateObject state = new StateObject();  
        state.workSocket = client;  
  
        // Begin receiving the data from the remote device.  
        client.BeginReceive( state.buffer, 0, StateObject.BufferSize, 0,  
            new AsyncCallback(ReceiveCallback), state);  
    } catch (Exception e) {  
        Console.WriteLine(e.ToString());  
    }  
}  
```  
  
 O método de retorno de chamada de recebimento `ReceiveCallback` implementa o representante **AsyncCallback**. Ele recebe os dados do dispositivo de rede e cria uma cadeia de caracteres de mensagem. Ele lê um ou mais bytes de dados da rede no buffer de dados e, em seguida, chama o método **BeginReceive** novamente até que os dados enviados pelo cliente estejam completos. Depois que todos os dados são lidos do cliente, `ReceiveCallback` sinaliza ao thread de aplicativo que os dados estão completos definindo o **ManualResetEvent como ** `sendDone`.  
  
 O exemplo de código a seguir implementa o método `ReceiveCallback`. Ele supõe uma cadeia de caracteres global chamada `response` que contém a cadeia de caracteres recebida e um **ManualResetEvent** global chamado `receiveDone`. O servidor deve desligar o soquete de cliente normalmente para encerrar a sessão de rede.  
  
```vb  
Private Shared Sub ReceiveCallback(ar As IAsyncResult)  
    Try  
        ' Retrieve the state object and the client socket
        ' from the asynchronous state object.  
        Dim state As StateObject = CType(ar.AsyncState, StateObject)  
        Dim client As Socket = state.workSocket  
  
        ' Read data from the remote device.  
        Dim bytesRead As Integer = client.EndReceive(ar)  
  
        If bytesRead > 0 Then  
            ' There might be more data, so store the data received so far.  
            state.sb.Append(Encoding.ASCII.GetString(state.buffer, 0, _  
                bytesRead))  
  
            '  Get the rest of the data.  
            client.BeginReceive(state.buffer, 0, state.BufferSize, 0, _  
                AddressOf ReceiveCallback, state)  
        Else  
            ' All the data has arrived; put it in response.  
            If state.sb.Length > 1 Then  
                response = state.sb.ToString()  
            End If  
            ' Signal that all bytes have been received.  
            receiveDone.Set()  
        End If  
    Catch e As Exception  
        Console.WriteLine(e.ToString())  
    End Try  
End Sub 'ReceiveCallback  
```  
  
```csharp  
private static void ReceiveCallback( IAsyncResult ar ) {  
    try {  
        // Retrieve the state object and the client socket
        // from the asynchronous state object.  
        StateObject state = (StateObject) ar.AsyncState;  
        Socket client = state.workSocket;  
        // Read data from the remote device.  
        int bytesRead = client.EndReceive(ar);  
        if (bytesRead > 0) {  
            // There might be more data, so store the data received so far.  
            state.sb.Append(Encoding.ASCII.GetString(state.buffer,0,bytesRead));  
                //  Get the rest of the data.  
            client.BeginReceive(state.buffer,0,StateObject.BufferSize,0,  
                new AsyncCallback(ReceiveCallback), state);  
        } else {  
            // All the data has arrived; put it in response.  
            if (state.sb.Length > 1) {  
                response = state.sb.ToString();  
            }  
            // Signal that all bytes have been received.  
            receiveDone.Set();  
        }  
    } catch (Exception e) {  
        Console.WriteLine(e.ToString());  
    }  
}  
```  
  
## <a name="see-also"></a>Confira também

- [Usando um soquete de cliente síncrono](using-a-synchronous-client-socket.md)
- [Escutando com soquetes](listening-with-sockets.md)
- [Exemplo de soquete de cliente assíncrono](asynchronous-client-socket-example.md)
