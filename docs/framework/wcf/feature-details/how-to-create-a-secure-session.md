---
title: Como criar uma sessão segura
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- security [WCF], creating a session
ms.assetid: b6f42b5a-bbf7-45cf-b917-7ec9fa7ae110
ms.openlocfilehash: 80973a31050cf1ede03d4a3919066c62625ae590
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84593406"
---
# <a name="how-to-create-a-secure-session"></a>Como criar uma sessão segura
Com exceção da [\<basicHttpBinding>](../../configure-apps/file-schema/wcf/basichttpbinding.md) associação, as associações fornecidas pelo sistema no Windows Communication Foundation (WCF) usam automaticamente sessões seguras quando a segurança da mensagem está habilitada.  
  
 Por padrão, as sessões seguras não sobrevivem a um servidor Web que é reciclado. Quando uma sessão segura é estabelecida, o cliente e o serviço armazenam em cache a chave que está associada à sessão segura. À medida que as mensagens são trocadas, apenas um identificador para a chave armazenada em cache é trocado. Se o servidor Web for reciclado, o cache também será reciclado, de modo que o servidor Web não possa recuperar a chave armazenada em cache para o identificador. Se isso acontecer, uma exceção será gerada de volta para o cliente. As sessões seguras que usam um identificador de contexto de segurança com estado (SCT) podem sobreviver a um servidor Web sendo reciclado. Para obter mais informações sobre como usar um SCT com estado em uma sessão segura, consulte [como: criar um token de contexto de segurança para uma sessão segura](how-to-create-a-security-context-token-for-a-secure-session.md).  
  
### <a name="to-specify-that-a-service-uses-secure-sessions-by-using-one-of-the-system-provided-bindings"></a>Para especificar que um serviço usa sessões seguras usando uma das associações fornecidas pelo sistema  
  
- Configure um serviço para usar uma associação fornecida pelo sistema que dê suporte à segurança de mensagens.  
  
     Com exceção da [\<basicHttpBinding>](../../configure-apps/file-schema/wcf/basichttpbinding.md) associação, quando as associações fornecidas pelo sistema são configuradas para usar a segurança da mensagem, o WCF usa automaticamente as sessões seguras. A tabela a seguir lista as associações fornecidas pelo sistema que dão suporte à segurança da mensagem e se a segurança da mensagem é o mecanismo de segurança padrão.  
  
    |Associação fornecida pelo sistema|Elemento de configuração|Segurança de mensagem ativada por padrão|  
    |------------------------------|---------------------------|------------------------------------|  
    |<xref:System.ServiceModel.BasicHttpBinding>|[\<basicHttpBinding>](../../configure-apps/file-schema/wcf/basichttpbinding.md)|Não|  
    |<xref:System.ServiceModel.WSHttpBinding>|[\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md)|Sim|  
    |<xref:System.ServiceModel.WSDualHttpBinding>|[\<wsDualHttpBinding>](../../configure-apps/file-schema/wcf/wsdualhttpbinding.md)|Sim|  
    |<xref:System.ServiceModel.WSFederationHttpBinding>|[\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md)|Sim|  
    |<xref:System.ServiceModel.NetTcpBinding>|[\<netTcpBinding>](../../configure-apps/file-schema/wcf/nettcpbinding.md)|Não|  
    |<xref:System.ServiceModel.NetMsmqBinding>|[\<netMsmqBinding>](../../configure-apps/file-schema/wcf/netmsmqbinding.md)|Não|  
  
     O exemplo de código a seguir usa a configuração para especificar uma associação denominada `wsHttpBinding_Calculator` que usa o [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) , segurança de mensagem e sessões seguras.  
  
    ```xml  
    <bindings>  
      <WSHttpBinding>  
       <binding name = "wsHttpBinding_Calculator">  
         <security mode="Message">  
           <message clientCredentialType="Windows"/>  
         </security>  
        </binding>  
      </WSHttpBinding>  
    </bindings>  
    ```  
  
     O exemplo de código a seguir especifica que a [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) , segurança de mensagem e sessões seguras são usadas para proteger o `secureCalculator` serviço.  
  
     [!code-csharp[c_CreateSecureSession#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_createsecuresession/cs/secureservice.cs#1)]
     [!code-vb[c_CreateSecureSession#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_createsecuresession/vb/secureservice.vb#1)]  
  
    > [!NOTE]
    > As sessões seguras podem ser desativadas para o [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) definindo o `establishSecurityContext` atributo como `false` . Para as outras associações fornecidas pelo sistema, as sessões seguras só podem ser desativadas com a criação de uma associação personalizada.  
  
### <a name="to-specify-that-a-service-uses-secure-sessions-by-using-a-custom-binding"></a>Para especificar que um serviço usa sessões seguras usando uma associação personalizada  
  
- Crie uma associação personalizada que especifica que as mensagens SOAP são protegidas por uma sessão segura.  
  
     Para obter mais informações sobre como criar uma associação personalizada, consulte [como: personalizar uma associação fornecida pelo sistema](../extending/how-to-customize-a-system-provided-binding.md).  
  
     O exemplo de código a seguir usa a configuração para especificar uma ligação personalizada que mensagens usando uma sessão segura.  
  
    ```xml  
    <bindings>  
      <!-- configure a custom binding -->  
      <customBinding>  
        <binding name="customBinding_Calculator">  
          <security authenticationMode="SecureConversation" />  
          <secureConversationBootstrap authenticationMode="SspiNegotiated" />  
          <textMessageEncoding messageVersion="Soap12WSAddressing10" writeEncoding="utf-8"/>  
          <httpTransport/>  
        </binding>  
      </customBinding>  
    </bindings>  
    ```  
  
     O exemplo de código a seguir cria uma associação personalizada que usa o <xref:System.ServiceModel.Configuration.AuthenticationMode.MutualCertificate> modo de autenticação para inicializar uma sessão segura.  
  
     [!code-csharp[c_CreateSecureSession#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_createsecuresession/cs/secureservice.cs#2)]
     [!code-vb[c_CreateSecureSession#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_createsecuresession/vb/secureservice.vb#2)]  
  
## <a name="see-also"></a>Consulte também

- [Visão geral de associações do WCF](../bindings-overview.md)
