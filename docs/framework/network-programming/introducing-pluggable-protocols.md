---
title: Apresentando protocolos conectáveis
description: Saiba mais sobre protocolos conectáveis, que dão suporte ao desenvolvimento de aplicativos que usam recursos da Internet, independentemente dos detalhes do protocolo que os recursos usam.
ms.date: 03/30/2017
helpviewer_keywords:
- data requests, pluggable protocols
- WebRequest class, pluggable protocols
- response to Internet request, pluggable protocols
- URI
- Windows Sockets
- request/response model
- sending data, pluggable protocols
- pluggable protocols
- WebClient class, about WebClient class
- pluggable protocols, about pluggable protocols
- Internet, pluggable protocols
- path identifiers
- Uniform Resource Identifier
- application development [.NET Framework], pluggable protocols
- requesting data from Internet, pluggable protocols
- receiving data, pluggable protocols
- protocols, pluggable
- server identifiers
- scheme identifiers
ms.assetid: 4b48e22d-e4e5-48f0-be80-d549bda97415
ms.openlocfilehash: 0bc2d0d005e50b04aff360866a146f6fe6b0ea02
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502347"
---
# <a name="introducing-pluggable-protocols"></a>Apresentando protocolos conectáveis
O Microsoft .NET Framework fornece uma implementação dos serviços de Internet em camadas, extensível e gerenciada que pode ser rápida e facilmente integrada aos aplicativos. Classes de acesso à Internet nos namespaces <xref:System.Net> e <xref:System.Net.Sockets> podem ser usadas para implementar tanto aplicativos baseados na Web quanto baseados na Internet.  
  
## <a name="internet-applications"></a>Aplicativos da Internet  
 Aplicativos da Internet podem ser classificados amplamente em dois tipos: aplicativos cliente que solicitam informações e aplicativos para servidores que respondem às solicitações de informações feitas por clientes. O aplicativo cliente-servidor clássico de Internet é a World Wide Web, em que as pessoas usam navegadores para acessar documentos e outros dados armazenados em servidores Web em todo o mundo.  
  
 Os aplicativos não se limitam a apenas uma dessas funções; por exemplo, o servidor de aplicativos familiar de camada intermediária responde a solicitações de clientes solicitando dados em outro servidor, caso em que ele está atuando como um servidor e também como um cliente.  
  
 O aplicativo cliente faz uma solicitação identificando o recurso da Internet solicitado e o protocolo de comunicação a usar para a solicitação e resposta. Se necessário, o cliente também fornece quaisquer dados adicionais necessários para concluir a solicitação, assim como autenticação ou local de informações de proxy (nome de usuário, senha e assim por diante). Depois que a solicitação é formada, ela pode ser enviada ao servidor.  
  
## <a name="identifying-resources"></a>Identificar os recursos  
 O .NET Framework usa URI (Uniform Resource Identifier) para identificar o protocolo de comunicação e recursos de Internet solicitado. O URI consiste em pelo menos três, possivelmente quatro fragmentos: o identificador do esquema, que identifica o protocolo de comunicação de solicitação e resposta; o identificador do servidor, que consiste em um nome de host do sistema de nome de domínio (DNS) ou um endereço TCP que identifica exclusivamente o servidor na Internet; o identificador do caminho, que localiza as informações solicitadas no servidor e, por fim, uma cadeia de caracteres de consulta opcional, que passa informações do cliente para o servidor. Por exemplo, o URI `http://www.contoso.com/whatsnew.aspx?date=today` é composto pelo identificador de esquema `http`, o identificador de servidor `www.contoso.com`, o caminho `/whatsnew.aspx` e a cadeia de caracteres de consulta `?date=today`.  
  
 Após o servidor ter recebido a solicitação e processado a resposta, ele retorna a resposta ao aplicativo cliente. A resposta inclui informações complementares, como o tipo de conteúdo (texto não processado ou dados XML, por exemplo).  
  
