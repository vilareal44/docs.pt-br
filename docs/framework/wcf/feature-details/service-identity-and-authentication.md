---
title: Identidade e autenticação de serviço
description: Saiba mais sobre a identidade do ponto de extremidade de um serviço, um valor gerado a partir do WSDL do serviço, que o WCF usa para autenticar o serviço.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- authentication [WCF], specifying the identity of a service
ms.assetid: a4c8f52c-5b30-45c4-a545-63244aba82be
ms.openlocfilehash: ae217b4a2c3432321c7ef2e663922a87b82acbea
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246565"
---
# <a name="service-identity-and-authentication"></a>Identidade e autenticação de serviço
A identidade do *ponto de extremidade* de um serviço é um valor gerado a partir do serviço WSDL (Web Services Description Language). Esse valor, propagado para qualquer cliente, é usado para autenticar o serviço. Depois que o cliente inicia uma comunicação com um ponto de extremidade e o serviço se autentica para o cliente, o cliente compara o valor de identidade do ponto de extremidade com o valor real que o processo de autenticação do ponto de extremidade retornou. Se eles corresponderem, o cliente terá a garantia de que ele contatou o ponto de extremidade de serviço esperado. Isso funciona como uma proteção contra *phishing* , impedindo que um cliente seja redirecionado para um ponto de extremidade hospedado por um serviço mal-intencionado.  
  
 Para obter um aplicativo de exemplo que demonstre a configuração de identidade, consulte [exemplo de identidade de serviço](../samples/service-identity-sample.md). Para obter mais informações sobre pontos de extremidade e endereços de ponto de extremidades, consulte [endereços](endpoint-addresses.md).  
  
> [!NOTE]
> Quando você usa NT LanMan (NTLM) para autenticação, a identidade do serviço não é verificada porque, em NTLM, o cliente não consegue autenticar o servidor. O NTLM é usado quando os computadores fazem parte de um grupo de trabalho do Windows ou ao executar uma versão mais antiga do Windows que não oferece suporte à autenticação Kerberos.  
  
 Quando o cliente inicia um canal seguro para enviar uma mensagem a um serviço por ele, a infraestrutura Windows Communication Foundation (WCF) autentica o serviço e só envia a mensagem se a identidade do serviço corresponder à identidade especificada no endereço do ponto de extremidade que o cliente usa.  
  
 O processamento de identidade consiste nos seguintes estágios:  
  
- No momento do design, o desenvolvedor do cliente determina a identidade do serviço dos metadados do ponto de extremidade (expostos por meio do WSDL).  
  
- Em tempo de execução, o aplicativo cliente verifica as declarações das credenciais de segurança do serviço antes de enviar qualquer mensagem para o serviço.  
  
 O processamento de identidades no cliente é análogo à autenticação de cliente no serviço. Um serviço seguro não executa código até que as credenciais do cliente tenham sido autenticadas. Da mesma forma, o cliente não envia mensagens para o serviço até que as credenciais do serviço tenham sido autenticadas com base no que é conhecido com antecedência dos metadados do serviço.  
  
 A <xref:System.ServiceModel.EndpointAddress.Identity%2A> propriedade da <xref:System.ServiceModel.EndpointAddress> classe representa a identidade do serviço chamado pelo cliente. O serviço publica o <xref:System.ServiceModel.EndpointAddress.Identity%2A> em seus metadados. Quando o desenvolvedor cliente executa a [ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) em relação ao ponto de extremidade de serviço, a configuração gerada contém o valor da Propriedade do serviço <xref:System.ServiceModel.EndpointAddress.Identity%2A> . A infraestrutura do WCF (se configurada com segurança) verifica se o serviço possui a identidade especificada.  
  
> [!IMPORTANT]
> Os metadados contêm a identidade esperada do serviço, portanto, é recomendável que você exponha os metadados do serviço por meio de meios seguros, por exemplo, criando um ponto de extremidade HTTPS para o serviço. Para obter mais informações, consulte [como: proteger pontos de extremidade de metadados](how-to-secure-metadata-endpoints.md).  
  
