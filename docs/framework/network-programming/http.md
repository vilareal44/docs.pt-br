---
title: HTTP
description: Saiba mais sobre o suporte abrangente para HTTP que o .NET Framework fornece usando as classes HttpWebRequest e HttpWebResponse.
ms.date: 03/30/2017
helpviewer_keywords:
- protocols, HTTP
- sending data, HTTP
- HttpWebResponse class, sending and receiving data
- HTTP
- receiving data, HTTP
- application protocols, HTTP
- Internet, HTTP
- network resources, HTTP
- HTTP, about HTTP
- HttpWebRequest class, sending and receiving data
ms.assetid: 985fe5d8-eb71-4024-b361-41fbdc1618d8
ms.openlocfilehash: ffb7a5d027ef7691d03caf0ac45d4a3dd9bdb652
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502412"
---
# <a name="http"></a>HTTP
O .NET Framework fornece suporte abrangente ao protocolo HTTP, que compõe a maioria de todo o tráfego da Internet, com as classes <xref:System.Net.HttpWebRequest> e <xref:System.Net.HttpWebResponse>. Essas classes, derivadas de <xref:System.Net.WebRequest> e <xref:System.Net.WebResponse>, são retornadas por padrão sempre que o método estático <xref:System.Net.WebRequest.Create%2A?displayProperty=nameWithType> encontra um URI começando com "http" ou "https". Na maioria dos casos, as classes **WebRequest** e **WebResponse** fornecem tudo o que é necessário para fazer a solicitação, mas se você precisar acessar os recursos específicos ao HTTP expostos como propriedades, poderá fazer a conversão de tipo dessas classes em **HttpWebRequest** ou **HttpWebResponse**.  
  
 **HttpWebRequest** e **HttpWebResponse** encapsulam uma transação de solicitação e resposta HTTP padrão e fornecem acesso a cabeçalhos HTTP comuns. Essas classes também dão suporte à maioria dos recursos de HTTP 1.1, incluindo pipelining, envio e recebimento de dados em partes, autenticação, pré-autenticação, criptografia, suporte a proxy, validação de certificado do servidor e gerenciamento de conexão. Cabeçalhos personalizados e cabeçalhos não fornecidos por meio de propriedades podem ser armazenados em e acessados por meio da propriedade **Headers**.  
  
 **HttpWebRequest** é a classe padrão usada pelo **WebRequest** e não precisa ser registrada antes que você possa passar um URI para o método **WebRequest**.  
  
 Você pode fazer com que o aplicativo siga redirecionamentos HTTP automaticamente definindo a propriedade <xref:System.Net.HttpWebRequest.AllowAutoRedirect%2A> como **true** (o padrão). O aplicativo redirecionará as solicitações e a propriedade <xref:System.Net.HttpWebResponse.ResponseUri%2A> de **HttpWebResponse** conterá o recurso da Web real que tiver respondido à solicitação. Se você definir **AllowAutoRedirect** para **false**, seu aplicativo deverá ser capaz de tratar redirecionamentos como erros de protocolo HTTP.  
  
 Aplicativos recebem erros de protocolo HTTP capturando uma <xref:System.Net.WebException> com o <xref:System.Net.WebException.Status%2A> definido como <xref:System.Net.WebExceptionStatus>. A propriedade <xref:System.Net.WebException.Response%2A> contém a **WebResponse** enviada pelo servidor e indica o erro HTTP real encontrado.  
  
## <a name="see-also"></a>Confira também

- [Acessando a Internet por meio de um proxy](accessing-the-internet-through-a-proxy.md)
- [Usando protocolos de aplicativo](using-application-protocols.md)
- [Como acessar propriedades específicas de HTTP](how-to-access-http-specific-properties.md)
