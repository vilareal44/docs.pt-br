---
ms.openlocfilehash: 2c8a5c1ec87918a91600a3557c679a42cae74896
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556315"
---
### <a name="principalpermissionattribute-is-obsolete-as-error"></a>PrincipalPermissionattribute está obsoleto como erro

O <xref:System.Security.Permissions.PrincipalPermissionAttribute> Construtor é obsoleto e produz um erro de tempo de compilação. Não é possível instanciar esse atributo ou aplicá-lo a um método.

#### <a name="change-description"></a>Descrição da alteração

No .NET Framework e no .NET Core, você pode anotar métodos com o <xref:System.Security.Permissions.PrincipalPermissionAttribute> atributo. Por exemplo:

```csharp
[PrincipalPermission(SecurityAction.Demand, Role = "Administrators")]
public void MyMethod()
{
    // Code that should only run when the current user is an administrator.
}
```

A partir do .NET 5,0, você não pode aplicar o <xref:System.Security.Permissions.PrincipalPermissionAttribute> atributo a um método. O construtor do atributo é obsoleto e produz um erro de tempo de compilação. Ao contrário de outros avisos do obsoletion, não é possível suprimir o erro.

#### <a name="reason-for-change"></a>Motivo da alteração

O <xref:System.Security.Permissions.PrincipalPermissionAttribute> tipo, como outros tipos dos quais a subclasse faz <xref:System.Security.Permissions.SecurityAttribute> parte. Infraestrutura de CAS (segurança de acesso ao código) da rede. No .NET Framework 2. x-4. x, o tempo de execução impõe <xref:System.Security.Permissions.PrincipalPermissionAttribute> anotações na entrada do método, mesmo que o aplicativo esteja sendo executado em um cenário de confiança total. O .NET Core e o .NET 5,0 e posterior não dão suporte a atributos de CAS e o tempo de execução os ignora.

Essa diferença no comportamento de .NET Framework para .NET Core e .NET 5,0 pode resultar em um cenário de "falha aberta", em que o acesso deveria ter sido bloqueado, mas, em vez disso, ter sido permitido. Para evitar o cenário de "falha aberta", você não pode mais aplicar o atributo no código que tem como destino o .NET 5,0 ou posterior.

#### <a name="version-introduced"></a>Versão introduzida

5,0 Preview 7

#### <a name="recommended-action"></a>Ação recomendada

Se você encontrar o erro obsoletion, deverá executar uma ação.

- **Se você estiver aplicando o atributo a um método de ação ASP.NET MVC:**

  Considere o uso do ASP. A infraestrutura de autorização interna da rede. O código a seguir demonstra como anotar um controlador com um <xref:System.Web.Mvc.AuthorizeAttribute> atributo. O tempo de execução do ASP.NET autorizará o usuário antes de executar a ação.

  ```csharp
  using Microsoft.AspNetCore.Authorization;

  namespace MySampleApp
  {
      [Authorize(Roles = "Administrator")]
      public class AdministrationController : Controller
      {
          public ActionResult MyAction()
          {
              // This code won't run unless the current user
              // is in the 'Administrator' role.
          }
      }
  }
  ```

  Para obter mais informações, consulte [autorização baseada em função no ASP.NET Core](/aspnet/core/security/authorization/roles) e [introdução à autorização no ASP.NET Core](/aspnet/core/security/authorization/introduction).

- **Se você estiver aplicando o atributo ao código de biblioteca fora do contexto de um aplicativo Web:**

  Execute as verificações manualmente no início do método. Isso pode ser feito usando o <xref:System.Security.Principal.IPrincipal.IsInRole(System.String)?displayProperty=nameWithType> método.

  ```csharp
  using System.Threading;

  void DoSomething()
  {
      if (Thread.CurrentPrincipal == null
          || !Thread.CurrentPrincipal.IsInRole("Administrators"))
      {
          throw new Exception("User is anonymous or isn't an admin.");
      }

      // Code that should run only when user is an administrator.
  }
  ```

#### <a name="category"></a>Categoria

- Bibliotecas principais do .NET
- Segurança

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Security.Permissions.PrincipalPermissionAttribute?displayProperty=fullName>

<!--

#### Affected APIs

- `T:System.Security.Permissions.PrincipalPermissionAttribute`

-->
