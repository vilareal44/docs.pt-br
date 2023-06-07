---
title: 'Como: criar um objeto WindowsPrincipal'
ms.date: 07/15/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WindowsPrincipal objects, creating
- security [.NET], creating a WindowsPrincipal object
- security [.NET], principals
- principal objects, creating
ms.assetid: 56eb10ca-e61d-4ed2-af7a-555fc4c25a25
ms.openlocfilehash: d99d63dc766f37e7cc30888d2e77657595f909af
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87557028"
---
# <a name="how-to-create-a-windowsprincipal-object"></a>Como: criar um objeto WindowsPrincipal

> [!NOTE]
> Este artigo aplica-se ao Windows.
>
> Para obter informações sobre ASP.NET Core, consulte [segurança do ASP.NET Core](/aspnet/core/security/).

Há duas maneiras de criar um <xref:System.Security.Principal.WindowsPrincipal> objeto, dependendo se o código deve executar repetidamente a validação baseada em função ou deve executá-la apenas uma vez.  
  
Se o código precisar executar a validação baseada em função repetidamente, o primeiro dos procedimentos a seguir produzirá menos sobrecarga. Quando o código precisa fazer validações baseadas em função apenas uma vez, você pode criar um <xref:System.Security.Principal.WindowsPrincipal> objeto usando o segundo dos procedimentos a seguir.  
  
### <a name="to-create-a-windowsprincipal-object-for-repeated-validation"></a>Para criar um objeto WindowsPrincipal para validação repetida  
  
1. Chame o <xref:System.AppDomain.SetPrincipalPolicy%2A> método no <xref:System.AppDomain> objeto que é retornado pela <xref:System.AppDomain.CurrentDomain%2A?displayProperty=nameWithType> propriedade estática, passando o método um valor de <xref:System.Security.Principal.PrincipalPolicy> enumeração que indica qual deve ser a nova política. Os valores com suporte são <xref:System.Security.Principal.PrincipalPolicy.NoPrincipal>, <xref:System.Security.Principal.PrincipalPolicy.UnauthenticatedPrincipal> e <xref:System.Security.Principal.PrincipalPolicy.WindowsPrincipal>. O código a seguir demonstra essa chamada de método.  
  
    ```csharp  
    AppDomain.CurrentDomain.SetPrincipalPolicy(  
        PrincipalPolicy.WindowsPrincipal);  
    ```  
  
    ```vb  
    AppDomain.CurrentDomain.SetPrincipalPolicy( _  
        PrincipalPolicy.WindowsPrincipal)  
    ```  
  
2. Com a política definida, use a <xref:System.Threading.Thread.CurrentPrincipal%2A?displayProperty=nameWithType> propriedade estática para recuperar a entidade de segurança que encapsula o usuário atual do Windows. Como o tipo de retorno da propriedade é <xref:System.Security.Principal.IPrincipal> , você deve converter o resultado em um <xref:System.Security.Principal.WindowsPrincipal> tipo. O código a seguir inicializa um novo <xref:System.Security.Principal.WindowsPrincipal> objeto para o valor da entidade de segurança associada ao thread atual.  
  
    ```csharp  
    WindowsPrincipal myPrincipal =
        (WindowsPrincipal) Thread.CurrentPrincipal;  
    ```  
  
    ```vb  
    Dim myPrincipal As WindowsPrincipal = _  
        CType(Thread.CurrentPrincipal, WindowsPrincipal)
    ```  
  
3. Quando o objeto principal tiver sido criado, você poderá usar um dos vários métodos para validá-lo.  
  
### <a name="to-create-a-windowsprincipal-object-for-a-single-validation"></a>Para criar um objeto WindowsPrincipal para uma única validação  
  
1. Inicialize um novo <xref:System.Security.Principal.WindowsIdentity> objeto chamando o método estático <xref:System.Security.Principal.WindowsIdentity.GetCurrent%2A?displayProperty=nameWithType> , que consulta a conta atual do Windows e coloca as informações sobre essa conta no objeto de identidade recém-criado. O código a seguir cria um novo <xref:System.Security.Principal.WindowsIdentity> objeto e o inicializa para o usuário autenticado atual.  
  
    ```csharp  
    WindowsIdentity myIdentity = WindowsIdentity.GetCurrent();  
    ```  
  
    ```vb  
    Dim myIdentity As WindowsIdentity = WindowsIdentity.GetCurrent()  
    ```  
  
2. Crie um novo <xref:System.Security.Principal.WindowsPrincipal> objeto e passe-o para o valor do <xref:System.Security.Principal.WindowsIdentity> objeto criado na etapa anterior.  
  
    ```csharp  
    WindowsPrincipal myPrincipal = new WindowsPrincipal(myIdentity);  
    ```  
  
    ```vb  
    Dim myPrincipal As New WindowsPrincipal(myIdentity)  
    ```  
  
3. Quando o objeto principal tiver sido criado, você poderá usar um dos vários métodos para validá-lo.  
  
## <a name="see-also"></a>Confira também

- [Objetos Principal e Identity](principal-and-identity-objects.md)
- [Segurança de ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/)
