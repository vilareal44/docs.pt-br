---
title: Escutando com soquetes
description: Saiba como criar um serviço remoto, em que um soquete de servidor abre uma porta na rede e aguarda que um cliente se conecte a essa porta.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- application protocols, sockets
- sending data, sockets
- sockets, listening with sockets
- data requests, sockets
- requesting data from Internet, sockets
- receiving data, sockets
- protocols, sockets
- listening with sockets
- Internet, sockets
ms.assetid: 40e426cc-13db-4371-95eb-f7388bd23ebf
ms.openlocfilehash: 0b6de67772bae397373e307ec02ce69a71b0542e
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502308"
---
# <a name="listening-with-sockets"></a>Escutando com soquetes
Soquetes de ouvinte ou de servidor abrem uma porta na rede e aguardam até que um cliente se conecte a essa porta. Embora existam outros protocolos e famílias de endereços de rede, este exemplo mostra como criar um serviço remoto para uma rede TCP/IP.  
  
 O endereço exclusivo de um serviço de TCP/IP é definido combinando-se o endereço IP do host com o número da porta do serviço para criar um ponto de extremidade para o serviço. A classe <xref:System.Net.Dns> fornece métodos que retornam informações sobre os endereços de rede com suporte pelo dispositivo de rede local. Quando o dispositivo de rede local tem mais de um endereço de rede ou se o sistema local dá suporte a mais de um dispositivo de rede, a classe **Dns** retorna informações sobre todos os endereços de rede e o aplicativo deve escolher o endereço adequado para o serviço. A IANA (Internet Assigned Numbers Authority) define números de porta para serviços comuns; para obter mais informações, consulte [nome do serviço e registro do número da porta do protocolo de transporte](https://www.iana.org/assignments/port-numbers). Outros serviços podem ter números de porta registrados no intervalo de 1.024 a 65.535.  
  
 O exemplo a seguir cria um <xref:System.Net.IPEndPoint> para um servidor ao combinar o primeiro endereço IP retornado pelo **Dns** para o computador host com um número da porta escolhido dentre o intervalo de números da porta registrados.  
  
```vb  
Dim ipHostInfo As IPHostEntry = Dns.GetHostEntry(Dns.GetHostName())  
Dim ipAddress As IPAddress = ipHostInfo.AddressList(0)  
Dim localEndPoint As New IPEndPoint(ipAddress, 11000)  
```  
  
```csharp  
IPHostEntry ipHostInfo = Dns.GetHostEntry(Dns.GetHostName());  
IPAddress ipAddress = ipHostInfo.AddressList[0];  
IPEndPoint localEndPoint = new IPEndPoint(ipAddress, 11000);  
```  
  
 Depois que o ponto de extremidade local é determinado, o <xref:System.Net.Sockets.Socket> deve ser associado a esse ponto de extremidade usando o método <xref:System.Net.Sockets.Socket.Bind%2A> e definido para escutar no ponto de extremidade usando o método <xref:System.Net.Sockets.Socket.Listen%2A>. **Bind** lança uma exceção se a combinação específica de endereço e porta já está em uso. O exemplo a seguir demonstra a associação de um **soquete** a um **IPEndPoint**.  
  
```vb  
Dim listener As New Socket(ipAddress.AddressFamily, _  
    SocketType.Stream, ProtocolType.Tcp)
listener.Bind(localEndPoint)  
listener.Listen(100)  
```  
  
```csharp  
Socket listener = new Socket(ipAddress.AddressFamily,
    SocketType.Stream, ProtocolType.Tcp);
listener.Bind(localEndPoint);  
listener.Listen(100);  
```  
  
 O método **Listen** usa um único parâmetro que especifica quantas conexões pendentes para o **soquete** são permitidas antes que um erro de servidor ocupado seja retornado para o cliente em processo de conexão. Nesse caso, até 100 clientes são colocados na fila de conexão antes que uma resposta de servidor ocupado seja retornada ao cliente número 101.  
  
## <a name="see-also"></a>Confira também

- [Usando um soquete de servidor síncrono](using-a-synchronous-server-socket.md)
- [Usando um soquete de servidor assíncrono](using-an-asynchronous-server-socket.md)
- [Usando soquetes do cliente](using-client-sockets.md)
- [Como criar um soquete](how-to-create-a-socket.md)
- [Soquetes](sockets.md)
