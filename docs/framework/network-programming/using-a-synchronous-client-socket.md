---
title: Usando um soquete de cliente síncrono
description: Este exemplo mostra um soquete de cliente síncrono no .NET Framework, que suspende o programa de aplicativo enquanto a operação de rede é concluída.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- application protocols, sockets
- sending data, sockets
- data requests, sockets
- requesting data from Internet, sockets
- synchronous client sockets
- Socket class, synchronous client sockets
- receiving data, sockets
- sockets, synchronous client sockets
- protocols, sockets
- Internet, sockets
- client sockets
ms.assetid: 945d00c6-7202-466c-9df9-140b84156d43
ms.openlocfilehash: ef682af33c10cf06ffc398c22e4a7dc1adf8290e
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502061"
---
# <a name="using-a-synchronous-client-socket"></a>Usando um soquete de cliente síncrono
Um soquete de cliente síncrona suspende o programa de aplicativo enquanto a operação de rede é concluída. Soquetes síncronos não são adequados para aplicativos que fazem uso intenso da rede para sua operação, mas podem permitir o acesso simples aos serviços de rede para outros aplicativos.  
  
 Para enviar dados, passe uma matriz de bytes para um dos métodos de envio de dados da classe <xref:System.Net.Sockets.Socket> (<xref:System.Net.Sockets.Socket.Send%2A> e <xref:System.Net.Sockets.Socket.SendTo%2A>). O exemplo a seguir codifica uma cadeia de caracteres em um buffer de matriz de bytes usando a propriedade <xref:System.Text.Encoding.ASCII%2A?displayProperty=nameWithType> e, em seguida, transmite o buffer para o dispositivo de rede usando o método **Send**. O método **Send** retorna o número de bytes enviados para o dispositivo de rede.  
  
```vb  
Dim msg As Byte() = _  
    System.Text.Encoding.ASCII.GetBytes("This is a test.")  
Dim bytesSent As Integer = s.Send(msg)  
```  
  
```csharp  
byte[] msg = System.Text.Encoding.ASCII.GetBytes("This is a test");  
int bytesSent = s.Send(msg);  
```  
  
 O método **Send** remove os bytes do buffer e coloca-os na fila com o adaptador de rede a ser enviado para o dispositivo de rede. O adaptador de rede pode não enviar os dados imediatamente, mas ele os enviará no final, desde que a conexão seja fechada normalmente com o método <xref:System.Net.Sockets.Socket.Shutdown%2A>.  
  
 Para receber dados de um dispositivo de rede, passe um buffer para um dos métodos de recebimento de dados da classe **Socket** (<xref:System.Net.Sockets.Socket.Receive%2A> e <xref:System.Net.Sockets.Socket.ReceiveFrom%2A>). Os soquetes síncronos suspenderão o aplicativo até que os bytes sejam recebidos da rede ou até que o soquete seja fechado. O exemplo a seguir recebe dados da rede e, em seguida, exibe-o no console. O exemplo supõe que os dados provenientes da rede são um texto codificado em ASCII. O método **Receive** retorna o número de bytes recebidos da rede.  
  
```vb  
Dim bytes(1024) As Byte  
Dim bytesRec = s.Receive(bytes)  
Console.WriteLine("Echoed text = {0}", _  
    System.Text.Encoding.ASCII.GetString(bytes, 0, bytesRec))  
```  
  
```csharp  
byte[] bytes = new byte[1024];  
int bytesRec = s.Receive(bytes);  
Console.WriteLine("Echoed text = {0}",  
    System.Text.Encoding.ASCII.GetString(bytes, 0, bytesRec));  
```  
  
 Quando o soquete não for mais necessário, você precisará liberá-lo chamando o método <xref:System.Net.Sockets.Socket.Shutdown%2A> e, em seguida, chamando o método **Close**. O exemplo a seguir libera um **Socket**. A enumeração <xref:System.Net.Sockets.SocketShutdown> define constantes que indicam se o soquete deve ser fechado para envio, recebimento ou para ambos.  
  
```vb  
s.Shutdown(SocketShutdown.Both)  
s.Close()  
```  
  
```csharp  
s.Shutdown(SocketShutdown.Both);  
s.Close();  
```  
  
## <a name="see-also"></a>Confira também

- [Usando um soquete de cliente assíncrono](using-an-asynchronous-client-socket.md)
- [Escutando com soquetes](listening-with-sockets.md)
- [Exemplo de soquete de cliente síncrono](synchronous-client-socket-example.md)
