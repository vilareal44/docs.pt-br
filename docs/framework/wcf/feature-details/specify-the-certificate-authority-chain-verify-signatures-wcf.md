---
title: Como especificar a cadeia de certificados da autoridade de certificação utilizada para verificar assinaturas (WCF)
ms.date: 03/30/2017
helpviewer_keywords:
- certificates [WCF], specifying the certificate authority certificate chain
- certificates [WCF], verifying signatures
ms.assetid: 7c719355-aa41-4567-80d0-5115a8cf73fd
ms.openlocfilehash: 103d68d4ccb4cc243d28037260c1f9f380485ff6
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600303"
---
# <a name="how-to-specify-the-certificate-authority-certificate-chain-used-to-verify-signatures-wcf"></a>Como especificar a cadeia de certificados da autoridade de certificação utilizada para verificar assinaturas (WCF)
Quando Windows Communication Foundation (WCF) recebe uma mensagem SOAP assinada usando um certificado X. 509, por padrão, ele verifica se o certificado X. 509 foi emitido por uma autoridade de certificação confiável. Isso é feito examinando um repositório de certificados e determinando se o certificado para essa autoridade de certificação foi designado como confiável. Para que o WCF faça essa determinação, a cadeia de certificados de autoridade de certificação deve ser instalada no repositório de certificados correto.  
  
### <a name="to-install-a-certification-authority-certificate-chain"></a>Para instalar uma cadeia de certificados de autoridade de certificação  
  
- Para cada autoridade de certificação que um destinatário de mensagem SOAP pretende confiar em certificados X. 509 emitidos pelo, instale a cadeia de certificados de autoridade de certificação no repositório de certificados que o WCF está configurado para recuperar certificados X. 509 de.  
  
     Por exemplo, se um destinatário de mensagem SOAP pretender confiar em certificados X. 509 emitidos pela Microsoft, a cadeia de certificados da autoridade de certificação para Microsoft deverá ser instalada no repositório de certificados que o WCF está configurado para procurar por X. 509 certificados de. O repositório de certificados no qual o WCF procura por X. 509 certificados podem ser especificados no código ou na configuração. Por exemplo, isso pode ser especificado no código usando o <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A> método ou na configuração de algumas maneiras, incluindo o [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-clientcredentials-element.md) .  
  
     Como o Windows é fornecido com um conjunto de cadeias de certificados padrão para autoridades de certificação confiáveis, pode não ser necessário instalar a cadeia de certificados para todas as autoridades de certificação.  
  
    1. Exporte a cadeia de certificados de autoridade de certificação.  
  
         Exatamente como isso é feito depende da autoridade de certificação. Se a autoridade de certificação estiver executando os serviços de certificados da Microsoft, selecione **baixar um certificado de autoridade de certificação, uma cadeia de certificados ou uma CRL**e, em seguida, escolha **baixar certificado de AC**.  
  
    2. Importe a cadeia de certificados de autoridade de certificação.  
  
         No console de gerenciamento Microsoft (MMC), abra o snap-in certificados. Para o repositório de certificados que o WCF está configurado para recuperar certificados X. 509 de, selecione a pasta **autoridades de certificação** **raiz confiáveis** . Na pasta **autoridades de certificação raiz confiáveis** , clique com o botão direito do mouse na pasta **certificados** , aponte para **todas as tarefas**e clique em **importar**. Forneça o arquivo exportado na etapa a.  
  
         Para obter mais informações sobre como usar o snap-in de certificados com o MMC, consulte [How to: View Certificates with the MMC Snap-in](how-to-view-certificates-with-the-mmc-snap-in.md).  
  
## <a name="see-also"></a>Consulte também

- [Trabalhando com certificados](working-with-certificates.md)
