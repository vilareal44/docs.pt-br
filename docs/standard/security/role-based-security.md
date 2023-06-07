---
title: Segurança baseada em função
ms.date: 07/15/2020
ms.technology: dotnet-standard
helpviewer_keywords:
- role-based security, about role-based security
- user authentication, principals
- principals [.NET]
- security [.NET], role-based
- permissions [.NET], principals
- authentication [.NET], principals
- role-based security, principals
ms.assetid: 578cc32b-5654-4d8b-9d8c-ebcbc5c75390
ms.openlocfilehash: ed6c33be5a5da066e238c160da8bff8d25cb68fc
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87555663"
---
# <a name="role-based-security"></a>Segurança baseada em função

As funções geralmente são usadas em aplicativos financeiros ou de negócios para impor a política. Por exemplo, um aplicativo pode impor limites quanto ao tamanho da transação que está sendo processada dependendo se o usuário que está fazendo a solicitação é membro de uma função especificada. Os auxiliares podem ter autorização para processar transações que são menores que um limite especificado, os supervisores podem ter um limite maior e o presidentes pode ter um limite ainda maior (ou nenhum limite). A segurança baseada em função também pode ser usada quando um aplicativo requer várias aprovações para concluir uma ação. Esse caso pode ser um sistema de compra no qual qualquer funcionário possa gerar uma solicitação de compra, mas apenas um agente de compras pode converter essa solicitação em uma ordem de compra que pode ser enviada a um fornecedor.  
  
 A segurança baseada em função do .NET dá suporte à autorização, disponibilizando informações sobre a entidade, que é construída com base em uma identidade associada, disponível para o thread atual. A identidade (e a entidade de segurança que ajuda a definir) pode ser baseada em uma conta do Windows ou ser uma identidade personalizada não relacionada a uma conta do Windows. Os aplicativos .NET podem tomar decisões de autorização com base na identidade da entidade de segurança ou na associação de função, ou ambos. Uma função é um conjunto nomeado de entidades que têm os mesmos privilégios em relação à segurança (como um ou um gerente). Uma entidade de segurança pode ser um membro de uma ou mais funções. Portanto, os aplicativos podem usar a associação de função para determinar se uma entidade de segurança está autorizada a executar uma ação solicitada.  
  
 Para fornecer facilidade de uso e consistência com a segurança de acesso ao código, a segurança baseada em função do .NET fornece <xref:System.Security.Permissions.PrincipalPermission?displayProperty=nameWithType> objetos que permitem que o Common Language Runtime execute a autorização de forma semelhante às verificações de segurança de acesso ao código. A <xref:System.Security.Permissions.PrincipalPermission> classe representa a identidade ou função que o principal deve corresponder e é compatível com verificações de segurança declarativas e imperativas. Você também pode acessar as informações de identidade de uma entidade de segurança diretamente e executar verificações de função e de identidade em seu código quando necessário.  
  
 O .NET fornece suporte à segurança baseada em funções que é flexível e extensível o suficiente para atender às necessidades de um amplo espectro de aplicativos. Você pode optar por interoperar com as infraestruturas de autenticação existentes, como os serviços do COM+ 1,0, ou para criar um sistema de autenticação personalizado. A segurança baseada em função é especialmente adequada para uso em aplicativos Web ASP.NET, que são processados principalmente no servidor. No entanto, a segurança baseada em função do .NET pode ser usada no cliente ou no servidor.  
  
 Antes de ler esta seção, verifique se você entendeu o material apresentado nos [principais conceitos de segurança](key-security-concepts.md).  
  
## <a name="see-also"></a>Confira também
  
- [Objetos Principal e Identity](principal-and-identity-objects.md)
- [Conceitos principais de segurança](key-security-concepts.md)
- <xref:System.Security.Permissions.PrincipalPermission?displayProperty=nameWithType>
- [Segurança de ASP.NET Core](/aspnet/core/security/)
