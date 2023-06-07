---
title: Usando fluxos na rede
description: O .NET Framework representa recursos de rede como fluxos. A classe NetworkStream implementa a classe Stream para uso com recursos de rede.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- requesting data from Internet, streams
- Networking
- response to Internet request, streams
- network resources, stream capabilities
- receiving data, stream capabilities
- Network Resources
- sending data, stream capabilities
- downloading Internet resources, streams
- streams, capabilities
- Internet, streams
- streams
ms.assetid: 02b05fba-7235-45ce-94e5-060436ee0875
ms.openlocfilehash: f8d35b43c9b46a77bfd0c78f7d0118093b6fe824
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501970"
---
# <a name="using-streams-on-the-network"></a>Usando fluxos na rede
Os recursos de rede são representados no .NET Framework como fluxos. Tratando os fluxos de forma genérica, o .NET Framework oferece as seguintes funcionalidades:  
  
- Uma maneira comum de enviar e receber dados da Web. Seja qual for o conteúdo real do arquivo – HTML, XML ou qualquer outra coisa –, o aplicativo usará <xref:System.IO.Stream.Write%2A?displayProperty=nameWithType> e <xref:System.IO.Stream.Read%2A?displayProperty=nameWithType> para enviar e receber dados.  
  
- Compatibilidade com fluxos no .NET Framework. Os fluxos são usados em todo o .NET Framework, que tem uma infraestrutura avançada para manipulá-los. Por exemplo, é possível modificar um aplicativo que lê dados XML de um <xref:System.IO.FileStream> para ler dados de um <xref:System.Net.Sockets.NetworkStream> alterando apenas as poucas linhas de código que inicializam o fluxo. As principais diferenças entre a classe **NetworkStream** e outros fluxos são que **NetworkStream** não é pesquisável, a propriedade <xref:System.Net.Sockets.NetworkStream.CanSeek%2A> sempre retorna **false** e os métodos <xref:System.Net.Sockets.NetworkStream.Seek%2A> e <xref:System.Net.Sockets.NetworkStream.Position%2A> geram uma <xref:System.NotSupportedException>.  
  
- Processamento de dados conforme eles são recebidos. Os fluxos fornecem acesso aos dados conforme eles são recebidos da rede, em vez de forçar o aplicativo a aguardar que um conjunto de dados inteiro seja baixado.  
  
 O namespace <xref:System.Net.Sockets> contém uma classe **NetworkStream** que implementa a classe <xref:System.IO.Stream> especificamente para uso com recursos de rede. As classes do namespace <xref:System.Net.Sockets> usam a classe **NetworkStream** para representar fluxos.  
  
 Para enviar dados para a rede usando o fluxo retornado, chame <xref:System.Net.WebRequest.GetRequestStream%2A> na <xref:System.Net.WebRequest>. O **WebRequest** enviará cabeçalhos de solicitação ao servidor; em seguida, você pode enviar dados para o recurso de rede chamando o <xref:System.IO.Stream.BeginWrite%2A> <xref:System.IO.Stream.EndWrite%2A> método, ou <xref:System.IO.Stream.Write%2A> no fluxo retornado. Alguns protocolos, como HTTP, podem exigir a definição de propriedades específicas ao protocolo antes de enviar dados. O exemplo de código a seguir mostra como definir propriedades específicas de HTTP para envio de dados. Ele supõe que a variável `sendData` contém os dados a serem enviados e que a variável `sendLength` é o número de bytes de dados a serem enviados.  
  
```csharp  
HttpWebRequest request =
   (HttpWebRequest) WebRequest.Create("http://www.contoso.com/");  
request.Method = "POST";  
request.ContentLength = sendLength;  
try  
{  
   Stream sendStream = request.GetRequestStream();  
   sendStream.Write(sendData,0,sendLength);  
   sendStream.Close();  
}  
catch  
{  
   // Handle errors . . .  
}  
```  
  
```vb  
Dim request As HttpWebRequest = _  
   CType(WebRequest.Create("http://www.contoso.com/"), HttpWebRequest)  
request.Method = "POST"  
request.ContentLength = sendLength  
Try  
   Dim sendStream As Stream = request.GetRequestStream()  
   sendStream.Write(sendData, 0, sendLength)  
   sendStream.Close()  
Catch  
   ' Handle errors . . .  
End Try  
```  
  
 Para receber dados da rede, chame <xref:System.Net.WebResponse.GetResponseStream%2A> na <xref:System.Net.WebResponse>. Em seguida, você poderá ler dados do recurso de rede chamando o método <xref:System.IO.Stream.BeginRead%2A>, <xref:System.IO.Stream.EndRead%2A> ou <xref:System.IO.Stream.Read%2A> no fluxo retornado.  
  
 Ao usar fluxos em recursos de rede, tenha em mente os seguintes pontos:  
  
- A propriedade **CanSeek** sempre retorna **false**, pois a classe **NetworkStream** não pode mudar de posição no fluxo. Os métodos **Seek** e **Position** geram uma **NotSupportedException**.  
  
- Quando você usa **WebRequest** e **WebResponse**, as instâncias de fluxo criadas com a chamada a **GetResponseStream** são somente leitura e as instâncias de fluxo criadas com a chamada a **GetRequestStream** são somente gravação.  
  
- Use a classe <xref:System.IO.StreamReader> para facilitar a codificação. O exemplo de código a seguir usa um **StreamReader** para ler um fluxo codificado em ASCII de uma **WebResponse** (o exemplo não mostra a criação da solicitação).  
  
- A chamada a **GetResponse** poderá ser bloqueada se os recursos de rede não estiverem disponíveis. Considere o uso de uma solicitação assíncrona com os métodos <xref:System.Net.WebRequest.BeginGetResponse%2A> e <xref:System.Net.WebRequest.EndGetResponse%2A>.  
  
- A chamada a **GetRequestStream** pode ser bloqueada enquanto a conexão com o servidor é criada. Considere o uso de uma solicitação assíncrona para o fluxo com os métodos <xref:System.Net.WebRequest.BeginGetRequestStream%2A> e <xref:System.Net.WebRequest.EndGetRequestStream%2A>.  
  
```csharp  
// Create a response object.  
WebResponse response = request.GetResponse();  
// Get a readable stream from the server.  
StreamReader sr =
   new StreamReader(response.GetResponseStream(), Encoding.ASCII);  
// Use the stream. Remember when you are through with the stream to close it.  
sr.Close();  
```  
  
```vb  
' Create a response object.  
Dim response As WebResponse = request.GetResponse()  
' Get a readable stream from the server.  
Dim sr As _
   New StreamReader(response.GetResponseStream(), Encoding.ASCII)  
' Use the stream. Remember when you are through with the stream to close it.  
sr.Close()  
```  
  
## <a name="see-also"></a>Confira também

- [Como solicitar dados usando a classe WebRequest](how-to-request-data-using-the-webrequest-class.md)
- [Solicitando dados](requesting-data.md)
