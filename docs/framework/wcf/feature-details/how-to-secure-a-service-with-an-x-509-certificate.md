---
title: Como proteger um serviço com um certificado X.509
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 2d06c2aa-d0d7-4e5e-ad7e-77416aa1c10b
ms.openlocfilehash: 10d6db63368ee55040f85f922b9483982e8ff264
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596962"
---
# <a name="how-to-secure-a-service-with-an-x509-certificate"></a>Como proteger um serviço com um certificado X.509
A proteção de um serviço com um certificado X. 509 é uma técnica básica que a maioria das ligações em Windows Communication Foundation (WCF) usa. Este tópico percorre as etapas de configuração de um serviço hospedado internamente com um certificado X. 509.  
  
 Um pré-requisito é um certificado válido que pode ser usado para autenticar o servidor. O certificado deve ser emitido para o servidor por uma autoridade de certificação confiável. Se o certificado não for válido, qualquer cliente que tente usar o serviço não confiará no serviço e, consequentemente, nenhuma conexão será feita. Para obter mais informações sobre como usar certificados, consulte [trabalhando com certificados](working-with-certificates.md).  
  
### <a name="to-configure-a-service-with-a-certificate-using-code"></a>Para configurar um serviço com um certificado usando código  
  
1. Crie o contrato de serviço e o serviço implementado. Para obter mais informações, consulte [projetando e implementando serviços](../designing-and-implementing-services.md).  
  
2. Crie uma instância da <xref:System.ServiceModel.WSHttpBinding> classe e defina seu modo de segurança como <xref:System.ServiceModel.SecurityMode.Message> , conforme mostrado no código a seguir.  
  
     [!code-csharp[C_SecureWithCertificate#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewithcertificate/cs/source.cs#1)]
     [!code-vb[C_SecureWithCertificate#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewithcertificate/vb/source.vb#1)]  
  
3. Crie duas <xref:System.Type> variáveis, uma para o tipo de contrato e o contrato implementado, conforme mostrado no código a seguir.  
  
     [!code-csharp[C_SecureWithCertificate#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewithcertificate/cs/source.cs#2)]
     [!code-vb[C_SecureWithCertificate#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewithcertificate/vb/source.vb#2)]  
  
4. Crie uma instância da <xref:System.Uri> classe para o endereço base do serviço. Como o `WSHttpBinding` usa o transporte http, o Uniform Resource Identifier (URI) deve começar com esse esquema, ou Windows Communication Foundation (WCF) gerará uma exceção quando o serviço for aberto.  
  
     [!code-csharp[C_SecureWithCertificate#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewithcertificate/cs/source.cs#3)]
     [!code-vb[C_SecureWithCertificate#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewithcertificate/vb/source.vb#3)]  
  
5. Crie uma nova instância da <xref:System.ServiceModel.ServiceHost> classe com a variável de tipo de contrato implementado e o URI.  
  
     [!code-csharp[C_SecureWithCertificate#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewithcertificate/cs/source.cs#4)]
     [!code-vb[C_SecureWithCertificate#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewithcertificate/vb/source.vb#4)]  
  
6. Adicione um <xref:System.ServiceModel.Description.ServiceEndpoint> ao serviço usando o <xref:System.ServiceModel.ServiceHost.AddServiceEndpoint%2A> método. Passe o contrato, a associação e um endereço de ponto de extremidade para o construtor, conforme mostrado no código a seguir.  
  
     [!code-csharp[C_SecureWithCertificate#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewithcertificate/cs/source.cs#5)]
     [!code-vb[C_SecureWithCertificate#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewithcertificate/vb/source.vb#5)]  
  
7. Opcional. Para recuperar metadados do serviço, crie um novo <xref:System.ServiceModel.Description.ServiceMetadataBehavior> objeto e defina a <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpGetEnabled%2A> propriedade como `true` .  
  
     [!code-csharp[C_SecureWithCertificate#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewithcertificate/cs/source.cs#6)]
     [!code-vb[C_SecureWithCertificate#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewithcertificate/vb/source.vb#6)]  
  
8. Use o <xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential.SetCertificate%2A> método da <xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential> classe para adicionar o certificado válido ao serviço. O método pode usar um dos vários métodos para localizar um certificado. Este exemplo usa a <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindBySubjectName> enumeração. A enumeração Especifica que o valor fornecido é o nome da entidade para a qual o certificado foi emitido.  
  
     [!code-csharp[C_SecureWithCertificate#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewithcertificate/cs/source.cs#7)]
     [!code-vb[C_SecureWithCertificate#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewithcertificate/vb/source.vb#7)]  
  
9. Chame o <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> método para iniciar o serviço de escuta. Se você estiver criando um aplicativo de console, chame o <xref:System.Console.ReadLine%2A> método para manter o serviço no estado de escuta.  
  
     [!code-csharp[C_SecureWithCertificate#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewithcertificate/cs/source.cs#8)]
     [!code-vb[C_SecureWithCertificate#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewithcertificate/vb/source.vb#8)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o <xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential.SetCertificate%2A> método para configurar um serviço com um certificado X. 509.  
  
 [!code-csharp[C_SecureWithCertificate#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewithcertificate/cs/source.cs#9)]
 [!code-vb[C_SecureWithCertificate#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewithcertificate/vb/source.vb#9)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Os namespaces a seguir são necessários para compilar o código:  
  
- <xref:System>  
  
- <xref:System.ServiceModel>  
  
- <xref:System.ServiceModel.Channels>  
  
- <xref:System.Web.Services.Description>  
  
- <xref:System.Security.Cryptography.X509Certificates>  
  
- <xref:System.Runtime.Serialization>  
  
## <a name="see-also"></a>Consulte também

- [Trabalhando com certificados](working-with-certificates.md)
