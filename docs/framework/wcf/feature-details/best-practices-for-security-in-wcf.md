---
title: Práticas recomendadas para segurança no WCF
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- best practices [WCF], security
ms.assetid: 3639de41-1fa7-4875-a1d7-f393e4c8bd69
ms.openlocfilehash: c99ab5e1e72aefc688df1692091e60caf930d5e4
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597612"
---
# <a name="best-practices-for-security-in-wcf"></a>Práticas recomendadas para segurança no WCF
As seções a seguir listam as melhores práticas a serem consideradas durante a criação de aplicativos seguros usando o WCF (Windows Communication Foundation). Para obter mais informações sobre segurança, confira [Considerações sobre segurança](security-considerations-in-wcf.md), [Considerações sobre segurança de dados](security-considerations-for-data.md) e [Considerações sobre segurança com metadados](security-considerations-with-metadata.md).  
  
## <a name="identify-services-performing-windows-authentication-with-spns"></a>Identificar os serviços que executam a Autenticação do Windows com SPNs  
 Os serviços podem ser identificados com nomes UPN ou SPNs. Os serviços executados em contas de computador como um serviço de rede têm uma identidade do SPN correspondente ao computador que eles estão executando. Os serviços executados em contas de usuário têm uma identidade do UPN correspondente ao usuário como quem eles estão sendo executados, embora a ferramenta `setspn` possa ser usada para atribuir um SPN à conta de usuário. A configuração de um serviço para que ele possa ser identificado por meio do SPN e a configuração dos clientes que se conectam ao serviço para que eles usem esse SPN pode dificultar alguns ataques. Essas diretrizes se aplicam às associações que usam o Kerberos ou a negociação SSPI.  Os clientes ainda devem especificar um SPN quando o SSPI faz fallback para o NTLM.  
  
## <a name="verify-service-identities-in-wsdl"></a>Verificar as identidades de serviço no WSDL  
 O WS-SecurityPolicy permite que os serviços publiquem informações sobre suas próprias identidades nos metadados. Quando recuperadas por meio de `svcutil` ou por outros métodos, como <xref:System.ServiceModel.Description.WsdlImporter>, essas informações de identidade são convertidas nas propriedades de identidade dos endereços do ponto de extremidade de serviço WCF. Os clientes que não verificam se essas identidades de serviço estão corretas e válidas ignoram efetivamente a autenticação de serviço. Um serviço mal-intencionado pode explorar esses clientes para executar o encaminhamento de credenciais e outros ataques "man-in-the-middle" alterando a identidade declarada em seu WSDL.  
  
## <a name="use-x509-certificates-instead-of-ntlm"></a>Usar certificados X509 em vez do NTLM  
 O WCF oferece dois mecanismos para a autenticação ponto a ponto: os certificados X509 (usados pelo canal par) e a autenticação do Windows, em que uma negociação SSPI faz downgrade do Kerberos para o NTLM.  A autenticação baseada em certificado usando tamanhos de chave de 1.024 bits ou superior é preferencial para o NTLM por vários motivos:  
  
- a disponibilidade da autenticação mútua;  
  
- o uso de algoritmos de criptografia mais fortes; e  
  
- a maior dificuldade em utilizar credenciais X509 encaminhadas.  

