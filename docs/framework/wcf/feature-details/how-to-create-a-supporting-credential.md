---
title: Como criar uma credencial de suporte
ms.date: 03/30/2017
ms.assetid: d0952919-8bb4-4978-926c-9cc108f89806
ms.openlocfilehash: b8e7ddcd6118c77e14e090a0b1fa8d65aeb8e3df
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597144"
---
# <a name="how-to-create-a-supporting-credential"></a>Como criar uma credencial de suporte
É possível ter um esquema de segurança personalizado que exija mais de uma credencial. Por exemplo, um serviço pode exigir do cliente não apenas um nome de usuário e uma senha, mas também uma credencial que comprova que o cliente está acima da idade de 18. A segunda credencial é uma *credencial de suporte*. Este tópico explica como implementar essas credenciais em um cliente Windows Communication Foundation (WCF).  
  
> [!NOTE]
> A especificação para dar suporte a credenciais faz parte da especificação WS-SecurityPolicy. Para obter mais informações, consulte [especificações de especificação Web Services Security](https://docs.microsoft.com/previous-versions/dotnet/articles/ms951273(v=msdn.10)).  
  
## <a name="supporting-tokens"></a>Tokens com suporte  
 Em resumo, quando você usa a segurança da mensagem, uma *credencial primária* é sempre usada para proteger a mensagem (por exemplo, um certificado X. 509 ou um tíquete Kerberos).  
  
 Conforme definido pela especificação, uma associação de segurança usa *tokens* para proteger a troca de mensagens. Um *token* é uma representação de uma credencial de segurança.  
  
 A associação de segurança usa um token primário identificado na política de associação de segurança para criar uma assinatura. Essa assinatura é conhecida como a *assinatura da mensagem*.  
  
 Tokens adicionais podem ser especificados para aumentar as declarações fornecidas pelo token associado à assinatura da mensagem.  
  
## <a name="endorsing-signing-and-encrypting"></a>Endossando, assinando e criptografando  
 Uma credencial de suporte resulta em um *token de suporte* transmitido dentro da mensagem. A especificação WS-SecurityPolicy define quatro maneiras de anexar um token de suporte à mensagem, conforme descrito na tabela a seguir.  
  
|Finalidade|Descrição|  
|-------------|-----------------|  
|Com sinal|O token de suporte é incluído no cabeçalho de segurança e é assinado pela assinatura da mensagem.|  
|Endosso|Um *token de endosso* assina a assinatura da mensagem.|  
|Assinado e endossador|Assinados, os tokens de endosso assinam todo o `ds:Signature` elemento produzido da assinatura da mensagem e são assinados por essa assinatura de mensagem; ou seja, ambos os tokens (o token usado para a assinatura de mensagem e o token de endosso assinado) se assinam uns aos outros.|  
|Assinado e criptografado|Tokens de suporte criptografados assinados são tokens de suporte assinados que também são criptografados quando aparecem no `wsse:SecurityHeader` .|  
  
## <a name="programming-supporting-credentials"></a>Programando credenciais de suporte  
 Para criar um serviço que usa tokens de suporte, você deve criar um [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) . (Para obter mais informações, consulte [como: criar uma associação personalizada usando o SecurityBindingElement](how-to-create-a-custom-binding-using-the-securitybindingelement.md).)  
  
 A primeira etapa ao criar uma associação personalizada é criar um elemento de associação de segurança, que pode ser um dos três tipos:  
  
- <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement>  
  
- <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement>  
  
- <xref:System.ServiceModel.Channels.TransportSecurityBindingElement>  
  
 Todas as classes herdam do <xref:System.ServiceModel.Channels.SecurityBindingElement> , que inclui quatro propriedades relevantes:  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.EndpointSupportingTokenParameters%2A>  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.OperationSupportingTokenParameters%2A>  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.OptionalEndpointSupportingTokenParameters%2A>  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.OptionalOperationSupportingTokenParameters%2A>  
  
#### <a name="scopes"></a>Escopos  
 Existem dois escopos para dar suporte a credenciais:  
  
- Os *tokens de suporte do ponto de extremidade* oferecem suporte a todas as operações de um ponto Ou seja, a credencial que o token de suporte representa pode ser usada sempre que qualquer operação de ponto de extremidade for invocada.  
  
- Os *tokens de suporte de operação* dão suporte apenas a uma operação de ponto de extremidade específica  
  
 Conforme indicado pelos nomes de propriedade, as credenciais de suporte podem ser obrigatórias ou opcionais. Ou seja, se a credencial de suporte for usada se estiver presente, embora não seja necessário, mas a autenticação não falhará se ela não estiver presente.  
  
## <a name="procedures"></a>Procedimentos  
  
#### <a name="to-create-a-custom-binding-that-includes-supporting-credentials"></a>Para criar uma associação personalizada que inclui credenciais de suporte  
  
1. Crie um elemento de associação de segurança. O exemplo a seguir cria um <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> com o `UserNameForCertificate` modo de autenticação. Use o método <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateUserNameForCertificateBindingElement%2A>.  
  
2. Adicione o parâmetro de suporte à coleção de tipos retornada pela propriedade apropriada ( `Endorsing` ,, `Signed` `SignedEncrypted` ou `SignedEndorsed` ). Os tipos no <xref:System.ServiceModel.Security.Tokens> namespace incluem tipos comumente usados, como o <xref:System.ServiceModel.Security.Tokens.X509SecurityTokenParameters> .  
  
## <a name="example"></a>Exemplo  
  
### <a name="description"></a>Descrição  
 O exemplo a seguir cria uma instância do <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> e adiciona uma instância da <xref:System.ServiceModel.Security.Tokens.KerberosSecurityTokenParameters> classe à coleção que a propriedade de endosso retornou.  
  
### <a name="code"></a>Código  
 [!code-csharp[c_SupportingCredential#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_supportingcredential/cs/source.cs#1)]  
  
## <a name="see-also"></a>Consulte também

- [Como criar uma associação personalizada utilizando o SecurityBindingElement](how-to-create-a-custom-binding-using-the-securitybindingelement.md)
