---
title: Como criar um token personalizado
description: Saiba como criar um token de segurança personalizado no WCF usando a classe SecurityToken e como integrá-lo a um provedor de token de segurança e a um autenticador.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- SecurityTokenParameters class
- security [WCF], creating custom tokens
- WSSecurityTokenSerializer class
- SecurityToken class
ms.assetid: 6d892973-1558-4115-a9e1-696777776125
ms.openlocfilehash: a95d663c2669186fcb3eb1fb2f0c426ade945f1c
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247527"
---
# <a name="how-to-create-a-custom-token"></a>Como criar um token personalizado
Este tópico mostra como criar um token de segurança personalizado usando a <xref:System.IdentityModel.Tokens.SecurityToken> classe e como integrá-lo a um provedor de token de segurança personalizado e autenticador. Para obter um exemplo de código completo, consulte o exemplo de [token personalizado](../samples/custom-token.md) .  
  
 Um *token de segurança* é essencialmente um elemento XML que é usado pela estrutura de segurança do Windows Communication Foundation (WCF) para representar declarações sobre um remetente dentro da mensagem SOAP. A segurança do WCF fornece vários tokens para modos de autenticação fornecidos pelo sistema. Os exemplos incluem um token de segurança de certificado X. 509 representado pela <xref:System.IdentityModel.Tokens.X509SecurityToken> classe ou um token de segurança de nome de usuário representado pela <xref:System.IdentityModel.Tokens.UserNameSecurityToken> classe.  
  
 Às vezes, um modo de autenticação ou credencial não é suportado pelos tipos fornecidos. Nesse caso, é necessário criar um token de segurança personalizado para fornecer uma representação XML da credencial personalizada dentro da mensagem SOAP.  
  
 Os procedimentos a seguir mostram como criar um token de segurança personalizado e como integrá-lo à infraestrutura de segurança do WCF. Este tópico cria um token de cartão de crédito que é usado para passar informações sobre o cartão de crédito do cliente para o servidor.  
  
 Para obter mais informações sobre credenciais personalizadas e Gerenciador de token de segurança, consulte [Walkthrough: criando credenciais personalizadas de cliente e serviço](walkthrough-creating-custom-client-and-service-credentials.md).  
  
 Consulte o <xref:System.IdentityModel.Tokens> namespace para obter mais classes que representam tokens de segurança.  
  
## <a name="procedures"></a>Procedimentos  
 Um aplicativo cliente deve ser fornecido com uma maneira de especificar informações de cartão de crédito para a infraestrutura de segurança. Essas informações são disponibilizadas para o aplicativo por uma classe de credenciais de cliente personalizada. A primeira etapa é criar uma classe para representar as informações de cartão de crédito para credenciais de cliente personalizadas.  
  
#### <a name="to-create-a-class-that-represents-credit-card-information-inside-client-credentials"></a>Para criar uma classe que representa informações de cartão de crédito dentro de credenciais do cliente  
  
1. Defina uma nova classe que representa as informações de cartão de crédito para o aplicativo. O exemplo a seguir nomeia a classe `CreditCardInfo` .  
  
