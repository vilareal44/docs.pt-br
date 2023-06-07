---
title: Integração de System.Web.Routing
ms.date: 03/30/2017
ms.assetid: 31fe2a4f-5c47-4e5d-8ee1-84c524609d41
ms.openlocfilehash: 059f14c94bb7502a2e4f4616ca2c5e6ac5273afa
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600731"
---
# <a name="systemwebrouting-integration"></a>Integração de System.Web.Routing
Ao hospedar um serviço de Windows Communication Foundation (WCF) no serviço de informações da Internet (IIS), você coloca um arquivo. svc no diretório virtual. Esse arquivo. svc especifica a fábrica de hosts de serviço a ser usada, bem como a classe que implementa o serviço. Ao fazer solicitações para o serviço, você especifica o arquivo. svc no URI, por exemplo: `http://contoso.com/EmployeeServce.svc` . Para programadores que escrevem serviços REST, esse tipo de URI não é o ideal. Os URIs para serviços REST especificam um recurso específico e normalmente não têm nenhuma extensão. O <xref:System.Web.Routing> recurso de integração permite que você hospede um serviço REST do WCF que responde a URIs sem uma extensão. Para obter mais informações sobre roteamento, consulte [ASP.net Routing](https://docs.microsoft.com/previous-versions/aspnet/cc668201(v=vs.100)).  
  
## <a name="using-systemwebrouting-integration"></a>Usando a integração de System. Web. Routing  
 Para usar o <xref:System.Web.Routing> recurso de integração, você usa a <xref:System.ServiceModel.Activation.ServiceRoute> classe para criar uma ou mais rotas e adicioná-las ao <xref:System.Web.Routing.RouteTable> em um arquivo global. asax. Essas rotas especificam os URIs relativos aos quais o serviço responde. O exemplo a seguir mostra como fazer isso.  
  
```aspx-csharp  
<%@ Application Language="C#" %>  
<%@ Import Namespace="System.Web.Routing" %>  
<%@ Import Namespace="System.ServiceModel.Activation" %>  
<%@ Import Namespace="System.ServiceModel.Web " %>  
  
<script RunAt="server">  
    void Application_Start(object sender, EventArgs e)  
    {  
        RegisterRoutes(RouteTable.Routes);  
    }  
  
    private void RegisterRoutes(RouteCollection routes)  
    {  
        routes.Add(new ServiceRoute("Customers", new WebServiceHostFactory(), typeof(Service)));
   }  
</script>  
```  
  
 Isso roteia todas as solicitações com um URI relativo que começa com os clientes para o `Service` serviço.  
  
 No arquivo Web. config, você deve adicionar o `System.Web.Routing.UrlRoutingModule` módulo, definir o `runAllManagedModulesForAllRequests` atributo como `true` e adicionar o `UrlRoutingHandler` manipulador ao elemento, `<system.webServer>` conforme mostrado no exemplo a seguir.  
  
```xml  
<system.webServer>  
      <modules runAllManagedModulesForAllRequests="true">  
        <add name="UrlRoutingModule" type="System.Web.Routing.UrlRoutingModule, System.Web, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" />  
      </modules>  
      <handlers>  
        <add name="UrlRoutingHandler" preCondition="integratedMode" verb="*" path="UrlRouting.axd"/>  
      </handlers>  
    </system.webServer>  
```  
  
 Isso carrega um módulo e um manipulador necessários para o roteamento. Para obter mais informações, consulte [Roteamento](routing.md). Você também deve definir o `aspNetCompatibilityEnabled` atributo como `true` no `<serviceHostingEnvironment>` elemento, conforme mostrado no exemplo a seguir.  
  
```xml  
<system.serviceModel>  
    <serviceHostingEnvironment aspNetCompatibilityEnabled="true"/>  
        <!-- ... -->  
    </system.serviceModel>  
```  
  
 A classe que implementa o serviço deve habilitar os requisitos de compatibilidade do ASP.NET, conforme mostrado no exemplo a seguir.  
  
```csharp
[ServiceContract]  
[AspNetCompatibilityRequirements(RequirementsMode=AspNetCompatibilityRequirementsMode.Allowed)]  
    public class Service  
    {  
        // ...  
    }  
```  
  
## <a name="see-also"></a>Consulte também

- [Modelo de programação WCF Web HTTP](wcf-web-http-programming-model.md)
- [Roteamento de ASP.NET](https://docs.microsoft.com/previous-versions/aspnet/cc668201(v=vs.100))
