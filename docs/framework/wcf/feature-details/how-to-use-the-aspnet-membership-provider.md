---
title: Como utilizar o provedor de associação do ASP.NET
description: Saiba como o provedor de associação do ASP.NET dá suporte a sites que permitem que os usuários criem um nome de usuário e senha para acesso sem ter uma conta de domínio do Windows.
ms.date: 03/30/2017
helpviewer_keywords:
- WCF and ASP.NET
- WCF, authorization
- WCF, security
ms.assetid: 322c56e0-938f-4f19-a981-7b6530045b90
ms.openlocfilehash: 6d527993dcf1fc5d5cd39bf22c3e772baf60e62f
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246721"
---
# <a name="how-to-use-the-aspnet-membership-provider"></a>Como utilizar o provedor de associação do ASP.NET

O provedor de associação do ASP.NET é um recurso que permite que os desenvolvedores do ASP.NET criem sites da Web que permitem que os usuários criem combinações exclusivas de nome de usuário e senha. Com esse recurso, qualquer usuário pode estabelecer uma conta com o site e entrar para ter acesso exclusivo ao site e aos seus serviços. Isso é diferente da segurança do Windows, o que exige que os usuários tenham contas em um domínio do Windows. Em vez disso, qualquer usuário que forneça suas credenciais (a combinação nome de usuário/senha) pode usar o site e seus serviços.

Para um aplicativo de exemplo, consulte [associação e provedor de função](../samples/membership-and-role-provider.md). Para obter informações sobre como usar o recurso de provedor de função ASP.NET, consulte [como: usar o provedor de função ASP.NET com um serviço](how-to-use-the-aspnet-role-provider-with-a-service.md).

O recurso de associação requer o uso de um banco de dados SQL Server para armazenar as informações do usuário. O recurso também inclui métodos para solicitar uma pergunta a qualquer usuário que tenha esquecido sua senha.

Os desenvolvedores de Windows Communication Foundation (WCF) podem aproveitar esses recursos para fins de segurança. Quando integrado a um aplicativo WCF, os usuários devem fornecer uma combinação de nome de usuário/senha para o aplicativo cliente do WCF. Para transferir os dados para o serviço WCF, use uma associação que ofereça suporte a credenciais de nome de usuário/senha, como <xref:System.ServiceModel.WSHttpBinding> (em configuração, o [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) ) e defina o tipo de credencial do cliente como `UserName` . No serviço, a segurança do WCF autentica o usuário com base no nome de usuário e senha e também atribui a função especificada pela função ASP.NET.

> [!NOTE]
> O WCF não fornece métodos para popular o banco de dados com combinações de nome de usuário/senha ou outras informações de usuário.

### <a name="to-configure-the-membership-provider"></a>Para configurar o provedor de associação

1. No arquivo Web.config, no `system.web` elemento <>, crie um `membership` elemento <>.

2. No `<membership>` elemento, crie um `<providers>` elemento.

3. Como um filho para o `providers` elemento <>, adicione um `<clear />` elemento para liberar a coleção de provedores.

4. No `<clear />` elemento, crie um elemento <`add`> com os seguintes atributos definidos como valores apropriados:,,,,,, `name` `type` `connectionStringName` `applicationName` `enablePasswordRetrieval` `enablePasswordReset` `requiresQuestionAndAnswer` `requiresUniqueEmail` e `passwordFormat` . O `name` atributo é usado posteriormente como um valor no arquivo de configuração. O exemplo a seguir define como `SqlMembershipProvider` .

    O exemplo a seguir mostra a seção de configuração.

    ```xml
    <!-- Configure the Sql Membership Provider -->
    <membership defaultProvider="SqlMembershipProvider" userIsOnlineTimeWindow="15">
      <providers>
        <clear />
          <add
            name="SqlMembershipProvider"
            type="System.Web.Security.SqlMembershipProvider"
            connectionStringName="SqlConn"
            applicationName="MembershipAndRoleProviderSample"
            enablePasswordRetrieval="false"
            enablePasswordReset="false"
            requiresQuestionAndAnswer="false"
            requiresUniqueEmail="true"
            passwordFormat="Hashed" />
      </providers>
    </membership>
    ```

### <a name="to-configure-service-security-to-accept-the-user-namepassword-combination"></a>Para configurar a segurança do serviço para aceitar a combinação de nome de usuário/senha

