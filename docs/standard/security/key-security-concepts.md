---
title: Conceitos principais de segurança
ms.date: 07/15/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- unauthorized access
- permissions [.NET]
- security [.NET], about security
ms.assetid: 3cfced4f-ea02-4e66-ae98-d69286363e98
ms.openlocfilehash: 259723b903377f7e79731e1ff79b3d512581102f
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87555261"
---
# <a name="key-security-concepts"></a>Conceitos principais de segurança

> [!NOTE]
> Este artigo aplica-se ao Windows.
>
> Para obter informações sobre ASP.NET Core, consulte [visão geral da segurança do ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/).

O .NET oferece segurança baseada em função para ajudar a resolver questões de segurança sobre o código móvel e fornecer suporte que permite que os componentes determinem o que os usuários estão autorizados a fazer.  
  
## <a name="type-safety-and-security"></a>Proteção de tipo e segurança  

O código de tipo seguro acessa apenas os locais de memória que ele está autorizado a acessar. (Para essa discussão, a segurança de tipo se refere especificamente à segurança do tipo de memória e não deve ser confundida com a segurança de tipo em um aspecto mais amplo.) Por exemplo, o código de tipo seguro não pode ler valores dos campos particulares de outro objeto. Ele acessa tipos apenas de maneiras bem definidas e permitidas.  
  
Durante a compilação JIT (just-in-time), um processo de verificação opcional examina os metadados e a MSIL (Microsoft Intermediate Language) de um método para ser compilado por JIT no código do computador nativo para verificar se eles são de tipo seguro. Esse processo será ignorado se o código tiver permissão para ignorar a verificação. Para obter mais informações sobre a verificação, consulte [processo de execução gerenciada](../managed-execution-process.md).  
  
Embora a verificação da segurança de tipo não seja obrigatória para executar código gerenciado, o tipo de segurança desempenha uma função crucial no isolamento de assembly e na imposição de segurança. Quando o código é de tipo seguro, o Common Language Runtime pode isolar completamente os assemblies uns dos outros. Esse isolamento ajuda a garantir que os assemblies não possam afetar negativamente uns aos outros e aumenta a confiabilidade do aplicativo. Os componentes de tipo seguro podem ser executados com segurança no mesmo processo, mesmo se forem confiáveis em diferentes níveis. Quando o código não é de tipo seguro, podem ocorrer efeitos colaterais indesejados. Por exemplo, o tempo de execução não pode impedir que o código gerenciado chame em código nativo (não gerenciado) e execute operações mal-intencionadas. Quando o código é de tipo seguro, o mecanismo de imposição de segurança do tempo de execução garante que ele não acesse o código nativo, a menos que tenha permissão para fazer isso. Todo o código que não é de tipo seguro deve ter sido concedido <xref:System.Security.Permissions.SecurityPermission> com o membro de enumeração passado <xref:System.Security.Permissions.SecurityPermissionAttribute.SkipVerification%2A> para ser executado.  
  
Para obter mais informações, consulte [Noções Básicas da Segurança de Acesso do Código](../../framework/misc/code-access-security-basics.md).  
  
## <a name="principal"></a>Principal  

Uma entidade de segurança representa a identidade e a função de um usuário e age em nome do usuário. A segurança baseada em função no .NET dá suporte a três tipos de entidades:  
  
- As entidades de segurança genéricas representam usuários e funções que existem independentemente de usuários e funções do Windows.  
  
- As entidades de segurança do Windows representam usuários do Windows e suas funções (ou seus grupos do Windows). Uma entidade de segurança do Windows pode representar outro usuário, o que significa que a entidade de segurança pode acessar um recurso em nome de um usuário ao apresentar a identidade que pertence a esse usuário.  
  
- Entidades personalizadas podem ser definidas por um aplicativo de qualquer forma que seja necessária para esse aplicativo específico. Eles podem estender a noção básica da identidade e das funções da entidade de segurança.  
  
