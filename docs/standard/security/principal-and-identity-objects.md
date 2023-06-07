---
title: Objetos Principal e Identity
description: Leia sobre objetos de identidade, que representam usuários no .NET. Leia também sobre objetos de entidade de segurança, que encapsulam um objeto de identidade & uma função.
ms.date: 07/15/2020
ms.technology: dotnet-standard
helpviewer_keywords:
- WindowsIdentity objects
- GenericIdentity objects
- GenericPrincipal objects
- identity objects, about identity objects
- security [.NET], identity objects
- principal objects, about principal objects
- security [.NET], principals
- WindowsPrincipal objects
ms.assetid: aa5930ad-f3d7-40aa-b6f6-c6edcd5c64f7
ms.openlocfilehash: 79caeed6ed64a07238e398af1e12f51640b88b62
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87555248"
---
# <a name="principal-and-identity-objects"></a>Objetos Principal e Identity

> [!NOTE]
> Este artigo aplica-se ao Windows.
>
> Para obter informações sobre ASP.NET Core, consulte [segurança do ASP.NET Core](/aspnet/core/security/).

O código gerenciado pode descobrir a identidade ou a função de uma entidade de segurança por meio de um <xref:System.Security.Principal.IPrincipal> objeto, que contém uma referência a um <xref:System.Security.Principal.IIdentity> objeto. Pode ser útil comparar objetos de identidade e de entidade de segurança com conceitos familiares, como contas de usuário e de grupo. Na maioria dos ambientes de rede, as contas de usuário representam pessoas ou programas, enquanto as contas de grupo representam determinadas categorias de usuários e os direitos que eles possuem. Da mesma forma, os objetos de identidade do .NET representam usuários, enquanto as funções representam associações e contextos de segurança. No .NET, o objeto principal encapsula tanto um objeto de identidade quanto uma função. Os aplicativos .NET concedem direitos para a entidade de segurança com base em sua identidade ou, mais comumente, sua associação de função.  
  
## <a name="identity-objects"></a>Objetos de identidade

O objeto Identity encapsula informações sobre o usuário ou a entidade que está sendo validada. No nível mais básico, os objetos de identidade contêm um nome e um tipo de autenticação. O nome pode ser um nome de usuário ou o nome de uma conta do Windows, enquanto o tipo de autenticação pode ser um protocolo de logon com suporte, como Kerberos V5 ou um valor personalizado. O .NET define um <xref:System.Security.Principal.GenericIdentity> objeto que pode ser usado para a maioria dos cenários de logon personalizados e um objeto mais especializado <xref:System.Security.Principal.WindowsIdentity> que pode ser usado quando você deseja que seu aplicativo dependa da autenticação do Windows. Além disso, você pode definir sua própria classe de identidade que encapsula informações de usuário personalizadas.  
  
A <xref:System.Security.Principal.IIdentity> interface define as propriedades para acessar um nome e um tipo de autenticação, como Kerberos V5 ou NTLM. Todas as classes de **identidade** implementam a interface **IIdentity** . Não há nenhuma relação necessária entre um objeto de **identidade** e o token de processo do Windows NT sob o qual um thread está sendo executado no momento. No entanto, se o objeto de **identidade** for um objeto **WindowsIdentity** , a identidade será considerada para representar um token de segurança do Windows NT.  
  
## <a name="principal-objects"></a>Objetos principal

O objeto principal representa o contexto de segurança no qual o código está sendo executado. Aplicativos que implementam direitos de concessão de segurança baseada em função com base na função associada a um objeto principal. Semelhante aos objetos de identidade, o .NET fornece um <xref:System.Security.Principal.GenericPrincipal> objeto e um <xref:System.Security.Principal.WindowsPrincipal> objeto. Você também pode definir suas próprias classes de entidade personalizada.  
  
A <xref:System.Security.Principal.IPrincipal> interface define uma propriedade para acessar um objeto de **identidade** associado, bem como um método para determinar se o usuário identificado pelo objeto **principal** é membro de uma determinada função. Todas as classes de **entidade de segurança** implementam a interface **IPrincipal** , bem como quaisquer propriedades e métodos adicionais necessários. Por exemplo, o Common Language Runtime fornece a classe **WindowsPrincipal** , que implementa a funcionalidade adicional para mapear a associação de grupo do Windows NT ou do Windows 2000 para funções.  
  
Um objeto **principal** está associado a um objeto de contexto de chamada ( <xref:System.Runtime.Remoting.Messaging.CallContext> ) em um domínio de aplicativo ( <xref:System.AppDomain> ). Um contexto de chamada padrão sempre é criado com cada novo **AppDomain**, portanto, sempre há um contexto de chamada disponível para aceitar o objeto **principal** . Quando um novo thread é criado, um objeto **CallContext** também é criado para o thread. A referência do objeto **principal** é copiada automaticamente do thread de criação para o novo **CallContext**do thread. Se o tempo de execução não puder determinar qual objeto **principal** pertence ao criador do thread, ele seguirá a política padrão para a criação de objeto de **identidade** e **principal** .  
  
Uma política específica de domínio de aplicativo configurável define as regras para decidir que tipo de objeto **principal** associar a um novo domínio de aplicativo. Onde a política de segurança permite, o tempo de execução pode criar objetos **principal** e de **identidade** que refletem o token do sistema operacional associado ao thread atual de execução. Por padrão, o tempo de execução usa objetos **principal** e de **identidade** que representam usuários não autenticados. O tempo de execução não cria esses objetos de **identidade** e **principal** padrão até que o código tente acessá-los.  
  
O código confiável que cria um domínio de aplicativo pode definir a política de domínio do aplicativo que controla a construção dos objetos **principal** e de **identidade** padrão. Essa política específica de domínio de aplicativo se aplica a todos os threads de execução nesse domínio de aplicativo. Um host confiável e não gerenciado inerentemente tem a capacidade de definir essa política, mas o código gerenciado que define essa política deve ter o <xref:System.Security.Permissions.SecurityPermission?displayProperty=nameWithType> para controlar a política de domínio.  
  
Ao transmitir um objeto **principal** entre domínios de aplicativo, mas dentro do mesmo processo (e, portanto, no mesmo computador), a infraestrutura de comunicação remota copia uma referência ao objeto **principal** associado ao contexto do chamador ao contexto do receptor.  
  
## <a name="see-also"></a>Confira também

- [Como: criar um objeto WindowsPrincipal](how-to-create-a-windowsprincipal-object.md)
- [Como: criar objetos GenericPrincipal e GenericIdentity](how-to-create-genericprincipal-and-genericidentity-objects.md)
- [Substituindo um objeto Principal](replacing-a-principal-object.md)
- [Representando e revertendo](impersonating-and-reverting.md)
- [Segurança baseada em função](role-based-security.md)
- [Conceitos principais de segurança](key-security-concepts.md)
- [Segurança de ASP.NET Core](/aspnet/core/security/)
