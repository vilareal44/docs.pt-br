---
title: FTP – .NET
description: Saiba mais sobre o suporte abrangente para o protocolo FTP que o .NET Framework fornece usando as classes FtpWebRequest e FtpWebResponse.
ms.date: 03/30/2017
helpviewer_keywords:
- FTP
ms.assetid: 9b43f8b4-89d7-46a7-89fc-71aca916dd32
ms.openlocfilehash: d21ca43cd1041df358dc5e2add9560fb33e85d83
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502581"
---
# <a name="ftp"></a>FTP

O .NET Framework fornece suporte abrangente para o protocolo FTP com as classes <xref:System.Net.FtpWebRequest> e <xref:System.Net.FtpWebResponse>. Essas classes são derivadas de <xref:System.Net.WebRequest> e <xref:System.Net.WebResponse>. Na maioria dos casos, as classes <xref:System.Net.WebRequest> e <xref:System.Net.WebResponse> fornecem tudo o que é necessário para fazer a solicitação, mas se você precisar acessar os recursos específicos ao FTP expostos como propriedades, poderá fazer a conversão de tipo dessas classes em <xref:System.Net.FtpWebRequest> ou <xref:System.Net.FtpWebResponse>.

## <a name="examples"></a>Exemplos

Para obter mais informações, consulte os seguintes tópicos: [Como baixar arquivos com FTP](how-to-download-files-with-ftp.md), [Como carregar arquivos com FTP](how-to-upload-files-with-ftp.md) e [Como listar o conteúdo do diretório com FTP](how-to-list-directory-contents-with-ftp.md).

## <a name="ftp-and-proxies"></a>FTP e proxies

Se um proxy (especificado pela propriedade <xref:System.Net.FtpWebRequest.Proxy%2A>) for um proxy HTTP, haverá suporte apenas para os comandos <xref:System.Net.WebRequestMethods.Ftp.DownloadFile>, <xref:System.Net.WebRequestMethods.Ftp.ListDirectory> e <xref:System.Net.WebRequestMethods.Ftp.ListDirectoryDetails>.

## <a name="see-also"></a>Confira também

- [Acessando a Internet por meio de um proxy](accessing-the-internet-through-a-proxy.md)
- [Amostras de programação de rede](network-programming-samples.md)
- [Usando protocolos de aplicativo](using-application-protocols.md)
