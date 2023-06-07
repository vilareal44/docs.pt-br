---
title: 'Como: criar objetos GenericPrincipal e GenericIdentity'
ms.date: 07/15/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Creating Generic Identity Objects
- GenericPrincipal Objects
- Creating GenericPrincipal Objects
- GenericIdentity Objects
ms.assetid: 465694cf-258b-4747-9dae-35b01a5bcdbb
ms.openlocfilehash: 903d636938c47850951330d7936d95470441607e
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87557210"
---
# <a name="how-to-create-genericprincipal-and-genericidentity-objects"></a>Como: criar objetos GenericPrincipal e GenericIdentity

> [!NOTE]
> Este artigo aplica-se ao Windows.
>
> Para obter informações sobre ASP.NET Core, consulte [visão geral da segurança do ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/).

Você pode usar a <xref:System.Security.Principal.GenericIdentity> classe em conjunto com a <xref:System.Security.Principal.GenericPrincipal> classe para criar um esquema de autorização que existe independentemente de um domínio do Windows.

### <a name="to-create-a-genericprincipal-object"></a>Para criar um objeto GenericPrincipal

1. Crie uma nova instância da classe Identity e inicialize-a com o nome que você deseja que ela mantenha. O código a seguir cria um novo objeto **GenericIdentity** e inicializa-o com o nome `MyUser` .

    ```vb
    Dim myIdentity As New GenericIdentity("MyUser")
    ```

    ```csharp
    GenericIdentity myIdentity = new GenericIdentity("MyUser");
    ```

2. Crie uma nova instância da classe **GenericPrincipal** e inicialize-a com o objeto **GenericIdentity** criado anteriormente e uma matriz de cadeias de caracteres que representem as funções que você deseja associar a essa entidade de segurança. O exemplo de código a seguir especifica uma matriz de cadeias de caracteres que representam uma função de administrador e uma função de usuário. Em seguida, o **GenericPrincipal** é inicializado com o **GenericIdentity** anterior e a matriz de cadeia de caracteres.

    ```vb
    Dim myStringArray As String() = {"Manager", "Teller"}
    DIm myPrincipal As New GenericPrincipal(myIdentity, myStringArray)
    ```

    ```csharp
    String[] myStringArray = {"Manager", "Teller"};
    GenericPrincipal myPrincipal = new GenericPrincipal(myIdentity, myStringArray);
    ```

3. Use o código a seguir para anexar a entidade de segurança ao thread atual. Isso é valioso em situações em que a entidade de segurança deve ser validada várias vezes, ela deve ser validada por outro código em execução em seu aplicativo ou deve ser validada por um <xref:System.Security.Permissions.PrincipalPermission> objeto. Você ainda pode executar a validação baseada em função no objeto principal sem anexá-lo ao thread. Para obter mais informações, consulte [substituindo um objeto principal](replacing-a-principal-object.md).

    ```vb
    Thread.CurrentPrincipal = myPrincipal
    ```

    ```csharp
    Thread.CurrentPrincipal = myPrincipal;
    ```

## <a name="example"></a>Exemplo

O exemplo de código a seguir demonstra como criar uma instância de um **GenericPrincipal** e um **GenericIdentity**. Esse código exibe os valores desses objetos para o console.

```vb
Imports System.Security.Principal
Imports System.Threading

Public Class Class1

    Public Shared Sub Main()
        ' Create generic identity.
        Dim myIdentity As New GenericIdentity("MyIdentity")

        ' Create generic principal.
        Dim myStringArray As String() =  {"Manager", "Teller"}
        Dim myPrincipal As New GenericPrincipal(myIdentity, myStringArray)

        ' Attach the principal to the current thread.
        ' This is not required unless repeated validation must occur,
        ' other code in your application must validate, or the
        ' PrincipalPermission object is used.
        Thread.CurrentPrincipal = myPrincipal

        ' Print values to the console.
        Dim name As String = myPrincipal.Identity.Name
        Dim auth As Boolean = myPrincipal.Identity.IsAuthenticated
        Dim isInRole As Boolean = myPrincipal.IsInRole("Manager")

        Console.WriteLine("The name is: {0}", name)
        Console.WriteLine("The isAuthenticated is: {0}", auth)
        Console.WriteLine("Is this a Manager? {0}", isInRole)

    End Sub

End Class
```

```csharp
using System;
using System.Security.Principal;
using System.Threading;

public class Class1
{
    public static int Main(string[] args)
    {
    // Create generic identity.
    GenericIdentity myIdentity = new GenericIdentity("MyIdentity");

    // Create generic principal.
    String[] myStringArray = {"Manager", "Teller"};
    GenericPrincipal myPrincipal =
        new GenericPrincipal(myIdentity, myStringArray);

    // Attach the principal to the current thread.
    // This is not required unless repeated validation must occur,
    // other code in your application must validate, or the
    // PrincipalPermission object is used.
    Thread.CurrentPrincipal = myPrincipal;

    // Print values to the console.
    String name =  myPrincipal.Identity.Name;
    bool auth =  myPrincipal.Identity.IsAuthenticated;
    bool isInRole =  myPrincipal.IsInRole("Manager");

    Console.WriteLine("The name is: {0}", name);
    Console.WriteLine("The isAuthenticated is: {0}", auth);
    Console.WriteLine("Is this a Manager? {0}", isInRole);

    return 0;
    }
}
```

Quando executado, o aplicativo exibe uma saída semelhante à seguinte.

```console
The Name is: MyIdentity
The IsAuthenticated is: True
Is this a Manager? True
```

## <a name="see-also"></a>Confira também

- <xref:System.Security.Principal.GenericIdentity>
- <xref:System.Security.Principal.GenericPrincipal>
- <xref:System.Security.Permissions.PrincipalPermission>
- [Substituindo um objeto Principal](replacing-a-principal-object.md)
- [Objetos Principal e Identity](principal-and-identity-objects.md)