## <a name="always-revert-after-impersonation"></a>Sempre reverter após representação  
 Ao usar APIs que permitem a representação de um cliente, lembre-se de revertê-lo para a identidade original. Por exemplo, ao usar o <xref:System.Security.Principal.WindowsIdentity> e o <xref:System.Security.Principal.WindowsImpersonationContext>, use a instrução `using` do C# ou a instrução `Using` do Visual Basic, conforme mostrado no código a seguir. A classe <xref:System.Security.Principal.WindowsImpersonationContext> implementa a interface <xref:System.IDisposable> e, portanto, o CLR (Common Language Runtime) é revertido automaticamente para a identidade original depois que o código deixa o bloco `using`.  
  
 [!code-csharp[c_SecurityBestPractices#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securitybestpractices/cs/source.cs#1)]
 [!code-vb[c_SecurityBestPractices#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securitybestpractices/vb/source.vb#1)]  
  
## <a name="impersonate-only-as-needed"></a>Representar apenas conforme necessário  
 Usando o método <xref:System.Security.Principal.WindowsIdentity.Impersonate%2A> da classe <xref:System.Security.Principal.WindowsIdentity>, é possível usar a representação em um escopo muito controlado. Isso é diferente de usar a propriedade <xref:System.ServiceModel.OperationBehaviorAttribute.Impersonation%2A> do <xref:System.ServiceModel.OperationBehaviorAttribute>, que permite a representação para o escopo da operação inteira. Sempre que possível, controle o escopo da representação usando o método <xref:System.Security.Principal.WindowsIdentity.Impersonate%2A> mais preciso.  
  
## <a name="obtain-metadata-from-trusted-sources"></a>Obter metadados de fontes confiáveis  
 Garanta que você confie na origem dos metadados e garanta que ninguém tenha adulterado os metadados. Os metadados recuperados usando o protocolo HTTP são enviados em texto não criptografado e podem ser adulterados. Se o serviço usa as propriedades <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetEnabled%2A> e <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetUrl%2A>, use a URL fornecida pelo criador do serviço para baixar os dados usando o protocolo HTTPS.  
  
## <a name="publish-metadata-using-security"></a>Publicar metadados usando a segurança  
 Para evitar a adulteração dos metadados publicados de um serviço, proteja o ponto de extremidade de troca de metadados com segurança em nível da mensagem ou do transporte. Para obter mais informações, confira [Publicando pontos de extremidade de metadados](../publishing-metadata-endpoints.md) e [Como publicar metadados para um serviço usando o código](how-to-publish-metadata-for-a-service-using-code.md).  
  
## <a name="ensure-use-of-local-issuer"></a>Garantir o uso do emissor local  
 Se um endereço do emissor e a associação são especificados para uma associação fornecida, o emissor local não é usado para os pontos de extremidade que usam essa associação. Os clientes que pretendem sempre usar o emissor local devem garantir que não usem uma associação como essa ou que modifiquem a associação, de modo que o endereço do emissor seja nulo.  
  
## <a name="saml-token-size-quotas"></a>Cotas de tamanho do token SAML  
 Quando os tokens SAML (Security Assertions Markup Language) são serializados em mensagens, quando são emitidos por um STS (Serviço de Token de Segurança) ou quando os clientes apresentam esses tokens para serviços como parte da autenticação, a cota máxima de tamanho da mensagem precisa ser suficientemente grande para acomodar o token SAML e as outras partes da mensagem. Em casos normais, as cotas padrão de tamanho da mensagem são suficientes. No entanto, nos casos em que um token SAML é grande porque contém centenas de declarações, as cotas devem ser aumentadas para acomodar o token serializado. Para obter mais informações sobre cotas, confira [Considerações sobre segurança de dados](security-considerations-for-data.md).  
  
## <a name="set-securitybindingelementincludetimestamp-to-true-on-custom-bindings"></a>Definir SecurityBindingElement.IncludeTimestamp como True em associações personalizadas  
 Ao criar uma associação personalizada, você deverá definir <xref:System.ServiceModel.Channels.SecurityBindingElement.IncludeTimestamp%2A> como `true`. Caso contrário, se <xref:System.ServiceModel.Channels.SecurityBindingElement.IncludeTimestamp%2A> for definido como `false` e o cliente estiver usando um token baseado em chave assimétrica como um certificado X509, a mensagem não será assinada.  
  
## <a name="see-also"></a>Consulte também

- [Considerações sobre segurança](security-considerations-in-wcf.md)
- [Considerações de segurança para dados](security-considerations-for-data.md)
- [Considerações de segurança com metadados](security-considerations-with-metadata.md)
