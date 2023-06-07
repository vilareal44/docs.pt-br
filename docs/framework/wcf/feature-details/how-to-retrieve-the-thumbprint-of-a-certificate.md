---
title: Como recuperar a impressão digital de um certificado
description: Saiba como especificar declarações encontradas em um certificado X. 509, que é necessário ao desenvolver um aplicativo WCF que usa certificados para autenticação.
ms.date: 03/30/2017
helpviewer_keywords:
- certificates [WCF], retrieving thumbprint
ms.assetid: da3101aa-78cd-4c34-9652-d1f24777eeab
ms.openlocfilehash: 87c696323af442021af267f0d8c523418e2234f7
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246773"
---
# <a name="how-to-retrieve-the-thumbprint-of-a-certificate"></a>Como recuperar a impressão digital de um certificado
Ao escrever um aplicativo de Windows Communication Foundation (WCF) que usa um certificado X. 509 para autenticação, geralmente é necessário especificar as declarações encontradas no certificado. Por exemplo, você deve fornecer uma declaração de impressão digital ao usar a <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindByThumbprint> enumeração no <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A> método. Encontrar o valor da declaração requer duas etapas. Primeiro, abra o snap-in MMC (console de gerenciamento Microsoft) para certificados. (Consulte [como exibir certificados com o snap-in do MMC](how-to-view-certificates-with-the-mmc-snap-in.md).) Em segundo lugar, conforme descrito aqui, encontre um certificado apropriado e copie sua impressão digital (ou outros valores de declaração).  
  
 Se você estiver usando um certificado para autenticação de serviço, é importante observar o valor da coluna **emitido para** (a primeira coluna no console). Ao usar protocolo SSL (SSL) como uma segurança de transporte, uma das primeiras verificações feitas é comparar o URI (Uniform Resource Identifier) de endereço base de um serviço com o valor **emitido para** . Os valores devem corresponder ou o processo de autenticação é interrompido.  
  
 Você também pode usar o cmdlet New-SelfSignedCertificate do PowerShell para criar certificados temporários para uso somente durante o desenvolvimento. Por padrão, no entanto, esse certificado não é emitido por uma autoridade de certificação e não pode ser usado para fins de produção. Para obter mais informações, consulte [como criar certificados temporários para uso durante o desenvolvimento](how-to-create-temporary-certificates-for-use-during-development.md).  
  
### <a name="to-retrieve-a-certificates-thumbprint"></a>Para recuperar a impressão digital de um certificado  
  
1. Abra o snap-in do MMC (console de gerenciamento Microsoft) para certificados. (Consulte [como exibir certificados com o snap-in do MMC](how-to-view-certificates-with-the-mmc-snap-in.md).)  
  
2. No painel esquerdo da janela **raiz do console** , clique em **certificados (computador local)**.  
  
3. Clique na pasta **pessoal** para expandi-la.  
  
4. Clique na pasta **certificados** para expandi-la.  
  
5. Na lista de certificados, observe o título **finalidades pretendidas** . Localize um certificado que lista a **autenticação de cliente** como uma finalidade pretendida.  
  
6. Clique duas vezes no certificado.  
  
7. Na caixa de diálogo **Certificado**, clique na guia **Detalhes**.  
  
8. Percorra a lista de campos e clique em **impressão digital**.  
  
9. Copie os caracteres hexadecimais da caixa. Se essa impressão digital for usada no código para o `X509FindType` , remova os espaços entre os números hexadecimais. Por exemplo, a impressão digital "a9 09 50 2D D8 2a E4 14 33 E6 F8 38 86 B0 0d 42 77 a3 2a 7B" deve ser especificada como "a909502dd82ae41433e6f83886b00d4277a32a7b" no código.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindByThumbprint>
- <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A>
- [Como configurar uma porta com um certificado SSL](how-to-configure-a-port-with-an-ssl-certificate.md)
- [Como exibir certificados com o snap-in do MMC](how-to-view-certificates-with-the-mmc-snap-in.md)
- [Como criar certificados temporários para uso durante o desenvolvimento](how-to-create-temporary-certificates-for-use-during-development.md)
