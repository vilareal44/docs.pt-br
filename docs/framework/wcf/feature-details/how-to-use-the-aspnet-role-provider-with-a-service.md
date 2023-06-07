---
title: Como utilizar o provedor de função do ASP.NET com um serviço
ms.date: 03/30/2017
ms.assetid: 88d33a81-8ac7-48de-978c-5c5b1257951e
ms.openlocfilehash: 45eeda046e877b4379d7d0e5edd90fac305f5e44
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595291"
---
# <a name="how-to-use-the-aspnet-role-provider-with-a-service"></a>Como utilizar o provedor de função do ASP.NET com um serviço

O provedor de função ASP.NET (em conjunto com o provedor de associação ASP.NET) é um recurso que permite aos desenvolvedores de ASP.NET criar sites da Web que permitem aos usuários criar uma conta com um site e receber funções para fins de autorização. Com esse recurso, qualquer usuário pode estabelecer uma conta com o site e fazer logon para ter acesso exclusivo ao site e aos seus serviços. Isso é diferente da segurança do Windows, o que exige que os usuários tenham contas em um domínio do Windows. Em vez disso, qualquer usuário que forneça suas credenciais (a combinação nome de usuário/senha) pode usar o site e seus serviços.  
  
Para um aplicativo de exemplo, consulte [associação e provedor de função](../samples/membership-and-role-provider.md). Para obter mais informações sobre o recurso de provedor de associação do ASP.NET, consulte [como: usar o provedor de associação do ASP.net](how-to-use-the-aspnet-membership-provider.md).  
  
O recurso de provedor de função usa um banco de dados SQL Server para armazenar informações do usuário. Os desenvolvedores de Windows Communication Foundation (WCF) podem aproveitar esses recursos para fins de segurança. Quando integrado a um aplicativo WCF, os usuários devem fornecer uma combinação de nome de usuário/senha para o aplicativo cliente do WCF. Para habilitar o WCF a usar o banco de dados, você deve criar uma instância da <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior> classe, definir sua <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.PrincipalPermissionMode%2A> propriedade como <xref:System.ServiceModel.Description.PrincipalPermissionMode.UseAspNetRoles> e adicionar a instância à coleção de comportamentos para o <xref:System.ServiceModel.ServiceHost> que está hospedando o serviço.  
  
## <a name="configure-the-role-provider"></a>Configurar o provedor de função  
  
1. No arquivo Web. config, no `system.web` elemento < >, adicione um `roleManager` elemento < > e defina seu `enabled` atributo como `true` .  
  
2. Defina o `defaultProvider` atributo como `SqlRoleProvider` .  
  
3. Como um filho para o `roleManager` elemento <>, adicione um `providers` elemento <>.  
  
4. Como um filho para o `providers` elemento <>, adicione um `add` elemento <> com os seguintes atributos definidos para os valores apropriados: `name` ,, `type` `connectionStringName` e `applicationName` , conforme mostrado no exemplo a seguir.  
  
    ```xml  
    <!-- Configure the Sql Role Provider. -->  
    <roleManager enabled ="true"
     defaultProvider ="SqlRoleProvider" >  
       <providers>  
         <add name ="SqlRoleProvider"
           type="System.Web.Security.SqlRoleProvider"
           connectionStringName="SqlConn"
           applicationName="MembershipAndRoleProviderSample"/>  
       </providers>  
    </roleManager>  
    ```  
  
## <a name="configure-the-service-to-use-the-role-provider"></a>Configurar o serviço para usar o provedor de função  
  
1. No arquivo Web. config, adicione um [\<system.serviceModel>](../../configure-apps/file-schema/wcf/system-servicemodel.md) elemento.  
  
2. Adicione um [\<behaviors>](../../configure-apps/file-schema/wcf/behaviors.md) elemento ao elemento <`system.ServiceModel`>.  
  
3. Adicione um [\<serviceBehaviors>](../../configure-apps/file-schema/wcf/servicebehaviors.md) ao elemento <`behaviors`>.  
  
4. Adicione um [\<behavior>](../../configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md) elemento e defina o `name` atributo para um valor apropriado.  
  
5. Adicione um [\<serviceAuthorization>](../../configure-apps/file-schema/wcf/serviceauthorization-element.md) ao elemento <`behavior`>.  
  
6. Defina o `principalPermissionMode` atributo como `UseAspNetRoles` .  
  
7. Defina o `roleProviderName` atributo como `SqlRoleProvider` . O exemplo a seguir mostra um fragmento da configuração.  
  
    ```xml  
    <behaviors>  
     <serviceBehaviors>  
      <behavior name="CalculatorServiceBehavior">  
       <serviceAuthorization principalPermissionMode ="UseAspNetRoles"  
                             roleProviderName ="SqlRoleProvider" />  
      </behavior>  
     </serviceBehaviors>  
    </behaviors>  
    ```  
  
## <a name="see-also"></a>Consulte também

- [Provedor de função e associação](../samples/membership-and-role-provider.md)
- [Como utilizar o provedor de associação do ASP.NET](how-to-use-the-aspnet-membership-provider.md)
