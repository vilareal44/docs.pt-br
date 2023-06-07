---
title: Programando protocolos conectáveis
description: Saiba como as classes abstract WebRequest e WebResponse dão suporte a protocolos conectáveis, que permitem que um aplicativo obtenha dados sem especificar um protocolo.
ms.date: 03/30/2017
helpviewer_keywords:
- downloading Internet resources, pluggable protocols
- WebRequest class, pluggable protocols
- response to Internet request, pluggable protocols
- WebResponse class, pluggable protocols
- sending data, pluggable protocols
- network resources, pluggable protocols
- Internet, pluggable protocols
- programming pluggable protocols
- pluggable protocols, programming
- requesting data from Internet, pluggable protocols
- receiving data, pluggable protocols
- protocols, pluggable
ms.assetid: 66ef8456-7576-4e97-8956-959b216373db
ms.openlocfilehash: 510f616295abc13d93e0e0af5a37aca097d343e3
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502191"
---
# <a name="programming-pluggable-protocols"></a>Programando protocolos conectáveis
As classes abstratas <xref:System.Net.WebRequest> e <xref:System.Net.WebResponse> fornecem a base para protocolos conectáveis. Derivando classes específicas de protocolo de <xref:System.Net.WebRequest> e de <xref:System.Net.WebResponse>, um aplicativo pode solicitar dados de um recurso de Internet e ler a resposta sem especificar o protocolo que está sendo usado.  
  
 Antes de criar um <xref:System.Net.WebRequest> específico de protocolo, você deve registrar o respectivo método Create. Use o método <xref:System.Net.WebRequest.RegisterPrefix%28System.String%2CSystem.Net.IWebRequestCreate%29> estático de <xref:System.Net.WebRequest> para registrar um <xref:System.Net.WebRequest> descendente para lidar com um conjunto de solicitações para um determinado esquema de Internet, para um esquema e servidor ou para um esquema, servidor e caminho.  
  
 Na maioria dos casos, você será capaz de enviar e receber dados usando os métodos e propriedades da classe <xref:System.Net.WebRequest>. No entanto, se você precisar acessar propriedades específicas de protocolo, você pode fazer conversão de tipo de um <xref:System.Net.WebRequest> para uma instância de classe derivada específica.  
  
 Para tirar proveito dos protocolos conectáveis, seus descendentes de <xref:System.Net.WebRequest> devem fornecer uma transação de solicitação e resposta padrão que não requer que propriedades específicas de protocolo sejam definidas. Por exemplo, a classe <xref:System.Net.HttpWebRequest> que implementa a classe <xref:System.Net.WebRequest> para HTTP, fornece uma solicitação `GET` por padrão e retorna um <xref:System.Net.HttpWebResponse> que contém o fluxo retornado do servidor Web.  
  
## <a name="see-also"></a>Confira também

- [Derivando de WebRequest](deriving-from-webrequest.md)
- [Derivando de WebResponse](deriving-from-webresponse.md)
- [Programação de rede no .NET Framework](index.md)
- [Como fazer a conversão de tipo de uma WebRequest para acessar propriedades específicas de protocolo](how-to-typecast-a-webrequest-to-access-protocol-specific-properties.md)