1. No arquivo de configuração, no [\<system.serviceModel>](../../configure-apps/file-schema/wcf/system-servicemodel.md) elemento, adicione um [\<bindings>](../../configure-apps/file-schema/wcf/bindings.md) elemento.

2. Adicione um [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) à seção de associações. Para obter mais informações sobre como criar um elemento de associação do WCF, consulte [como especificar uma associação de serviço na configuração](../how-to-specify-a-service-binding-in-configuration.md).

3. Defina o atributo `mode` do elemento `<security>` como `Message`.

4. Defina o `clientCredentialType` atributo do elemento <`message`> como `UserName` . Isso especifica que um par de nome de usuário/senha será usado como a credencial do cliente.

    O exemplo a seguir mostra o código de configuração para a associação.

    ```xml
    <system.serviceModel>
    <bindings>
      <wsHttpBinding>
      <!-- Set up a binding that uses UserName as the client credential type -->
        <binding name="MembershipBinding">
          <security mode ="Message">
            <message clientCredentialType ="UserName"/>
          </security>
        </binding>
      </wsHttpBinding>
    </bindings>
    </system.serviceModel>
    ```

### <a name="to-configure-a-service-to-use-the-membership-provider"></a>Para configurar um serviço para usar o provedor de associação

1. Como um filho para o `<system.serviceModel>` elemento, adicione um [\<behaviors>](../../configure-apps/file-schema/wcf/behaviors.md) elemento

2. Adicione um [\<serviceBehaviors>](../../configure-apps/file-schema/wcf/servicebehaviors.md) ao elemento <`behaviors`>.

3. Adicione um [\<behavior>](../../configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md) e defina o `name` atributo como um valor apropriado.

4. Adicione um [\<serviceCredentials>](../../configure-apps/file-schema/wcf/servicecredentials.md) ao elemento <`behavior`>.

5. Adicione um [\<userNameAuthentication>](../../configure-apps/file-schema/wcf/usernameauthentication.md) ao `<serviceCredentials>` elemento.

6. Defina o `userNamePasswordValidationMode` atributo como `MembershipProvider` .

    > [!IMPORTANT]
    > Se o `userNamePasswordValidationMode` valor não for definido, o WCF usará a autenticação do Windows em vez do provedor de associação do ASP.net.

7. Defina o `membershipProviderName` atributo como o nome do provedor (especificado ao adicionar o provedor no primeiro procedimento neste tópico). O exemplo a seguir mostra o fragmento do `<serviceCredentials>` para este ponto.

    ```xml
    <behaviors>
       <serviceBehaviors>
          <behavior name="MyServiceBehavior">
             <serviceCredentials>
                <userNameAuthentication
                userNamePasswordValidationMode="MembershipProvider"
                membershipProviderName="SqlMembershipProvider" />
             </serviceCredentials>
          </behavior>
       </serviceBehaviors>
    </behaviors>
    ```

## <a name="example"></a>Exemplo

O código a seguir mostra a configuração de um serviço que usa o recurso associação ASP.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service behaviorConfiguration="MyServiceBehavior" name="Microsoft.Samples.GettingStarted.CalculatorService">
        <endpoint address="http://microsoft.com/WCFservices/Calculator"
          binding="wsHttpBinding" bindingConfiguration="MembershipBinding"
          name="ASPmemberUserName" contract="Microsoft.Samples.GettingStarted.ICalculator" />
      </service>
    </services>
    <behaviors>
      <serviceBehaviors>
        <behavior name="MyServiceBehavior">
          <serviceCredentials>
            <userNameAuthentication
              userNamePasswordValidationMode="MembershipProvider"
              membershipProviderName="SqlMembershipProvider" />
          </serviceCredentials>
        </behavior>
          </serviceBehaviors>
    </behaviors>
    <bindings>
      <wsHttpBinding>
        <binding name="MembershipBinding">
          <security mode="Message">
            <message clientCredentialType="UserName" />
          </security>
        </binding>
      </wsHttpBinding>
    </bindings>
  </system.serviceModel>
</configuration>
```

## <a name="see-also"></a>Veja também

- [Como utilizar o provedor de função do ASP.NET com um serviço](how-to-use-the-aspnet-role-provider-with-a-service.md)
- [Provedor de função e associação](../samples/membership-and-role-provider.md)