## <a name="identity-types"></a>Tipos de identidade  
 Um serviço pode fornecer seis tipos de identidades. Cada tipo de identidade corresponde a um elemento que pode estar contido dentro do `<identity>` elemento na configuração. O tipo usado depende do cenário e dos requisitos de segurança do serviço. A tabela a seguir descreve cada tipo de identidade.  
  
|Tipo de identidade|Description|Cenário típico|  
|-------------------|-----------------|----------------------|  
|Sistema de nome de domínio (DNS)|Use esse elemento com certificados X. 509 ou contas do Windows. Ele compara o nome DNS especificado na credencial com o valor especificado neste elemento.|Uma verificação de DNS permite que você use certificados com nomes DNS ou de entidade. Se um certificado for reemitido com o mesmo nome DNS ou da entidade, a verificação de identidade ainda será válida. Quando um certificado é reemitido, ele obtém uma nova chave RSA, mas retém o mesmo nome DNS ou assunto. Isso significa que os clientes não precisam atualizar suas informações de identidade sobre o serviço.|  
|Certificado. O padrão quando `ClientCredentialType` é definido como certificado.|Esse elemento especifica um valor de certificado X. 509 codificado na base64 para comparar com o cliente.<br /><br /> Use também esse elemento ao usar um CardSpace como uma credencial para autenticar o serviço.|Esse elemento restringe a autenticação a um único certificado com base no seu valor de impressão digital. Isso habilita a autenticação mais estrita porque os valores de impressão digital são exclusivos. Isso é fornecido com uma limitação: se o certificado for reemitido com o mesmo nome de assunto, ele também terá uma nova impressão digital. Portanto, os clientes não podem validar o serviço, a menos que a nova impressão digital seja conhecida. Para obter mais informações sobre como localizar a impressão digital de um certificado, consulte [como recuperar a impressão digital de um certificado](how-to-retrieve-the-thumbprint-of-a-certificate.md).|  
|Referência do certificado|Idêntico à opção de certificado descrita anteriormente. No entanto, esse elemento permite que você especifique um nome de certificado e o local de armazenamento do qual recuperar o certificado.|O mesmo que o cenário de certificado descrito anteriormente.<br /><br /> O benefício é que o local do repositório de certificados pode ser alterado.|  
|RSA|Esse elemento especifica um valor de chave RSA para comparar com o cliente. Isso é semelhante à opção de certificado, mas em vez de usar a impressão digital do certificado, a chave RSA do certificado é usada em vez disso.|Uma verificação RSA permite restringir especificamente a autenticação a um único certificado com base em sua chave RSA. Isso permite uma autenticação mais estrita de uma chave RSA específica às custas do serviço, o que não funciona mais com os clientes existentes se o valor da chave RSA for alterado.|  
|Nome UPN. O padrão quando o `ClientCredentialType` é definido como Windows e o processo de serviço não está em execução em uma das contas do sistema.|Esse elemento Especifica o UPN sob o qual o serviço está sendo executado. Consulte a seção protocolo e identidade Kerberos de [substituindo a identidade de um serviço para autenticação](../extending/overriding-the-identity-of-a-service-for-authentication.md).|Isso garante que o serviço esteja sendo executado em uma conta de usuário do Windows específica. A conta de usuário pode ser o usuário conectado no momento ou o serviço em execução em uma conta de usuário específica.<br /><br /> Essa configuração aproveita a segurança Kerberos do Windows se o serviço estiver sendo executado em uma conta de domínio dentro de um ambiente de Active Directory.|  
|SPN (nome da entidade de serviço). O padrão quando o `ClientCredentialType` é definido como Windows e o processo de serviço está em execução em uma das contas de sistema – LocalService, LocalSystem ou NetworkService.|Esse elemento Especifica o SPN associado à conta do serviço. Consulte a seção protocolo e identidade Kerberos de [substituindo a identidade de um serviço para autenticação](../extending/overriding-the-identity-of-a-service-for-authentication.md).|Isso garante que o SPN e a conta específica do Windows associada ao SPN identifiquem o serviço.<br /><br /> Você pode usar a ferramenta Setspn.exe para associar uma conta de computador à conta de usuário do serviço.<br /><br /> Essa configuração aproveita a segurança Kerberos do Windows se o serviço estiver em execução em uma das contas do sistema ou em uma conta de domínio que tenha um nome SPN associado e o computador seja membro de um domínio em um ambiente de Active Directory.|  
  
