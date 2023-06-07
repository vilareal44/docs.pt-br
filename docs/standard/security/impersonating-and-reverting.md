---
title: Representando e revertendo
ms.date: 07/15/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WindowsIdentity objects, impersonating
- security [.NET], impersonating Windows accounts
- impersonating Windows accounts
ms.assetid: b93d402c-6c28-4f50-b2bc-d9607dc3e470
ms.openlocfilehash: 7eecc7d6cb3fa4cc1c1bd971d36f9d3ca47a7144
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87555664"
---
# <a name="impersonating-and-reverting"></a>Representando e revertendo

> [!NOTE]
> Este artigo aplica-se ao Windows.
>
> Para obter informações sobre ASP.NET Core, consulte [segurança do ASP.NET Core](/aspnet/core/security/).

Às vezes, talvez seja necessário obter um token de conta do Windows para representar uma conta do Windows. Por exemplo, seu aplicativo baseado em ASP.NET pode ter que agir em nome de vários usuários em momentos diferentes. Seu aplicativo pode aceitar um token que representa um administrador do Serviços de Informações da Internet (IIS), representar esse usuário, executar uma operação e reverter para a identidade anterior. Em seguida, ele pode aceitar um token do IIS que representa um usuário com menos direitos, executar alguma operação e reverter novamente.  
  
 Em situações em que o aplicativo deve representar uma conta do Windows que não foi anexada ao thread atual pelo IIS, você deve recuperar o token dessa conta e usá-lo para ativar a conta. Você pode fazer isso executando as seguintes tarefas:  
  
1. Recuperar um token de conta para um usuário específico fazendo uma chamada para o método **LogonUser** não gerenciado. Esse método não está na biblioteca de classes base do .NET, mas está localizado no **advapi32.dll**não gerenciado. O acesso a métodos em código não gerenciado é uma operação avançada e está além do escopo desta discussão. Para obter mais informações, consulte [interoperação com código não gerenciado](../../framework/interop/index.md). Para obter mais informações sobre o método **LogonUser** e **advapi32.dll**, consulte a documentação do Platform SDK.  
  
2. Crie uma nova instância da classe **WindowsIdentity** , passando o token. O código a seguir demonstra essa chamada, onde `hToken` representa um token do Windows.  
  
    ```csharp  
    WindowsIdentity impersonatedIdentity = new WindowsIdentity(hToken);  
    ```  
  
    ```vb  
    Dim impersonatedIdentity As New WindowsIdentity(hToken)  
    ```  
  
3. Comece a representação criando uma nova instância da <xref:System.Security.Principal.WindowsImpersonationContext> classe e inicializando-a com o <xref:System.Security.Principal.WindowsIdentity.Impersonate%2A?displayProperty=nameWithType> método da classe inicializada, conforme mostrado no código a seguir.  
  
    ```csharp  
    WindowsImpersonationContext myImpersonation = impersonatedIdentity.Impersonate();  
    ```  
  
    ```vb  
    WindowsImpersonationContext myImpersonation = impersonatedIdentity.Impersonate()  
    ```  
  
4. Quando você não precisar mais representar, chame o <xref:System.Security.Principal.WindowsImpersonationContext.Undo%2A?displayProperty=nameWithType> método para reverter a representação, conforme mostrado no código a seguir.  
  
    ```csharp  
    myImpersonation.Undo();  
    ```  
  
    ```vb  
    myImpersonation.Undo()  
    ```  
  
 Se o código confiável já tiver anexado um <xref:System.Security.Principal.WindowsPrincipal> objeto ao thread, você poderá chamar o método de instância **Impersonate**, que não assume um token de conta. Observe que isso só é útil quando o objeto **WindowsPrincipal** no thread representa um usuário diferente daquele no qual o processo está em execução no momento. Por exemplo, você pode encontrar essa situação usando o ASP.NET com a autenticação do Windows ativada e a representação desativada. Nesse caso, o processo é executado em uma conta configurada no Serviços de Informações da Internet (IIS), enquanto a entidade atual representa o usuário do Windows que está acessando a página.  
  
 Observe que nem **Impersonate** nem **Undo** altera o objeto **principal** ( <xref:System.Security.Principal.IPrincipal> ) associado ao contexto de chamada atual. Em vez disso, a representação e a reversão alteram o token associado ao processo do sistema operacional atual.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Security.Principal.WindowsIdentity>
- <xref:System.Security.Principal.WindowsImpersonationContext>
- [Objetos Principal e Identity](principal-and-identity-objects.md)
- [Interoperação com código não gerenciado](../../framework/interop/index.md)
- [Segurança de ASP.NET Core](/aspnet/core/security/)
