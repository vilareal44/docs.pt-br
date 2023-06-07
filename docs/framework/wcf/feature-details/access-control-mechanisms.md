---
title: Mecanismos de controle de acesso
ms.date: 03/30/2017
helpviewer_keywords:
- WCF security
- access control [WCF]
ms.assetid: 9d576122-3f55-4425-9acf-b23d0781e966
ms.openlocfilehash: 27f2b7d3146199f1c3e9a228202618c992e2a1ea
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601355"
---
# <a name="access-control-mechanisms"></a>Mecanismos de controle de acesso
Você pode controlar o acesso de várias maneiras com o Windows Communication Foundation (WCF). Este tópico aborda brevemente os vários mecanismos e fornece sugestões sobre quando usar cada um deles; Ele destina-se a ajudá-lo a selecionar o mecanismo correto a ser usado. As tecnologias de acesso são listadas em ordem de complexidade. O mais simples é o <xref:System.Security.Permissions.PrincipalPermissionAttribute> ; o mais complexo é o modelo de identidade.  
  
 Além desses mecanismos, a representação e a delegação com o WCF são explicadas em [delegação e representação](delegation-and-impersonation-with-wcf.md).  
  
## <a name="principalpermissionattribute"></a>PrincipalPermissionAttribute  
 O <xref:System.Security.Permissions.PrincipalPermissionAttribute> é usado para restringir o acesso a um método de serviço. Quando o atributo é aplicado a um método, ele pode ser usado para solicitar uma identidade ou associação de um chamador específico em um grupo do Windows ou uma função ASP.NET. Se o cliente for autenticado usando um certificado X. 509, ele receberá uma identidade primária que consiste no nome da entidade mais a impressão digital do certificado.  
  
 Use o <xref:System.Security.Permissions.PrincipalPermissionAttribute> para controlar o acesso aos recursos no computador em que o serviço está sendo executado e, se os usuários do serviço forem sempre parte do mesmo domínio do Windows em que o serviço está sendo executado. Você pode criar facilmente grupos do Windows que tenham níveis especificados de acesso (como nenhum, somente leitura ou leitura e gravação).  
  
 Para obter mais informações sobre como usar o atributo, consulte [como: restringir o acesso com a classe PrincipalPermissionAttribute](../how-to-restrict-access-with-the-principalpermissionattribute-class.md). Para obter mais informações sobre identidade, consulte [identidade de serviço e autenticação](service-identity-and-authentication.md).  
  
## <a name="aspnet-membership-provider"></a>Provedor de associação do ASP.NET  
 Um recurso do ASP.NET é o provedor de associação. Embora o provedor de associação não seja tecnicamente um mecanismo de controle de acesso, ele permite controlar o acesso ao serviço, limitando o conjunto de possíveis identidades que podem acessar o ponto de extremidade do serviço. O recurso de associação inclui um banco de dados que pode ser preenchido com combinações de nome de usuário/senha que permitem que os usuários de um site estabeleçam contas com o site. Para acessar um serviço que usa o provedor de associação, um usuário deve fazer logon com seu nome de usuário e senha.  
  
> [!NOTE]
> Primeiro, você deve preencher o banco de dados usando o recurso ASP.NET antes que um serviço WCF possa usá-lo para fins de autorização.  
  
 Você também pode usar o recurso de associação se já tiver um banco de dados de associação de um site ASP.NET existente e desejar habilitar os mesmos usuários para usar seu serviço, autorizado com os mesmos nomes de usuário e senhas.  
  
 Para obter mais informações sobre como usar o recurso de associação em um serviço WCF, consulte [como: usar o provedor de associação ASP.net](how-to-use-the-aspnet-membership-provider.md).  
  
## <a name="aspnet-role-provider"></a>Provedor de função ASP.NET  
 Outro recurso do ASP.NET é a capacidade de gerenciar a autorização usando funções. O provedor de função ASP.NET permite que um desenvolvedor crie funções para usuários e atribua a cada usuário uma função ou funções. Assim como com o provedor de associação, as funções e atribuições são armazenadas em um banco de dados e podem ser populadas usando as ferramentas fornecidas por uma implementação específica do provedor de função ASP.NET. Assim como com o recurso de associação, os desenvolvedores do WCF podem usar as informações no banco de dados para autorizar usuários de serviço por funções. Eles podem, por exemplo, usar o provedor de função em combinação com o `PrincipalPermissionAttribute` mecanismo de controle de acesso descrito acima.  
  
 Você também pode usar o provedor de função ASP.NET se tiver um banco de dados de provedor de função do ASP.NET existente e quiser usar o mesmo conjunto de regras e atribuições de usuário em seu serviço WCF.  
  
 Para obter mais informações sobre como usar o recurso de provedor de função, consulte [como: usar o provedor de função ASP.NET com um serviço](how-to-use-the-aspnet-role-provider-with-a-service.md).  
  
