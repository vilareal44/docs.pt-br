---
title: Como desabilitar sessões seguranças em uma WSFederationHttpBinding
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, federation
- federation
ms.assetid: 675fa143-6a4e-4be3-8afc-673334ab55ec
ms.openlocfilehash: df057d64feb89d1e43b938b36cb48f2f103b17d0
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595382"
---
# <a name="how-to-disable-secure-sessions-on-a-wsfederationhttpbinding"></a>Como desabilitar sessões seguranças em uma WSFederationHttpBinding

Alguns serviços podem exigir credenciais federadas, mas não dar suporte a sessões seguras. Nesse caso, você deve desabilitar o recurso de sessão segura. Ao contrário do <xref:System.ServiceModel.WSHttpBinding> , a <xref:System.ServiceModel.WSFederationHttpBinding> classe não fornece uma maneira de desabilitar sessões seguras ao se comunicar com um serviço. Em vez disso, você deve criar uma associação personalizada que substitua as configurações de sessão segura por uma inicialização.

Este tópico demonstra como modificar os elementos de associação contidos em um <xref:System.ServiceModel.WSFederationHttpBinding> para criar uma associação personalizada. O resultado é idêntico ao <xref:System.ServiceModel.WSFederationHttpBinding> , exceto que ele não usa sessões seguras.

## <a name="to-create-a-custom-federated-binding-without-secure-session"></a>Para criar uma associação federada personalizada sem sessão segura

1. Crie uma instância da classe de forma <xref:System.ServiceModel.WSFederationHttpBinding> imperativa no código ou carregando uma do arquivo de configuração.

2. Clone o <xref:System.ServiceModel.WSFederationHttpBinding> em um <xref:System.ServiceModel.Channels.CustomBinding> .

3. Localize o <xref:System.ServiceModel.Channels.SecurityBindingElement> no <xref:System.ServiceModel.Channels.CustomBinding> .

4. Localize o <xref:System.ServiceModel.Security.Tokens.SecureConversationSecurityTokenParameters> no <xref:System.ServiceModel.Channels.SecurityBindingElement> .

5. Substitua o original <xref:System.ServiceModel.Channels.SecurityBindingElement> pelo elemento de associação de segurança de bootstrap do <xref:System.ServiceModel.Security.Tokens.SecureConversationSecurityTokenParameters> .

## <a name="example"></a>Exemplo

Este exemplo a seguir cria uma associação federada personalizada sem sessão segura.

[!code-csharp[c_CustomFederationBinding#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customfederationbinding/cs/c_customfederationbinding.cs#0)]
[!code-vb[c_CustomFederationBinding#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customfederationbinding/vb/c_customfederationbinding.vb#0)]

## <a name="compiling-the-code"></a>Compilando o código

- Para compilar o exemplo de código, crie um projeto que referencie o assembly System. ServiceModel. dll.

## <a name="see-also"></a>Consulte também

- [Associações e segurança](bindings-and-security.md)