2. Adicione Propriedades apropriadas à classe para permitir que um aplicativo defina as informações necessárias para o token personalizado. Neste exemplo, a classe tem três propriedades: `CardNumber` , `CardIssuer` e `ExpirationDate` .  
  
     [!code-csharp[c_CustomToken#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#4)]
     [!code-vb[c_CustomToken#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#4)]  
  
 Em seguida, uma classe que representa o token de segurança personalizado deve ser criada. Essa classe é usada pelas classes do provedor de token de segurança, do autenticador e do serializador para passar informações sobre o token de segurança de e para a infraestrutura de segurança do WCF.  
  
#### <a name="to-create-a-custom-security-token-class"></a>Para criar uma classe de token de segurança personalizada  
  
1. Defina uma nova classe derivada da <xref:System.IdentityModel.Tokens.SecurityToken> classe. Este exemplo cria uma classe chamada `CreditCardToken` .  
  
2. Substituir a <xref:System.IdentityModel.Tokens.SecurityToken.Id%2A> propriedade. Essa propriedade é usada para obter o identificador local do token de segurança que é usado para apontar para a representação XML do token de segurança de outros elementos dentro da mensagem SOAP. Neste exemplo, um identificador de token pode ser passado para ele como um parâmetro de construtor ou um novo aleatório é gerado toda vez que uma instância de token de segurança é criada.  
  
3. Implemente a propriedade <xref:System.IdentityModel.Tokens.SecurityToken.SecurityKeys%2A>. Essa propriedade retorna uma coleção de chaves de segurança que a instância do token de segurança representa. Essas chaves podem ser usadas pelo WCF para assinar ou criptografar partes da mensagem SOAP. Neste exemplo, o token de segurança de cartão de crédito não pode conter nenhuma chave de segurança; Portanto, a implementação sempre retorna uma coleção vazia.  
  
4. Substitua as <xref:System.IdentityModel.Tokens.SecurityToken.ValidFrom%2A> <xref:System.IdentityModel.Tokens.SecurityToken.ValidTo%2A> Propriedades e. Essas propriedades são usadas pelo WCF para determinar a validade da instância de token de segurança. Neste exemplo, o token de segurança do cartão de crédito tem apenas uma data de expiração, portanto, a `ValidFrom` propriedade retorna um <xref:System.DateTime> que representa a data e a hora da criação da instância.  
  
     [!code-csharp[c_CustomToken#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#1)]
     [!code-vb[c_CustomToken#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#1)]  
  
 Quando um novo tipo de token de segurança é criado, ele requer uma implementação da <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters> classe. A implementação é usada na configuração do elemento de associação de segurança para representar o novo tipo de token. A classe de parâmetros de token de segurança serve como um modelo que é usado para corresponder a instância de token de segurança real a quando uma mensagem é processada. O modelo fornece propriedades adicionais que um aplicativo pode usar para especificar os critérios que o token de segurança deve corresponder para ser usado ou autenticado. O exemplo a seguir não adiciona nenhuma propriedade adicional, portanto, somente o tipo de token de segurança é correspondido quando a infraestrutura do WCF procura uma instância de token de segurança para usar ou para validar.  
  
#### <a name="to-create-a-custom-security-token-parameters-class"></a>Para criar uma classe de parâmetros de token de segurança personalizado  
  
1. Defina uma nova classe derivada da <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters> classe.  
  
2. Implementar o método de <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters.CloneCore%2A> . Copie todos os campos internos definidos em sua classe, se houver. Este exemplo não define nenhum campo adicional.  
  
3. Implemente a <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters.SupportsClientAuthentication%2A> propriedade somente leitura. Essa propriedade retorna `true` se o tipo de token de segurança representado por essa classe pode ser usado para autenticar um cliente para um serviço. Neste exemplo, o token de segurança de cartão de crédito pode ser usado para autenticar um cliente para um serviço.  
  
4. Implemente a <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters.SupportsServerAuthentication%2A> propriedade somente leitura. Essa propriedade retorna `true` se o tipo de token de segurança representado por essa classe pode ser usado para autenticar um serviço para um cliente. Neste exemplo, o token de segurança de cartão de crédito não pode ser usado para autenticar um serviço para um cliente.  
  
5. Implemente a <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters.SupportsClientWindowsIdentity%2A> propriedade somente leitura. Essa propriedade retornará `true` se o tipo de token de segurança representado por essa classe puder ser mapeado para uma conta do Windows. Nesse caso, o resultado da autenticação é representado por uma <xref:System.Security.Principal.WindowsIdentity> instância de classe. Neste exemplo, o token não pode ser mapeado para uma conta do Windows.  
  
6. Implementar o método de <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters.CreateKeyIdentifierClause%28System.IdentityModel.Tokens.SecurityToken%2CSystem.ServiceModel.Security.Tokens.SecurityTokenReferenceStyle%29> . Esse método é chamado pela estrutura de segurança do WCF quando requer uma referência à instância de token de segurança representada por essa classe de parâmetros de token de segurança. A instância de token de segurança real e <xref:System.ServiceModel.Security.Tokens.SecurityTokenReferenceStyle> que especifica o tipo da referência que está sendo solicitada são passadas para esse método como argumentos. Neste exemplo, somente referências internas são suportadas pelo token de segurança do cartão de crédito. A <xref:System.IdentityModel.Tokens.SecurityToken> classe tem funcionalidade para criar referências internas; portanto, a implementação não requer código adicional.  
  
7. Implementar o método de <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters.InitializeSecurityTokenRequirement%28System.IdentityModel.Selectors.SecurityTokenRequirement%29> . Esse método é chamado pelo WCF para converter a instância da classe de parâmetros de token de segurança em uma instância da <xref:System.IdentityModel.Selectors.SecurityTokenRequirement> classe. O resultado é usado por provedores de token de segurança para criar a instância de token de segurança apropriada.  
  
     [!code-csharp[c_CustomToken#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#2)]
     [!code-vb[c_CustomToken#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#2)]  
  
 Os tokens de segurança são transmitidos dentro de mensagens SOAP, o que exige um mecanismo de conversão entre a representação do token de segurança na memória e a representação durante a transmissão. O WCF usa um serializador de token de segurança para realizar essa tarefa. Cada token personalizado deve ser acompanhado por um serializador de token de segurança personalizado que pode serializar e desserializar o token de segurança personalizado da mensagem SOAP.  
  
> [!NOTE]
> As chaves derivadas são habilitadas por padrão. Se você criar um token de segurança personalizado e usá-lo como o token primário, o WCF derivará uma chave dele. Ao fazer isso, ele chama o serializador de token de segurança personalizado para gravar o <xref:System.IdentityModel.Tokens.SecurityKeyIdentifierClause> para o token de segurança personalizado ao serializar o `DerivedKeyToken` para a conexão. Na extremidade de recebimento, ao desserializar o token para fora do fio, o `DerivedKeyToken` serializador espera um `SecurityTokenReference` elemento como o filho de nível superior em si mesmo. Se o serializador de token de segurança personalizado não adicionou um `SecurityTokenReference` elemento ao serializar seu tipo de cláusula, uma exceção será lançada.  
  
#### <a name="to-create-a-custom-security-token-serializer"></a>Para criar um serializador de token de segurança personalizado  
  
1. Defina uma nova classe derivada da <xref:System.ServiceModel.Security.WSSecurityTokenSerializer> classe.  
  
2. Substitua o <xref:System.ServiceModel.Security.WSSecurityTokenSerializer.CanReadTokenCore%28System.Xml.XmlReader%29> método, que se baseia em um <xref:System.Xml.XmlReader> para ler o fluxo XML. O método retornará `true` se a implementação do serializador puder desserializar o token de segurança com base em um determinado elemento atual. Neste exemplo, esse método verifica se o elemento XML atual do leitor de XML tem o namespace e o nome do elemento corretos. Caso contrário, ele chamará a implementação da classe base desse método para manipular o elemento XML.  
  
3. Substituir o método <xref:System.ServiceModel.Security.WSSecurityTokenSerializer.ReadTokenCore%28System.Xml.XmlReader%2CSystem.IdentityModel.Selectors.SecurityTokenResolver%29>. Esse método lê o conteúdo XML do token de segurança e constrói a representação na memória apropriada para ele. Se não reconhecer o elemento XML no qual o leitor de XML passado está em pé, ele chamará a implementação da classe base para processar os tipos de token fornecidos pelo sistema.  
  
4. Substituir o método <xref:System.ServiceModel.Security.WSSecurityTokenSerializer.CanWriteTokenCore%28System.IdentityModel.Tokens.SecurityToken%29>. Esse método retorna `true` se ele pode converter a representação de token na memória (passada como um argumento) para a representação XML. Se não for possível converter, ele chamará a implementação da classe base.  
  
5. Substituir o método <xref:System.ServiceModel.Security.WSSecurityTokenSerializer.WriteTokenCore%28System.Xml.XmlWriter%2CSystem.IdentityModel.Tokens.SecurityToken%29>. Esse método converte uma representação de token de segurança na memória em uma representação XML. Se o método não puder ser convertido, ele chamará a implementação da classe base.  
  
     [!code-csharp[c_CustomToken#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#3)]
     [!code-vb[c_CustomToken#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#3)]  
  
 Depois de concluir os quatro procedimentos anteriores, integre o token de segurança personalizado com as credenciais do provedor de token de segurança, do autenticador, do Gerenciador e do cliente e do serviço.  
  
#### <a name="to-integrate-the-custom-security-token-with-a-security-token-provider"></a>Para integrar o token de segurança personalizado a um provedor de token de segurança  
  
1. O provedor de token de segurança cria, modifica (se necessário) e retorna uma instância do token. Para criar um provedor personalizado para o token de segurança personalizado, crie uma classe que herda da <xref:System.IdentityModel.Selectors.SecurityTokenProvider> classe. O exemplo a seguir substitui o <xref:System.IdentityModel.Selectors.SecurityTokenProvider.GetTokenCore%2A> método para retornar uma instância do `CreditCardToken` . Para obter mais informações sobre provedores de token de segurança personalizados, consulte [como criar um provedor de token de segurança personalizado](how-to-create-a-custom-security-token-provider.md).  
  
     [!code-csharp[c_CustomToken#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#6)]
     [!code-vb[c_CustomToken#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#6)]  
  
#### <a name="to-integrate-the-custom-security-token-with-a-security-token-authenticator"></a>Para integrar o token de segurança personalizado a um autenticador de token de segurança  
  
1. O autenticador de token de segurança valida o conteúdo do token de segurança quando ele é extraído da mensagem. Para criar um autenticador personalizado para o token de segurança personalizado, crie uma classe que herda da <xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator> classe. O exemplo a seguir substitui o <xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator.ValidateTokenCore%2A> método. Para obter mais informações sobre autenticadores de token de segurança personalizados, consulte [como criar um autenticador de token de segurança personalizado](how-to-create-a-custom-security-token-authenticator.md).  
  
     [!code-csharp[c_CustomToken#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#7)]
     [!code-vb[c_CustomToken#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#7)]  
  
     [!code-csharp[c_CustomToken#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#12)]
     [!code-vb[c_CustomToken#12](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#12)]  
  
#### <a name="to-integrate-the-custom-security-token-with-a-security-token-manager"></a>Para integrar o token de segurança personalizado a um Gerenciador de token de segurança  
  
1. O Gerenciador de token de segurança cria as instâncias de provedor de token, autenticador de segurança e serializador de token apropriadas. Para criar um Gerenciador de token personalizado, crie uma classe que herda da <xref:System.ServiceModel.ClientCredentialsSecurityTokenManager> classe. Os principais métodos da classe usam um <xref:System.IdentityModel.Selectors.SecurityTokenRequirement> para criar o provedor apropriado e as credenciais de cliente ou serviço. Para obter mais informações sobre gerenciadores de tokens de segurança personalizados, consulte [Walkthrough: criando credenciais personalizadas de cliente e serviço](walkthrough-creating-custom-client-and-service-credentials.md).  
  
     [!code-csharp[c_CustomToken#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#8)]
     [!code-vb[c_CustomToken#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#8)]  
  
     [!code-csharp[c_CustomToken#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#9)]
     [!code-vb[c_CustomToken#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#9)]  
  
#### <a name="to-integrate-the-custom-security-token-with-custom-client-and-service-credentials"></a>Para integrar o token de segurança personalizado com credenciais de cliente e de serviço personalizadas  
  
1. As credenciais personalizadas do cliente e do serviço devem ser adicionadas para fornecer uma API para o aplicativo permitir a especificação de informações de token personalizadas que são usadas pela infraestrutura de token de segurança personalizada criada anteriormente para fornecer e autenticar o conteúdo do token de segurança personalizado. Os exemplos a seguir mostram como isso pode ser feito. Para obter mais informações sobre as credenciais de cliente e de serviço personalizadas, consulte [Walkthrough: criando credenciais personalizadas de cliente e serviço](walkthrough-creating-custom-client-and-service-credentials.md).  
  
     [!code-csharp[c_CustomToken#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#10)]
     [!code-vb[c_CustomToken#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#10)]  
  
     [!code-csharp[c_customToken#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#11)]
     [!code-vb[c_customToken#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#11)]  
  
 A classe de parâmetros de token de segurança personalizado criada anteriormente é usada para informar à estrutura de segurança do WCF que um token de segurança personalizado deve ser usado ao se comunicar com um serviço. O procedimento a seguir mostra como isso pode ser feito.  
  
#### <a name="to-integrate-the-custom-security-token-with-the-binding"></a>Para integrar o token de segurança personalizado com a associação  
  
1. A classe Custom Security Token Parameters deve ser especificada em uma das coleções de parâmetros de token que são expostas na <xref:System.ServiceModel.Channels.SecurityBindingElement> classe. O exemplo a seguir usa a coleção retornada por `SignedEncrypted` . O código adiciona o token personalizado de cartão de crédito a todas as mensagens enviadas do cliente para o serviço com seu conteúdo assinado e criptografado automaticamente.  
  
     [!code-csharp[c_CustomToken#13](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#13)]
     [!code-vb[c_CustomToken#13](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#13)]  
  
 Este tópico mostra as várias partes de código necessárias para implementar e usar um token personalizado. Para ver um exemplo completo de como todas essas partes de código se ajustam, consulte [token personalizado](../samples/custom-token.md).  
  
## <a name="see-also"></a>Veja também

- <xref:System.IdentityModel.Tokens.SecurityToken>
- <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters>
- <xref:System.ServiceModel.Security.WSSecurityTokenSerializer>
- <xref:System.IdentityModel.Selectors.SecurityTokenProvider>
- <xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator>
- <xref:System.IdentityModel.Policy.IAuthorizationPolicy>
- <xref:System.IdentityModel.Selectors.SecurityTokenRequirement>
- <xref:System.IdentityModel.Selectors.SecurityTokenManager>
- <xref:System.ServiceModel.Description.ClientCredentials>
- <xref:System.ServiceModel.Description.ServiceCredentials>
- <xref:System.ServiceModel.Channels.SecurityBindingElement>
- [Passo a passo de criação de credenciais de serviço e cliente personalizados](walkthrough-creating-custom-client-and-service-credentials.md)
- [Como criar um autenticador de token de segurança personalizado](how-to-create-a-custom-security-token-authenticator.md)
- [Como criar um fornecedor de token de segurança personalizado](how-to-create-a-custom-security-token-provider.md)
