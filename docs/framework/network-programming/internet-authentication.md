---
title: Autenticação da Internet
description: Saiba mais sobre a variedade de mecanismos de autenticação de cliente que o System.Net classes dá suporte para seus aplicativos no .NET Framework.
ms.date: 03/30/2017
helpviewer_keywords:
- authentication [.NET Framework], classes
- IAuthenticationModule interface
- ICredentialLookup interface
- CredentialCache class, about CredentialCache class
- receiving data, authentication
- AuthenticationManager class, about AuthenticationManager class
- Internet, authentication
- sending data, authentication
- network resources, authentication
- user authentication, classes for authentication
- NetworkCredential class, about NetworkCredential class
- client authentication, classes for authentication
ms.assetid: d342e87c-f672-4660-a513-41a2f2b80c4a
ms.openlocfilehash: a1f0829aa0e9e4bcc68168b73443578c3a34310b
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502373"
---
# <a name="internet-authentication"></a>Autenticação da Internet
As classes <xref:System.Net> dão suporte a uma variedade de mecanismos de autenticação de cliente, incluindo os métodos de autenticação padrão de Internet básico, digest, negotiate, NTLM e a autenticação Kerberos, bem como métodos personalizados criados por você.  
  
 As credenciais de autenticação são armazenadas nas classes <xref:System.Net.NetworkCredential> e <xref:System.Net.CredentialCache> que implementam a interface <xref:System.Net.ICredentials>. Quando uma dessas classes é consultada para credenciais, ela retorna uma instância da classe **NetworkCredential**. O processo de autenticação é gerenciado pela classe <xref:System.Net.AuthenticationManager> e o processo de autenticação real é executado por uma classe de módulo de autenticação que implementa a interface <xref:System.Net.IAuthenticationModule>. Você deve registrar um módulo de autenticação personalizado com o **AuthenticationManager** antes que ele possa ser usado; módulos para métodos de autenticação básica, digest, negotiate, NTLM e Kerberos são registrados por padrão.  
  
 **NetworkCredential** armazena um conjunto de credenciais associadas a um único recurso de Internet identificado por um URI e os retorna em resposta a qualquer chamada para o método <xref:System.Net.NetworkCredential.GetCredential%2A>. A classe **NetworkCredential** normalmente é usada por aplicativos que acessam um número limitado de recursos da Internet ou por aplicativos que usam o mesmo conjunto de credenciais em todos os casos.  
  
 A classe **CredentialCache** armazena uma coleção de credenciais para vários recursos da Web. Quando o método <xref:System.Net.CredentialCache.GetCredential%2A> é chamado, **CredentialCache** retorna o conjunto apropriado de credenciais, conforme determinado pelo URI do recurso da Web e o esquema de autenticação solicitado. Aplicativos que usam uma variedade de recursos de Internet com diferentes esquemas de autenticação se beneficiam do uso da classe **CredentialCache**, visto que ele armazena todas as credenciais e as fornece conforme solicitado.  
  
 Quando um recurso de Internet solicita autenticação, o método <xref:System.Net.WebRequest.GetResponse%2A?displayProperty=nameWithType> envia o <xref:System.Net.WebRequest> para o **AuthenticationManager** junto com a solicitação de credenciais. A solicitação é então autenticada acordo com o processo a seguir:  
  
1. O **AuthenticationManager** chama o método <xref:System.Net.IAuthenticationModule.Authenticate%2A> em cada um dos módulos de autenticação registrados na ordem em que eles foram registrados. O **AuthenticationManager** usa o primeiro módulo que não retorna **nulo** para executar o processo de autenticação. Os detalhes do processo variam dependendo do tipo de módulo de autenticação envolvido.  
  
2. Quando o processo de autenticação é concluído, o módulo de autenticação retorna um <xref:System.Net.Authorization> para o **WebRequest** que contém as informações necessárias para acessar o recurso de Internet.  
  
 Alguns esquemas de autenticação podem autenticar um usuário sem primeiro fazer uma solicitação para um recurso. Um aplicativo pode economizar tempo pré-autenticando o usuário com o recurso, eliminando assim pelo menos uma viagem de ida e volta ao servidor. Ou então, pode executar a autenticação durante a inicialização de programa a fim de ser mais responsivo para o usuário mais tarde. Esquemas de autenticação que podem usar pré-autenticação definem a propriedade <xref:System.Net.IAuthenticationModule.PreAuthenticate%2A> como **true**.  
  
## <a name="see-also"></a>Confira também

- [Autenticação básica e resumida](basic-and-digest-authentication.md)
- [Autenticação NTLM e Kerberos](ntlm-and-kerberos-authentication.md)
- [Segurança na programação de rede](security-in-network-programming.md)
