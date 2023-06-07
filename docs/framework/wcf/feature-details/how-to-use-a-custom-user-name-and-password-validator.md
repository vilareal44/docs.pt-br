---
title: Como usar um validador personalizado de nome de usuário e senha
description: Saiba como incorporar um validador de senha personalizado para aplicativos WFC no lugar do nome de usuário e da validação de senha padrão do Windows.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, username and password
ms.assetid: 8e08b74b-fa44-4018-b63d-0d0805f85e3f
ms.openlocfilehash: 7f69db7bf8248593b64cdae4378983c2460de597
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246786"
---
# <a name="how-to-use-a-custom-user-name-and-password-validator"></a>Como usar um validador personalizado de nome de usuário e senha

Por padrão, quando um nome de usuário e uma senha são usados para autenticação, Windows Communication Foundation (WCF) usa o Windows para validar o nome de usuário e a senha. No entanto, o WCF permite esquemas de autenticação de senha e nome de usuário personalizados, também conhecidos como *validadores*. Para inserir um validador personalizado de nome de usuário e senha, crie uma classe que deriva de <xref:System.IdentityModel.Selectors.UserNamePasswordValidator> e configure-a.

Para obter um aplicativo de exemplo, consulte o [validador de senha do nome de usuário](../samples/user-name-password-validator.md).

### <a name="to-create-a-custom-user-name-and-password-validator"></a>Para criar um validador personalizado de nome de usuário e senha

