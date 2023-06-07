---
title: Bibliotecas de classes e APIs adicionais
description: Explore bibliotecas de classes e APIs adicionais no .NET, incluindo projetos de OOB (fora de banda), bibliotecas específicas de plataforma e APIs privadas.
ms.date: 08/11/2020
helpviewer_keywords:
- Additional class libraries
- Additional managed libraries
- .NET Framework out-of-band releases
- out-of-band releases
ms.assetid: cf2d9006-b631-4e5d-81cd-20aab78c60f1
ms.topic: conceptual
ms.openlocfilehash: c6404df5d4f0be381bc0a9c1924fcf82cf078306
ms.sourcegitcommit: 70d6a7e4f7187cbfa332f0f8be76566f7828cfcd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88075469"
---
# <a name="additional-class-libraries-and-apis"></a>Bibliotecas de classes e APIs adicionais

Este artigo lista .NET Framework APIs que foram lançadas fora de banda, visam uma plataforma específica ou são tipos privados ou internos.

## <a name="oob-projects"></a>Projetos OOB

Para melhorar o desenvolvimento de plataforma cruzada e introduzir novas funcionalidades no início, alguns recursos .NET Framework foram lançados fora de banda (OOB).

| Projeto | Descrição |  
| ------- | ----------- |  
| <xref:System.Collections.Immutable> | Fornece coleções que são thread-safe e têm garantias de que seu conteúdo nunca será alterado. |
| <xref:System.Net.Http.WinHttpHandler> | Fornece um manipulador de mensagens para <xref:System.Net.Http.HttpClient> o com base na interface WinHTTP do Windows. |
| <xref:System.Numerics> | Fornece uma biblioteca de tipos de vetor que podem aproveitar a aceleração baseada em hardware SIMD.|
| <xref:System.Threading.Tasks.Dataflow> | A Biblioteca de Fluxo de Dados TPL fornece componentes de fluxo de dados para ajudar a aumentar a robustez de aplicativos habilitados para simultaneidade. |  

## <a name="platform-specific-libraries"></a>Bibliotecas específicas da plataforma

Algumas bibliotecas têm como destino plataformas específicas. Por exemplo, a <xref:System.Text.CodePagesEncodingProvider> classe torna codificações de página de código disponíveis para aplicativos UWP desenvolvidos usando .NET Framework.
  
| Projeto | Descrição |  
| ------- | ----------- |  
| <xref:System.Text.CodePagesEncodingProvider> | Estende a <xref:System.Text.EncodingProvider> classe para tornar as codificações de página de código disponíveis para aplicativos direcionados ao plataforma universal do Windows. |  
  
## <a name="private-apis"></a>APIs privadas  

Essas APIs dão suporte à infraestrutura do produto e não são pretendidas nem têm suporte para serem usadas diretamente do seu código.  
  
