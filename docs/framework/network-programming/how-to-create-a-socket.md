---
title: Como criar um soquete
description: Saiba como inicializar um soquete para se comunicar com dispositivos remotos. Use a classe Socket para especificar a família de endereços, o tipo de soquete e o tipo de protocolo.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- application protocols, sockets
- Networking
- sending data, sockets
- data requests, sockets
- requesting data from Internet, sockets
- Socket class, creating sockets
- Network Resources
- receiving data, sockets
- protocols, sockets
- Internet, sockets
- sockets, creating
ms.assetid: c64a049c-5981-43bc-a2dc-1851473589c7
ms.openlocfilehash: 1d56ddea721b54192a7dd47d144b6c41bbb9a5d7
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502542"
---
# <a name="how-to-create-a-socket"></a>Como criar um soquete
Antes de poder usar um soquete para se comunicar com dispositivos remotos, o soquete deve ser inicializado com as informações de protocolo e endereço de rede. O construtor da classe <xref:System.Net.Sockets.Socket> tem parâmetros que especificam a família de endereços, o tipo de soquete e o tipo de protocolo usado pelo soquete para fazer conexões.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir cria um Socket que pode ser usado para se comunicar em uma rede baseada em TCP/IP, como a Internet.  
  
```csharp  
Socket s = new Socket(AddressFamily.InterNetwork,
   SocketType.Stream, ProtocolType.Tcp);  
```  
  
```vb  
Dim s as New Socket(AddressFamily.InterNetwork, _  
   SocketType.Stream, ProtocolType.Tcp)  
```  
  
 Para usar o UDP em vez do TCP, altere o tipo de protocolo, como no seguinte exemplo:  
  
```csharp  
Socket s = new Socket(AddressFamily.InterNetwork,
   SocketType.Dgram, ProtocolType.Udp);  
```  
  
```vb  
Dim s as New Socket(AddressFamily.InterNetwork, _  
   SocketType.Dgram, ProtocolType.Udp)  
```  
  
 A enumeração <xref:System.Net.Sockets.AddressFamily> especifica as famílias de endereços padrão usadas pela classe **Socket** para resolver endereços de rede (por exemplo, o membro **AddressFamily.InterNetwork** especifica a família de endereços IP versão 4).  
  
 A enumeração <xref:System.Net.Sockets.SocketType> especifica o tipo de soquete (por exemplo, o membro **SocketType.Stream** indica um soquete padrão para envio e recebimento de dados com o controle de fluxo).  
  
 A enumeração <xref:System.Net.Sockets.ProtocolType> especifica o protocolo de rede a ser usado ao se comunicar no **Socket** (por exemplo, **ProtocolType.Tcp** indica que o soquete usa TCP; **ProtocolType.Udp** indica que o soquete usa UDP).  
  
 Depois que um **Socket** for criado, ele poderá iniciar uma conexão com um ponto de extremidade remoto ou receber conexões de dispositivos remotos.  
  
## <a name="see-also"></a>Confira também

- [Usando soquetes do cliente](using-client-sockets.md)
- [Escutando com soquetes](listening-with-sockets.md)
