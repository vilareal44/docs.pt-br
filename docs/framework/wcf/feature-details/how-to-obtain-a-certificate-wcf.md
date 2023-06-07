---
title: Como obter um certificado (WCF)
ms.date: 03/30/2017
helpviewer_keywords:
- certificates [WCF], obtaining
ms.assetid: d53762fd-15ea-42dc-b0ea-6a6597aa23f7
ms.openlocfilehash: d020f3e97023d07abb572d30dd53896bfec1da46
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597014"
---
# <a name="how-to-obtain-a-certificate-wcf"></a>Como obter um certificado (WCF)
Para usar qualquer um dos recursos de Windows Communication Foundation (WCF) que usam certificados X. 509, primeiro você obtém certificados.  
  
### <a name="to-obtain-an-x509-certificate"></a>Para obter um certificado X. 509  
  
1. Escolha uma destas opções:  
  
    - Compre um certificado de uma autoridade de certificação, como a VeriSign, Inc.  
  
    - Configure seu próprio serviço de certificado e faça com que uma autoridade de certificação assine os certificados. O Windows Server 2003, o Windows 2000 Server, o Windows 2000 Server Datacenter e o Windows 2000 Datacenter Server incluem serviços de certificados que dão suporte à PKI (infraestrutura de chave pública). No Windows Server 2008, use a função de [serviços de certificados Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731564(v=ws.10)) para gerenciar uma autoridade de certificação.  
  
    - Configure seu próprio serviço de certificado e não tenha os certificados assinados.  
  
    > [!NOTE]
    > Seja qual for a abordagem adotada, o destinatário da solicitação SOAP que contém o certificado X. 509 deve confiar no certificado X. 509. Isso significa que o certificado X. 509 ou um emissor na cadeia de certificados está no repositório de certificados de pessoas confiáveis e que o certificado X. 509 não está no repositório de certificados não confiáveis.  
  
## <a name="see-also"></a>Consulte também

- [Trabalhando com certificados](working-with-certificates.md)
- [Como criar certificados temporários para uso durante o desenvolvimento](how-to-create-temporary-certificates-for-use-during-development.md)
