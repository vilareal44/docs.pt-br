---
title: Como definir o modo de segurança
description: 'Saiba como definir os três modos de segurança comuns do WCF na maioria das associações predefinidas: transporte, mensagem e TransportWithMessageCredential.'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Mode property
- WCF, security mode
- WCF, security
ms.assetid: 6e01dd9f-b5dd-4474-b24c-06e124de4ff7
ms.openlocfilehash: 2f834e1930b7676592f6cbc29a577424d75ebc01
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244537"
---
# <a name="how-to-set-the-security-mode"></a>Como definir o modo de segurança

A segurança do Windows Communication Foundation (WCF) tem três modos de segurança comuns encontrados na maioria das associações predefinidas: transporte, mensagem e "transporte com credencial de mensagem". Dois modos adicionais são específicos de duas associações: o modo "somente credencial de transporte" encontrado no <xref:System.ServiceModel.BasicHttpBinding> , e o modo "ambos", encontrado no <xref:System.ServiceModel.NetMsmqBinding> . No entanto, este tópico se concentra nos três modos de segurança comuns: <xref:System.ServiceModel.SecurityMode.Transport> , <xref:System.ServiceModel.SecurityMode.Message> e <xref:System.ServiceModel.SecurityMode.TransportWithMessageCredential> .

Observe que nem toda Associação predefinida dá suporte a todos esses modos. Este tópico define o modo com as <xref:System.ServiceModel.WSHttpBinding> <xref:System.ServiceModel.NetTcpBinding> classes e e demonstra como definir o modo de forma programática e por meio da configuração.

Para obter mais informações, consulte segurança do WCF, consulte [visão geral de segurança](./feature-details/security-overview.md), [protegendo serviços](securing-services.md)e [protegendo serviços e clientes](./feature-details/securing-services-and-clients.md). Para obter mais informações sobre o modo de transporte e a mensagem, consulte [segurança de transporte](./feature-details/transport-security.md) e [segurança de mensagem](./feature-details/message-security-in-wcf.md).

## <a name="to-set-the-security-mode-in-code"></a>Para definir o modo de segurança no código

1. Crie uma instância da classe de associação que você está usando. Para obter uma lista de associações predefinidas, consulte [associações fornecidas pelo sistema](system-provided-bindings.md). Este exemplo cria uma instância da <xref:System.ServiceModel.WSHttpBinding> classe.

