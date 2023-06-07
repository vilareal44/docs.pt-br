---
title: Diferentes validações de certificado entre segurança de SOAP, HTTPS, SSL através de TCP
description: Saiba mais sobre certificados com segurança SOAP (camada de mensagem) que o WCF oferece, além de HTTPS ou TCP, e como o WCF valida esses certificados.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- certificates [WCF], validation differences
ms.assetid: 953a219f-4745-4019-9894-c70704f352e6
ms.openlocfilehash: 97d51e5b65ebf20e80a69512370b68a51eeb28a7
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245265"
---
# <a name="certificate-validation-differences-between-https-ssl-over-tcp-and-soap-security"></a>Diferentes validações de certificado entre segurança de SOAP, HTTPS, SSL através de TCP
Você pode usar certificados no Windows Communication Foundation (WCF) com segurança de camada de mensagem (SOAP), além da segurança de camada de transporte (TLS) sobre HTTP (HTTPS) ou TCP. Este tópico descreve as diferenças na forma como esses certificados são validados.  
  
## <a name="validation-of-https-client-certificates"></a>Validação de certificados de cliente HTTPS  
 Ao usar HTTPS para se comunicar entre um cliente e um serviço, o certificado que o cliente usa para autenticar para o serviço deve dar suporte à relação de confiança de cadeia. Ou seja, ele deve encadear uma autoridade de certificação raiz confiável. Caso contrário, a camada HTTP gera um <xref:System.Net.WebException> com a mensagem "o servidor remoto retornou um erro: (403) proibido". O WCF superfícies essa exceção como um <xref:System.ServiceModel.Security.MessageSecurityException> .  
  
## <a name="validation-of-https-service-certificates"></a>Validação de certificados de serviço HTTPS  
 Ao usar HTTPS para se comunicar entre um cliente e um serviço, o certificado com o qual o servidor é autenticado deve suportar a relação de confiança de cadeia por padrão. Ou seja, ele deve encadear uma autoridade de certificação raiz confiável. Nenhuma verificação online é executada para ver se o certificado foi revogado. Você pode substituir esse comportamento registrando um <xref:System.Net.Security.RemoteCertificateValidationCallback> retorno de chamada, conforme mostrado no código a seguir.  
  
 [!code-csharp[c_CertificateValidationDifferences#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_certificatevalidationdifferences/cs/source.cs#1)]
 [!code-vb[c_CertificateValidationDifferences#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_certificatevalidationdifferences/vb/source.vb#1)]  
  
 em que a assinatura do `ValidateServerCertificate` é a seguinte:  
  
 [!code-csharp[c_CertificateValidationDifferences#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_certificatevalidationdifferences/cs/source.cs#2)]
 [!code-vb[c_CertificateValidationDifferences#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_certificatevalidationdifferences/vb/source.vb#2)]  
  
 A implementação `ValidateServerCertificate` do pode executar qualquer verificação que o desenvolvedor do aplicativo cliente julgar necessário para validar o certificado de serviço.  
  
## <a name="validation-of-client-certificates-in-ssl-over-tcp-or-soap-security"></a>Validação de certificados de cliente no SSL sobre segurança TCP ou SOAP  
 Ao usar protocolo SSL (SSL) sobre TCP ou mensagem (SOAP) de segurança, os certificados de cliente são validados de acordo com o <xref:System.ServiceModel.Security.X509ClientCertificateAuthentication.CertificateValidationMode%2A> valor da propriedade da <xref:System.ServiceModel.Security.X509ClientCertificateAuthentication> classe. A propriedade é definida como um dos <xref:System.ServiceModel.Security.X509CertificateValidationMode> valores. A verificação de revogação é executada de acordo com os valores do <xref:System.ServiceModel.Security.X509ClientCertificateAuthentication.RevocationMode%2A> valor da propriedade da <xref:System.ServiceModel.Security.X509ClientCertificateAuthentication> classe. A propriedade é definida como um dos <xref:System.Security.Cryptography.X509Certificates.X509RevocationMode> valores.  
  
 [!code-csharp[c_CertificateValidationDifferences#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_certificatevalidationdifferences/cs/source.cs#3)]
 [!code-vb[c_CertificateValidationDifferences#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_certificatevalidationdifferences/vb/source.vb#3)]  
  
## <a name="validation-of-service-certificate-in-ssl-over-tcp-and-soap-security"></a>Validação do certificado de serviço no SSL sobre TCP e segurança SOAP  
 Ao usar a segurança de mensagem SSL sobre TCP ou (SOAP), os certificados de serviço são validados de acordo com o <xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication.CertificateValidationMode%2A> valor da propriedade da <xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication> classe. A propriedade é definida como um dos <xref:System.ServiceModel.Security.X509CertificateValidationMode> valores.  
  
 A verificação de revogação é executada de acordo com os valores do <xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication.RevocationMode%2A> valor da propriedade da <xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication> classe. A propriedade é definida como um dos <xref:System.Security.Cryptography.X509Certificates.X509RevocationMode> valores.  
  
 [!code-csharp[c_CertificateValidationDifferences#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_certificatevalidationdifferences/cs/source.cs#4)]
 [!code-vb[c_CertificateValidationDifferences#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_certificatevalidationdifferences/vb/source.vb#4)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.Net.Security.RemoteCertificateValidationCallback>
- [Trabalhando com certificados](working-with-certificates.md)