* [Propriedade Microsoft. SqlServer. Server. SmiOrderProperty. Item](microsoft.sqlserver.server.smiorderproperty.item.md)
* [Método System. Exception. PrepForRemoting](system.exception.prepforremoting.md)
* [Propriedade System. Data. SqlTypes. SqlChars. Stream](system.data.sqltypes.sqlchars.stream.md)
* [Construtor System. Data. SqlTypes. SqlStreamChars](system.data.sqltypes.sqlstreamchars.-ctor.md)
* [Propriedade System. Data. SqlTypes. SqlStreamChars. CanSeek](system.data.sqltypes.sqlstreamchars.canseek.md)
* [Propriedade System. Data. SqlTypes. SqlStreamChars. IsNull](system.data.sqltypes.sqlstreamchars.isnull.md)
* [Propriedade System. Data. sqltipations. SqlStreamChars. Length](system.data.sqltypes.sqlstreamchars.length.md)
* [Método System. Data. SqlTypes. SqlStreamChars. Close](system.data.sqltypes.sqlstreamchars.close.md)
* [Método System. Data. SqlTypes. SqlStreamChars. Dispose](system.data.sqltypes.sqlstreamchars.dispose.md)
* [Método System. Data. sqltipations. SqlStreamChars. Flush](system.data.sqltypes.sqlstreamchars.flush.md)
* [Método System. Data. SqlTypes. SqlStreamChars. Read](system.data.sqltypes.sqlstreamchars.read.md)
* [Método System. Data. SqlTypes. SqlStreamChars. Seek](system.data.sqltypes.sqlstreamchars.seek.md)
* [Método System. Data. SqlTypes. SqlStreamChars. SetLength](system.data.sqltypes.sqlstreamchars.setlength.md)
* [Método System. Data. SqlTypes. SqlStreamChars. Write](system.data.sqltypes.sqlstreamchars.write.md)
* [Método System. IO. MemoryStream. InternalGetOriginAndLength](system.io.memorystream.internalgetoriginandlength.md)
* [Classe System .net. ComNetOS](system.net.comnetos.md)
* [Classe System .net. Connection](connection.md)
* [Campo de gravação System .net. Connection. m \_](m_writelist.md)
* [Classe System .net. ConnectionObject](connectiongroup.md)
* [Campo de conexão do sistema .net. Connectionlist. m \_](m_connectionlist.md)
* [Propriedade System .net. ConnectStream. Connection](system.net.connectstream.connection.md)
* [Classe System .net. CoreResponseData](coreresponsedata.md)
* [Campo System .net. CoreResponseData. m \_ ResponseHeaders](coreresponsedata_m_responseheaders.md)
* [Campo de StatusCode System .net. CoreResponseData. m \_](coreresponsedata_m_statuscode.md)
* [Classe System .net. ExceptionHelper](system.net.exceptionhelper.md)
* [Classe System .net. HttpStatusDescription](system.net.httpstatusdescription.md)
* [System .net. HttpWebRequest. \_ Campo de redirecionamento](_autoredirects.md)
* [System .net. HttpWebRequest. \_ Campo CoreResponse](httpwebrequest__coreresponse.md)
* [System .net. HttpWebRequest. \_ Campo HttpResponse](_httpresponse.md)
* [Classe System .net. Logging](system.net.logging.md)
* [Classe System .net. mail. MailAddressParser](system.net.mail.mailaddressparser.md)
* [Classe System .net. mail. QuotedPairReader](system.net.mail.quotedpairreader.md)
* [Classe System .net. MIME. MailBnfHelper](system.net.mime.mailbnfhelper.md)
* [Propriedade System .net. PooledStream. NetworkStream](system.net.pooledstream.networkstream.md)
* [Classe System .net. RtcState](system.net.rtcstate.md)
* [Propriedade System .net. Security. SslState. SslProtocol](system.net.security.sslstate.sslprotocol.md)
* [Campo System .net. ConnectionGroupList. m \_](m_connectiongrouplist.md)
* [Método System .net. ServicePointManager. CloseConnectionGroups](system.net.servicepointmanager.closeconnectiongroups.md)
* [Campo System .net. ServicePointManager. s do \_ Objectpointtable](s_servicepointtable.md)
* [Campo System .net. TlsStream. m_Worker](system.net.tlsstream.m_worker.md)
* [Classe System .net. UnsafeNclNativeMethods](system.net.unsafenclnativemethods.md)
* [Método System .net. WebHeaderCollection. addinterna](system.net.webheadercollection.addinternal.md)
* [Método System. ServiceModel. Channels. Message. BodyToString](system.servicemodel.channels.message.bodytostring.md)
* [Método System. ServiceModel. Channels. Message. WriteStartHeaders](system.servicemodel.channels.message.writestartheaders.md)
* [Classe System. Web. Compilation. ControlBuilderInterceptor](controlbuilderinterceptor-class.md)
* [Campo System. Windows. Diagnostics. VisualDiagnostics. s \_ isDebuggerCheckDisabledForTestPurposes](s-isdebuggercheckdisabledfortestpurposes-field.md)
* [Classe System. Windows. Forms. Design. DataMemberFieldEditor](datamemberfieldeditor-class.md)
* [Classe System. Windows. Forms. Design. DataMemberListEditor](datamemberlisteditor-class.md)
* [MétodoSystem.Xml.XmlReader. CreateSqlReader](system.xml.xmlreader.createsqlreader.md)
* [ActiveX. Interface de conexão](adodb.connection.md)
* [ActiveX. EventReason enum](adodb.eventreasonenum.md)
* [ActiveX. EventStatus enum](adodb.eventstatusenum.md)
* [stdole. Estrutura DISPPARAMS](stdole.dispparams.md)
* [stdole. Estrutura EXCEPINFO](stdole.excepinfo.md)
* [stdole. Propriedade IFont.Name](stdole.ifont.name.md)
* [stdole. Interface IFontDisp](stdole.ifontdisp.md)
* [stdole. Propriedade IPicture. Handle](stdole.ipicture.handle.md)
* [stdole. Propriedade IPictureDisp. Handle](stdole.ipicturedisp.handle.md)
* [stdole. Interface StdFont](stdole.stdfont.md)
* [stdole. Interface StdPicture](stdole.stdpicture.md)
  
## <a name="see-also"></a>Confira também

* [.NET Framework e versões fora de banda](../get-started/the-net-framework-and-out-of-band-releases.md)
