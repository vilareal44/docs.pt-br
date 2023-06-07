---
title: Using Multiple Authentication Schemes with WCF
ms.date: 03/30/2017
ms.assetid: f32a56a0-e2b2-46bf-a302-29e1275917f9
ms.openlocfilehash: 1874963573a6ec12939bd12b79574f1e2c889bfd
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600212"
---
# <a name="using-multiple-authentication-schemes-with-wcf"></a>Using Multiple Authentication Schemes with WCF
O WCF agora permite que você especifique vários esquemas de autenticação em um único ponto de extremidade. Além disso, os serviços hospedados na Web podem herdar suas configurações de autenticação diretamente do IIS. Os serviços auto-hospedados podem especificar quais esquemas de autenticação podem ser usados. Para obter mais informações sobre como definir configurações de autenticação no IIS, consulte [autenticação do IIS](https://go.microsoft.com/fwlink/?LinkId=232458)  
  
## <a name="iis-hosted-services"></a>Serviços hospedados no IIS  
 Para serviços hospedados no IIS, defina os esquemas de autenticação que você deseja usar no IIS. Em seguida, no arquivo Web. config do serviço, na sua configuração de associação, especifique o tipo clientCredential como "InheritedFromHost", conforme mostrado no trecho de código XML a seguir:  
  
```xml  
<bindings>  
      <basicHttpBinding>  
        <binding name="secureBinding">  
          <security mode="Transport">  
            <transport clientCredentialType="InheritedFromHost" />  
          </security>  
        </binding>  
      </basicHttpBinding>  
    </bindings>  
```  
  
 Você pode especificar que você deseja que apenas um subconjunto de esquemas de autenticação seja usado com seu serviço usando o ServiceAuthenticationBehavior ou o \<serviceAuthenticationManager> elemento. Ao configurar isso no código, use o ServiceAuthenticationBehavior conforme mostrado no trecho de código a seguir.  
  
```csharp  
// ...  
ServiceAuthenticationBehavior sab = null;  
sab = serviceHost.Description.Behaviors.Find<ServiceAuthenticationBehavior>();  
  
if (sab == null)  
{  
    sab = new ServiceAuthenticationBehavior();  
    sab.AuthenticationSchemes = AuthenticationSchemes.Basic | AuthenticationSchemes.Negotiate | AuthenticationSchemes.Digest;  
    serviceHost.Description.Behaviors.Add(sab);  
}  
else  
{  
     sab.AuthenticationSchemes = AuthenticationSchemes.Basic | AuthenticationSchemes.Negotiate | AuthenticationSchemes.Digest;  
}  
// ...  
```  
  
 Ao configurar isso em um arquivo de configuração, use o \<serviceAuthenticationManager> elemento, conforme mostrado no trecho de código XML a seguir.  
  
```xml  
<behaviors>  
      <serviceBehaviors>  
        <behavior name="limitedAuthBehavior">  
          <serviceAuthenticationManager authenticationSchemes="Negotiate, Digest, Basic"/>  
          <!-- ... -->  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
```  
  
 Isso garantirá que apenas um subconjunto dos esquemas de autenticação listados aqui será considerado para ser aplicado no ponto de extremidade do serviço, dependendo do que está selecionado no IIS. Isso significa que um desenvolvedor pode excluir, por exemplo, a autenticação básica da lista omitindo-a da listagem de serviços de autenticação e, mesmo que esteja habilitada no IIS, ela não será aplicada no ponto de extremidade de serviço  
  
## <a name="self-hosted-services"></a>Serviços hospedados internamente  
 Os serviços auto-hospedados são configurados de maneira um pouco diferente, já que não há nenhum IIS para herdar as configurações. Aqui, você usa o \<serviceAuthenticationManager> elemento ou ServiceAuthenticationBehavior para especificar as configurações de autenticação que serão herdadas. No código, ele tem esta aparência:  
  
```csharp  
// ...  
ServiceAuthenticationBehavior sab = null;  
sab = serviceHost.Description.Behaviors.Find<ServiceAuthenticationBehavior>();  
  
if (sab == null)  
{  
    sab = new ServiceAuthenticationBehavior();  
    sab.AuthenticationSchemes = AuthenticationSchemes.Basic | AuthenticationSchemes.Negotiate | AuthenticationSchemes.Digest;  
    serviceHost.Description.Behaviors.Add(sab);  
}  
else  
{  
     sab.AuthenticationSchemes = AuthenticationSchemes.Basic | AuthenticationSchemes.Negotiate | AuthenticationSchemes.Digest;  
}  
// ...  
```  
  
 Em configuração, ele tem a seguinte aparência:  
  
```xml  
<behaviors>  
      <serviceBehaviors>  
        <behavior name="limitedAuthBehavior">  
          <serviceAuthenticationManager authenticationSchemes="Negotiate, Digest, Basic"/>  
          <!-- ... -->  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
```  
  
 E, em seguida, você pode especificar InheritFromHost em suas configurações de associação, conforme mostrado no trecho de código XML a seguir.  
  
```xml  
<bindings>  
      <basicHttpBinding>  
        <binding name="secureBinding">  
          <security mode="Transport">  
            <transport clientCredentialType="InheritedFromHost" />  
          </security>  
        </binding>  
      </basicHttpBinding>  
    </bindings>  
```  
  
 Como alternativa, você pode especificar os esquemas de autenticação em uma associação personalizada, definindo os esquemas de autenticação no elemento de associação de transporte HTTP, conforme mostrado no trecho de configuração a seguir.  
  
```xml  
<binding name="multipleBinding">  
      <textMessageEncoding/>  
      <httpTransport authenticationScheme="Negotiate, Ntlm, Digest, Basic" />  
    </binding>  
```  
  
## <a name="see-also"></a>Consulte também

- [Associações e segurança](bindings-and-security.md)
- [Pontos de extremidade: endereços, associações e contratos](endpoints-addresses-bindings-and-contracts.md)
- [Configurando associações fornecidas pelo sistema](configuring-system-provided-bindings.md)
- [Recursos de segurança com associações personalizadas](security-capabilities-with-custom-bindings.md)
- [Associações](bindings.md)
- [Associações personalizadas](../extending/custom-bindings.md)