2. Defina a `Mode` Propriedade do objeto retornado pela `Security` propriedade.

     [!code-csharp[c_SettingSecurityMode#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_settingsecuritymode/cs/source.cs#1)]
     [!code-vb[c_SettingSecurityMode#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_settingsecuritymode/vb/source.vb#1)]

     Como alternativa, defina o modo como mensagem, conforme mostrado no código a seguir.

     [!code-csharp[c_SettingSecurityMode#2](../../../samples/snippets/csharp/VS_Snippets_CFX/c_settingsecuritymode/cs/source.cs#2)]
     [!code-vb[c_SettingSecurityMode#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_settingsecuritymode/vb/source.vb#2)]

     Ou defina o modo como transporte com credenciais de mensagem, conforme mostrado no código a seguir.

     [!code-csharp[c_SettingSecurityMode#3](../../../samples/snippets/csharp/VS_Snippets_CFX/c_settingsecuritymode/cs/source.cs#3)]
     [!code-vb[c_SettingSecurityMode#3](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_settingsecuritymode/vb/source.vb#3)]

3. Você também pode definir o modo no construtor da associação, conforme mostrado no código a seguir.

     [!code-csharp[c_SettingSecurityMode#4](../../../samples/snippets/csharp/VS_Snippets_CFX/c_settingsecuritymode/cs/source.cs#4)]
     [!code-vb[c_SettingSecurityMode#4](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_settingsecuritymode/vb/source.vb#4)]

## <a name="setting-the-clientcredentialtype-property"></a>Definindo a propriedade ClientCredentialtype

Definir o modo como um dos três valores determina como você define a `ClientCredentialType` propriedade. Por exemplo, usando a <xref:System.ServiceModel.WSHttpBinding> classe, definir o modo como `Transport` significa que você deve definir a <xref:System.ServiceModel.HttpTransportSecurity.ClientCredentialType%2A> propriedade da <xref:System.ServiceModel.HttpTransportSecurity> classe como um valor apropriado.

### <a name="to-set-the-clientcredentialtype-property-for-transport-mode"></a>Para definir a propriedade ClientCredentialtype para o modo de transporte

1. Crie uma instância da associação.

2. Defina a propriedade `Mode` como `Transport`.

3. Defina a propriedade `ClientCredential` com um valor apropriado. O código a seguir define a propriedade como `Windows` .

     [!code-csharp[c_SettingSecurityMode#5](../../../samples/snippets/csharp/VS_Snippets_CFX/c_settingsecuritymode/cs/source.cs#5)]
     [!code-vb[c_SettingSecurityMode#5](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_settingsecuritymode/vb/source.vb#5)]

### <a name="to-set-the-clientcredentialtype-property-for-message-mode"></a>Para definir a propriedade ClientCredentialtype para o modo de mensagem

1. Crie uma instância da associação.

2. Defina a propriedade `Mode` como `Message`.

3. Defina a propriedade `ClientCredential` com um valor apropriado. O código a seguir define a propriedade como `Certificate` .

     [!code-csharp[c_SettingSecurityMode#6](../../../samples/snippets/csharp/VS_Snippets_CFX/c_settingsecuritymode/cs/source.cs#6)]
     [!code-vb[c_SettingSecurityMode#6](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_settingsecuritymode/vb/source.vb#6)]

### <a name="to-set-the-mode-and-clientcredentialtype-property-in-configuration"></a>Para definir o modo e a propriedade ClientCredentialtype na configuração

1. Adicione um elemento de associação apropriado ao [\<bindings>](../configure-apps/file-schema/wcf/bindings.md) elemento do arquivo de configuração. O exemplo a seguir adiciona um [\<wsHttpBinding>](../configure-apps/file-schema/wcf/wshttpbinding.md) elemento.

2. Adicione um `<binding>` elemento e defina seu `name` atributo como um valor apropriado.

3. Adicione um `<security>` elemento e defina o `mode` atributo como `Message` , `Transport` ou `TransportWithMessageCredential` .

4. Se o modo for definido como `Transport` , adicione um `<transport>` elemento e defina o `clientCredential` atributo como um valor apropriado.

     O exemplo a seguir define o modo como " `Transport"` e, em seguida, define o `clientCredentialType` atributo do `<transport>` elemento como" `Windows"` .

    ```xml
    <wsHttpBinding>
    <binding name="TransportSecurity">
        <security mode="Transport" >
           <transport clientCredentialType = "Windows" />
        </security>
    </binding>
    </wsHttpBinding >
    ```

     Como alternativa, defina `security mode` como " `Message"` , seguido por um `<"message">` elemento. Este exemplo define o `clientCredentialType` como " `Certificate"` .

    ```xml
    <wsHttpBinding>
    <binding name="MessageSecurity">
        <security mode="Message" >
           <message clientCredentialType = "Certificate" />
        </security>
    </binding>
    </wsHttpBinding >
    ```

     Usar o <xref:System.ServiceModel.BasicHttpSecurityMode.TransportWithMessageCredential> valor é um caso especial e é explicado abaixo.

### <a name="using-transportwithmessagecredential"></a>Usando TransportWithMessageCredential

Ao definir o modo de segurança como `TransportWithMessageCredential` , o transporte determina o mecanismo real que fornece a segurança em nível de transporte. Por exemplo, o protocolo HTTP usa protocolo SSL (SSL) via HTTP (HTTPS). Portanto, a definição da `ClientCredentialType` propriedade de qualquer objeto de segurança de transporte (como <xref:System.ServiceModel.HttpTransportSecurity> ) é ignorada.  Em outras palavras, você só pode definir o `ClientCredentialType` do objeto de segurança da mensagem (para a `WSHttpBinding` associação, o <xref:System.ServiceModel.NonDualMessageSecurityOverHttp> objeto).

Para obter mais informações, consulte [como: usar segurança de transporte e credenciais de mensagem](./feature-details/how-to-use-transport-security-and-message-credentials.md).

## <a name="see-also"></a>Veja também

- [Como configurar uma porta com um certificado SSL](./feature-details/how-to-configure-a-port-with-an-ssl-certificate.md)
- [Como utilizar credenciais de mensagem e segurança de transporte](./feature-details/how-to-use-transport-security-and-message-credentials.md)
- [Segurança de transporte](./feature-details/transport-security.md)
- [Segurança de mensagem](./feature-details/message-security-in-wcf.md)
- [Visão geral de segurança](./feature-details/security-overview.md)
- [Associações fornecidas pelo sistema](system-provided-bindings.md)
- [\<security>](../configure-apps/file-schema/wcf/security-of-wshttpbinding.md)
- [\<security>](../configure-apps/file-schema/wcf/security-of-basichttpbinding.md)
- [\<security>](../configure-apps/file-schema/wcf/security-of-nettcpbinding.md)
