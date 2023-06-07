---
title: Melhores práticas para classes System.Net
description: Siga estas recomendações para usar as classes contidas no System.Net para sua melhor vantagem em .NET Framework programação.
ms.date: 03/30/2017
helpviewer_keywords:
- sending data, best practices
- requesting data from Internet, best practices
- WebRequest class, best practices
- data requests, best practices
- WebResponse class, best practices
- best practices, data requests
- receiving data, best practices
ms.assetid: 716decc6-5952-47b7-9c5a-ba6fc5698684
ms.openlocfilehash: 583fa5e57c7c4d60252dddfd425596e7acad7c0d
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502672"
---
# <a name="best-practices-for-systemnet-classes"></a>Melhores práticas para classes System.Net
As seguintes recomendações ajudarão você a usar as classes contidas em <xref:System.Net> para seu melhor proveito:  
  
- Para obter as melhores práticas do protocolo TLS, confira [Melhores práticas do protocolo TLS com o .NET Framework](tls.md).

- Use <xref:System.Net.WebRequest> e <xref:System.Net.WebResponse> sempre que possível, em vez da conversão de tipo para classes descendentes. Os aplicativos que usam **WebRequest** e **WebResponse** podem aproveitar os novos protocolos da Internet sem a necessidade de alterações de código extensas.  
  
- Ao escrever aplicativos ASP.NET executados em um servidor usando as classes **System.Net**, geralmente é melhor, de um ponto de vista de desempenho, usar os métodos assíncronos para <xref:System.Net.WebRequest.GetResponse%2A> e <xref:System.Net.WebResponse.GetResponseStream%2A>.  
  
- O número de conexões abertas para um recurso da Internet pode ter um impacto significativo no desempenho da rede e na taxa de transferência. **System.Net** usa duas conexões por aplicativo e host, por padrão. A configuração da propriedade <xref:System.Net.ServicePoint.ConnectionLimit%2A> no <xref:System.Net.ServicePoint> para o aplicativo pode aumentar esse número para um host específico. A configuração da propriedade <xref:System.Net.ServicePointManager.DefaultPersistentConnectionLimit?displayProperty=nameWithType> pode aumentar esse padrão para todos os hosts.  
  
- Ao gravar protocolos no nível do soquete, tente usar <xref:System.Net.Sockets.TcpClient> ou <xref:System.Net.Sockets.UdpClient> sempre que possível, em vez de gravar diretamente em um <xref:System.Net.Sockets.Socket>. Essas duas classes de cliente encapsulam a criação de soquetes TCP e UDP sem a necessidade de lidar com os detalhes da conexão.  
  
- Ao acessar sites que exigem credenciais, use a classe <xref:System.Net.CredentialCache> para criar um cache de credenciais, em vez de fornecê-las com cada solicitação. A classe **CredentialCache** procura o cache para localizar as credenciais apropriadas a serem apresentadas com uma solicitação, liberando-o da responsabilidade de criar e apresentar credenciais baseadas na URL.  
  
## <a name="see-also"></a>Confira também

- [Programação de rede no .NET Framework](index.md)
