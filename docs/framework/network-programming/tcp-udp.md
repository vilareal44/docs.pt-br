---
title: TCP-UDP
description: Saiba como as classes TcpClient, TcpListener e UdpClient lidam com os serviços TCP e UDP, que cuidam dos detalhes da transferência de dados no .NET Framework.
ms.date: 03/30/2017
helpviewer_keywords:
- protocols, TCP/UDP
- network resources, TCP/UDP
- sending data, TCP/UDP
- TCP/UDP
- TcpClient class, about TcpClient class
- application protocols, TCP/UDP
- receiving data, TCP/UDP
- TcpListener class, about TcpListener class
- Socket class, about Socket class
- UDP
- data requests, TCP/UDP
- requesting data from Internet, TCP/UDP
- Internet, TCP/UDP
ms.assetid: df29b4b0-49e8-4923-82b9-13150dfc40f5
ms.openlocfilehash: ae6d2f9ced2235aa1b9b8fada8064d7e4be970a0
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502087"
---
# <a name="tcp-udp"></a>TCP-UDP
Os aplicativos podem usar os serviços dos protocolos TCP e UDP com as classes <xref:System.Net.Sockets.TcpClient>, <xref:System.Net.Sockets.TcpListener> e <xref:System.Net.Sockets.UdpClient>. Essas classes de protocolo são criadas com base na classe <xref:System.Net.Sockets.Socket?displayProperty=nameWithType> e cuidam dos detalhes da transferência de dados.  
  
 As classes de protocolo usam os métodos síncronos da classe **Socket** para fornecer acesso simples e direto aos serviços de rede, sem a sobrecarga de manter informações de estado ou de conhecer os detalhes da configuração de soquetes específicos ao protocolo. Para usar métodos **Socket** assíncronos, use os métodos assíncronos fornecidos pela classe <xref:System.Net.Sockets.NetworkStream>. Para acessar os recursos da classe **Socket** não expostos pelas classes de protocolo, use a classe **Socket**.  
  
 **TcpClient** e **TcpListener** representam a rede que usa a classe **NetworkStream**. Use o método <xref:System.Net.Sockets.TcpClient.GetStream%2A> para retornar o fluxo de rede e, em seguida, chamar os métodos <xref:System.Net.Sockets.NetworkStream.Read%2A> e <xref:System.Net.Sockets.NetworkStream.Write%2A> do fluxo. O **NetworkStream** não possui o soquete subjacente das classes de protocolo e, portanto, seu fechamento não afeta o soquete.  
  
 A classe **UdpClient** usa uma matriz de bytes para armazenar o datagrama UDP. Use o método <xref:System.Net.Sockets.UdpClient.Send%2A> para enviar os dados para a rede e o método <xref:System.Net.Sockets.UdpClient.Receive%2A> para receber um datagrama de entrada.  
  
## <a name="see-also"></a>Confira também

- [Usando serviços de TCP](using-tcp-services.md)
- [Usando serviços de UDP](using-udp-services.md)
- [Usando fluxos na rede](using-streams-on-the-network.md)
- [Usando um soquete de servidor assíncrono](using-an-asynchronous-server-socket.md)
- [Usando um soquete de cliente assíncrono](using-an-asynchronous-client-socket.md)
- [Usando protocolos de aplicativo](using-application-protocols.md)