## <a name="requests-and-responses-in-the-net-framework"></a>Solicitações e respostas no .NET Framework  
 O .NET Framework usa classes específicas para fornecer as três partes de informações necessárias para acessar recursos da Internet através de um modelo de solicitação/resposta: a classe <xref:System.Uri>, que contém o URI do recurso de Internet que você está procurando; a classe <xref:System.Net.WebRequest>, que contém uma solicitação para o recurso e, por fim, a classe <xref:System.Net.WebResponse>, que fornece um contêiner para a resposta de entrada.  
  
 Aplicativos cliente criam instâncias de `WebRequest`, passando o URI do recurso de rede para o método <xref:System.Net.WebRequest.Create%2A>. Esse método estático cria um `WebRequest` para um protocolo específico, tal como HTTP. O `WebRequest` que é retornado fornece acesso às propriedades que controlam ambos a solicitação para o servidor e o acesso ao fluxo de dados que é enviado quando a solicitação é feita. O método <xref:System.Net.WebRequest.GetResponse%2A> no `WebRequest` envia a solicitação do aplicativo cliente para o servidor identificado no URI. Em casos em que a resposta pode ser atrasada, a solicitação pode ser feita de forma assíncrona usando o método <xref:System.Net.WebRequest.BeginGetResponse%2A> no **WebRequest** e a resposta pode ser retornada em um momento posterior usando o método <xref:System.Net.WebRequest.EndGetResponse%2A>.  
  
 Os métodos **GetResponse** e **EndGetResponse** retornam um **WebResponse** que fornece acesso aos dados retornados pelo servidor. Já que esses dados são fornecidos ao aplicativo solicitante como um fluxo pelo método <xref:System.Net.WebResponse.GetResponseStream%2A>, eles podem ser usados em um aplicativo em qualquer lugar em que fluxos de dados são usados.  
  
 As classes **WebRequest** e **WebResponse** são a base dos protocolos conectáveis – uma implementação de serviços de rede que permite que você desenvolva aplicativos que usam recursos de Internet sem se preocupar com detalhes específicos do protocolo usado por cada recurso. Classes descendentes de **WebRequest** são registradas com a classe **WebRequest** para gerenciar os detalhes de fazer as verdadeiras conexões aos recursos da Internet.  
  
 Por exemplo, a classe <xref:System.Net.HttpWebRequest> gerencia os detalhes de se conectar a um recurso da Internet usando HTTP. Por padrão, quando o método **WebRequest.Create** encontra um URI que começa com "http:" ou "https:" (os identificadores de protocolo para HTTP e HTTP seguro), o **WebRequest** que é retornado pode ser usado como está ou ele pode passar por conversão de tipo para **HttpWebRequest** a fim de acessar propriedades específicas de protocolo. Na maioria dos casos, o **WebRequest** fornece todas as informações necessárias para fazer uma solicitação.  
  
 Qualquer protocolo que pode ser representado como uma transação de solicitação/resposta pode ser usado em uma **WebRequest**. Você pode derivar classes específicas de protocolo de **WebRequest** e **WebResponse** e, em seguida, registrá-las para uso pelo aplicativo com o método <xref:System.Net.WebRequest.RegisterPrefix%2A?displayProperty=nameWithType> estático.  
  
 Quando a autorização do cliente para solicitações de Internet é necessária, a propriedade <xref:System.Net.WebRequest.Credentials%2A> do **WebRequest** fornece as credenciais necessárias. Essas credenciais podem ser um par nome/senha simples para HTTP básico ou autenticação digest ou um conjunto de nome/senha/domínio para autenticação NTLM ou Kerberos. Um conjunto de credenciais pode ser armazenado em uma instância de <xref:System.Net.NetworkCredential> ou vários conjuntos podem ser armazenados simultaneamente em uma instância de <xref:System.Net.CredentialCache>. O **CredentialCache** usa o URI da solicitação e o esquema de autenticação com suporte no servidor para determinar quais credenciais devem ser enviados ao servidor.  
  
## <a name="simple-requests-with-webclient"></a>Solicitações simples com WebClient  
 Para aplicativos que precisam fazer solicitações simples para recursos da Internet, a classe <xref:System.Net.WebClient> fornece métodos comuns para carregar dados para ou baixar dados de um servidor de Internet. O **WebClient** depende da classe **WebRequest** para fornecer acesso aos recursos da Internet; portanto, a classe **WebClient** pode usar qualquer protocolo conectável registrado.  
  
 Para aplicativos que não podem usar o modelo de solicitação/resposta, ou para aplicativos que precisam escutar na rede, bem como solicitações de envio, o namespace **System .net. Sockets** fornece as <xref:System.Net.Sockets.TcpClient> <xref:System.Net.Sockets.TcpListener> classes, e <xref:System.Net.Sockets.UdpClient> . Essas classes tratam dos detalhes de fazer conexões usando protocolos de transporte diferentes e expõem a conexão de rede para o aplicativo como um fluxo.  
  
 Os desenvolvedores familiarizados com a interface do Windows Sockets ou aqueles que precisam de controle fornecido por programação no nível de soquete descobrirão que as classes **Sockets** satisfazem suas necessidades. As classes **Sockets** são um ponto de transição de código gerenciado para código nativo dentro das classes **System.Net**. Na maioria dos casos, classes **Sockets** realizam marshaling de dados em seus equivalentes do Windows de 32 bits, além de lidarem com quaisquer verificações de segurança necessárias.  
  
## <a name="see-also"></a>Confira também

- [Programando protocolos conectáveis](programming-pluggable-protocols.md)
- [Programação de rede no .NET Framework](index.md)
- [Amostras de programação de rede](network-programming-samples.md)