## <a name="specifying-identity-at-the-service"></a>Especificando a identidade no serviço  
 Normalmente, você não precisa definir a identidade em um serviço porque a seleção de um tipo de credencial de cliente determina o tipo de identidade exposto nos metadados do serviço. Para obter mais informações sobre como substituir ou especificar a identidade do serviço, consulte [substituindo a identidade de um serviço para autenticação](../extending/overriding-the-identity-of-a-service-for-authentication.md).  
  
## <a name="using-the-identity-element-in-configuration"></a>Usando o \<identity> elemento na configuração  
 Se você alterar o tipo de credencial do cliente na associação anteriormente mostrada para `Certificate,` , o WSDL gerado conterá um certificado X. 509 serializado na base64 para o valor de identidade, conforme mostrado no código a seguir. Esse é o padrão para todos os tipos de credencial do cliente que não sejam o Windows.  

 Você pode alterar o valor da identidade do serviço padrão ou alterar o tipo da identidade usando o `<identity>` elemento na configuração ou definindo a identidade no código. O código de configuração a seguir define uma identidade de DNS (sistema de nomes de domínio) com o valor `contoso.com` .  

## <a name="setting-identity-programmatically"></a>Definindo a identidade programaticamente  
 O serviço não precisa especificar uma identidade explicitamente, pois o WCF a determina automaticamente. No entanto, o WCF permite que você especifique uma identidade em um ponto de extremidade, se necessário. O código a seguir adiciona um novo ponto de extremidade de serviço com uma identidade DNS específica.  
  
 [!code-csharp[C_Identity#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_identity/cs/source.cs#5)]
 [!code-vb[C_Identity#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_identity/vb/source.vb#5)]  
  
## <a name="specifying-identity-at-the-client"></a>Especificando a identidade no cliente  
 Em tempo de design, um desenvolvedor cliente normalmente usa a [ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) para gerar a configuração do cliente. O arquivo de configuração gerado (destinado ao uso pelo cliente) contém a identidade do servidor. Por exemplo, o código a seguir é gerado de um serviço que especifica uma identidade DNS, conforme mostrado no exemplo anterior. Observe que o valor de identidade do ponto de extremidade do cliente corresponde ao do serviço. Nesse caso, quando o cliente recebe as credenciais do Windows (Kerberos) para o serviço, ele espera que o valor seja `contoso.com` .  

 Se, em vez do Windows, o serviço especificar um certificado como o tipo de credencial do cliente, espera-se que a propriedade DNS do certificado seja o valor `contoso.com` . (Ou se a propriedade DNS for `null` , o nome da entidade do certificado deverá ser `contoso.com` .)  
  
#### <a name="using-a-specific-value-for-identity"></a>Usando um valor específico para identidade  
 O arquivo de configuração do cliente a seguir mostra como espera-se que a identidade do serviço seja um valor específico. No exemplo a seguir, o cliente pode se comunicar com dois pontos de extremidade. O primeiro é identificado com uma impressão digital do certificado e o segundo com uma chave RSA de certificado. Ou seja, um certificado que contém apenas um par chave pública/chave privada, mas não é emitido por uma autoridade confiável.  

## <a name="identity-checking-at-run-time"></a>Verificação de identidade em tempo de execução  
 Em tempo de design, um desenvolvedor cliente determina a identidade do servidor por meio de seus metadados. Em tempo de execução, a verificação de identidade é executada antes de chamar qualquer ponto de extremidade no serviço.  
  
 O valor de identidade está vinculado ao tipo de autenticação especificado pelos metadados; em outras palavras, o tipo de credenciais usado para o serviço.  
  
 Se o canal estiver configurado para autenticar usando protocolo SSL de nível de mensagem ou de transporte (SSL) com certificados X. 509 para autenticação, os seguintes valores de identidade serão válidos:  
  
- DNS. O WCF garante que o certificado fornecido durante o handshake SSL contenha um atributo DNS ou `CommonName` (CN) igual ao valor especificado na identidade DNS no cliente. Observe que essas verificações são feitas além da determinação da validade do certificado do servidor. Por padrão, o WCF valida que o certificado do servidor é emitido por uma autoridade raiz confiável.  
  
- Certificado. Durante o handshake de SSL, o WCF garante que o ponto de extremidade remoto forneça o valor de certificado exato especificado na identidade.  
  
- Referência de certificado. O mesmo que o certificado.  
  
- RSA. Durante o handshake de SSL, o WCF garante que o ponto de extremidade remoto forneça a chave RSA exata especificada na identidade.  
  
 Se o serviço se autenticar usando SSL de nível de transporte ou de mensagem com uma credencial do Windows para autenticação e negociar a credencial, os seguintes valores de identidade serão válidos:  
  
- DNS. A negociação passa o SPN do serviço para que o nome DNS possa ser verificado. O SPN está no formato `host/<dns name>` .  
  
- SPNs. Um SPN de serviço explícito é retornado, por exemplo, `host/myservice` .  
  
- Sufixo. O UPN da conta de serviço. O UPN está no formato `username` @ `domain` . Por exemplo, quando o serviço está sendo executado em uma conta de usuário, pode ser `username@contoso.com` .  
  
 Especificar a identidade programaticamente (usando a <xref:System.ServiceModel.EndpointAddress.Identity%2A> Propriedade) é opcional. Se nenhuma identidade for especificada e o tipo de credencial do cliente for Windows, o padrão será SPN com o valor definido para a parte hostname do endereço do ponto de extremidade de serviço prefixado com o literal "host/". Se nenhuma identidade for especificada e o tipo de credencial do cliente for um certificado, o padrão será `Certificate` . Isso se aplica à segurança em nível de transporte e de mensagem.  
  
## <a name="identity-and-custom-bindings"></a>Identidades e associações personalizadas  
 Como a identidade de um serviço depende do tipo de associação usado, verifique se uma identidade apropriada é exposta ao criar uma associação personalizada. Por exemplo, no exemplo de código a seguir, a identidade exposta não é compatível com o tipo de segurança, porque a identidade para a associação de inicialização de conversa segura não corresponde à identidade da associação no ponto de extremidade. A associação de conversa segura define a identidade DNS, enquanto o <xref:System.ServiceModel.Channels.WindowsStreamSecurityBindingElement> define a identidade UPN ou SPN.  
  
 [!code-csharp[C_Identity#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_identity/cs/source.cs#8)]
 [!code-vb[C_Identity#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_identity/vb/source.vb#8)]  
  
 Para obter mais informações sobre como empilhar os elementos de associação corretamente para uma associação personalizada, consulte [Criando associações definidas pelo usuário](../extending/creating-user-defined-bindings.md). Para obter mais informações sobre como criar uma associação personalizada com o <xref:System.ServiceModel.Channels.SecurityBindingElement> , consulte [como: criar um SecurityBindingElement para um modo de autenticação especificado](how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md).  
  
## <a name="see-also"></a>Veja também

- [Como criar uma associação personalizada utilizando o SecurityBindingElement](how-to-create-a-custom-binding-using-the-securitybindingelement.md)
- [Como criar um SecurityBindingElement para um modo de autenticação especificado](how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)
- [Como criar um verificador de identidade de cliente personalizado](../extending/how-to-create-a-custom-client-identity-verifier.md)
- [Selecionando um tipo de credencial](selecting-a-credential-type.md)
- [Trabalhando com certificados](working-with-certificates.md)
- [Ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)
- [Criando associações definidas pelo usuário](../extending/creating-user-defined-bindings.md)
- [Como recuperar a impressão digital de um certificado](how-to-retrieve-the-thumbprint-of-a-certificate.md)