1. Crie uma classe que deriva de <xref:System.IdentityModel.Selectors.UserNamePasswordValidator>.

    [!code-csharp[C_CustomUsernameAndPasswordValidator#3](~/samples/snippets/csharp/VS_Snippets_CFX/c_customusernameandpasswordvalidator/cs/service.cs#3)]
    [!code-vb[C_CustomUsernameAndPasswordValidator#3](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_customusernameandpasswordvalidator/vb/service.vb#3)]

2. Implemente o esquema de autenticação personalizado substituindo o método <xref:System.IdentityModel.Selectors.UserNamePasswordValidator.Validate%2A>.

    Não use o código no exemplo a seguir que substitui o método <xref:System.IdentityModel.Selectors.UserNamePasswordValidator.Validate%2A> em um ambiente de produção. Substitua o código pelo esquema personalizado de validação de nome de usuário e senha, o que pode envolver recuperar pares de nome de usuário e senha de um banco de dados.

    Para retornar erros de autenticação de volta para o cliente, gere um <xref:System.ServiceModel.FaultException> no método <xref:System.IdentityModel.Selectors.UserNamePasswordValidator.Validate%2A>.

    [!code-csharp[C_CustomUsernameAndPasswordValidator#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customusernameandpasswordvalidator/cs/service.cs#4)]
    [!code-vb[C_CustomUsernameAndPasswordValidator#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customusernameandpasswordvalidator/vb/service.vb#4)]

### <a name="to-configure-a-service-to-use-a-custom-user-name-and-password-validator"></a>Para configurar um serviço para usar um validador personalizado de nome de usuário e senha

1. Configure uma associação que usa segurança de mensagem sobre qualquer transporte ou segurança em nível de transporte sobre HTTP.

    Ao usar a segurança da mensagem, adicione uma das associações fornecidas pelo sistema, como um [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) , ou um [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) que dê suporte à segurança da mensagem e ao `UserName` tipo de credencial.

    Ao usar a segurança em nível de transporte por HTTP (S), adicione o [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) ou [\<basicHttpBinding>](../../configure-apps/file-schema/wcf/basichttpbinding.md) , um [\<netTcpBinding>](../../configure-apps/file-schema/wcf/nettcpbinding.md) ou um [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) que usa http (s) e o esquema de `Basic` autenticação.

    > [!NOTE]
    > Ao usar o .NET Framework 3,5 ou versões posteriores, você pode usar um validador de nome de usuário e senha personalizados com segurança de mensagens e transporte. Com o WinFX, um nome de usuário personalizado e um validador de senha só podem ser usados com a segurança da mensagem.

    > [!TIP]
    > Para obter mais informações sobre como usar \<netTcpBinding> esse contexto, consulte [\<security>](../../configure-apps/file-schema/wcf/security-of-nettcpbinding.md) .

    1. No arquivo de configuração, no [\<system.serviceModel>](../../configure-apps/file-schema/wcf/system-servicemodel.md) elemento, adicione um [\<bindings>](../../configure-apps/file-schema/wcf/bindings.md) elemento.

    2. Adicione um [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) [\<basicHttpBinding>](../../configure-apps/file-schema/wcf/basichttpbinding.md) elemento ou à seção de associações. Para obter mais informações sobre como criar um elemento de associação do WCF, consulte [como especificar uma associação de serviço na configuração](../how-to-specify-a-service-binding-in-configuration.md).

    3. Defina o `mode` atributo de [\<security>](../../configure-apps/file-schema/wcf/security-of-wshttpbinding.md) ou [\<security>](../../configure-apps/file-schema/wcf/security-of-basichttpbinding.md) para `Message` , `Transport` ou `TransportWithMessageCredential` .

    4. Defina o `clientCredentialType` atributo do [\<message>](../../configure-apps/file-schema/wcf/message-of-wshttpbinding.md) ou [\<transport>](../../configure-apps/file-schema/wcf/transport-of-wshttpbinding.md) .

        Ao usar a segurança da mensagem, defina o `clientCredentialType` atributo do [\<message>](../../configure-apps/file-schema/wcf/message-of-wshttpbinding.md) para `UserName` .

        Ao usar a segurança em nível de transporte por HTTP (S), defina o `clientCredentialType` atributo de [\<transport>](../../configure-apps/file-schema/wcf/transport-of-wshttpbinding.md) ou [\<transport>](../../configure-apps/file-schema/wcf/transport-of-basichttpbinding.md) para `Basic` .

        > [!NOTE]
        > Quando um serviço WCF é hospedado no Serviços de Informações da Internet (IIS) usando a segurança em nível de transporte e a <xref:System.ServiceModel.Security.UserNamePasswordServiceCredential.UserNamePasswordValidationMode%2A> propriedade é definida como <xref:System.ServiceModel.Security.UserNamePasswordValidationMode.Custom> , o esquema de autenticação personalizado usa um subconjunto da autenticação do Windows. Isso ocorre porque, nesse cenário, o IIS executa a autenticação do Windows antes do WCF invocando o autenticador personalizado.

    Para obter mais informações sobre como criar um elemento de associação do WCF, consulte [como especificar uma associação de serviço na configuração](../how-to-specify-a-service-binding-in-configuration.md).

    O exemplo a seguir mostra o código de configuração para a associação:

    ```xml
    <system.serviceModel>
      <bindings>
      <wsHttpBinding>
          <binding name="Binding1">
            <security mode="Message">
              <message clientCredentialType="UserName" />
            </security>
          </binding>
        </wsHttpBinding>
      </bindings>
    </system.serviceModel>
    ```

2. Configure um comportamento que especifica que um validador personalizado de nome de usuário e senha é usado para validar os pares de nome de usuário e senha para tokens de segurança de entrada do <xref:System.IdentityModel.Tokens.UserNameSecurityToken>.

    1. Como um filho para o [\<system.serviceModel>](../../configure-apps/file-schema/wcf/system-servicemodel.md) elemento, adicione um [\<behaviors>](../../configure-apps/file-schema/wcf/behaviors.md) elemento.

    2. Adicione um [\<serviceBehaviors>](../../configure-apps/file-schema/wcf/servicebehaviors.md) ao [\<behaviors>](../../configure-apps/file-schema/wcf/behaviors.md) elemento.

    3. Adicione um [\<behavior>](../../configure-apps/file-schema/wcf/behavior-of-servicebehaviors.md) elemento e defina o `name` atributo para um valor apropriado.

    4. Adicione um [\<serviceCredentials>](../../configure-apps/file-schema/wcf/servicecredentials.md) ao [\<behavior>](../../configure-apps/file-schema/wcf/behavior-of-servicebehaviors.md) elemento.

    5. Adicione um [\<userNameAuthentication>](../../configure-apps/file-schema/wcf/usernameauthentication.md) ao [\<serviceCredentials>](../../configure-apps/file-schema/wcf/servicecredentials.md) .

    6. Defina o `userNamePasswordValidationMode` para `Custom`.

        > [!IMPORTANT]
        > Se o `userNamePasswordValidationMode` valor não for definido, o WCF usará a autenticação do Windows em vez do nome de usuário e do validador de senha personalizados.

    7. Defina `customUserNamePasswordValidatorType` como o tipo que representa o seu validador personalizado de nome de usuário e senha.

    O exemplo a seguir mostra o `<serviceCredentials>` fragmento para este ponto:

    ```xml
    <serviceCredentials>
      <userNameAuthentication userNamePasswordValidationMode="Custom" customUserNamePasswordValidatorType="Microsoft.ServiceModel.Samples.CalculatorService.CustomUserNameValidator, service" />
    </serviceCredentials>
    ```

## <a name="example"></a>Exemplo

O exemplo de código a seguir demonstra como criar um validador personalizado de nome de usuário e senha. Não use o código que substitui o método <xref:System.IdentityModel.Selectors.UserNamePasswordValidator.Validate%2A> em um ambiente de produção. Substitua o código pelo esquema personalizado de validação de nome de usuário e senha, o que pode envolver recuperar pares de nome de usuário e senha de um banco de dados.

[!code-csharp[C_CustomUsernameAndPasswordValidator#1](~/samples/snippets/csharp/VS_Snippets_CFX/c_customusernameandpasswordvalidator/cs/service.cs#1)]
[!code-vb[C_CustomUsernameAndPasswordValidator#1](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_customusernameandpasswordvalidator/vb/service.vb#1)]
[!code-csharp[C_CustomUsernameAndPasswordValidator#2](~/samples/snippets/csharp/VS_Snippets_CFX/c_customusernameandpasswordvalidator/cs/service.cs#2)]
[!code-vb[C_CustomUsernameAndPasswordValidator#2](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_customusernameandpasswordvalidator/vb/service.vb#2)]

## <a name="see-also"></a>Veja também

- <xref:System.IdentityModel.Selectors.UserNamePasswordValidator>
- [Como utilizar o provedor de associação do ASP.NET](how-to-use-the-aspnet-membership-provider.md)
- [Autenticação](authentication-in-wcf.md)