## <a name="authorization-manager"></a>Gerenciador de Autorização  
 Outro recurso combina o AzMan (Gerenciador de autorização) com o provedor de função ASP.NET para autorizar clientes. Quando o ASP.NET hospeda um serviço Web, o AzMan pode ser integrado ao aplicativo para que a autorização para o serviço seja feita por meio do AzMan. O Gerenciador de funções do ASP.NET fornece uma API que permite que você gerencie funções de aplicativo, adicione e remova usuários de funções e verifique a associação de função, mas não permite que você consulte se um usuário está autorizado a executar uma tarefa ou operação nomeada. O AzMan permite que você defina operações individuais e combine-as em tarefas. Com o AZMan, além das verificações de função, você também pode verificar se um usuário pode executar uma tarefa. A atribuição de função e a autorização de tarefa podem ser configuradas fora do aplicativo ou executadas programaticamente no aplicativo. O snap-in do MMC (console de gerenciamento Microsoft) de administração do AzMan permite que os administradores alterem as tarefas que uma função pode executar em tempo de execução e para gerenciar a associação de funções de cada usuário.  
  
 Você também pode usar o AzMan e o provedor de função ASP.NET se você já tiver acesso a uma instalação existente do AzMan e quiser autorizar os usuários de serviço usando os recursos da combinação de provedor de AzMan/função.  
  
 Para obter mais informações sobre o AzMan e o provedor de função ASP.NET, consulte [como: usar o Gerenciador de autorização (AzMan) com o ASP.NET 2,0](https://docs.microsoft.com/previous-versions/msp-n-p/ff649313(v=pandp.10)). Para obter mais informações sobre como usar o AzMan e o provedor de função para serviços WCF, consulte [como: usar o provedor de função Gerenciador de autorização ASP.NET com um serviço](how-to-use-the-aspnet-authorization-manager-role-provider-with-a-service.md).  
  
## <a name="identity-model"></a>Modelo de identidade  
 O modelo de identidade é um conjunto de APIs que permite que você gerencie declarações e políticas para autorizar clientes. Com o modelo de identidade, você pode examinar todas as declarações contidas nas credenciais que o chamador usou para se autenticar no serviço, comparar as declarações com o conjunto de políticas para o serviço e conceder ou negar acesso com base na comparação.  
  
 Use o modelo de identidade se você precisar de controle fino e a capacidade de definir critérios específicos antes de conceder acesso. Por exemplo, ao usar o <xref:System.Security.Permissions.PrincipalPermissionAttribute> , o critério é simplesmente que a identidade do usuário é autenticada e pertence a uma função específica. Por outro lado, usando o modelo de identidade, você pode criar uma política que declara que o usuário deve ter mais de 18 anos de idade e possuir uma licença de driver válida antes de ter permissão para exibir um documento.  
  
 Um exemplo em que você pode se beneficiar do controle de acesso baseado em declaração de modelo de identidade é ao usar credenciais de Federação no cenário de token emitido. Para obter mais informações sobre Federação e tokens emitidos, consulte [Federação e tokens emitidos](federation-and-issued-tokens.md).  
  
 Para obter mais informações sobre o modelo de identidade, consulte [Gerenciando declarações e autorização com o modelo de identidade](managing-claims-and-authorization-with-the-identity-model.md).  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Security.Permissions.PrincipalPermissionAttribute>
- [Como restringir o acesso com a classe PrincipalPermissionAttribute](../how-to-restrict-access-with-the-principalpermissionattribute-class.md)
- [Como utilizar o provedor de função do ASP.NET com um serviço](how-to-use-the-aspnet-role-provider-with-a-service.md)
- [Como usar o provedor de função do gerenciador de autorização do ASP.NET com um serviço](how-to-use-the-aspnet-authorization-manager-role-provider-with-a-service.md)
- [Gerenciamento de declarações e autorizações com o modelo de identidade](managing-claims-and-authorization-with-the-identity-model.md)
- [Delegação e representação](delegation-and-impersonation-with-wcf.md)
