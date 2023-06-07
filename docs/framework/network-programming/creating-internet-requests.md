---
title: Criando solicitações da Internet
description: Saiba como os aplicativos criam instâncias de WebRequest por meio do método WebRequest. Create, que cria uma classe derivada com base no esquema de URI passado para ela.
ms.date: 03/30/2017
helpviewer_keywords:
- WebRequest class, sending and receiving data
- Networking
- HttpWebResponse class, sending and receiving data
- requesting data from Internet, creating requests
- Network Resources
- Internet, requesting data
- data requests, creating requests
ms.assetid: faab683e-3f1e-4eee-b5e9-59f7245033d5
ms.openlocfilehash: 389c7bf5969eca4322aa680a6f28dec0b899709a
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325631"
---
# <a name="create-internet-requests"></a>Criar solicitações da Internet

Os aplicativos criam instâncias <xref:System.Net.WebRequest> por meio do método <xref:System.Net.WebRequest.Create%2A?displayProperty=nameWithType>. <xref:System.Net.WebRequest.Create%2A?displayProperty=nameWithType>é um método estático que cria uma classe derivada de <xref:System.Net.WebRequest> com base no esquema de URI passado para ela.  
  
## <a name="web-file-and-ftp-requests"></a>Solicitações de Web, arquivo e FTP

.NET Framework fornece a <xref:System.Net.HttpWebRequest> classe, derivada de `WebRequest` , para lidar com solicitações HTTP e HTTPS. Na maioria dos casos, a `WebRequest` classe fornece todas as propriedades necessárias para fazer uma solicitação; no entanto, se necessário, você pode converter `WebRequest` objetos criados pelo `WebRequest.Create` método no `HttpWebRequest` tipo para acessar as propriedades específicas de http da solicitação. Da mesma forma, o `HttpWebResponse` objeto manipula as respostas de solicitações HTTP e HTTPS. Para acessar as propriedades específicas de HTTP do `HttpWebResponse` objeto, você precisa converter `WebResponse` objetos no `HttpWebResponse` tipo.  
  
O .NET Framework também fornece <xref:System.Net.FileWebRequest> as <xref:System.Net.FileWebResponse> classes e para lidar com solicitações de recursos que usam o esquema de URI "file:". Da mesma forma, as classes <xref:System.Net.FtpWebRequest> e <xref:System.Net.FtpWebResponse> são fornecidas para manipular solicitações para recursos que usam o esquema “ftp:”. Se sua solicitação for para um recurso que usa qualquer um desses esquemas, você poderá usar o `WebRequest.Create` método para obter um objeto com o qual fazer sua solicitação.  
  
Para lidar com solicitações que usam outros protocolos de nível de aplicativo, implemente classes específicas de protocolo derivadas de `WebRequest` e `WebResponse` . Para obter mais informações, consulte [Programming Pluggable Protocols](programming-pluggable-protocols.md).  
  
## <a name="see-also"></a>Veja também

- [Como solicitar dados usando a classe WebRequest](how-to-request-data-using-the-webrequest-class.md)
- [Solicitando dados](requesting-data.md)
