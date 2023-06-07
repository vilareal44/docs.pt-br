---
title: Soquetes
description: Saiba mais sobre a classe de soquete .NET Framework, que é uma versão de código gerenciado dos serviços de soquete fornecidos pela API Winsock32.
ms.date: 03/30/2017
helpviewer_keywords:
- application protocols, sockets
- sending data, sockets
- data requests, sockets
- Windows Sockets
- sockets, about sockets
- Socket class, about Socket class
- sockets
- requesting data from Internet, sockets
- network, sockets
- receiving data, sockets
- protocols, sockets
- Internet, sockets
ms.assetid: 10d22735-bd37-42c1-a2be-c1932f979a7d
ms.openlocfilehash: b44409a0fafc770625be55881ccef3b57045acef
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502126"
---
# <a name="sockets"></a>Soquetes
O namespace <xref:System.Net.Sockets> contém uma implementação gerenciada da interface do Windows Sockets. Todas as outras classes de acesso à rede no namespace <xref:System.Net> são criadas com base nessa implementação de soquetes.  
  
 A classe <xref:System.Net.Sockets.Socket> do .NET Framework é uma versão de código gerenciado dos serviços de soquete fornecidos pela API do Winsock32. Na maioria dos casos, os métodos da classe **Socket** simplesmente realizam marshaling dos dados em seus equivalentes nativos do Win32 e manipulam as verificações de segurança necessárias.  
  
 A classe **Socket** dá suporte a dois modos básicos: síncrono e assíncrono. No modo síncrono, as chamadas a funções que executam operações de rede (como <xref:System.Net.Sockets.Socket.Send%2A> e <xref:System.Net.Sockets.Socket.Receive%2A>) aguardam até que a operação seja concluída antes de retornar o controle ao programa de chamada. No modo assíncrono, essas chamadas são retornadas imediatamente.  
  
## <a name="see-also"></a>Confira também

- [Como criar um soquete](how-to-create-a-socket.md)

- [Usando protocolos de aplicativo](using-application-protocols.md)
