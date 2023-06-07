---
title: Solicitando dados
description: Saiba como os protocolos conectáveis permitem desenvolver aplicativos que usam uma única interface para recuperar dados de vários protocolos.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- sending data
- WebRequest class, sending and receiving data
- requesting data from Internet, about requesting data
- WebClient class, sending and receiving data
- network, requesting data
- receiving data
- sending data, about sending data
- response to Internet request, about responding to Internet requests
- data requests
- receiving data, about receiving data
- Internet, requesting data
ms.assetid: df6f1e1d-6f2a-45dd-8141-4a85c3dafe1d
ms.openlocfilehash: 19350d685a81d56657ca0a117d61b50ae24fab6a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502178"
---
# <a name="requesting-data"></a>Solicitando dados
Desenvolver aplicativos que são executados no ambiente operacional distribuído da Internet de hoje requer um método eficiente e fácil de usar para recuperar dados de recursos de todos os tipos. Protocolos conectáveis permitem desenvolver aplicativos que usam uma única interface para recuperar dados de vários protocolos de Internet.  
  
## <a name="uploading-and-downloading-data-from-an-internet-server"></a>Carregando e baixando dados de um servidor de Internet  
 Para transações simples de solicitação e resposta, a classe <xref:System.Net.WebClient> fornece o método mais fácil para carregar dados para ou fazer o download de dados de um servidor de Internet. **WebClient** fornece métodos para carregar e baixar arquivos, enviar e receber fluxos e enviar um buffer de dados para o servidor e receber uma resposta. O **WebClient** usa as classes <xref:System.Net.WebRequest> e <xref:System.Net.WebResponse> para fazer as conexões reais para o recurso de Internet, de modo que qualquer protocolo conectável registrado fica disponível para uso.  
  
 Aplicativos cliente que precisam fazer transações mais complexas solicitam dados de servidores usando a classe **WebRequest** e seus descendentes. **WebRequest** encapsula os detalhes da conexão com o servidor, do envio da solicitação do recebimento da resposta. **WebRequest** é uma classe abstrata que define um conjunto de propriedades e métodos que estão disponíveis para todos os aplicativos que usam protocolos conectáveis. Descendentes de **WebRequest**, tais como <xref:System.Net.HttpWebRequest>, implementam as propriedades e métodos definidos pelo **WebRequest** de maneira consistente com o protocolo subjacente.  
  
 A classe **WebRequest** cria instâncias específicas de protocolo de descendentes de **WebRequest**, usando o valor do URI passado para o método <xref:System.Net.WebRequest.Create%2A> a fim de determinar a instância da classe derivada específica para criar. Aplicativos indicam qual descendente de **WebRequest** deve ser usado para lidar com uma solicitação ao registrar o construtor do descendente com o método <xref:System.Net.WebRequest.RegisterPrefix%2A?displayProperty=nameWithType>.  
  
 É feita uma solicitação para o recurso de Internet chamando o método <xref:System.Net.WebRequest.GetResponse%2A> no **WebRequest**. O método **GetResponse** constrói a solicitação específica de protocolo nas propriedades da **WebRequest**, faz a conexão de soquete TCP ou UDP para o servidor e envia a solicitação. Para solicitações que enviam dados para o servidor, tais como solicitações HTTP **Post** ou FTP **Put**, o método <xref:System.Net.WebRequest.GetRequestStream%2A?displayProperty=nameWithType> fornece um fluxo de rede no qual enviar os dados.  
  
 O método **GetResponse** retorna uma **WebResponse** específica de protocolo que corresponde a **WebRequest.**  
  
 A classe **WebResponse** é também uma classe abstrata que define propriedades e métodos que estão disponíveis para todos os aplicativos que usam protocolos conectáveis. Descendentes de **WebResponse** implementam essas propriedades e métodos para o protocolo subjacente. A classe <xref:System.Net.HttpWebResponse>, por exemplo, implementa a classe **WebResponse** para HTTP.  
  
 Os dados retornados pelo servidor são apresentados para o aplicativo no fluxo retornado pelo método <xref:System.Net.WebResponse.GetResponseStream%2A?displayProperty=nameWithType>. Você pode usar esse fluxo como qualquer outro, conforme mostrado no exemplo a seguir.  
  
```csharp  
StreamReader sr =  
   new StreamReader(resp.GetResponseStream(), Encoding.ASCII);  
```  
  
```vb  
Dim sr As StreamReader  
sr = New StreamReader(resp.GetResponseStream(), Encoding.ASCII)  
```  
  
## <a name="see-also"></a>Confira também

- [Programação de rede no .NET Framework](index.md)
- [Como solicitar uma página da Web e recuperar os resultados como um fluxo](how-to-request-a-web-page-and-retrieve-the-results-as-a-stream.md)
- [Como recuperar uma WebResponse específica de protocolo que corresponde a uma WebRequest](how-to-retrieve-a-protocol-specific-webresponse-that-matches-a-webrequest.md)