Para obter mais informações, consulte [objetos principal e de identidade](principal-and-identity-objects.md).  
  
## <a name="authentication"></a>Autenticação  
A autenticação é o processo de descoberta e verificação da identidade de uma entidade de segurança examinando as credenciais do usuário e validando essas credenciais em alguma autoridade. As informações obtidas durante a autenticação do são diretamente utilizáveis pelo seu código. Você também pode usar a segurança baseada em função do .NET para autenticar o usuário atual e para determinar se deve permitir que essa entidade acesse seu código. Consulte as sobrecargas do <xref:System.Security.Principal.WindowsPrincipal.IsInRole%2A?displayProperty=nameWithType> método para obter exemplos de como autenticar a entidade de segurança para funções específicas. Por exemplo, você pode usar a <xref:System.Security.Principal.WindowsPrincipal.IsInRole%28System.String%29?displayProperty=nameWithType> sobrecarga para determinar se o usuário atual é membro do grupo Administradores.  
  
Uma variedade de mecanismos de autenticação são usados hoje, muitos dos quais podem ser usados com a segurança baseada em função do .NET. Alguns dos mecanismos mais comumente usados são Basic, Digest, Passport, sistema operacional (como NTLM ou Kerberos) ou mecanismos definidos pelo aplicativo.  
  
### <a name="example"></a>Exemplo  

O exemplo a seguir requer que a entidade de segurança ativa seja um administrador. O `name` parâmetro é `null` , que permite que qualquer usuário que seja um administrador passe a demanda.  
  
> [!NOTE]
> No Windows Vista, o UAC (controle de conta de usuário) determina os privilégios de um usuário. Se for um membro do grupo Administradores Internos, você receberá dois tokens de acesso do tempo de execução: um token de acesso do usuário padrão e um token de acesso do administrador. Por padrão, você está na função de usuário padrão. Para executar o código que exige que você seja um administrador, você deve primeiro elevar seus privilégios do usuário padrão para o administrador. Você pode fazer isso ao iniciar um aplicativo clicando com o botão direito do mouse no ícone do aplicativo e indicando que deseja executar como administrador.  
  
 [!code-cpp[Classic PrincipalPermission Example#1](../../../samples/snippets/cpp/VS_Snippets_CLR_Classic/classic PrincipalPermission Example/CPP/source.cpp#1)]
 [!code-csharp[Classic PrincipalPermission Example#1](../../../samples/snippets/csharp/VS_Snippets_CLR_Classic/classic PrincipalPermission Example/CS/source.cs#1)]
 [!code-vb[Classic PrincipalPermission Example#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_Classic/classic PrincipalPermission Example/VB/source.vb#1)]  
  
 O exemplo a seguir demonstra como determinar a identidade da entidade de segurança e as funções disponíveis para a entidade de segurança. Um aplicativo deste exemplo pode ser para confirmar se o usuário atual está em uma função que você permite para usar seu aplicativo.  
  
 [!code-cpp[System.Security.Principal.WindowsBuiltInRole Example#1](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.Security.Principal.WindowsBuiltInRole Example/CPP/source.cpp#1)]
 [!code-csharp[System.Security.Principal.WindowsBuiltInRole Example#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Security.Principal.WindowsBuiltInRole Example/CS/source.cs#1)]
 [!code-vb[System.Security.Principal.WindowsBuiltInRole Example#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Security.Principal.WindowsBuiltInRole Example/VB/source.vb#1)]  
  
## <a name="authorization"></a>Autorização  

A autorização é o processo de determinar se uma entidade de segurança tem permissão para executar uma ação solicitada. A autorização ocorre após a autenticação e usa informações sobre a identidade e as funções da entidade de segurança para determinar quais recursos a entidade de segurança pode acessar. Você pode usar a segurança baseada em função do .NET para implementar a autorização.

## <a name="see-also"></a>Confira também

- [Segurança de ASP.NET Core](/aspnet/core/security/)
